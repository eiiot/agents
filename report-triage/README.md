# Report triage agent

Automated receiver for Tuft `/report` submissions. Configure a Tuft webhook to
use this repository and the `report-triage` subdirectory, then set the router's
`TUFT_REPORT_WEBHOOK_URL` to the generated private URL.

## Webhook configuration

- Repository: `eiiot/agents`
- Subdirectory: `report-triage`
- Target: a private Slack channel for concise outcomes
- MCPs: GitHub; add Linear only if issue creation becomes part of the policy
- Environment:
  - `TUFT_REPORT_REPOSITORY=expo/tuft`
  - `TUFT_REPORT_MODE=diagnose` or `pr`

`diagnose` is the safe initial rollout. It gathers evidence and reports a
root-cause assessment without changing code. `pr` may create a fix PR when the
evidence is strong and the fix is bounded.

The router includes a short-lived download grant scoped to the submitted report
object. The receiver needs no Tuft account or machine-local credentials. Keep
the URL out of output and logs, download immediately, and delete extracted
diagnostics when the run finishes.

## Payload

See `fixtures/report.uploaded.json`. The ingress policy rejects other event
types and strips unknown fields before an agent run starts.
