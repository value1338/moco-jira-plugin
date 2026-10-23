# Changelog

## 3.0.1 candidate — 2026-10-23

- Original AB3 eye: yellow ready, red with recording dot while tracking, grey disconnected/unreachable.
- Matching toolbar, ticket button, dialog header and error graphics, with accessible status labels and setup legend.
- Read-only status refresh on visible Jira tabs every 30 seconds and on visibility return.
- Serialize indicator updates and invalidate reads after account/site changes; reset old per-tab icons on disconnect.
- Production build and installed-browser validation remain separate checks; no automatic time booking.

## 3.0.0 candidate — 2026-10-23

- Public multi-agency Jira-focused fork, original connector branding, DE/EN documentation.
- Explicit Epic/project, Task/service and inherited Subtask mapping with manual fallback.
- Toolbar-based Jira origin and unique numeric MOCO field discovery; isolated settings overlay.
- Personal credentials local only; exact optional domain grants and dynamic injection.
- Sender-tab, current-ticket, request-generation and connection-revision checks.
- Timeouts, visible start/stop errors, duplicate-start protection, no automatic write retries.
- Compatible dependency security updates, pinned-action CI, security reporting policy.
- Installed-Chrome live booking acceptance remains open; this entry is not a Store release announcement.

Based on upstream 2.19.0, commit 1254670424556d359ffe7748e35338ab18c8e23f. Upstream authors retain their original MIT copyright.
