# best vps for n8n: how much RAM you really need, what each pick costs per year, and where RackNerd's budget plans fit in

Self-hosting n8n is cheap to run and easy to undersize. The app idles under a gigabyte of RAM, then briefly wants two or three times that the moment a workflow pulls a big API response or loops over a few hundred rows. Size the server for the idle state and it dies on the first busy Tuesday.

Short answer before anything else: get at least 2GB of RAM, prefer 2 vCPUs, and pick a provider whose uptime you don't have to think about, because a webhook that arrives mid-reboot is lost for good. The comparison posts ranking for this question all converge on the same names — Hetzner, Hostinger, DigitalOcean, with Vultr and Contabo as alternates. All reasonable picks. None of them cover the budget end, where RackNerd sells annual KVM plans for roughly what the others charge per month, and that option changes the math for anyone hosting n8n for themselves or a small team.

## Quick picks

> - **Best value for most self-hosters:** RackNerd's 2.5GB KVM — 2 vCPU, 50GB SSD, around $26 for a full year on the current promo. You install Docker yourself; the guide below takes about 15 minutes. 👉 [Check RackNerd's current annual VPS specials](https://bit.ly/my-rn)
> - **Easiest setup:** Hostinger ships a pre-configured n8n template with SSL handled, but renewal pricing roughly doubles once the intro term ends.
> - **Best for EU data residency:** Hetzner's CX22 (4GB RAM, 40GB NVMe) at roughly $4.15–4.59/month, hosted in GDPR-compliant European data centers.
> - **Best developer tooling:** DigitalOcean — one-click n8n deployment, a $200 trial credit, and managed Postgres if you'd rather not run your own database.

## What n8n actually needs from a VPS

An idle n8n instance sits at 100–500MB of RAM depending on the database backend. That number will tempt you into a tiny plan, and it's the wrong number to size on. One workflow that loads a large payload into memory can push another 1–2GB during execution, and three workflows firing at once on a 2GB box means swapping. CPU is rarely the constraint for typical webhook and HTTP work; n8n only gets compute-hungry when you run AI nodes locally or process images inside Code nodes.

| Resource | Minimum | Realistic recommendation |
| --- | --- | --- |
| RAM | 2 GB | 4 GB |
| CPU | 1 vCPU | 2+ vCPUs |
| Storage | 20 GB SSD | 40–50 GB NVMe |
| Bandwidth | 1 TB | 2 TB+ |
| OS | Ubuntu 22.04+ | Ubuntu 24.04 LTS |

In practice, three tiers cover almost everyone:

- **Personal or low-volume:** 2GB RAM, 1–2 vCPU, 25GB storage. Fine for scheduled jobs and light webhooks under roughly 100 executions a day.
- **Small-business production:** 4GB RAM, 2 vCPU, 40GB+ NVMe. The realistic baseline once n8n runs actual work, with Postgres living on the same machine.
- **Heavy webhooks, AI agents, or queue mode:** 8GB RAM, 4 vCPU, 80GB storage. Queue mode (a main process, Redis, and separate workers) needs more headroom than a single container.

One sizing rule the usual comparisons tend to skip: past roughly 50 concurrent executions or 20GB of Postgres history, stop upgrading one box and split the components instead — move the database to its own cheap VPS or a managed service. Two 4GB boxes handle n8n better than one 8GB box at that point.

So the honest floor for anything beyond tinkering is 2GB, and 4GB buys comfort. That's why the 2.5GB tier is the one most people should start with. 👉 [Check the 2.5GB plan's current price on RackNerd](https://bit.ly/my-rn)

## RackNerd's plans and pricing

RackNerd built its name on annual KVM plans priced at levels the mainstream clouds don't offer — think a couple of dollars a month equivalent rather than ten. Everything is unmanaged, support runs through tickets, and each plan includes a dedicated IPv4. Data centers cover Los Angeles, San Jose, Seattle, Dallas, Chicago, New York, and Atlanta in the US, plus Amsterdam and Dublin in Europe, so you can sit the server close to whichever webhook sources you use most.

Here are the tiers most relevant to n8n:

| Plan | RAM | vCPU | SSD | Promo price (billed yearly) | Order |
| --- | --- | --- | --- | --- | --- |
| 768MB KVM | 768 MB | 1 | 15 GB | $8.99/yr (~$0.75/mo) | [ Order this plan](https://bit.ly/my-rn) |
| 1GB KVM | 1 GB | 1 | 20 GB | $10.99/yr (~$0.92/mo) | [ Order this plan](https://bit.ly/my-rn) |
| 2.5GB KVM | 2.5 GB | 2 | 50 GB | $25.88/yr (~$2.16/mo) | [ Order this plan](https://bit.ly/my-rn) |
| 4GB KVM | 4 GB | 2.5 | 65 GB | $35.99/yr (~$3.00/mo) | [ Order this plan](https://bit.ly/my-rn) |
| 5GB KVM | 5 GB | 3 | 80 GB | $44.99/yr (~$3.75/mo) | [ Order this plan](https://bit.ly/my-rn) |

A few notes before you order:

- Prices are in USD, paid annually upfront. These are promotional rates, and RackNerd rotates its specials regularly, so treat the numbers above as the typical going rate and confirm the current price at checkout. Larger plans beyond these tiers are listed on the specials page too.
- Bandwidth runs from about 1TB on the small plans up to 5TB on the 5GB tier — far more than an n8n instance will ever push.
- For n8n specifically: the 768MB and 1GB plans are for experimenting and learning the platform. The 2.5GB plan is the sensible starting point for real automations, and the 4GB or 5GB tiers fit production use with Postgres plus room for bursts.

## What n8n on RackNerd costs over three years

Over three years, the 2.5GB RackNerd plan costs about $78 — less than seven months of DigitalOcean's 2GB droplet at list price. That's the comparison the monthly-price tables never quite surface, so here it is explicitly:

| Option | Specs | Year 1 | ~3-year spend |
| --- | --- | --- | --- |
| RackNerd 2.5GB KVM | 2.5GB / 2 vCPU / 50GB | ~$26 | ~$78, assuming the promo renews at the same rate (confirm terms at checkout) |
| Hetzner CX22 | 4GB / 2 vCPU / 40GB NVMe | ~$50–55 | ~$150–250; the intro price may not hold on renewal, with an $8.49 regular rate shown on some listings |
| Hostinger KVM 2 | 8GB / 2 vCPU / 100GB NVMe | ~$84–108 intro | renewals roughly double after the intro term |
| DigitalOcean Basic | 2GB / 1 vCPU / 50GB | $144 | $432, flat list price |
| n8n Cloud (hosted) | managed, with execution limits | $240 | $720 |

Two things jump out of that table. First, trial credits blur year one: DigitalOcean hands new accounts $200 and Vultr advertises $250+, which can cover a year or more of small instances before the meter starts. Second, intro pricing doesn't always survive the renewal date — Hostinger's discounts come with roughly doubled renewals, and Hetzner's promo rates sit well below some listed regular prices. RackNerd's approach inverts this: no credits, with the discount baked into a flat annual price. Its specials have historically renewed at the same rate, which is what makes the three-year math hold up. 👉 [Lock in RackNerd's flat annual pricing](https://bit.ly/my-rn)

## Where RackNerd is the wrong pick

RackNerd wins on cost and loses on hand-holding, and pretending otherwise helps nobody. Four situations call for something else:

- **You don't want to touch a terminal.** Hostinger's n8n template handles the Docker stack and SSL in minutes, and DigitalOcean's marketplace does one-click n8n deployment. On RackNerd, you install everything yourself. The setup below is genuinely short, but if "SSH into a server" sounds unpleasant, pay the difference for the template.
- **You need short, disposable test environments.** RackNerd's deals are annual and paid upfront. For spinning up an instance to test a risky workflow and destroying it an hour later, a provider with hourly billing and custom sizing (Kamatera works this way) or a credit-backed trial fits better.
- **You want a managed database.** DigitalOcean sells managed Postgres you can point n8n at instead of maintaining your own. On RackNerd, Postgres runs in a container on the same VPS, or you add a second cheap box for it — which, per the sizing rule above, you'd likely want anyway once usage grows.
- **EU data residency is a hard requirement.** Hetzner's German data centers and GDPR posture are the usual reason EU-based teams pick it, though note that support there is ticket-only with no formal uptime SLA. RackNerd does have Amsterdam and Dublin, but its center of gravity is clearly US locations. Worth knowing if you go the Hetzner route: its US data centers include only 1TB of traffic versus 20TB in Europe — an odd asymmetry if you were planning to host stateside with them.

Also worth stating plainly: RackNerd support is ticket-based. No phone line, no live chat, no managed anything. For a single n8n instance that's usually fine — the app rarely needs a provider's help — but it's part of why the prices look the way they do.

And for maximum-specs-per-dollar shoppers: Contabo's 8GB tier (4 vCPU) runs around €4.99–6.99 per month, which undercuts RackNerd on raw RAM price. If 8GB for as little as possible is the goal, it's a legitimate alternative to compare.

## Setting up n8n on a RackNerd VPS

Four steps and about fifteen minutes, assuming a fresh Ubuntu install. RackNerd's welcome email contains the server IP and root password; everything after that is standard Docker work.

1. **Order and connect.** Pick a plan, choose Ubuntu 24.04 as the OS, and SSH in as root.
2. **Install Docker.**
   bash
   apt update && apt upgrade -y
   apt install -y docker.io
   
3. **Run n8n.**
   bash
   docker run -d --restart always \
     -p 5678:5678 \
     -v n8n_data:/home/node/.n8n \
     --name n8n \
     n8nio/n8n
   
   The `--restart always` flag brings the container back after a reboot, and the named volume keeps your workflows and credentials out of the container itself.
4. **Put a reverse proxy in front.** Point a domain at the server's IP, run Caddy or Nginx with a Let's Encrypt certificate, and proxy traffic to port 5678. n8n needs HTTPS for OAuth callbacks from services like Google or Slack to work, so don't skip this step.

Three follow-ups that matter once the instance is live:

- **Switch to Postgres early.** SQLite handles a first look, but it struggles under concurrent writes, and queue mode requires Postgres anyway. Migrating later means moving data, so start with it if the instance will matter, using the `DB_TYPE=postgresdb` environment variables from n8n's documentation.
- **Prune execution history.** Set n8n to keep successful execution data for 7 days and error data for 30. Execution records grow surprisingly fast and will quietly eat a small disk.
- **Back up the data volume and the encryption key inside it.** The key encrypts every credential stored in n8n; lose it and your backups become unreadable. A weekly cron that exports workflows with `n8n export:workflow --all`, plus a copy of the volume, covers most disasters.

## FAQ

**Can a $9-a-year VPS really run n8n?**
For learning and light scheduled jobs, yes. But 2GB is the honest floor for anything you depend on: an idle instance fits under 1GB, while one heavy execution can briefly demand several times that. The 768MB and 1GB plans make great playgrounds and utility boxes; real automations belong on 2.5GB or more.

**SQLite or Postgres?**
Postgres, once the instance matters. Execution history grows fast, SQLite gets unhappy with concurrent writes, and queue mode requires Postgres regardless. Starting on Postgres avoids a data migration later.

**How much RAM does queue mode need?**
Give the main node 4GB and either keep Redis and workers on the same 8GB box or split workers onto a second VPS. Queue mode exists because workflows start overlapping, and the memory math is per-worker — so plan headroom or a second box.

**Can I run self-hosted n8n for my business?**
Internal business use is permitted under n8n's Sustainable Use License. What's not allowed is reselling n8n or offering it to others as a hosted service. Read the current license terms before building a product on top of it.

**What happens to webhooks while the server is down?**
They're lost — most sending services don't retry. That's why provider reliability and backups matter more for n8n than raw speed, and why the encryption key deserves a backup from day one.

## So which one should you actually order?

- **Just learning n8n, or automating personal stuff:** RackNerd's 2.5GB KVM at around $26 a year costs less than two months of a DigitalOcean 2GB droplet, and leaves room for bursts. 👉 [Start with RackNerd's 2.5GB KVM plan](https://bit.ly/my-rn)
- **Running n8n for a business with Postgres and a growing workflow count:** the 4GB or 5GB tier, then a second cheap box for the database once concurrent executions pile up.
- **Allergic to the command line:** Hostinger's n8n template or DigitalOcean's one-click deploy will save you an hour and some swearing.
- **EU data residency is non-negotiable:** Hetzner's CX22 in Europe.

Before you click order on any of them: confirm the current promo price and server location at checkout, and once the instance is up, back up the encryption key first. Everything else can be rebuilt. The key can't.
