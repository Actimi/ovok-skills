# Ontoly for codebase structure

Use Ontoly Software Graph when the task asks about an Ovok app repository's architecture, request paths, dependencies, configuration, security boundaries, or change impact—and graph tooling or an existing graph is available.

1. Load the `ontoly-software-graph` skill and only the task-specific Ontoly skill(s) that fit: `architecture-review`, `codebase-onboarding`, `configuration-analysis`, `dead-code-analysis`, `dependency-analysis`, `documentation`, `framework-analysis`, `impact-analysis`, `migration-analysis`, `performance-analysis`, `refactoring`, `request-tracing`, `sdk-generation`, or `security-review`.
2. Check graph freshness and coverage. Use it to locate routes, owners, callers, dependencies, and configuration use.
3. Confirm behavior in current source, installed SDK declarations, and current Ovok docs. A graph describes code structure; it does not establish deployed behavior, endpoint availability, device support, FHIR behavior, or clinical workflow.

Do not force graph analysis into docs-only or copywriting tasks. If a graph is missing/stale or the CLI/MCP is unavailable, say so briefly and continue with source and docs. Follow the Ontoly skill's approval guidance before installing dependencies or generating graph files.
