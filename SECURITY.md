# Security

Supported public candidate: 3.0.x. This is not a guarantee that no vulnerabilities exist.

Report suspected vulnerabilities privately to **servus@alpenblickdrei.com**, subject “Security: MOCO Jira Connector”. Include affected version, reproducible steps and expected impact. Do not include live API keys, passwords or customer information. Use public GitHub Issues for non-sensitive functional bugs.

## Trust boundaries

- Personal MOCO key lives in extension-local storage, not Chrome Sync. This is browser profile storage, not a hardware-backed secret vault.
- MOCO destinations are fixed to validated tenant labels under `https://*.mocoapp.com`; redirect following is disabled. Development builds also use HTTPS MOCO only.
- No personal key is sent to Jira, the publisher, analytics providers or a proxy.
- Jira metadata/hierarchy reads use the current tab's session, same-origin GET requests, explicit fields and bounded traversal. No Jira writes.
- Optional origins are requested for the two exact configured sites, not all HTTPS sites at runtime. The manifest declares optional `https://*/*` to allow user-specific Jira custom domains; it is not a blanket grant.
- Dynamic injection targets the configured origin. `activeTab` permits one-off setup discovery after a toolbar click.
- Settings render in an extension-origin iframe. All ancestor origins must match the trusted configured/click-captured Jira origin; unrelated and nested hostile frames receive no credential form. No key is posted to the web page.
- Mutation handlers check the sender tab, ticket identity and current connection revision. Opening/discovery never creates time entries.
- Explicit mutation responses are not retried automatically. Unknown write outcomes require checking MOCO first.
- Disconnect deletes connection settings and releases optional host grants. MOCO records/timers are unaffected.

## Verification

Installed-Chrome integration and Store review remain open. The dependency audit covers known registry advisories at execution time, not undiscovered vulnerabilities. Code review and unit tests do not replace installed-Chrome integration tests, independent penetration testing or ongoing maintenance.

Tests and development files are kept locally and are not included in this distribution repository. No production credentials are required for the local automated checks.
