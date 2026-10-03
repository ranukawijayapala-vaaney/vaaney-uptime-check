# vaaney-uptime-check

A zero-cost uptime check for the Vaaney **staging** service. Every 15 minutes (best effort) GitHub Actions reads the public readiness page `https://vaaney-core-staging.up.railway.app/health/ready`. If staging is not ready after three attempts, the run fails and GitHub emails the repository owner.

- **Public on purpose:** GitHub-hosted runners cost nothing for public repositories, so this never uses the private application repository's build minutes.
- **No secrets:** it reads only the public readiness page and prints only the HTTP status and the readiness word.
- **Coverage:** database and schema only. Object storage is checked by an operations-only page that this public check cannot and should not reach.
- **Limits:**
  - schedules can be delayed or skipped;
  - GitHub disables schedules in a public repository after 60 days without activity, and warns by email first.
- **Test alert:** Actions → "Staging health" → Run workflow → tick "test_alert". The run fails on purpose without contacting staging, so you can confirm the email arrives.

Schedule maintenance (3 October 2026): checks are offset to minutes 7, 22, 37 and 52 UTC to avoid common scheduling peaks. GitHub documents schedule delays/dropped jobs: https://docs.github.com/en/actions/how-tos/troubleshoot-workflows . This does not guarantee timely execution. Only an observed schedule-event run verifies automatic execution; a manual run does not. The health-check job, retries, permissions and email behavior are unchanged.
