# Choosing components

Last reviewed: 2026-09-25. This is a scope comparison, not a performance benchmark.
Use upstream documentation to check current availability and plan requirements.

| Existing option | What it already does | Why consider these components |
|---|---|---|
| Cursor Cloud Agents + My Machines | Cloud-directed agents with tool execution on supported self-hosted machines | Skheri describes the environment independently of one agent runtime; it does not replace Cursor's cloud control plane |
| GitHub + official MCP | Repository, PR and CI operations for agents | Acahti offers a self-hosted Git/CI/package installation and consistent member identity |
| AgentGit | Session saving, sharing, continuation and remote collaboration | Benchyard addresses team task state; Acahti addresses delivery; session portability is a separate concern |
| agent-git-service | Self-hosted GitHub-compatible interfaces and first-class agent identity | Acahti prioritizes integrated delivery operations; it does not claim full GitHub API compatibility or equivalent task-scoped identity |
| Existing Kubernetes/Helm | Workload scheduling and release management | Skheri adds a small opinionated workspace/release contract and a tested preview example, using those existing APIs |
| pytest / an observability stack | Tests and operational telemetry | Argos adds repeatable case runs and expected/actual evidence; it complements rather than replaces them |

## What is different here

The proposed combination keeps a task, its environment, the reviewed source and the
observed result related without requiring one proprietary editor to own all four.
This is a product direction and an interface discipline. Current components are
not yet one automatically integrated platform.

Choose Acahti for self-hosted delivery needs, Skheri for explicit environment
lifecycle, and Argos for repeatable evidence. Choose an existing integrated product
when its workflow already fits and you do not want to operate infrastructure.

## Sources

- [Cursor My Machines](https://cursor.com/docs/cloud-agent/self-hosted/my-machines)
- [Cursor Origin](https://cursor.com/docs/origin)
- [GitHub's official MCP server](https://github.com/github/github-mcp-server)
- [AgentGit](https://agent-git.com/)
- [agent-git-service](https://github.com/ngaut/agent-git-service)
- [Kubernetes multi-tenancy](https://kubernetes.io/docs/concepts/security/multi-tenancy/)

No claim of fewer tokens, faster delivery or greater reliability is made without
an equivalent task set, comparable agent/model configuration and published results.
