# Logs Filter — Label / Placeholder Changes (App Log + Access Log)

Tracking doc for the **hardcoded** string changes made in the logs filter UI, for the
User Education / i18n team to formalize into i18n keys.

**Files where the hardcode lives:**
- Filter labels/placeholders → `…/components/templates/operations/logs/logs-filter-slider.html`
- Column headers + Tags value input → `…/mixins/operations/logs-util.js`
- i18n keys → `static/…/Dashboard/Creator/en.js`

**Legend — Action for i18n team:**
- `value` — key exists; just change its English value (then propagate to other locales).
- `NEW` — key missing / shared; create a new logs-specific key.
- `shared` — current key is `zc.c6.usagelog.type`, used elsewhere (`logs-data.html`, usage log) — must NOT be repurposed → create new key.

---

## 1. APP LOG filter (already hardcoded)

| # | Field (scope) | i18n key | Current en.js value | Hardcoded string (now) | Action |
|---|---|---|---|---|---|
| 1 | Subtype — placeholder | `zc.logs.applog.quickview.subtype.placeholder` | `Select Category` | `Select Subtype` | value |
| 2 | Event — label **+ column** | `zc.logs.applog.quickview.eventortaskname` | `Event Type/Deluge Task` | `Event` | value |
| 3 | Record ID — label **+ column** | `zc.logs.applog.quickview.recordid` | `Record ID` | `Record ID (s)` | value |
| 4 | Access Log ID — placeholder | `zc.logs.applog.quickview.accesslogid.placeholder` | `Access Log ID` | `Enter Access Log ID` | value |
| 5 | Service — label **+ column** | `zc.logs.applog.quickview.servicename` | `Service Name/Domain` | `Service` | value |
| 6 | Application Link Name — placeholder | `zc.logs.applog.quickview.applinkname.placeholder` | `Select Application link Name` | `Select Application Link Name` | value |
| 7 | Workflow Link Name — placeholder | `zc.logs.applog.quickview.workflowlinkname.placeholder` | `Select Workflow Name` | `Select Workflow Link Name` | value |
| 8 | Duration operator — placeholder | `zc.c6.usagelog.type` | *(shared; not in Creator/en.js)* | `Select Operation` | **NEW / shared** |
| 9 | Status — placeholder | `zc.logs.applog.quickview.status.placeholder` | `Type` | `Select Status` | value |
| 10 | Error Code — placeholder | `zc.logs.applog.quickview.errorcode.placeholder` | `Error Code` | `Enter Error Code` | value |
| 11 | Message — placeholder | `zc.logs.applog.quickview.filterpane.message.placeholder` | `Message` | `Enter Message` | value |

Column headers (rows 2, 3, 5) are hardcoded in `logs-util.js` → `appLogColumnMeta`
(`apl-col-event`, `apl-col-record-id`, `apl-col-service-name`) by blanking `i18nKey` and
setting `label`. Applies to the App Log table only (Task Log `tlog-col-*` left unchanged).

---

## 2. ACCESS LOG filter (this change)

| # | Field (scope) | i18n key | Current en.js value | Hardcoded string (now) | Action |
|---|---|---|---|---|---|
| 1 | Access Log ID — placeholder | `zc.logs.accesslog.quickview.filterpane.accesslogid.placeholder` | `Access Log ID` | `Enter Access Log ID` | value |
| 2 | Application Link Name — placeholder | `zc.logs.accesslog.quickview.filterpane.applinkname.placeholder` | `Select Application Link Name` | *(already correct — NOT hardcoded)* | none |
| 3 | Component Link Name — placeholder | `zc.logs.accesslog.quickview.filterpane.complinkname.placeholder` | `Select Component Name` | `Select Component Link Name` | value |
| 4 | User Email Address — placeholder | `zc.logs.accesslog.quickview.filterpane.useremail.placeholder` | `Enter Email` | `Enter User Email Address` | value |
| 5 | Duration operator — placeholder | `zc.c6.usagelog.type` | *(shared; not in Creator/en.js)* | `Select Operation` | **NEW / shared** |
| 6 | Error Code — placeholder | `zc.logs.accesslog.quickview.filterpane.errorcode.placeholder` | *(KEY MISSING in en.js)* | `Enter Error Code` | **NEW** |
| 7 | Tags — value input placeholder | `zc.logs.accesslog.quickview.filterpane.tags.inputvalue` | `Input Value` | `Enter Value` | value |

Notes:
- **#2 Application Link Name** was already `Select Application Link Name` in en.js — no code change made; listed here only for completeness.
- **#6 Error Code** — the placeholder key does not exist in `en.js` today (the field had no resolved placeholder). i18n team should **create** `zc.logs.accesslog.quickview.filterpane.errorcode.placeholder = "Enter Error Code"`.
- **#7 Tags** value input is generated in `logs-util.js` (two spots); the `_I18N[...tags.inputvalue] || 'Input Value'` expression was replaced with the literal `Enter Value`.

---

## Shared key that needs a NEW key (both tabs)

`zc.c6.usagelog.type` currently powers the **Duration operator** placeholder in *both* the App Log and Access Log filters, and is also used in `logs-data.html` / the usage log. Create a dedicated logs key, e.g.:

```
zc.logs.quickview.duration.operator.placeholder = "Select Operation"
```

and repoint both `app-log-filter-timetaken` and `access-log-filter-timetaken` placeholders to it.

---

## Revert note
All rows marked `value` are temporary literals in the template / column meta / tags-input JS.
Once the i18n team updates or creates the keys above, revert each field back to
`{{zc_i18n('…')}}` (and restore the blanked column `i18nKey`s) so localization works again.
