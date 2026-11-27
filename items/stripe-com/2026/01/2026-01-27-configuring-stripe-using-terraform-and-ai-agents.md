---
title: Configuring Stripe using Terraform and AI agents
link: https://stripe.dev/blog/ai-agents-terraform-stripe-infrastructure
source: stripe-com
published: 2026-01-27T00:00:00Z
updated: 2026-01-27T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Michael Selander
summary: Learn why using AI agents to author Terraform code is safer than direct API calls for Stripe configuration. Get transparent, consistent, and auditable infrastructure with code review workflows.
content: extracted
html: 2026-01-27-configuring-stripe-using-terraform-and-ai-agents.html
remote_preview:
  url: https://stripe.dev/og/blog/ai-agents-terraform-stripe-infrastructure.png
  alt: 'Stripe Developer Blog: Configuring Stripe using Terraform and AI agents'
extra:
  content:interactive: true
---

Setting up a Stripe configuration with AI agents can feel like a superpower. You describe what you want, the agent makes the necessary API calls, and your account is ready in minutes.

The problem is that those successful one-off changes rarely become a durable source of truth. A week later, it is hard to answer questions such as what products and prices exist, how your sandbox and livemode accounts differ, and what exactly changed over time without digging through old threads or reverse engineering the Stripe Dashboard.

This post shows a safer pattern: use AI agents to author Terraform, not to operate your Stripe account directly. With the [Stripe Terraform provider](https://docs.stripe.com/terraform), you can define products, prices, and webhook endpoints as code, review changes in pull requests, and apply them consistently across Stripe sandboxes and live mode. The end result is repeatable Stripe configuration you can audit, reproduce in new environments, and evolve over time without accumulating mystery state from ad hoc agent runs.

If you already use Terraform (or want to), the goal is straightforward. Describe the Stripe objects you want in .tf files, let the agent generate the initial configuration, and rely on the `terraform plan` and `terraform apply` CLI commands to make changes safely.

## The challenge of using AI agents for infrastructure

There are three major challenges to setting up infrastructure using AI agents.

**Transparency:** Monitoring one-off API calls within an agent run is difficult. Agents often perform multiple tasks initiated by a single user prompt, burying API calls somewhere in the middle of long threads. Furthermore, AI agent runs are often ephemeral, used for a specific task and then discarded. Locating details of an API call from an AI agent days or even hours after the fact often requires developers to sift through many dozens of old threads often to no avail.

**Consistency:** AI agents are notoriously stochastic. Giving the exact same prompt twice to the same agent often does not produce the same output, leading to inconsistent configuration between development and production environments. This drift between environments can cause subtle bugs downstream as everything seems to work in your test environment but then fails when moving to production.

**Auditability:** API calls are not descriptive of an object’s current state, only the intended destination of how you are changing the object. This means even if you have a full record of all the API calls you have made, it can be difficult to see the exact state at a single moment in time, especially when you have made many edits over time.

## Terraform: A better way to configure your Stripe resources

Terraform elegantly solves the problems of transparency, consistency, and auditability. With Terraform, your Stripe configuration lives in code (.tf files) that you can review, version, test, and deploy using the same workflows you already use for application code.

Terraform solves the core problems listed above:

**Transparency (you can see what exists):** Terraform makes the desired state explicit. Instead of guessing what an agent may have created through scattered API calls, you can open your repository and see the full set of Stripe resources as code.

**Consistency (you can reproduce it):** Terraform is declarative: you describe what you want, and Terraform converges on that state deterministically. That means you can:

- Reuse the same modules across dev/staging/prod
- Parameterize environment differences (for example, webhook URLs) via variables
- Get repeatable, predictable results even if the AI agent is stochastic

**Auditability (you can track change over time):** Once Stripe infrastructure is expressed in Terraform, you automatically inherit standard software auditing:

- Git history shows who changed what and when
- Pull request reviews capture why a change was made

In other words, you move from “an agent did something at some point” to “here is the exact diff that was reviewed and applied.”

## Example: Define a pricing plan in Terraform

If you ask an AI agent to “set up a $20/month plan and a $200/year plan in Stripe using Terraform,” this is the kind of concrete, reviewable configuration it should produce: a product, two prices, and a webhook endpoint captured as code.

Create a [`main.tf`](http://main.tf) file:

```
terraform {
  required_providers {
    stripe = {
      source  = "stripe/stripe"
      version = "~> 0.1"
    }
  }
}

provider "stripe" {
  # API key is read from STRIPE_API_KEY environment variable
}

variable "webhook_url" {
  type        = string
  description = "Public URL Stripe should send webhook events to"
}

# One product...
resource "stripe_product" "standard_plan" {
  name        = "Standard Plan"
  description = "Simple subscription with monthly and annual billing"
}

# ...with a $20/month price
resource "stripe_price" "standard_monthly" {
  product     = stripe_product.standard_plan.id
  currency    = "usd"
  unit_amount = 2000 # $20.00

  recurring {
    interval = "month"
  }
}

# ...and a $200/year price
resource "stripe_price" "standard_yearly" {
  product     = stripe_product.standard_plan.id
  currency    = "usd"
  unit_amount = 20000 # $200.00

  recurring {
    interval = "year"
  }
}

resource "stripe_webhook_endpoint" "billing" {
  url = var.webhook_url

  enabled_events = [
    "checkout.session.completed",
    "invoice.paid",
    "invoice.payment_failed",
    "customer.subscription.created",
    "customer.subscription.updated",
    "customer.subscription.deleted",
  ]
}

output "monthly_price_id" {
  value = stripe_price.standard_monthly.id
}

output "yearly_price_id" {
  value = stripe_price.standard_yearly.id
}
```

Run it in your Stripe sandbox:

```
export STRIPE_API_KEY="sk_test_..."
terraform init
terraform plan  -var='webhook_url=https://api.example.com/webhooks/stripe'
terraform apply -var='webhook_url=https://api.example.com/webhooks/stripe'
```

## Best practices

The safest way to combine AI agents with infrastructure is to make the agent a code author, not an operator.

Instead of prompting an agent to “create a Stripe product and price,” prompt it to “set up my pricing structure XYZ using Terraform”. The agent then updates the Terraform files that you can review like any other code change.

This is the workflow:

1. You describe the Stripe setup (for example, $20/month subscription or $200/year)
2. The AI agent translates that into the necessary Stripe objects (products, prices, webhook endpoints, etc.)
3. The AI agent creates or edits Terraform files
4. In development, you run `terraform plan` to validate the changes, then `terraform apply`
5. In production, your CI/CD pipeline runs `terraform apply` once the changes have been approved and merged

**Using multiple environments:** When you’re managing both Stripe sandbox mode and livemode, it’s important to keep them isolated so you don’t accidentally apply sandbox changes to production (or vice versa).

Terraform workspaces help by giving each environment its own separate state, while you keep the same Terraform configuration.

Instead of maintaining two different copies of your Terraform code, you keep one set of `.tf` files and use:

- A sandbox workspace for Stripe sandbox mode
- A livemode workspace for Stripe production

This is the workflow:

- You create two Terraform workspaces: one for sandbox and one for livemode
- You select the workspace you want to work in
- You set the matching Stripe API key for that environment
- You run `terraform plan` to confirm what will change, then `terraform apply` to apply it
- You repeat the same process for the other environment when you’re ready

Create workspaces for sandbox and livemode:

```
terraform workspace new sandbox
terraform workspace new livemode
```

List and verify available workspaces:

```
terraform workspace list
```

Work in sandbox (Stripe sandbox mode):

```
terraform workspace select sandbox
export STRIPE_API_KEY="sk_test_..."
terraform plan
terraform apply
```

Work in livemode (Stripe production):

```
terraform workspace select livemode
export STRIPE_API_KEY="sk_live_..."
terraform plan
terraform apply
```

The key idea is that the selected workspace determines which state file Terraform is using, and the STRIPE\_API\_KEY you export determines which Stripe environment you’re actually modifying. Keeping those two aligned makes this approach safe.

## Conclusion

AI agents are powerful tools for building software, but they shouldn’t be your source of truth for your Stripe infrastructure. Terraform provides an elegant solution to the difficulties of setting up your Stripe account in a world of AI development. With Terraform, your resources are easy to review, diff, and audit over time, leading to faster development, safer deploys, and much more stable resources.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers) and take a look at the additional resources linked below.
