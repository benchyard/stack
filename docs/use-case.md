# Working example: change → preview → evidence

This example exercises Skheri and Argos with standard tools. It does not require
Benchyard, Acahti, a model subscription, a public Git repository or an open laptop
after a properly configured cloud agent takes over execution.

![From live source to a reviewed release](assets/lifecycle.svg)

## 1. Prepare the workspace

Follow [Skheri's quickstart](https://github.com/benchyard/skheri#try-it) on a development
cluster. It installs a workspace, copies the Vite example and starts port-forwarding
to port 5173. Open the page and confirm the dev server is ready.

For the simplest trial, use local port-forwarding. For phone access without a laptop,
use a cloud cluster and authenticated HTTPS ingress. Do not expose an unauthenticated
development server to the internet.

## 2. Make an uncommitted edit

```bash
kubectl --context "$SKHERI_CONTEXT" -n skheri-demo exec deploy/demo -- \
  sed -i 's/Preview before commit./Reviewed from the cloud./g' /workspace/app/index.html
```

The running Vite server reads the changed file and updates the browser. A new image
or Git commit is not needed. Do not rebuild the application for every keystroke.

## 3. Verify the observed result

Install the [public Argos pack](https://github.com/benchyard/argos-pack) in a Python
3.12+ environment. With the port-forward still running:

```bash
export ARGOS_BASE_URL=http://127.0.0.1:5173
export ARGOS_EXPECT_TEXT='Reviewed from the cloud.'
argos run web:preview --env preview
```

The report records the target, expected content, HTTP result and latency. Repeat
with an intentionally wrong expected string to see a failed case and nonzero exit.
The pack's tests cover failed HTTP responses, missing content and exceeded latency.

Share the local report or upload it to your own Argos Dash. A dashboard is optional.
Do not attach credentials or private response bodies to public tasks.

## 4. Review a stable revision

A live page can change after someone reviews it. Export or commit the reviewed
source and associate that revision with the evidence. If using Acahti, the agent
can open a PR, read CI failures and check the current head's status through MCP.
If using another forge, keep its normal PR/check workflow.

Acahti's merge check and Argos's acceptance assertions serve different roles.
Acahti observes configured pipeline success; your pipeline must actually enforce
required checks. Do not silently skip a required acceptance case.

## 5. Release a built artifact

Build the reviewed source with the example Dockerfile and push to your registry.
Record its digest, then run Skheri's application chart with that digest and your
staging values. Run the same checks against the deployed endpoint. Promote the
same digest to production under your approval policy.

The development PVC never becomes the production application volume. Keep database
migrations, backups and external side effects explicit; Helm rollback cannot undo them.

## Add the rest gradually

- Add Acahti when self-hosted Git, CI/package identity and structured delivery
  operations solve a real team problem. Keep one authoritative Git remote.
- Add Benchyard for shared tasks and execution. Automatic Skheri workspace binding
  remains future work; do not mistake the architecture diagram for an adapter.
- Add Argos Dash when multiple people need to browse and discuss runs.

This example deliberately uses two small components first so you can validate the
workflow before installing an entire platform.
