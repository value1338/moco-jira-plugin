# MOCO Jira Connector

[Deutsch](README.md) · [Privacy](PRIVACY.md) · [Security](SECURITY.md)

Track MOCO time from a Jira ticket, with the correct service selected **before the first booking**. An independent fork of [MOCO Zeiterfassung](https://github.com/everii-Group/mocoapp-browser-extension), provided by Riccardo Reichle.

**Version: 3.0.2.** Fixes floating-eye and dialog conflicts when the original extension is installed alongside this fork. The version approved by Google is available in the [Chrome Web Store](https://chromewebstore.google.com/detail/dddmgcfghhfpdhlfmclijiaenbjcolch).

## Why this fork?

The official extension supports many tools. This fork focuses on agencies whose Jira tickets already contain MOCO mappings. Comparison is against the reviewed [upstream 2.19.0 source](https://github.com/everii-Group/mocoapp-browser-extension/tree/1254670424556d359ffe7748e35338ab18c8e23f).

| Workflow                              | Reviewed upstream behavior                                                                    | This fork                                                                                                             |
| ------------------------------------- | --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| First booking on a new Jira ticket    | Defaults from previous bookings/project identifiers, with fallbacks down to the first project | Explicit Jira MOCO IDs take precedence; incomplete mappings require manual selection                                  |
| Epic/project and Task/service mapping | Jira adapter does not read these custom ID fields                                             | Resolves the hierarchy and validates IDs against the returned MOCO catalog                                            |
| Subtasks                              | No hierarchical mapping in the Jira adapter                                                   | Inherits the nearest mapped service; an explicit valid override is supported; preserves the subtask's own key and URL |
| Setup                                 | General extension options                                                                     | In-page setup overlay; detects the actual Jira origin and a uniquely named numeric MOCO field                         |
| Site access                           | General integrations                                                                          | Runtime grants for the two specific configured sites; dynamically registered content script                           |
| Personal API key                      | Chrome Sync storage in reviewed source                                                        | Extension-local storage; no Chrome Sync or developer backend                                                          |
| Start failures                        | Reviewed background handler does not display non-422 failures                                 | Bounded waits, visible errors, no automatic booking retry, duplicate start-click guard                                |
| Ticket/account changes                | No equivalent pre-write verification in reviewed handler                                      | Rechecks the sending tab, ticket identity and connection revision                                                     |

This fork does not create Jira issues, synchronize projects, or provision the Jira↔MOCO mapping. It is not an official MOCO or Atlassian product. Their names remain their owners' trademarks.

## Mapping model

```text
Jira Epic       → MOCO project
  Jira Task     → MOCO service (called task in MOCO)
    Subtask     → inherited service, its own Jira ticket identity
```

The same numeric Jira custom field holds a **MOCO project ID on an Epic** and a **MOCO service ID on a Task**. Synthetic example: Epic `DEMO-1` maps to project `1001`; Task `DEMO-2` maps to service `2001`. Its subtask books against service `2001`, retaining its own issue key and URL in the time entry.

The mapped service must belong to the mapped project and appear in the authenticated user's MOCO catalog. Missing permissions, ambiguous IDs, incomplete parents or invalid mappings require manual selection. Opening the dialog never starts a timer.

## Setup

1. Have a personal MOCO API key with booking access and an existing Jira Cloud browser session. The mapping field must already exist and contain correct IDs.
2. Download and extract the plugin ZIP, open `chrome://extensions`, enable Developer mode and load the folder containing `manifest.json` unpacked. The repository contains the same files in `Chrome/`. The original extension can remain installed for comparison: our eye and dialog are isolated. Use one of the two controls for each booking.
3. Open a Jira ticket and click the extension toolbar icon. Setup opens as an overlay. The actual tab origin is detected; exactly one numeric custom field containing “MOCO” in its name is preselected. Multiple candidates require selection; otherwise enter the field ID supplied by your Jira administrator.
4. Enter your MOCO subdomain and **personal** MOCO API key (profile → integrations). Review and save. Chrome requests access to the two specific sites. No Jira API token is required.
5. Reload an already open Jira tab if necessary. Open a Task, review customer/project/service, then explicitly start the timer or save a duration.

Use “Einstellungen” in the time dialog to edit/disconnect the connection. If the overlay is unavailable, setup opens as its own extension page. One agency connection per Chrome profile; each person uses their own key. UI currently uses German labels.

## MOCO booking access and employee onboarding

A Jira assignment and valid MOCO IDs are not enough: the owner of the personal MOCO key must be assigned and active for time tracking on the MOCO project. The project and service must also be bookable. This applies to ad hoc work received through Jira as well. The extension does not create staff assignments or expand permissions.

MOCO already provides standard tools for this. Distinguish these two cases:

- **New project, existing employees:** MOCO **Settings → Projects** can add all people when creating a project. Each agency must check this setting; the extension does not enable it.
- **New employee, existing projects:** Under **Staff → Person → Projects → +**, assign selected or all existing projects for time tracking. Do not assume the new-project default retroactively adds a new employee to old projects. Include this check in onboarding.
- **One-off task:** Have the employee added for time tracking on the relevant MOCO project. Then close and reopen the Jira time dialog to reload the personal project catalog. Entries still use the mapped service and retain the Jira ticket reference.

If the extension reports that the Jira project is not uniquely bookable despite correct IDs, check the personal project assignment, active services and the MOCO user whose key is configured. Manual selection cannot make an unavailable MOCO project bookable either. The extension never silently selects a different project.

[MOCO: People and teams for time tracking](https://www.mocoapp.com/en/features/17-projects/content/22-projects-general-information) · [MOCO: Missing projects in time tracking](https://intercom.help/mocoapp/de/articles/6704447-zeiterfassung)

## The AB3 eye

- **Yellow:** connected to MOCO; no running timer reported.
- **Red with a recording dot:** MOCO reports a running timer. The time dialog shows what it is tracking.
- **Grey:** not configured, disconnected, or MOCO currently unreachable.

The AB3 eye belongs to this fork. When the original eye is also visible, ours appears above it; otherwise it stays in the bottom-right corner. The two time dialogs remain separate. Color, a recording dot and labels communicate its state. Visible Jira tabs refresh immediately on load, every 30 seconds and when returning to the tab; changes in other windows can appear with a short delay. Status reads never start or stop a timer.

## Limitations and recovery

- Implemented for Jira Cloud's `parent` hierarchy and a numeric mapping field. Jira Server/Data Center, legacy Epic Link fields and multiple simultaneous agency connections are not guaranteed.
- Redirects are detected through the final tab origin. Custom HTTPS Jira domains can be configured, but REST discovery depends on Atlassian allowing those calls. [Atlassian documents current limitations](https://jira.atlassian.com/browse/AX-667). Manual assignment remains available; custom-domain compatibility is not guaranteed.
- Hidden or differently named fields cannot be safely inferred, so the field ID remains editable.
- MOCO controls personal booking permissions and available services. An existing timer is displayed; opening does not modify it.
- A lost write response may hide a successful booking. The extension never retries automatically; check MOCO before reopening and trying again.
- Disconnect removes local credentials and domain grants; it does not stop an existing MOCO timer.
- Uses upstream's MOCO browser-extension interface `/api/browser_extensions`; changes by MOCO may require updates.

## Distribution and security

This repository contains the ready-to-install extension (`Chrome/` and `moco-jira-connector-3.0.2.zip`), documentation, privacy policy and license. Tests, fixtures, development files and internal verification logs are not published here. The extension JavaScript is included in the shipped bundles. The plugin ZIP contains no tests or credentials.

Manifest V3, locally bundled code, no remotely executed code. Users enter their own personal MOCO key during setup in their own browser. Security checks find known issues; they cannot guarantee vulnerability-free software. Never post personal keys or customer data in public issues. [Report vulnerabilities privately](SECURITY.md).

MIT licensed, preserving upstream attribution. The public distribution excludes internal exports, credentials and private development history.
