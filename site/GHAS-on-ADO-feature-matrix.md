# GitHub Advanced Security (GHAS) on Azure DevOps Feature Matrix

This document helps answer the question **"Is this GitHub security feature available in GitHub Advanced Security for Azure DevOps?"**

The sections, feature names, and ordering follow the [GHAS-on-GHES feature matrix](https://github.com/advanced-security/advanced-security-material/blob/main/GHAS-on-GHES-feature-matrix.md) as closely as possible. Core capabilities and relevant cloud-only features are included where they help explain Azure DevOps support.

**Last reviewed: November 11, 2026.** Scope: **Azure DevOps Services / Azure Repos Git**, not Azure DevOps Server or TFVC. Support is based exclusively on public product documentation and published release notes.

#### Contents

- [How do I read this document?](#how-do-i-read-this-document)
- [Release notes](#release-notes)
- [Secret Protection](#secret-protection)
- [Code Security](#code-security)
  - [Code scanning](#code-scanning)
  - [Copilot Autofix and AI security](#copilot-autofix-and-ai-security)
  - [Dependabot alerts](#dependabot-alerts)
  - [Dependabot updates](#dependabot-updates)
  - [Dependency graph, dependency review and snapshot submission API](#dependency-graph-dependency-review-and-snapshot-submission-api)
- [Security Overview](#security-overview)
- [Administration](#administration)
- [Dependencies](#dependencies)

#### How do I read this document?

Each row represents a feature. There is only one support column: **Azure DevOps**.

| Cell | Meaning |
|---|---|
| ✅ | Generally available. |
| ☑️ | Public preview, including limited public preview. |
| 🔒 | Private preview. |
| Empty | No documented native Azure DevOps support established for this feature. |

**Empty cells are intentionally left blank**, including features available on GitHub but not documented on Azure DevOps. A blank is not a prediction or proof that a capability is technically impossible. In particular, version-dependent CodeQL CLI functionality is not automatically equivalent to a supported Azure DevOps product integration.

Every **Feature** link points to public Azure DevOps documentation. For a feature without documented ADO support, the link points to the closest relevant ADO capability or API documentation; it does **not** imply that the missing feature is described or supported there. Notes distinguish native equivalents, narrower implementations, and unverified details.

The dependency-scanning rows compare equivalent security outcomes: Azure DevOps calls its capability **dependency scanning**, not Dependabot. Third-party extensions and customer-built automation do not count as native support.

## Release notes

- [CodeQL default setup GA: Sprint 276](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-276-update).
- [PR security status checks GA: Sprint 275](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-275-update).
- [Dependency default setup and work-item linking GA: Sprint 264](https://learn.microsoft.com/en-us/azure/devops/release-notes/2025/sprint-264-update).
- [Copilot Autofix limited public preview: Sprint 281](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-281-update).

## Secret Protection

Secret scanning identifies credentials in repository content and history. Push protection blocks supported secrets before they enter the repository.

- [Secret scanning documentation](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops).
- [Supported secret patterns](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scan-patterns?view=azure-devops).
- [Advanced Security REST API documentation](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2).

| Feature | Azure DevOps | Notes |
|---|---|---|
| [Partner patterns / supported secret types](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scan-patterns?view=azure-devops) | ✅ | Use the ADO pattern table; detection, push protection, and validity-check coverage differ by secret type. |
| [Non-provider patterns](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | ✅ | Available through the **Other** confidence filter. |
| [User-space secret scanning](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | No personal-repository/user-space counterpart established; ADO repositories belong to projects. |
| [Push protection bypass](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | ✅ | Contributor bypass uses `skip-secret-scanning:true` in the commit message; an alert is still generated. This is not delegated approval. |
| [Detect secrets in discussions and PR titles, bodies, and comments](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | Documented scanning scope is repository source content/history. |
| [Push protection on web-interface commits](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | ✅ | Explicitly documented for the Azure Repos web interface. |
| [Push protection on file uploads](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | Web-commit protection is documented; a separate file-upload coverage guarantee was not established. |
| [Audit log events for non-provider patterns](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | |
| [Create a push protection bypass through REST API](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | |
| [Organization-level enablement of non-provider patterns](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | Detection exists; a separately configurable organization-wide non-provider-pattern toggle was not established. |
| [REST API for secret scanning scan history](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | |
| [Delegated alert dismissal for secret scanning](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-permissions?view=azure-devops) | | Dismissal permissions exist; a request/reviewer/approval queue was not established. |
| [Secret scanning completion events through audit log and webhooks](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-268-update) | | Validity-status service hooks are documented, but do not establish scan-completion event parity. |
| [Base64-encoded GitHub token detection](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scan-patterns?view=azure-devops) | | No explicit encoded-token support statement established. |
| [Push protection bypass request management through REST API](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | |
| [Free secret risk assessment for organization/security administrators](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | |
| [Configurable push protection patterns](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | No documented selection of which provider patterns receive push protection. |
| [Secret scanning in wikis](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | No explicit Azure DevOps wiki coverage established. |
| [Secret scanning REST fields: first location detected and additional locations](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | ADO APIs have their own schema; GitHub field-level compatibility was not established. |
| [Secret scanning validity checks](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | ✅ | Automatic for supported types; also supports on-demand verification and validation-status filtering. |
| [Secret scanning alert assignment](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | Azure Boards work-item linking is separate from a native alert-assignee field. |
| [Enterprise-level push protection delegated bypass](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | |
| [Push protection exemptions for apps, teams, and roles](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | |
| [Sort bypass and alert-dismissal approval requests by date](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | |
| [Custom validity states for custom-pattern alerts through REST API](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | |
| [Repository-level push protection exemptions for trusted actors](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | |
| [Secret scanning across full Git history and all branches](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | ✅ | A unique credential is consolidated into one alert across its occurrences. |
| [Secret push protection for Git pushes](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | ✅ | Native Azure Repos Git enforcement for supported high-confidence patterns. |
| [Custom secret regex patterns](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | No documented custom-pattern configuration. |
| [Dry runs and push protection for custom secret patterns](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | |
| [AI detection of unstructured secrets / generic passwords](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scan-patterns?view=azure-devops) | | Non-provider structured patterns do not establish AI password detection. |
| [Public monitoring for leaked enterprise credentials](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | | No ADO-native equivalent established. |

## Code Security

### Code scanning

Code scanning uses CodeQL to identify vulnerabilities. Azure DevOps also accepts findings from third-party tools.

- [Code scanning documentation](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops).
- [Initialize CodeQL task reference](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-codeql-init-v1?view=azure-pipelines).
- [Publish third-party SARIF results](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-publish-v1?view=azure-pipelines).

| Feature | Azure DevOps | Notes |
|---|---|---|
| [CodeQL toolcache / version management](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-codeql-init-v1?view=azure-pipelines) | ✅ | Agent/task-managed rather than a fixed appliance-bundled version; automatic installation can obtain the latest CodeQL version. |
| [Kotlin language support](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | Uses the Java/Kotlin analyzer (`java`); requires the appropriate build configuration. |
| [Swift language support](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | Advanced setup; Swift is not supported by ADO default setup. |
| [CodeQL threat models for detecting relevant alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | Not separately established in the reviewed ADO configuration documentation. |
| [Code scanning merge protection](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | ✅ | ADO branch-policy status checks, not GitHub rulesets; new/all high-and-critical checks. |
| [CodeQL threat-model configuration in default setup](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | |
| [CodeQL model packs for organizations/repositories](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | No matching managed organization/default-setup integration established. |
| [CodeQL scans Java projects without a build](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | Default setup uses `none` build mode; advanced setup also exposes build-mode configuration. |
| [CodeQL scans C# projects without a build](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | |
| [CodeQL dependency caching for advanced setup](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | No ADO-specific equivalent of the managed caching integration established. |
| [Zstandard archives for CodeQL tools](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-codeql-init-v1?view=azure-pipelines) | | Archive support is CLI/version-dependent, not a separately documented ADO feature. |
| [Delegated alert dismissal for code scanning](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-permissions?view=azure-devops) | | ADO permission-based dismissal is not approval-based dismissal. |
| [Code scanning alert-related audit log events](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | Exact event coverage was not established. |
| [Development section to track alert resolution progress](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | Work-item linking exists, but an equivalent alert development/progress section was not established. |
| [CodeQL scanning of GitHub Actions workflows](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | The ADO language/task documentation does not establish this integration. |
| [Rust language support](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | Listed in default setup and the current Initialize CodeQL task reference. |
| [CodeQL scans C/C++ projects without a build](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | C/C++ is included in ADO default setup, which uses `none` build mode. |
| [Code scanning alert assignment](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | Native assignment to a human owner was not established. |
| [Periodic code scanning of inactive repositories](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | Scheduled default-branch scans provide this outcome; no new commit is required to trigger the weekly schedule. |
| [Incremental CodeQL analysis for pull requests](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | No ADO integration statement established; do not infer support from CLI releases alone. |
| [Code scanning alerts linked to work-management items](https://learn.microsoft.com/en-us/azure/devops/release-notes/2025/sprint-264-update) | ✅ | Azure Boards work items rather than GitHub Issues. Programmatic linking is also documented in [Sprint 268](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-268-update). |
| [CodeQL data extensions for custom barriers and sanitizer models](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | CLI/query-pack-version dependent; not separately established in ADO documentation. |
| [CodeQL analysis of composite action metadata files](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | No ADO-specific support statement established. |
| [CodeQL static analysis and repository alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | |
| [Licensed standalone CodeQL CLI analysis](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | For repositories covered by a paid GHAzDO license; publish SARIF to ADO. |
| [Third-party code scanning / SARIF ingestion](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-publish-v1?view=azure-pipelines) | ✅ | |
| [Custom queries, query suites, and configuration files](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-codeql-init-v1?view=azure-pipelines) | ✅ | Advanced setup exposes query-suite and configuration-file inputs. |
| [Automatic CodeQL installation on self-hosted agents](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-codeql-init-v1?view=azure-pipelines) | ✅ | `enableAutomaticCodeQLInstall`. |
| [CodeQL default setup without YAML changes](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-276-update) | ✅ | Automatic language detection and an initial scan when enabled at organization/project level. |
| [PR-triggered CodeQL default setup](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | Documented default setup scans the default branch only. Use advanced setup/build validation for PR analysis. |
| [Go and Swift in CodeQL default setup](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | Both require advanced setup on ADO. |
| [PR annotations for code scanning](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | Requires configured build validation and appropriate baseline scans. |
| [Configurable severity thresholds and tool-specific merge rules](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | ADO's built-in checks cover new/all high-and-critical alerts, not an arbitrary matching threshold/ruleset configuration. |
| [Configurable default-setup agent pool and scan schedule](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | ✅ | Organization settings support Microsoft-hosted, self-hosted, and Managed DevOps Pools; weekly schedule selection. |

### Copilot Autofix and AI security

These additional rows capture relevant capabilities beyond the GHES matrix's code-scanning release list.

**Preview access caveat:** the [current how-to](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) labels Autofix public preview; [Sprint 281](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-281-update) calls it **limited public preview** and states that additional participants are not being accepted. Treat the feature as public preview with restricted/uncertain enrollment, not GA.

| Feature | Azure DevOps | Notes |
|---|---|---|
| [Copilot Autofix for code-scanning alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | ☑️ | Alert-driven remediation: the coding agent explores repository context, refines changes, and opens a fix PR. Preview-access caveat above applies. |
| [Autofix for third-party code-scanning alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | ☑️ | Supported alongside CodeQL alerts. |
| [Autofix for dependency vulnerabilities](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | ☑️ | Not deterministic scheduled Dependabot security/version updates. |
| [Repository custom instructions and remediation skills](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | ☑️ | Documented `.github` and `.agents` instructions/skill locations. |
| [Automatic inline Autofix suggestions on existing PR alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | | Documented alert-driven fix-PR generation does not establish automatic inline PR suggestions. |
| [Multiple alerts fixed in a single Autofix PR](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | | |
| [Custom Autofix agent pools](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | | Current Autofix prerequisites specify the Microsoft-hosted **Azure Pipelines** pool. This differs from configurable CodeQL scan pools. |
| [AI repository scanning beyond CodeQL-supported languages](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | |
| [AI application/agent scanning for prompt injection](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | | |

### Dependabot alerts

The native ADO equivalent for vulnerability detection is **dependency scanning**. It finds vulnerable direct and transitive dependencies using GitHub-reviewed advisories. This does not imply the availability of the Dependabot service or all its workflows.

- [Dependency scanning documentation](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops).
- [Supported package ecosystems](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning-ecosystems?view=azure-devops).

| Feature | Azure DevOps | Notes |
|---|---|---|
| [Vulnerable function-call / reachability information](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | Dependency paths and root dependencies are documented; vulnerable function-call reachability was not established. |
| [Dependabot auto-triage rules](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | No native rule-based auto-triage equivalent established. |
| [CVSS 4.0 schema for advisory scoring](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | CVSS-based severity is documented; explicit ADO CVSS 4.0 presentation/parity was not established. |
| [EPSS information for vulnerability prioritization](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | ADO displays **EPSS percentile**, not an identical advisory-score UI. |
| [EPSS filtering in dependency alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | Percentile range filter; metadata is not available for every advisory. |
| [Dependabot-specific metrics page](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | ADO Risk/Coverage/Alerts views exist; an equivalent dedicated metrics page was not established. |
| [Dependency vulnerability alert assignment](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | Work-item linking is separate from native alert assignment. |
| [Delegated alert dismissal for dependency alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-permissions?view=azure-devops) | | |
| [Alerts for known malicious package versions](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | 🔒 | Current ADO documentation labels malware findings **private preview**. |
| [Vulnerability alerts for direct and transitive dependencies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | Uses GitHub-reviewed advisories from the GitHub Advisory Database. |
| [Root-dependency and dependency-path remediation context](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | Helps locate the direct dependency responsible for transitive exposure. |
| [Development/runtime dependency scope](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | Scope badges and filters when the result includes scope metadata. |
| [Branch, pipeline, package, and severity filters](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | The alerts hub does not display scans completed on PR branches. |
| [Automatic closure when a vulnerable component is removed/fixed](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | Based on latest relevant pipeline results. |
| [Dependency-scanning default setup](https://learn.microsoft.com/en-us/azure/devops/release-notes/2025/sprint-264-update) | ✅ | Injects the scanning task into suitable pipelines; a pipeline must still run. |
| [Pipeline-independent dependency scanning](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | The documented ADO setup requires pipeline execution. |
| [PR dependency-vulnerability annotations and merge checks](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | ✅ | Requires build validation and baseline scanning; high/critical checks do not constitute full dependency review. |

### Dependabot updates

No native deterministic Dependabot security/version-update service was established in the reviewed ADO product documentation. The rows below deliberately retain the feature granularity of the GHES matrix, with empty support cells. **Agentic dependency Autofix is a separate public-preview capability**, listed above.

Feature links point to ADO dependency-remediation documentation for context, not to documentation claiming these Dependabot features exist on ADO.

| Feature | Azure DevOps | Notes |
|---|---|---|
| [Dependabot security-update PRs](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Scheduled Dependabot version-update PRs](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Dependabot support for devcontainer.json](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Dependabot access to Cargo private registries](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Pause scheduled update jobs after repeated failures](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Grouped security updates](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Private-registry support for target-branch configuration](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Updates for pnpm workspace catalogs](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Updates for .NET SDK global.json](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for Bun dependencies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for Docker Compose dependencies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for uv dependencies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Organization-wide Dependabot access grants](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for Helm dependencies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Custom update frequencies using cron expressions](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Gradle lockfile updates](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Minimum package-age configuration](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Cross-ecosystem grouped update PRs](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Central management of private-registry configurations](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for vcpkg and Rust toolchains](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Updates for private Go modules](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for Conda dependencies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [OIDC authentication for private registries](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Security updates for uv / Python](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for OpenTofu](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for Bazel](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for Julia](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for pre-commit hooks](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Organization-level OIDC for Dependabot/code-scanning registries](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Multiple private registries per ecosystem](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for Deno](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for sbt](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version/security updates for Poetry v2](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Cross-organization access to internal/private repositories](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Version updates for the Nix ecosystem](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Swift package updates in Xcode projects](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |

### Dependency graph, dependency review and snapshot submission API

ADO dependency scanning stores component snapshots and dependency relationships. That underlying data is **not evidence of a complete dependency-inventory UI, dependency-review API, or SBOM-export feature**.

| Feature | Azure DevOps | Notes |
|---|---|---|
| [Package URLs in generated SBOMs](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | Native SBOM export and package-URL coverage were not established. |
| [Component snapshots and dependency relationships](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | ✅ | Updated by dependency-scanning pipeline runs. |
| [Repository dependency-inventory UI](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | A vulnerability alert list is not a complete dependency inventory. |
| [Dependency graph REST API](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | Alert APIs do not establish a dependency-graph API equivalent. |
| [Dependency snapshot submission API](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | No GitHub-style general-purpose snapshot-submission API established. |
| [Native SBOM export through UI/API](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | Separate pipeline-based SBOM tools do not count as native export. |
| [Dependency review showing added, removed, and changed PR dependencies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | PR vulnerability annotations are available, but a full dependency-review diff was not established. |
| [Merge blocking for new high/critical dependency vulnerabilities](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | ✅ | ADO branch-policy status checks provide this narrower prevention outcome. |
| [Dependency-review license allow/deny policies](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | | |
| [Enterprise-wide open-source license-policy management](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | |

## Security Overview

Security overview summarizes security risk and scanning coverage across projects and repositories **within an Azure DevOps organization**.

- [Security Overview documentation](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops).

| Feature | Azure DevOps | Notes |
|---|---|---|
| [Historical enablement-trends dashboard](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | Coverage shows enablement/scanning status; historical enablement-trend parity was not established. |
| [Enterprise-level secret-scanning metrics and enablement trends](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | No multi-organization enterprise equivalent established. |
| [Security Overview group by tool](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | Tool filtering is documented; an equivalent group-by-tool dashboard was not established. |
| [Security Overview filter by security tool](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | ✅ | |
| [CodeQL pull-request alerts metrics view](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | |
| [SAST vulnerability summary in Security Overview](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | ✅ | Risk table filtered to code scanning; not an identical GitHub-specific dashboard. |
| [Prevention, detection, and remediation metrics dashboards](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | Risk counts do not establish full prevention/bypass/MTTR dashboard parity. |
| [CSV export from the CodeQL pull-request alerts metrics view](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | General ADO CSV export exists, but not an established equivalent PR-metrics view. |
| [Security Overview accessibility enhancements](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | No matching feature-specific release established; this is not a statement that ADO is inaccessible. |
| [Filter bar for delegated bypass requests](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | |
| [Overview includes PR-analysis CodeQL alerts on non-default branches](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | Documented Risk counts use default-branch findings. |
| [Organization-wide Risk, Coverage, and Alerts views](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | ✅ | Cross-project/repository views in organization settings. |
| [Organization-level Risk and Coverage rollup metrics](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | 🔒 | Explicitly labeled private preview in current documentation. |
| [Cross-repository alert search and tool-specific filters](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | ✅ | |
| [CSV export of risk, coverage, and alerts](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | ✅ | Alerts export is limited to the first **1,000** filtered alerts. |
| [Shareable filtered remediation views / security campaigns](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | ✅ | ADO calls shared URL-filtered alert views **security campaigns**. |
| [Managed campaigns with owners, notifications, and progress workflow](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | Shared filtered views do not establish the managed campaign lifecycle available on GitHub. |
| [Campaign-level bulk remediation with Copilot](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | |

## Administration

ADO supports organization/project/repository enablement and its own permissions and APIs. These should not be equated automatically with GitHub security configurations or GitHub-specific roles.

| Feature | Azure DevOps | Notes |
|---|---|---|
| [Enable security features at scale](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | ✅ | Organization/project bulk enablement; not a reusable named security-configuration object. |
| [Enterprise-level security configurations](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | No multi-organization reusable configuration/enforcement equivalent established. |
| [Security configurations for archived repositories](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | |
| [Advanced repository selection by security-feature status](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | | Coverage filters exist, but matching configuration-assignment selection was not established. |
| [Security manager role assigned directly to individual users](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-permissions?view=azure-devops) | | ADO has assignable permissions; no identical GitHub security-manager role established. |
| [Standalone Code Security and Secret Protection products](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | ✅ | Bundled arrangements also remain documented. |
| ["Not set" option in reusable security configurations](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | |
| [Security configurations selecting default/advanced CodeQL setup](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | Both setup modes exist; a reusable security-configuration object was not established. |
| [Enterprise Security Manager role](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-permissions?view=azure-devops) | | |
| [Code scanning default setup overriding Actions policies](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | GitHub Actions policy is not an ADO control. ADO has its own pipeline/task prerequisites. |
| [Audit log events for Dependabot configuration changes](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | | |
| [Security configurations setup wizard](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | No corresponding named configuration workflow established. |
| [Enterprise-wide disablement for private/internal repositories](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | | ADO enablement is organization/project/repository scoped, not a multi-organization enterprise control. |
| [Automatic enablement for future repositories/projects](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | ✅ | |
| [Granular alert/settings permissions](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-permissions?view=azure-devops) | ✅ | View alerts, manage/dismiss alerts, and manage settings. |
| [Advanced Security alert and enablement REST APIs](https://learn.microsoft.com/en-us/rest/api/azure/devops/advancedsecurity/alerts/list?view=azure-devops-rest-7.2) | ✅ | ADO-specific endpoints and schemas, not GitHub API compatibility. |
| [Programmatic alert-to-work-item linking](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-268-update) | ✅ | |
| [Secret validity-status service hooks](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/ghazdo/sprint-268-update) | ✅ | This does not imply identical coverage for every GitHub webhook/audit event. |

## Dependencies

The following requirements apply to the ADO capabilities that are documented as available or in preview.

| Feature | License required? | Azure Pipelines / agent capacity required? | Notes |
|---|---|---|---|
| [Security Overview](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-security-overview?view=azure-devops) | No, to access the overview | No | Available in organization settings even without enabled repositories; findings/coverage depend on the enabled products. |
| [Secret scanning](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | Secret Protection or GHAS bundle | No | Native repository/history scanning. |
| [Secret push protection](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | Secret Protection or GHAS bundle | No | Native Git/web-commit checks for supported patterns. |
| [Validity checks](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-secret-scanning?view=azure-devops) | Secret Protection or GHAS bundle | No | Automatically included for supported types. |
| [Dependency scanning](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-dependency-scanning?view=azure-devops) | Code Security or GHAS bundle | Yes | Default setup injects a task into suitable pipelines; a pipeline must execute. |
| [CodeQL default setup](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | Code Security or GHAS bundle | Yes, agent capacity | No YAML changes; managed scheduled scans. Pool/schedule configuration is separate from Autofix execution. |
| [CodeQL advanced setup](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-code-scanning?view=azure-devops) | Code Security or GHAS bundle | Yes for documented pipeline tasks | Standalone licensed CLI analysis with SARIF publishing is also supported. |
| [Third-party SARIF findings](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/advanced-security-publish-v1?view=azure-pipelines) | Code Security or GHAS bundle | Yes for the documented Publish task | Scanner execution and third-party licensing are separate. |
| [PR security annotations/status checks](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops) | Relevant security product | Yes | Requires configured scanning/build validation and baseline scans of relevant branches. |
| [Copilot Autofix](https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-autofix?view=azure-devops) | GHAS / Code Security entitlement; preview access | Yes, Microsoft-hosted Azure Pipelines pool | Requires a parallel job. Fix generation consumes separately Azure-billed AI credits; GitHub Copilot budgets do not govern this meter. |
