# Logs feature — i18n conversion: ACTION ITEMS (pending)

Only the **pending** work. Applied conversions are logged in **`logs-i18n-applied.md`**.

Legend: 🟡 key exists but value differs (needs a decision) · 🔴 no key (client to add to `en.js`) · ⚠ existing key value is wrong.

## Batch status

| Batch | Scope | Status |
|-------|-------|--------|
| 1 | `logs-filter-slider.html` | Conversions applied; only 🔴 missing keys + ⚠ key fixes pending |
| 2a | `logs-util.js` column labels | Applied (5); 2 missing keys below |
| 2b | `dynamic-logs-list.html` | Applied (7 reuses); 3 missing/no-key items below |
| 3 | `access/app/task-log-summary.html` (detail views) | Done — visible text already i18n; aria-labels left hardcoded (per feature convention) |
| 4 | `logs-filter-slider.js` / `dynamic-logs-list.js` (JS toasts, placeholders) | Applied; missing keys below |
| 5 | `operations-logs.html` + onboarding | Done — already i18n; only missing onboarding keys below |

---

## 🔴 Missing keys — client to ADD to `Creator/en.js` (Batch 1)

Left hardcoded until the key exists.

| Line | Element | Value to use | Suggested key |
|------|---------|--------------|---------------|
| 263 | Access · Error Code placeholder | `Enter Error Code` | `zc.logs.accesslog.quickview.filterpane.errorcode.placeholder` |
| 24  | Access · Trigger-type option "Schedules" | `Schedules` | `zc.logs.accesslog.quickview.filterpane.logtype.schedules` |
| 507 | App · Message placeholder | `Enter Message` | `zc.logs.applog.quickview.errormsg.placeholder` |

> Resolved by reuse (no new key needed): app Component placeholder (449), app Environment
> label (422), and app Duration operator placeholder (462) — see `logs-i18n-applied.md`.

---

## ⚠ Existing keys the client should FIX (bad values)

| Key | Current value | Issue |
|-----|---------------|-------|
| `zc.logs.accesslog.quickview.filterpane.duration.placeholder` | defined twice (`Select` and `Value`) | duplicate key — keep one |
| `zc.logs.applog.quickview.status.placeholder` | `Type` | likely should be `Select Status` |
| `zc.logs.applog.quickview.applinkname.placeholder` | `Select Application link Name` | lowercase "link" typo |

---

# Batch 2 — pending

Conversions applied are in `logs-i18n-applied.md`. Only these remain:

### 🔴 Missing keys — client to ADD to `en.js`
| File | Line(s) | String | Suggested key |
|------|---------|--------|---------------|
| `dynamic-logs-list.html` | 78 / 180 | `Filtering by:` | new key, e.g. `zc.logs.commonmsg.filteringby` |
| `dynamic-logs-list.html` | 119 / 223 | `Actions` (column header) | no generic key exists — add `zc.c6.commonmsg.button.actions` (only feature-specific "Actions" keys exist; not reused to avoid cross-feature coupling) |

### ✏️ Value change request for user-education team
| Key | Current value | Requested value | Why |
|-----|---------------|-----------------|-----|
| `zc.logs.accesslog.quickview.recordid` | `Record ID` | `Record ID(s)` | Access-log record id is plural; this key now drives the Access column label + header (App key `zc.logs.applog.quickview.recordid` stays `Record ID`) |

---

# Batch 4 — pending

Code changes applied (see `logs-i18n-applied.md`). These keys are **referenced in the JS** (via `_I18N['key'] || 'fallback'`) but do **not exist in `en.js`** yet, so they currently show the English fallback. Client to ADD:

| Key | Value to add | Used in |
|-----|--------------|---------|
| `zc.logs.accesslog.quickview.filterpane.duration.validationerror` | `Time Taken operator and unit are required when a value is provided` | filter-slider toast (access) |
| `zc.logs.applog.quickview.duration.validationerror` | `Time Taken operator and unit are required when a value is provided` | filter-slider toast (app) |
| `zc.logs.accesslog.quickview.filterpane.filtersapplied` | `Filters applied successfully` | filter-slider toast (access) |
| `zc.logs.applog.quickview.filtersapplied` | `Filters applied successfully` | filter-slider toast (app) |
| `zc.logs.common.sortedsuccess` | `Logs sorted successfully` | dynamic-logs-list sort toast |

---

# Batch 5 — pending

`operations-logs.html` is already i18n. The onboarding (`logs-util.js` → `showLogsOnBoarding`) already uses `_I18N['key'] || 'fallback'`, but **none of the keys exist in `en.js`** — they render the English fallback. Client to ADD:

| Key | Value to add |
|-----|--------------|
| `zc.logs.onboarding.popup.title` | `Logs 2.0` |
| `zc.logs.onboarding.popup.desc` | *(popup subtitle — currently empty fallback; ed team to supply)* |
| `zc.logs.onboarding.explore` | `Explore the All-New Logs` |
| `zc.logs.onboarding.slide1.title` | `Introducing the all-new Logs` |
| `zc.logs.onboarding.slide1.desc` | `We've upgraded Logs to provide deeper insights into user activity and application processes. The new update makes it easier to trace user actions, monitor triggered automations, and debug issues across your apps at a granular level.` |
| `zc.logs.onboarding.slide2.title` | `Access Logs` |
| `zc.logs.onboarding.slide2.desc` | `Access Logs show who accessed your app components, when they accessed it, and what actions they performed, helping you track events across your app for easier debugging and issue investigation.` |
| `zc.logs.onboarding.slide3.title` | `Application Logs` |
| `zc.logs.onboarding.slide3.desc` | `Application Logs capture triggered workflows and tasks across your app, helping you to track app behavior, identify failures, and analyze performance.` |
| `zc.logs.onboarding.slide4.title` | `Advanced Log Filters` |
| `zc.logs.onboarding.slide4.desc` | `Advanced filtering options enable you to quickly locate relevant logs to isolate specific events for debugging purposes. Narrow results by execution time, specific action, individual user, record ID, error codes, and more.` |

---

# Backend → client response: enum/constant values missing i18n keys (customer-visible)

These enum values are sent in the log response and shown to the customer (table cells / detail view / filter option labels). Their enum `getI18nKey()` is empty (or `AccessLogActionI18nMap` maps them to no key), so the raw English value is shown. Client to ADD these keys to `en.js`.

> Verified live against `en.js` (supersedes the stale `i18n-missing-keys.md`). Every *other* enum key already exists.
> Rendering note: `recordType` (Log Type) and `status` cells translate via `_I18N[key] || english`; the `trigger_type`/`activity`/`component_type`/`subtype`/`event` cells currently show the raw value (their `_i18n_key` companion isn't applied yet) — so the client also needs to use the `_i18n_key` for these to actually localize once keys exist.

### Access Log — `AccessLogEntity.Category` (Trigger Type)
| English value | Suggested key |
|---|---|
| App Edit Action | `zc.logs.accesslog.quickview.filterpane.logtype.appeditaction` |
| Queue | `zc.logs.accesslog.quickview.filterpane.logtype.queue` |
| Admin Tool Operation | `zc.logs.accesslog.quickview.filterpane.logtype.admintooloperation` |

### Access Log — `AccessLogEntity.ComponentType`
| English value | Suggested key |
|---|---|
| AI Skill | `zc.logs.accesslog.quickview.filterpane.comptype.aiskill` |
| Async Function | `zc.logs.accesslog.quickview.filterpane.comptype.asyncfunction` |

### Access Log — `AccessLogEntity.SubCategory`
| English value | Suggested key |
|---|---|
| External API | `zc.logs.accesslog.quickview.subcategory.externalapi` |
| Mobile API | `zc.logs.accesslog.quickview.subcategory.mobileapi` |
| Internal API | `zc.logs.accesslog.quickview.subcategory.internalapi` |
| AI Call | `zc.logs.accesslog.quickview.subcategory.aicall` |

### Access Log — `AccessLogEntity.Mode`
| English value | Suggested key |
|---|---|
| User | `zc.logs.accesslog.quickview.mode.user` |
| System | `zc.logs.accesslog.quickview.mode.system` |

### Access Log — Activity (`AccessLogActionI18nMap`, mapped to empty suffix)
| English value | Suggested key |
|---|---|
| Batch Process Execution | `zc.logs.accesslog.quickview.filterpane.activity.batchprocessexecution` |
| Batch Process Initiation | `zc.logs.accesslog.quickview.filterpane.activity.batchprocessinitiation` |
| Approval Action | `zc.logs.accesslog.quickview.filterpane.activity.approvalaction` |
| Report Action Button Execute | `zc.logs.accesslog.quickview.filterpane.activity.reportactionbuttonexecute` |
| Form Button Execute | `zc.logs.accesslog.quickview.filterpane.activity.formbuttonexecute` |
| Blueprint Execute | `zc.logs.accesslog.quickview.filterpane.activity.blueprintexecute` |

### App Log — `AppLogEntity.Category` (Subtype)
| English value | Suggested key |
|---|---|
| AI Assistance | `zc.logs.applog.quickview.logtype.aiassistance` |
| AI Skill - System Tool | `zc.logs.applog.quickview.logtype.aiskillsystemtool` |
| AI Skill | `zc.logs.applog.quickview.logtype.aiskill` |

### App Log — `AppLogEntity.Action` (Event)
| English value | Suggested key |
|---|---|
| On Start | `zc.logs.applog.quickview.eventortaskname.onapprovalstart` |
| Batch Initialisation | `zc.logs.applog.quickview.eventortaskname.batchinitialisation` |
| Batch Execution | `zc.logs.applog.quickview.eventortaskname.batchexecution` |
| Batch Success | `zc.logs.applog.quickview.eventortaskname.batchsuccess` |
| Batch Failure | `zc.logs.applog.quickview.eventortaskname.batchfailure` |
| Payment Initiated | `zc.logs.applog.quickview.eventortaskname.paymentinitiated` |
| Invoke AI Agent Tool | `zc.logs.applog.quickview.eventortaskname.invokeaiagenttool` |

# HTML — filter section headers with missing key value (render blank)

| Location | English value | Key (add to en.js) |
|---|---|---|
| `logs-filter-slider.html:117` (Access "Component Details") | Component Details | `zc.logs.accesslog.quickview.filterpane.compdetails` |
| `logs-filter-slider.html:408` (App "Component Details") | Component Details | `zc.logs.applog.quickview.compdetails` |
