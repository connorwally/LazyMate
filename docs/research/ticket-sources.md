# Ticket-source integration investigation

Status: Research, not an accepted implementation decision. Official documentation was checked; integrations have not been tested in the user's employer environment.

## Separate projects from ticket sources

A LazyMate project is a local Git repository. Its Git hosting remote does not determine where its tasks come from. A Jira project is a ticket grouping/query scope, not necessarily a repository.

Proposed conceptual boundaries:

- Project: local repository, main checkout, workflow items.
- Source connection: provider, host/site, approved account/CLI authentication reference.
- Source binding: project-specific search scope (GitHub repository or Jira project/JQL/filter).
- Imported task: local identity, source reference and remote ID, URL, title/description snapshot, workflow state.

This permits hobby GitHub and employer Jira projects in the same application. Multiple source bindings per project are possible but not yet an MVP requirement. Keep source identity distinct from display keys: `#41` and `TEAM-41` are not globally unique. Duplicate-import behavior across local projects remains to be decided.

## GitHub candidate

Wrap existing `gh` authentication and JSON commands. Use explicit repository arguments rather than relying on the process working directory. Read-only search and import are enough initially.

References:
- https://cli.github.com/manual/gh_issue_list
- https://cli.github.com/manual/gh_issue_view
- https://cli.github.com/manual/gh_auth_login

## Jira Cloud candidate

Atlassian's official ACLI offers JSON search and fetch:

```text
acli jira workitem search --jql "project = TEAM" --limit 50 --json
acli jira workitem view TEAM-123 --json
```

Documented authentication includes browser OAuth (`acli jira auth login --web`) and API-token login. Current token examples specify tokens without scopes; do not assume scoped-token compatibility. ACLI has macOS, Linux, and Windows installation documentation. It is a candidate Cloud adapter, not a universal Jira adapter.

References:
- https://developer.atlassian.com/cloud/acli/reference/commands/jira-workitem-search/
- https://developer.atlassian.com/cloud/acli/reference/commands/jira-workitem-view/
- https://developer.atlassian.com/cloud/acli/reference/commands/jira-auth-login/
- https://developer.atlassian.com/cloud/acli/

## Jira Data Center is a separate case

Do not assume ACLI Cloud support applies. Instance REST APIs and personal access tokens may be appropriate if supported and permitted. Data Center PATs use Bearer authentication; Cloud email/API-token authentication is different. Deployed version and organization settings matter.

Reference:
- https://confluence.atlassian.com/enterprise/using-personal-access-tokens-1026032365.html

## Access and data prerequisites

Browser access does not prove permission to use OAuth integrations or tokens. Employers can restrict both. Before selecting a transport, establish Cloud versus Data Center, approved authentication, CLI installation/network access, and permission to store ticket content locally and send it to the chosen AI provider.

No automatic workaround for blocked authentication. Do not store secrets in repository configuration, prompts, or logs. A custom OAuth integration is deferred unless justified; its registration and credential lifecycle add scope beyond using an approved CLI login.

References:
- https://support.atlassian.com/security-and-access-policies/docs/authentication-policy-settings-for-your-organizations/
- https://developer.atlassian.com/cloud/jira/platform/oauth-2-3lo-apps/

## Recommendation pending user answers

Keep narrow read-only source adapters. Evaluate `gh` for GitHub and ACLI for Jira Cloud if the employer permits it. Do not commit to Jira implementation or authentication until the deployment and policy constraints are known. Local tasks remain usable without remote access. Provider-specific search scopes and authentication should not leak into workflow logic.
