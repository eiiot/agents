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

## Required Tuft capability

The receiving agent must be able to materialize the bundle by opaque report ID:

```sh
tuft report download REPORT_ID --output REPORT.zip
```

That command/API does not exist yet. The webhook trigger can ship independently,
but automatic debugging is not end-to-end until Tuft provides an account-scoped,
audited download capability to webhook sessions. Do not put presigned R2 URLs in
the webhook payload; they expire, leak storage authority into prompts, and make
retries unreliable.

## Payload

See `fixtures/report.uploaded.json`. The ingress policy rejects other event
types and strips unknown fields before an agent run starts.

