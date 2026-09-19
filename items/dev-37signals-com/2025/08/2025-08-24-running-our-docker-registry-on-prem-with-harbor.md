---
title: Running our Docker registry on-prem with Harbor
link: https://dev.37signals.com/running-our-docker-registry-on-prem-with-harbor/
source: dev-37signals-com
published: 2025-08-24T17:00:00Z
updated: 2025-08-24T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Farah Schüller
summary: On hosting images without the price tag.
content: extracted
html: 2025-08-24-running-our-docker-registry-on-prem-with-harbor.html
preview:
  file: 2025-08-24-running-our-docker-registry-on-prem-with-harbor.preview-d726809d3e79.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#0d0d0d'
images:
- source: https://dev.37signals.com/assets/images/opengraph/running-our-docker-registry-on-prem-with-harbor.png
  original:
    file: 2025-08-24-running-our-docker-registry-on-prem-with-harbor.image-5c9e9119d7a4.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/running-our-docker-registry-on-prem-with-harbor.png
---

As of early 2025, we’re deploying all of our applications with [Kamal](https://kamal-deploy.org/) using Docker as our containerization platform. The container registry that holds our app images is one of the most integral pieces of our deployment pipeline.

Like many organizations, we’d been using external container registries for years. Our ecosystem was tightly coupled to both Dockerhub and Amazon’s Elastic Container Registry.

However, as part of our [cloud exit](https://world.hey.com/dhh/we-have-left-the-cloud-251760fb) and [kamalization](https://dev.37signals.com/bringing-our-apps-back-home/) journey, several issues started emerging:

- **Cost:** Not only does the paid license for Dockerhub produce a considerable invoice — pulling and pushing our images over the internet dozens of times a day caused us to hit the contracted bandwidth limit with our datacenter provider [Deft](https://www.summithq.com/) repeatedly. We tried working around this by running pull-through caches, but this still locked us to Dockerhub.
- **Performance:** Migrating HEY to Kamal and expanding the deployment to another continent caused deploy time penalties — up to 45 seconds on uncached pulls per host. This was exacerbated once our largest application [Basecamp 4](https://basecamp.com/) was moved to Kamal — suddenly deployments took minutes longer simply because of push/pull speeds out of our control.
- **Security and Governance:** We all hope to never leak credentials in our images, and yet it still happens — the scale ranging from easily mitigated to catastrophic. We wanted to eliminate that threat surface once and for all by keeping our artifacts where they belong — with us.
- **Independence:** Despite being on a paid account, we fell into the crunch of API limitations for arbitrary reasons a couple of times. In addition, we’d been keeping all of the images used in our Chef CI/CD infrastructure still on AWS.

Our criteria for the solution to pick were fairly simple: reliable, performant, easy to set up, open-source.

We evaluated running the [default distribution implementation](https://hub.docker.com/_/registry) as our registry, but quickly set our eyes on [Harbor](https://goharbor.io/). Harbor provided us with a more expandable and rich feature set right out of the box, and required minimal extra tooling to make it robust and scalable.

* * *

## Setting up Harbor

Harbor’s deployment is optimized for using it within Kubernetes environments, but the single-server setup using the pre-packaged docker-compose configuration proved to be exactly what we were looking for.

We had three key points to cover in our plan for the v1 of our on-premise registry:

- Use our own S3 storage.
- Make sure we have at least two replicating sites that can be easily failed over.
- Keep the storage footprint as small as possible by enabling retention policies.

### Configuring S3 storage

At 37signals, we’re running our own [Pure FlashBlade](https://www.purestorage.com/products/unstructured-data-storage/flashblade-s.html) storage cluster providing us with S3 object storage right out of the box, but for Harbor, any S3-compatible backend will do.

The configuration in Harbor was easy, but it was crucial to get the permissions set right on the Pure backend. You can obviously run an `s3:*` policy, but let’s be real, we want to do better! After some trial and error with broken image pushes, these are the minimal permissions needed on the bucket to operate Harbor with a custom S3 backend:

```
s3:AbortMultipartUpload
s3:DeleteObject
s3:GetBucketLocation
s3:GetObject
s3:ListBucket
s3:ListBucketMultipartUploads
s3:ListMultipartUploadParts
s3:PutObject
```

### Configuring multiple instances

For the v1 of the Harbor deployment, we opted to run two stand-alone instances at first: one in our Ashburn and one in our Chicago location.

Harbor comes with several components, such as PostgreSQL and Redis services, handling manifest/user management and job scheduling. We explored an elaborate HA per datacenter with colocated instances of those services, but decided to wait for the first results of the all-in-one stand-alone deployment before making it more complicated than it has to be.

This is an excerpt of our `harbor.yml` in use, which gives you a functional instance, including the s3 configuration and enabled monitoring:

```
hostname: "#{node['fqdn']}"
http:
  port: 80
data_volume: /data
harbor_admin_password: "#{admin_password}"
storage_service:
  s3:
    bucket: docker-registry-bucket
    accesskey: "#{bucket_credentials['access_key']}"
    secretkey: "#{bucket_credentials['secret_key']}"
    regionendpoint: "https://purestorage.#{node["domain"]}"
    region: us-east-1
    encrypt: false
    secure: true
    v4auth: true
    chunksize: 5242880
    loglevel: debug
metric:
  enabled: true
  port: 9090
  path: /metrics
database:
  password: "#{db_password}"
  max_idle_conns: 50
  max_open_conns: 100
clair:
  updaters_interval: 12
jobservice:
  max_job_workers: 20
  job_loggers:
    - FILE
  logger_sweeper_duration: 3600
log:
  level: info
  local:
    rotate_count: 50
    rotate_size: 200M
    location: /var/log/harbor
notification:
  webhook_job_max_retry: 3
  webhook_job_http_client_timeout: 10
_version: 2.10.0
```

As you can see, it is a fairly default config. Encapsulated in a Chef recipe, this will be executed on the respective nodes in each DC, setting the correct FQDN and pointing to the correct storage endpoint. These nodes are then fronted by our F5 loadbalancers for SSL termination and region-specific domains.

Each Harbor node is currently a virtual machine equipped with 64GB of RAM, 32 vCPU and 320GB of storage.

### Configuring replication

The initial Chef setup only needs to run once for bootstrapping. For further configuration we decided to rely on the [terraform provider for Harbor](https://registry.terraform.io/providers/goharbor/harbor/3.10.21). In addition to the initial user management setup, here we could also configure replication between the endpoints easily. We decided on a two-way replication scheme to keep it all in sync, inspired by [this setup](https://www.unix-experience.fr/en/devops/harbor_multidc_replicated/).

- Images are pushed to a registry endpoint.
- The endpoint pulls data from the opposite registry every 10 minutes.

```
resource "harbor_replication" "replication_push_sc_chi" {
  provider               = harbor.sc-chi
  name                   = "Replicate images on push to df-iad"
  action                 = "push"
  registry_id            = harbor_registry.df_iad.registry_id
  schedule               = "event_based"
  dest_namespace_replace = -1
  filters {
    name = "**"
  }
  filters {
    tag = "**"
  }
}

resource "harbor_replication" "replication_pull_sc_chi" {
  provider               = harbor.sc-chi
  name                   = "Replicate missing images/artifacts from df-iad"
  action                 = "pull"
  registry_id            = harbor_registry.df_iad.registry_id
  schedule               = "0 0/10 * * * *"
  enabled                = false
  dest_namespace_replace = -1
  filters {
    name = "**"
  }
  filters {
    tag = "**"
  }
}
```

In addition, we’re replicating the underlying S3 buckets directly on our Pure cluster for an extra failsafe backup mechanism. It’s important to note however that this is **not enough** to make Harbor aware of the data in the other location — explicit replication on the Harbor level like the setup above must be configured.

* * *

## Syncing the catalogue

You could of course start with an empty registry and fill it as you go, but this isn’t very feasible if you want a drop-in replacement for your current registry. In our case, we had to make sure that the entire image catalog from Dockerhub gets copied into Harbor — the challenge being that this meant dealing with 80+ individual image repositories. Thankfully, Harbor offers replication directly from Dockerhub, so we opted for that.

Sounds straightforward? Here’s a funny caveat — depending on the amount of repositories you want to fetch and replicate, Dockerhub is likely going to throttle you on the API level if you want to do this all at once. You could totally write a functional replication rule that just targets `**/**`, only to be showered with `429` — even on a paid account.

Thus, the replication has to happen in batches. For this, we chose to create individual replication rules per repository with a “manual” (also scripted) trigger to avoid overloading the API.

The definition for the replication rules in Terraform:

```
variable "repositories" {
  type = map(string)
}

resource "harbor_registry" "dockerhub" {
  provider      = harbor.sc-chi
  provider_name = "docker-hub"
  name          = "DockerHub"
  endpoint_url  = "https://registry-1.docker.io"
  description   = "Endpoint for replicating the existing catalogue"
  access_id     = var.dockerhub_username
  access_secret = var.dockerhub_password
}

resource "harbor_replication" "dockerhub_mirror" {
  for_each = var.repositories

  provider               = harbor.sc-chi
  name                   = "mirror-dockerhub-${each.key}"
  description            = "Replicate and mirror images from DockerHub"
  registry_id            = harbor_registry.dockerhub.registry_id
  dest_namespace         = "yourorg"
  override               = true
  dest_namespace_replace = 1
  copy_by_chunk          = true

  filters {
    name = "yourorg/${each.key}"
  }

  filters {
    tag = "**"
  }

  action = "pull"
}
```

The script to pull out a list of repositories from Dockerhub, make it accessible to Terraform and create the individual replication rules per repository:

dockerhub-to-harbor.sh

```
#!/usr/bin/env bash

set -euo pipefail

# -----------------------------
# Config
# -----------------------------
DOCKERHUB_USER="${DOCKERHUB_USER:-your-dockerhub-username}"
DOCKERHUB_PASSWORD="${DOCKERHUB_PASSWORD:-your-dockerhub-password}"
DOCKERHUB_ORG="yourorg"
TFVARS_DIR="./generated_tfvars"
TFVARS_FILE="$TFVARS_DIR/all_repos.tfvars.json"

mkdir -p "$TFVARS_DIR"

# -----------------------------
# Authentication
# -----------------------------
echo "🔐 Getting Docker Hub token..."
TOKEN=$(curl -s -X POST https://hub.docker.com/v2/users/login/ \
  -H "Content-Type: application/json" \
  -d '{"username": "'"$DOCKERHUB_USER"'", "password": "'"$DOCKERHUB_PASSWORD"'"}' |
  jq -r .token)

if [[ "$TOKEN" == "null" || -z "$TOKEN" ]]; then
  echo "❌ Failed to authenticate. Check Docker Hub credentials."
  exit 1
fi

export AUTH_HEADER="Authorization: Bearer $TOKEN"

# -----------------------------
# Helper Functions
# -----------------------------
fetch_repos_starting_with() {
  local letter="$1"
  local page=1
  local repos=()

  while :; do
    local url="https://hub.docker.com/v2/repositories/${DOCKERHUB_ORG}/?page=$page&page_size=100"
    local response=$(curl -s -H "$AUTH_HEADER" "$url")

    local matched=$(echo "$response" |
      jq -r ".results[] | select(.name | startswith(\"$letter\")) | .name")

    repos+=($matched)

    local next=$(echo "$response" | jq -r ".next")
    [[ "$next" == "null" ]] && break
    ((page++))
  done

  echo "${repos[@]}"
}

generate_tfvars_file() {
  local repos=("$@")
  local tfvars_file="$TFVARS_FILE"

  echo "{ \"repositories\": {" > "$tfvars_file"
  for repo in "${repos[@]}"; do
    echo "  \"$repo\": \"$repo\"," >> "$tfvars_file"
  done
  sed -i '' '$ s/,$//' "$tfvars_file"
  echo "} }" >> "$tfvars_file"

  >&2 echo "💾 Created tfvars file: $tfvars_file"
  >&2 ls -l "$tfvars_file"

  # Only echo the filename to stdout
  echo "$tfvars_file"
}

run_terraform_once() {
  local tfvars="$1"

  local abs_tfvars
  abs_tfvars=$(realpath "$tfvars")

  echo "🔁 Applying Terraform with $abs_tfvars"

  (
    cd harbor-production || exit 1
    local rel_tfvars
    rel_tfvars=$(python3 -c "import os.path; print(os.path.relpath('$abs_tfvars', '.'))")

    terraform apply -var-file="$rel_tfvars" -auto-approve
  )
}

# -----------------------------
# Execution
# -----------------------------
main() {
  echo "🚀 Starting repository sync..."
  letters=(a b c d e f g h i j k l m n o p q r s t u v w x y z)
  all_repos=()

  for letter in "${letters[@]}"; do
    echo "📦 Fetching repos for prefix: $letter"
    repos=($(fetch_repos_starting_with "$letter"))
    all_repos+=("${repos[@]}")
  done

  if [[ "${#all_repos[@]}" -eq 0 ]]; then
    echo "⚠️ No repositories found."
    exit 0
  fi

  tfvars_file=$(generate_tfvars_file "${all_repos[@]}" 2>/dev/null)
  run_terraform_once "$tfvars_file"

  echo "✅ All repositories synced."
}

main "$@"
```

And the script to enable those rules alphabetically in batches:

trigger-replication.sh

```
#!/usr/bin/env bash
set -euo pipefail

# -----------------------------
# Config
# -----------------------------
HARBOR_URL="${HARBOR_URL:-https://registry.yourdomain.com}"
HARBOR_USER="${HARBOR_USER:-your-harbor-user}"
HARBOR_PASSWORD="${HARBOR_PASSWORD:-your-harbor-password}"
BATCH_DELAY=10  # seconds between batches
PREFIX="mirror-dockerhub-"

# -----------------------------
# CLI Args
# -----------------------------
DRY_RUN=false
RANGE_START="a"
RANGE_END="z"

usage() {
  cat <<EOF
Usage: $0 [--dry-run] [--range <start-end>]

Options:
  --dry-run         Only print the rules that would be triggered, no API calls.
  --range a-d       Trigger only rules whose names start with '${PREFIX}' plus letter in <start-end>.
                    Example: --range a-d
EOF
  exit 1
}

while [[ $# -gt 0 ]]; do
  case "$1" in
    --dry-run)
      DRY_RUN=true
      shift
      ;;
    --range)
      if [[ "$2" =~ ^[a-zA-Z]-[a-zA-Z]$ ]]; then
        RANGE_START=$(echo "$2" | cut -d- -f1 | tr '[:upper:]' '[:lower:]')
        RANGE_END=$(echo "$2" | cut -d- -f2 | tr '[:upper:]' '[:lower:]')
        shift 2
      else
        echo "Invalid range format. Expected like 'a-d'."
        usage
      fi
      ;;
    *)
      echo "Unknown argument: $1"
      usage
      ;;
  esac
done

if [[ "$RANGE_START" > "$RANGE_END" ]]; then
  echo "Invalid range: start ($RANGE_START) > end ($RANGE_END)"
  exit 1
fi

# -----------------------------
# Auth & Token
# -----------------------------
echo "🔐 Authenticating with Harbor..."
AUTH_HEADER="Authorization: Basic $(echo -n "$HARBOR_USER:$HARBOR_PASSWORD" | base64)"

curl -s -H "$AUTH_HEADER" "$HARBOR_URL/api/v2.0/users/current" | jq -e .username > /dev/null || {
  echo "❌ Harbor auth failed"
  exit 1
}

echo "📋 Fetching all replication rules..."
rules=$(curl -s -H "$AUTH_HEADER" "$HARBOR_URL/api/v2.0/replication/policies?page_size=100")

declare -A letter_to_ids

while IFS= read -r rule; do
  id=$(echo "$rule" | jq -r '.id')
  name=$(echo "$rule" | jq -r '.name')

  # Filter by prefix first
  if [[ "$name" == "$PREFIX"* ]]; then
    # Get letter after the prefix
    suffix_letter=$(echo "${name#$PREFIX}" | cut -c1 | tr '[:upper:]' '[:lower:]')
    if [[ "$suffix_letter" < "$RANGE_START" || "$suffix_letter" > "$RANGE_END" ]]; then
      continue
    fi
    letter_to_ids["$suffix_letter"]+="$id "
  fi
done < <(echo "$rules" | jq -c '.[]')

echo "✅ Loaded replication rules matching prefix '$PREFIX'."

increment_letter() {
  local c=$1
  printf "\\$(printf '%03o' "$(( $(printf '%d' "'$c") + 1 ))")"
}

current="$RANGE_START"
while [[ $current < $RANGE_END || $current == $RANGE_END ]]; do
  ids=${letter_to_ids[$current]:-}
  if [[ -n "$ids" ]]; then
    echo "🚀 Processing prefix '$PREFIX$current' with rule IDs: $ids"
    for id in $ids; do
      if $DRY_RUN; then
        echo "   (dry-run) Would trigger rule ID: $id"
      else
        echo "   🔁 Triggering rule ID: $id"
        curl -s -X POST -H "$AUTH_HEADER" \
          -H "Content-Type: application/json" \
          -d "{\"policy_id\": $id}" \
          "$HARBOR_URL/api/v2.0/replication/executions" > /dev/null
      fi
    done
    if ! $DRY_RUN; then
      echo "⏳ Waiting $BATCH_DELAY seconds before next batch..."
      sleep "$BATCH_DELAY"
    fi
  fi
  current=$(increment_letter "$current")
done

echo "✅ Done."
```

The progress of all replication tasks at the same time is quite hard to monitor within Harbor’s UI (despite excellent logging). Thus, another small script helped summarize this:

harbor-replication-monitor.sh

```
#!/usr/bin/env bash

HARBOR_URL="${HARBOR_URL:-https://registry.yourdomain.com}"
HARBOR_USER="${HARBOR_USER:-your-harbor-user}"
HARBOR_PASSWORD="${HARBOR_PASSWORD:-your-harbor-password}"

# Check required env vars
if [[ -z "$HARBOR_USERNAME" || -z "$HARBOR_PASSWORD" || -z "$HARBOR_URL" ]]; then
  echo "❌ Missing HARBOR_USERNAME, HARBOR_PASSWORD or HARBOR_URL. Set them as env vars."
  exit 1
fi

# -----------------------------
# Auth
# -----------------------------
echo "🔐 Authenticating with Harbor..."
pong=$(curl -s -u "$HARBOR_USERNAME:$HARBOR_PASSWORD" "$HARBOR_URL/ping")
if [ "$pong" != "Pong" ]; then
  echo "❌ Authentication failed. Harbor did not return expected 'Pong'."
  echo "Response: $pong"
  exit 1
fi
echo "✅ Auth successful."

# -----------------------------
# Fetch executions
# -----------------------------
echo "📋 Fetching replication executions..."
executions=$(curl -s -u "$HARBOR_USERNAME:$HARBOR_PASSWORD" "$HARBOR_URL/replication/executions?page_size=100")

# Cache for policy ID to name mapping
declare -A POLICY_NAMES
get_policy_name() {
  local policy_id="$1"
  if [[ -n "${POLICY_NAMES[$policy_id]}" ]]; then
    echo "${POLICY_NAMES[$policy_id]}"
  else
    local name=$(curl -s -u "$HARBOR_USERNAME:$HARBOR_PASSWORD" "$HARBOR_URL/replication/policies/$policy_id" | jq -r '.name // "unknown"')
    POLICY_NAMES[$policy_id]="$name"
    echo "$name"
  fi
}

# -----------------------------
# Build table
# -----------------------------
rows=()
while IFS= read -r exec; do
  id=$(echo "$exec" | jq -r '.id')
  policy_id=$(echo "$exec" | jq -r '.policy_id')
  status=$(echo "$exec" | jq -r '.status')
  start_time=$(echo "$exec" | jq -r '.start_time' | sed 's/\.[0-9]*Z$/Z/')

  start_epoch=$(date -u -d "$start_time" +%s 2>/dev/null)
  now_epoch=$(date +%s)
  runtime_min=$(( (now_epoch - start_epoch) / 60 ))

  policy_name=$(get_policy_name "$policy_id")

  row=$(printf "%-8s %-10s %-25s %-12s %-14s %s" "$id" "$policy_id" "$policy_name" "$status" "$runtime_min" "$start_time")
  rows+=("$runtime_min $row")
done < <(echo "$executions" | jq -c '.[] | select(.status == "InProgress")')

echo
echo "🟡 In-progress replication tasks (sorted by runtime):"
printf "%-8s %-10s %-25s %-12s %-14s %s\n" "ID" "Policy_ID" "Rule Name" "Status" "Runtime(min)" "Start Time"

# -----------------------------
# Print
# -----------------------------
for line in "${rows[@]}"; do
  echo "$line"
done | sort -rn | cut -d' ' -f2-
```

After enabling those rules in batches, it’s also crucial to make sure enough job worker resources are available to sufficiently speed up this process.

* * *

## Analyzing performance

After migrating all our Kamal-ized apps to push and pull from our new on-premise registry, it was finally time to actually get some numbers on performance in. We grabbed this data directly from the deployment logs printed by Kamal.

You can use these quick one-liners for extracting the pull times from the Kamal log on your terminal:

```
# Mac
pbpaste | sort | sed 's/  INFO //' | grep -E "Running docker pull" -B1 | grep Finished | awk '{print $4}' | sort | tr '\n' ' '
# Linux
xclip -o | sort | sed 's/  INFO //' | grep -E "Running docker pull" -B1 | grep Finished | awk '{print $4}' | sort | tr '\n' ' '
```

Or from a logfile:

```
awk '
/Running .*docker pull/ {
  if (match($0, /\[([a-f0-9]+)\]/, m)) {
    id = m[1]
    running[id] = $0
  }
}
/Finished/ {
  if (match($0, /\[([a-f0-9]+)\]/, m)) {
    id = m[1]
    if (id in running) {
      print running[id]
      print $0
      print ""
      delete running[id]
    }
  }
}
' /path/to/kamal.log
```

After analyzing the numbers, we were quite happy to see that:

- The overall image pull timings on our fleet **decreased by up to 25 seconds** for HEY, Basecamp 4 and Basecamp 2 (our three largest apps), with the lion’s share of improvement on our HEY nodes on the Amsterdam outposts.
- Deploy times **decreased by 15 seconds for HEY**.

In addition, it allowed us to:

- Retire the Dockerhub cache setup, further detangling our infrastructure.
- Implement proper retention policies and garbage collection to decrease the overall storage quota from almost 9 TiB to 1.5 TiB.
- Save roughly $5k/year on subscription fees going forward.

**Remember:** this is basically a single-node infrastructure, with the primary endpoint being in Chicago, and the Ashburn site providing the backup. We found that this small setup has been reliable for roughly two months now. During this time, Harbor has served more than 32,000 pulls under company-wide use in day-to-day business.

* * *

## Conclusion

This project proved to us that it’s — again — worth considering a departure from large SaaS offerings and public cloud providers. We’ve been dependent on external registries keeping our app images for years, but the simplicity and benefits of our current setup give little reason to doubt that cutting the cord was the right decision: better performance at less cost with minimal infrastructure.
