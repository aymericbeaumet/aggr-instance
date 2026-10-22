---
title: Self-Hosted Orbs
link: https://ampcode.com/news/self-hosted-orbs
source: ampcode-com
published: 2026-10-22T00:00:00Z
updated: 2026-10-22T00:00:00Z
first_seen: 2026-10-22T17:33:41.202070908Z
summary: 'You can now self-host orbs. You provide the compute and Amp turns it into orbs. If you have compute that can quickly start new machines put them to sleep and resume them equally fast keep them around for a long time scale them to zero, so no one has to worry about having a lot of them sleeping then you are now able to replicate the orb experience within your own infrastructure. How It Works You run amp orb provider inside your infrastructure. The orb provider then reaches out to Amp and waits for requests to spawn, pause, or delete orbs. Like this: How you satisfy those requests is up to you, because amp orb provider will dispatch the requests for orb operations (spawn to create or resume orbs, pause, and delete) to a program that you provide, like so: $ amp orb provider ./dispatch.sh In that program, you can do whatever works best for your infrastructure. Here are some illustrative examples: Docker E2B K8s Apple containers Modal Vercel Daytona exe.dev #!/usr/bin/env bash # dispatch.sh set -euo pipefail case "$ORB_OPERATION" in spawn) docker start "$ORB_ID" 2>/dev/null || docker run --platform linux/amd64 -d --name "$ORB_ID" \ "$ORB_IMAGE" sleep infinity docker exec -d --env ORB_TOKEN "$ORB_ID" amp orb bootstrap ;; pause) docker stop "$ORB_ID" ;; delete) docker rm -f "$ORB_ID" ;; esac Install the E2B CLI and the e2b package, set E2B_API_KEY, and build a template: echo ''FROM us-central1-docker.pkg.dev/amp-prod-0/amp-orb/amp-orb:latest'' \ > e2b.Dockerfile e2b template create amp-orbs --dockerfile e2b.Dockerfile \ --cpu-count 4 --memory-mb 8192 --ready-cmd true export E2B_TEMPLATE=amp-orbs #!/usr/bin/env node // dispatch.mjs import { Sandbox } from ''e2b'' const { ORB_OPERATION, ORB_ID, ORB_TOKEN, AMP_URL, E2B_TEMPLATE, } = process.env const metadata = { orbId: ORB_ID } const [existing] = await Sandbox.list({ query: { metadata, state: [''running'', ''paused''] }, }).nextItems() if (ORB_OPERATION === ''spawn'') { const timeoutMs = 60 * 60 * 1000 const sandbox = existing ? await Sandbox.connect(existing.sandboxId, { timeoutMs }) : await Sandbox.create(E2B_TEMPLATE, { metadata, timeoutMs, lifecycle: { onTimeout: ''pause'' }, }) const envs = { ORB_TOKEN } if (AMP_URL) envs.AMP_URL = AMP_URL const bootstrap = await sandbox.commands.run(''amp orb bootstrap'', { user: ''user'', envs, background: true, timeoutMs: 0, }) await bootstrap.disconnect() } else if (ORB_OPERATION === ''pause'' && existing) { await Sandbox.pause(existing.sandboxId) } else if (ORB_OPERATION === ''delete'' && existing) { await Sandbox.kill(existing.sandboxId) } #!/usr/bin/env bash # dispatch.sh set -euo pipefail export PATH="/usr/local/bin:/opt/homebrew/bin:$PATH" # "$ORB_IMAGE" is the Amp orb image. ids=$(container list --all --quiet) exists() { grep -Fxq "$ORB_ID" <<<"$ids"; } case "$ORB_OPERATION" in spawn) if exists; then container start "$ORB_ID" else container run --detach --init --rosetta \ --platform linux/amd64 --cpus 4 --memory 8G \ --name "$ORB_ID" "$ORB_IMAGE" sleep infinity fi # A bare --env NAME copies the value from the host. container exec --detach --env ORB_TOKEN --env AMP_URL \ "$ORB_ID" amp orb bootstrap ;; pause) if exists; then container stop "$ORB_ID" fi ;; delete) if exists; then container delete --force "$ORB_ID" fi ;; *) exit 1 ;; esac #!/usr/bin/env python3 # dispatch.py import os import modal # Needs `pip install modal` plus MODAL_TOKEN_ID and MODAL_TOKEN_SECRET. APP = "amp-orbs" app = modal.App.lookup(APP, create_if_missing=True) snapshots = modal.Dict.from_name("amp-orb-snapshots", create_if_missing=True) orb_id = os.environ["ORB_ID"] try: sandbox = modal.Sandbox.from_name(APP, orb_id) except modal.exception.NotFoundError: sandbox = None match os.environ["ORB_OPERATION"]: case "spawn": if sandbox is None: # Modal has no pause, so resume from the last snapshot. snapshot = snapshots.get(orb_id) if snapshot: image = modal.Image.from_id(snapshot) else: image = modal.Image.from_registry(os.environ["ORB_IMAGE"]) sandbox = modal.Sandbox.create( "sleep", "infinity", app=app, name=orb_id, image=image, cpu=4, memory=(16384, 16384), timeout=24 * 60 * 60, ) # Modal execs run as root; point bootstrap at the image user''s home. bootstrap = "setsid amp orb bootstrap </dev/null >/dev/null 2>&1 &" sandbox.exec( "bash", "-c", bootstrap, env={ "ORB_TOKEN": os.environ["ORB_TOKEN"], "AMP_URL": os.environ.get("AMP_URL"), "HOME": "/home/user", "USER": "user", }, ).wait() case "pause": if sandbox: image_id = sandbox.snapshot_filesystem(timeout=120).object_id snapshots[orb_id] = image_id sandbox.terminate() case "delete": if sandbox: sandbox.terminate() snapshots.pop(orb_id, None) #!/usr/bin/env node // dispatch.mjs import { APIError, Sandbox, Snapshot } from ''@vercel/sandbox'' const { ORB_OPERATION, ORB_ID, ORB_IMAGE, ORB_TOKEN, AMP_URL } = process.env const credentials = { token: process.env.VERCEL_TOKEN, teamId: process.env.VERCEL_TEAM_ID, projectId: process.env.VERCEL_PROJECT_ID, } if (ORB_OPERATION === ''spawn'') { const sandbox = await Sandbox.getOrCreate({ ...credentials, name: ORB_ID, // Copy Amp''s image to VCR, then set ORB_IMAGE to its VCR URL. image: ORB_IMAGE, resources: { vcpus: 4 }, timeout: 60 * 60 * 1000, persistent: true, snapshotExpiration: 0, keepLastSnapshots: { count: 1, expiration: 0, deleteEvicted: true }, }) const setup = await sandbox.runCommand({ cmd: ''/bin/bash'', args: [''-c'', ''hostname localhost && mkdir -p /vercel/sandbox''], sudo: true, }) if (setup.exitCode !== 0) throw new Error(await setup.stderr()) await sandbox.runCommand({ cmd: ''/usr/bin/setpriv'', args: [ ''--reuid=user'', ''--regid=user'', ''--init-groups'', ''--'', ''/usr/bin/env'', `ORB_TOKEN=${ORB_TOKEN}`, ...(AMP_URL ? [`AMP_URL=${AMP_URL}`] : []), ''HOME=/home/user'', ''USER=user'', ''/bin/bash'', ''-lc'', ''exec amp orb bootstrap'', ], sudo: true, detached: true, }) } else if (ORB_OPERATION === ''pause'' || ORB_OPERATION === ''delete'') { try { const sandbox = await Sandbox.get({ ...credentials, name: ORB_ID, resume: false, }) if (ORB_OPERATION === ''delete'') { if (sandbox.status === ''running'') await sandbox.stop() for await (const saved of await sandbox.listSnapshots()) { if (saved.status !== ''created'') continue const snapshot = await Snapshot.get({ ...credentials, snapshotId: saved.id, }) await snapshot.delete() } await sandbox.delete() } else if (sandbox.status === ''running'') await sandbox.stop() } catch (error) { if (!(error instanceof APIError && error.response.status === 404)) { throw error } } } #!/usr/bin/env python3 # dispatch.py import os from uuid import uuid4 from daytona import ( Daytona, DaytonaNotFoundError, CreateSandboxFromImageParams, Resources, SessionExecuteRequest, SandboxState, ) daytona = Daytona() operation = os.environ[''ORB_OPERATION''] try: sandbox = daytona.get(os.environ[''ORB_ID'']) except DaytonaNotFoundError: sandbox = None if operation == ''spawn'': if sandbox is None: sandbox = daytona.create(CreateSandboxFromImageParams( name=os.environ[''ORB_ID''], image=os.environ[''ORB_IMAGE''], os_user=''user'', resources=Resources(cpu=4, memory=8, disk=10), auto_stop_interval=0, auto_delete_interval=-1, ), timeout=240) elif sandbox.state != SandboxState.STARTED: sandbox.start() env = {key: os.environ[key] for key in (''ORB_TOKEN'', ''AMP_URL'') if key in os.environ} sandbox.update_env(env, unset=[''AMP_URL'']) session = str(uuid4()) sandbox.process.create_session(session) sandbox.process.execute_session_command(session, SessionExecuteRequest( command="bash -lc ''exec amp orb bootstrap''", run_async=True, )) elif operation == ''pause'' and sandbox: if sandbox.state == SandboxState.STARTED: sandbox.stop() elif operation == ''delete'' and sandbox: sandbox.delete() #!/usr/bin/env bash # dispatch.sh set -euo pipefail # Pause/resume currently requires access enabled by exe.dev support. status=$(ssh exe.dev ls "$ORB_ID" --json | jq -r --arg id "$ORB_ID" ''.vms[] | select(.vm_name == $id) | .status'') case "$ORB_OPERATION" in spawn) if [[ -z "$status" ]]; then ssh exe.dev new --name="$ORB_ID" --image="$ORB_IMAGE" \ --cpu=2 --memory=8G --no-email elif [[ "$status" == paused ]]; then ssh exe.dev resume "$ORB_ID" fi { printf ''export ORB_TOKEN=%q\n'' "$ORB_TOKEN" if [[ -n "${AMP_URL:-}" ]]; then printf ''export AMP_URL=%q\n'' "$AMP_URL" fi cat <<''BOOTSTRAP'' sudo -u user --preserve-env=ORB_TOKEN,AMP_URL bash -lc \ ''cd "$HOME"; nohup amp orb bootstrap </dev/null >.amp-bootstrap.log 2>&1 &'' BOOTSTRAP } | ssh "$ORB_ID.exe.xyz" bash -s ;; pause) if [[ "$status" == running ]]; then ssh exe.dev pause "$ORB_ID" fi ;; delete) if [[ -n "$status" ]]; then ssh exe.dev rm "$ORB_ID" fi ;; esac Use Amp''s Helm chart to run orbs on GKE, EKS, or AKS. Under the hood, it runs amp orb provider with a supplied dispatch program that handles spawning, pausing, and deleting orbs. You don''t need to write a dispatch script yourself. Create a custom orb provider in your Amp workspace and copy its token. Prepare a dedicated cluster with worker nodes and snapshot storage. Paused orbs are saved to storage and can resume on another worker node. Download the Kubernetes setup files. The ZIP includes gke.md, eks.md, and aks.md with the setup and Helm commands for each cloud, plus Terraform examples. Use the instructions for your cloud to install the chart and test your first orb. These examples use Amp''s pre-built ORB_IMAGE, which includes the Amp CLI and other dependencies. You can use a different Linux image as long as you can start amp orb bootstrap in it (and it has the dependencies for other features). That process connects the orb to Amp so Amp can set up the environment and run the agent''s tools. Once you have your dispatch program, go to your workspace settings (individuals can create a workspace for free), add a Custom Orb Provider, copy the token, and run amp orb provider: $ chmod +x dispatch.sh $ export AMP_ORB_PROVIDER_TOKEN="<copied-token>" $ amp orb provider ./dispatch.sh Once that runs, open ampcode.com or the Amp macOS or iOS applications and create a new thread. Expand Orb and choose your provider under Self-Hosted. And then submit your prompt. Amp will send a request to your provider to spawn an orb, clone your repository, run your setup files, and start the agent. And thus begins, as they say, orbin'' time. Self-hosted. You Probably Shouldn''t Self-Host Orbs All that said: you probably shouldn''t self-host orbs. We built self-hosted orbs for enterprises that can''t use the orbs we host, because regulations might restrict where their code runs, because they need access to internal networks, or because they need custom hardware attached. If you''re on an Enterprise plan, let us know and we''ll help you set it up. That''s why the interface is so open: we want to support all the infrastructure our enterprise customers might have, as long as it meets the qualitative requirements above (fast-to-start, resumable machines that scale to zero, etc.). But that also means we only support our end of the contract, not every possible piece of infrastructure you can plug in. Pricing Self-hosted orbs are free for individuals and teams. Enterprises will be charged standard orb pricing starting October 29. Until then, self-hosted orbs are free for every workspace. Getting Started For more information on self-hosted orbs and how to run self-hosted orbs on Kubernetes, see the documentation.'
content: extracted
html: 2026-10-22-self-hosted-orbs.html
remote_preview:
  url: https://ampcode.com/og?_template=news&_version=7&title=Self-Hosted+Orbs&date=October+22%2C+2026&tagline=Run+orbs+on+your+own+infrastructure+with+your+security+policies%2C+internal+tools%2C+and+private+services.&sig=ebcfffe1f19abf1d6dd4eed6cf0881f404d489f34a1b7067237eb14578b5bb2e
---

You can now self-host [orbs](https://ampcode.com/docs/orbs). You provide the compute and Amp turns it into orbs.

If you have compute that can

- quickly start new machines
- put them to sleep and resume them equally fast
- keep them around for a long time
- scale them to zero, so no one has to worry about having a lot of them sleeping

then you are now able to replicate the orb experience within your own infrastructure.

## How It Works

You run `amp orb provider` inside your infrastructure. The `orb provider` then reaches out to Amp and waits for requests to spawn, pause, or delete orbs. Like this:

![1. amp orb provider opens a connection to ampcode.com. 2. Amp sends events to the provider. 3. The provider spawns an orb per thread. 4. Each orb runs in your infrastructure and connects back to its thread on ampcode.com. No inbound ports are needed.](https://static.ampcode.com/news/self-hosted-orbs-diagram-light.avif)

*How* you satisfy those requests is up to you, because `amp orb provider` will dispatch the requests for orb operations (`spawn` to create or resume orbs, `pause`, and `delete`) to a program that you provide, like so:

```shell-session
$ amp orb provider ./dispatch.sh
```

In that program, you can do whatever works best for your infrastructure. Here are some illustrative examples:

Docker E2B K8s Apple containers Modal Vercel Daytona exe.dev

```bash
#!/usr/bin/env bash
# dispatch.sh
set -euo pipefail

case "$ORB_OPERATION" in
	spawn)
		docker start "$ORB_ID" 2>/dev/null ||
			docker run --platform linux/amd64 -d --name "$ORB_ID" \
				"$ORB_IMAGE" sleep infinity
		docker exec -d --env ORB_TOKEN "$ORB_ID" amp orb bootstrap
		;;
	pause)
		docker stop "$ORB_ID"
		;;
	delete)
		docker rm -f "$ORB_ID"
		;;
esac
```

Install the E2B CLI and the `e2b` package, set `E2B_API_KEY`, and build a template:

```bash
echo 'FROM us-central1-docker.pkg.dev/amp-prod-0/amp-orb/amp-orb:latest' \
    > e2b.Dockerfile
e2b template create amp-orbs --dockerfile e2b.Dockerfile \
    --cpu-count 4 --memory-mb 8192 --ready-cmd true
export E2B_TEMPLATE=amp-orbs
```

```javascript
#!/usr/bin/env node
// dispatch.mjs
import { Sandbox } from 'e2b'

const {
	ORB_OPERATION, ORB_ID, ORB_TOKEN, AMP_URL, E2B_TEMPLATE,
} = process.env
const metadata = { orbId: ORB_ID }
const [existing] = await Sandbox.list({
	query: { metadata, state: ['running', 'paused'] },
}).nextItems()

if (ORB_OPERATION === 'spawn') {
	const timeoutMs = 60 * 60 * 1000
	const sandbox = existing
		? await Sandbox.connect(existing.sandboxId, { timeoutMs })
		: await Sandbox.create(E2B_TEMPLATE, {
			metadata, timeoutMs, lifecycle: { onTimeout: 'pause' },
		})
	const envs = { ORB_TOKEN }
	if (AMP_URL) envs.AMP_URL = AMP_URL
	const bootstrap = await sandbox.commands.run('amp orb bootstrap', {
		user: 'user', envs, background: true, timeoutMs: 0,
	})
	await bootstrap.disconnect()
} else if (ORB_OPERATION === 'pause' && existing) {
	await Sandbox.pause(existing.sandboxId)
} else if (ORB_OPERATION === 'delete' && existing) {
	await Sandbox.kill(existing.sandboxId)
}
```

```bash
#!/usr/bin/env bash
# dispatch.sh
set -euo pipefail
export PATH="/usr/local/bin:/opt/homebrew/bin:$PATH"

# "$ORB_IMAGE" is the Amp orb image.

ids=$(container list --all --quiet)
exists() { grep -Fxq "$ORB_ID" <<<"$ids"; }

case "$ORB_OPERATION" in
	spawn)
		if exists; then
			container start "$ORB_ID"
		else
			container run --detach --init --rosetta \
				--platform linux/amd64 --cpus 4 --memory 8G \
				--name "$ORB_ID" "$ORB_IMAGE" sleep infinity
		fi
		# A bare --env NAME copies the value from the host.
		container exec --detach --env ORB_TOKEN --env AMP_URL \
			"$ORB_ID" amp orb bootstrap
		;;
	pause)
		if exists; then
			container stop "$ORB_ID"
		fi
		;;
	delete)
		if exists; then
			container delete --force "$ORB_ID"
		fi
		;;
	*)
		exit 1
		;;
esac
```

```python
#!/usr/bin/env python3
# dispatch.py
import os

import modal

# Needs `pip install modal` plus MODAL_TOKEN_ID and MODAL_TOKEN_SECRET.
APP = "amp-orbs"
app = modal.App.lookup(APP, create_if_missing=True)
snapshots = modal.Dict.from_name("amp-orb-snapshots", create_if_missing=True)
orb_id = os.environ["ORB_ID"]

try:
    sandbox = modal.Sandbox.from_name(APP, orb_id)
except modal.exception.NotFoundError:
    sandbox = None

match os.environ["ORB_OPERATION"]:
    case "spawn":
        if sandbox is None:
            # Modal has no pause, so resume from the last snapshot.
            snapshot = snapshots.get(orb_id)
            if snapshot:
                image = modal.Image.from_id(snapshot)
            else:
                image = modal.Image.from_registry(os.environ["ORB_IMAGE"])
            sandbox = modal.Sandbox.create(
                "sleep", "infinity", app=app, name=orb_id, image=image,
                cpu=4, memory=(16384, 16384), timeout=24 * 60 * 60,
            )
        # Modal execs run as root; point bootstrap at the image user's home.
        bootstrap = "setsid amp orb bootstrap </dev/null >/dev/null 2>&1 &"
        sandbox.exec(
            "bash", "-c", bootstrap,
            env={
                "ORB_TOKEN": os.environ["ORB_TOKEN"],
                "AMP_URL": os.environ.get("AMP_URL"),
                "HOME": "/home/user", "USER": "user",
            },
        ).wait()
    case "pause":
        if sandbox:
            image_id = sandbox.snapshot_filesystem(timeout=120).object_id
            snapshots[orb_id] = image_id
            sandbox.terminate()
    case "delete":
        if sandbox:
            sandbox.terminate()
        snapshots.pop(orb_id, None)
```

```javascript
#!/usr/bin/env node
// dispatch.mjs
import { APIError, Sandbox, Snapshot } from '@vercel/sandbox'

const { ORB_OPERATION, ORB_ID, ORB_IMAGE, ORB_TOKEN, AMP_URL } = process.env
const credentials = {
	token: process.env.VERCEL_TOKEN,
	teamId: process.env.VERCEL_TEAM_ID,
	projectId: process.env.VERCEL_PROJECT_ID,
}

if (ORB_OPERATION === 'spawn') {
	const sandbox = await Sandbox.getOrCreate({
		...credentials,
		name: ORB_ID,
		// Copy Amp's image to VCR, then set ORB_IMAGE to its VCR URL.
		image: ORB_IMAGE,
		resources: { vcpus: 4 },
		timeout: 60 * 60 * 1000,
		persistent: true,
		snapshotExpiration: 0,
		keepLastSnapshots: { count: 1, expiration: 0, deleteEvicted: true },
	})
	const setup = await sandbox.runCommand({
		cmd: '/bin/bash',
		args: ['-c', 'hostname localhost && mkdir -p /vercel/sandbox'],
		sudo: true,
	})
	if (setup.exitCode !== 0) throw new Error(await setup.stderr())
	await sandbox.runCommand({
		cmd: '/usr/bin/setpriv',
		args: [
			'--reuid=user', '--regid=user', '--init-groups', '--',
			'/usr/bin/env', `ORB_TOKEN=${ORB_TOKEN}`,
			...(AMP_URL ? [`AMP_URL=${AMP_URL}`] : []),
			'HOME=/home/user', 'USER=user', '/bin/bash', '-lc',
			'exec amp orb bootstrap',
		],
		sudo: true,
		detached: true,
	})
} else if (ORB_OPERATION === 'pause' || ORB_OPERATION === 'delete') {
	try {
		const sandbox = await Sandbox.get({
			...credentials, name: ORB_ID, resume: false,
		})
		if (ORB_OPERATION === 'delete') {
			if (sandbox.status === 'running') await sandbox.stop()
			for await (const saved of await sandbox.listSnapshots()) {
				if (saved.status !== 'created') continue
				const snapshot = await Snapshot.get({
					...credentials, snapshotId: saved.id,
				})
				await snapshot.delete()
			}
			await sandbox.delete()
		} else if (sandbox.status === 'running') await sandbox.stop()
	} catch (error) {
		if (!(error instanceof APIError && error.response.status === 404)) {
			throw error
		}
	}
}
```

```python
#!/usr/bin/env python3
# dispatch.py
import os
from uuid import uuid4
from daytona import (
    Daytona, DaytonaNotFoundError, CreateSandboxFromImageParams,
    Resources, SessionExecuteRequest, SandboxState,
)

daytona = Daytona()
operation = os.environ['ORB_OPERATION']
try:
    sandbox = daytona.get(os.environ['ORB_ID'])
except DaytonaNotFoundError:
    sandbox = None

if operation == 'spawn':
    if sandbox is None:
        sandbox = daytona.create(CreateSandboxFromImageParams(
            name=os.environ['ORB_ID'], image=os.environ['ORB_IMAGE'],
            os_user='user', resources=Resources(cpu=4, memory=8, disk=10),
            auto_stop_interval=0, auto_delete_interval=-1,
        ), timeout=240)
    elif sandbox.state != SandboxState.STARTED:
        sandbox.start()
    env = {key: os.environ[key] for key in ('ORB_TOKEN', 'AMP_URL')
           if key in os.environ}
    sandbox.update_env(env, unset=['AMP_URL'])
    session = str(uuid4())
    sandbox.process.create_session(session)
    sandbox.process.execute_session_command(session, SessionExecuteRequest(
        command="bash -lc 'exec amp orb bootstrap'", run_async=True,
    ))
elif operation == 'pause' and sandbox:
    if sandbox.state == SandboxState.STARTED:
        sandbox.stop()
elif operation == 'delete' and sandbox:
    sandbox.delete()
```

```bash
#!/usr/bin/env bash
# dispatch.sh
set -euo pipefail

# Pause/resume currently requires access enabled by exe.dev support.
status=$(ssh exe.dev ls "$ORB_ID" --json |
    jq -r --arg id "$ORB_ID" '.vms[] | select(.vm_name == $id) | .status')

case "$ORB_OPERATION" in
    spawn)
        if [[ -z "$status" ]]; then
            ssh exe.dev new --name="$ORB_ID" --image="$ORB_IMAGE" \
                --cpu=2 --memory=8G --no-email
        elif [[ "$status" == paused ]]; then
            ssh exe.dev resume "$ORB_ID"
        fi
        {
            printf 'export ORB_TOKEN=%q\n' "$ORB_TOKEN"
            if [[ -n "${AMP_URL:-}" ]]; then
                printf 'export AMP_URL=%q\n' "$AMP_URL"
            fi
            cat <<'BOOTSTRAP'
sudo -u user --preserve-env=ORB_TOKEN,AMP_URL bash -lc \
    'cd "$HOME"; nohup amp orb bootstrap </dev/null >.amp-bootstrap.log 2>&1 &'
BOOTSTRAP
        } | ssh "$ORB_ID.exe.xyz" bash -s
        ;;
    pause)
        if [[ "$status" == running ]]; then
            ssh exe.dev pause "$ORB_ID"
        fi
        ;;
    delete)
        if [[ -n "$status" ]]; then
            ssh exe.dev rm "$ORB_ID"
        fi
        ;;
esac
```

Use [Amp's Helm chart](https://ampcode.com/docs/orbs/self-hosted/kubernetes#install) to run orbs on **GKE, EKS, or AKS**. Under the hood, it runs `amp orb provider` with a supplied dispatch program that handles spawning, pausing, and deleting orbs. You don't need to write a dispatch script yourself.

1. [Create a custom orb provider](https://ampcode.com/docs/orbs/self-hosted#set-up-a-provider) in your Amp workspace and copy its token.
2. Prepare a dedicated cluster with [worker nodes and snapshot storage](https://ampcode.com/docs/orbs/self-hosted/kubernetes#supported-platforms). Paused orbs are saved to storage and can resume on another worker node.
3. [Download the Kubernetes setup files](https://ampcode.com/docs/orbs/self-hosted/bundle.zip). The ZIP includes `gke.md`, `eks.md`, and `aks.md` with the setup and Helm commands for each cloud, plus Terraform examples. Use the instructions for your cloud to install the chart and test your first orb.

These examples use Amp's pre-built `ORB_IMAGE`, which includes the Amp CLI and other dependencies. You can use a different Linux image as long as you can start `amp orb bootstrap` in it (and [it has the dependencies for other features](https://ampcode.com/docs/orbs/self-hosted#bring-your-own-image)). That process connects the orb to Amp so Amp can set up the environment and run the agent's tools.

Once you have your dispatch program, go to your workspace settings (individuals can create a workspace for free), add a [**Custom Orb Provider**](https://ampcode.com/workspace#custom-orb-providers), copy the token, and run `amp orb provider`:

```shell-session
$ chmod +x dispatch.sh
$ export AMP_ORB_PROVIDER_TOKEN="<copied-token>"
$ amp orb provider ./dispatch.sh
```

Once that runs, open ampcode.com or the Amp macOS or iOS applications and create a new thread. Expand **Orb** and choose your provider under **Self-Hosted**.

![The full new thread composer with the executor picker open and our-k8s-orbs highlighted and selected under Self-Hosted](https://static.ampcode.com/news/self-hosted-orbs-editor-light.avif) ![The full new thread composer with the executor picker open and our-k8s-orbs highlighted and selected under Self-Hosted](https://static.ampcode.com/news/self-hosted-orbs-editor-dark.avif)

And then submit your prompt. Amp will send a request to your provider to spawn an orb, clone your repository, run your setup files, and start the agent.

And thus begins, as they say, orbin' time. Self-hosted.

## You Probably Shouldn't Self-Host Orbs

All that said: you probably shouldn't self-host orbs.

We built self-hosted orbs for enterprises that can't use the orbs we host, because regulations might restrict where their code runs, because they need access to internal networks, or because they need custom hardware attached.

If you're on an Enterprise plan, let us know and we'll help you set it up.

That's why the interface is so open: we want to support all the infrastructure our enterprise customers might have, as long as it meets the qualitative requirements above (fast-to-start, resumable machines that scale to zero, etc.). But that also means we only support our end of the contract, not every possible piece of infrastructure you can plug in.

## Pricing

Self-hosted orbs are free for individuals and teams. Enterprises will be charged [standard orb pricing](https://ampcode.com/docs/orbs/sizes-and-costs) starting October 29. Until then, self-hosted orbs are free for every workspace.

## Getting Started

For more information on self-hosted orbs and how to run self-hosted orbs on Kubernetes, [see the documentation](https://ampcode.com/docs/orbs/self-hosted).
