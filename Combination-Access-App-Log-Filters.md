# Logs 2.0 — Smart Filter: Combinations & Open Items

Picking a value in one field narrows the other two to only the combinations that can actually occur.

---
## 1. Access Log — valid combinations

Source of truth: `conf/access-log-resources.xml` (the URL/schedule/queue registry that sets `category`, `action` and `component_type` at request init), plus the Java write sites that overwrite those fields afterwards via `LogsUtil.fillAccessLog`.

`(no specific component)` = the entry records no component type. `(any)` = the trigger type is not one of the three the UI offers — Admin Action, Queue and App Edit Action — so it does not narrow the Trigger Type field. **†** = the component type is recorded on the log but is **not** offered in the Component Type dropdown.

| Component Type | Trigger Type | Activities |
|---|---|---|
| AI Skill † | API | AI Skill Execution |
| Approval workflow | User actions in apps | Record Approval, Record Recalled, Record Rejection |
| Batch workflow | Schedules | Batch Process Completion, Batch Process Execution, Batch Process Start |
| Blueprint | User actions in apps | Transition Trigger |
| Custom API † | API | Custom API Execution |
| Custom schedule | Schedules | Custom Schedule Execution |
| Form | API | Add Record(s), Create Bulk Insert Job, Delete Record by Id, Delete Record(s), Download Bulk Insert Result, Duplicate Record(s), Edit Record by Id, Edit Record(s), Get Bulk Insert Job Status, Get Fields Meta, Start/Abort Bulk Insert Job, Upload Bulk Insert File, Upload File, Upload File to Subform, Upsert Record(s) |
| Form | User actions in apps | Add Record(s), Add Subform Row, Add/Edit Field Input, Delete Record(s), Delete Subform Row, Duplicate Record(s), Edit Record(s), Form Load, Form Load Workflow Execution, Stateless Form Button Click |
| Form schedule | Schedules | Form Schedule Execution |
| _(no specific component)_ | API |  Get Applications Meta, Get Forms Meta, Get Pages Meta, Get Reports Meta, Get Sections Meta |
| Page | User actions in apps | Execute Page Function, Filter Page Chart Data, Page Button Execution, Print Page, View Page |
| Payment workflow | User actions in apps | On Payment |
| Report | API | Create Bulk Read Job, Download Bulk Read Result, Download File, Download File from Subform, Get Bulk Read Job Status, Get Record by Id, Get Record(s) |
| Report | User actions in apps | Add Comment, Delete Comment, Export Report, Print Report, Save Record, Save Report, Update Kanban Event, Update Record Event, View Record, View Report |
| Report workflow | User actions in apps | Report Button Click |

**Component types are recorded but unpickable.** AI Skill, Custom API, Event Pipeline are all set on the log — by `access-log-resources.xml` for the first three plus Chat Agent, and by the queue registry for Async Function — but the Component Type dropdown offers only the eleven app-level ones. Their activities are equally absent from the Activity dropdown, so today none of these rows is reachable from the filter at all, even though `component_type = "ai_skill"` and friends are valid in a log query.

**Workflow component types.** Approval workflow, Payment workflow, Report workflow, Blueprint, Batch workflow and Function all record activities of their own — the rows above. The smart-filter table in `logs-filter-slider.js` carries none of them, so today those six are "unknown" values that never constrain and are never disabled: picking one leaves the other two fields wide open.

---
## 2. Access Log — tags allowed per component type

Tags are an Access-Log-only filter. After you choose a Component Type, the Tag list shows only the tag keys that apply to it. **This is one-way only** — the Component Type narrows the Tag list, but changing a tag does not narrow the Component Type (unlike the fields above, which work both ways).

| Component Type | Tag keys available |
|---|---|
| Form | form_button, field, subform_field |
| Report | form, report, report_button |
| Page | page_element |
| Approval workflow | form, approval_level |
| Report workflow | report, report_button |
| Blueprint | form, transition |

`version` is available for every component type.

---
## 3. App Log — valid combinations

Hierarchy: Log Type → Subtype → Event.

| Log Type | Subtype | Events |
|---|---|---|
| Workflow | Form workflow | On Create - Form Load, On Create - On Success, On Create - Subform Add Row, On Create - Subform Delete Row, On Create - User Input of a Field, On Create - Validate, On Create/Edit - Form Load, On Create/Edit - On Success, On Create/Edit - Subform Add Row, On Create/Edit - Subform Delete Row, On Create/Edit - User Input of a Field, On Create/Edit - Validate, On Delete - On Success, On Delete - Validate, On Edit - Form Load, On Edit - On Field Update, On Edit - On Success, On Edit - Subform Add Row, On Edit - Subform Delete Row, On Edit - User Input of a Field, On Edit - Validate, On Stateless Form Button Trigger, On Create - Field Rules, On Edit - Field Rules, On Create/Edit - Field Rules |
| Workflow | Schedule | Custom Schedule, Form Schedule |
| Workflow | Approval | On Approval, On Approval Start, On Rejection |
| Workflow | Payment | On Payment Failure, On Payment Success, Payment Initiated |
| Workflow | Report workflow | On Click of Button |
| Workflow | Blueprint | On Transition Trigger |
| Workflow | Batch workflow | Batch Execution, Batch Failure, Batch Initialisation, Batch Success |
| Workflow | Page script | Page - HTML Snippet, Page - ZML Snippet, Page Script |
| Task | Web data | Integration Task, Invoke API, Invoke URL |
| Task | Notification | Email, SMS |
| Task | Debug | Info |
| Workflow | Function | _(no events — picking this locks the Event field)_ |


Six of the events above are **not in the Event dropdown** (it lists 42 options): On Approval Start, Payment Initiated, Batch Initialisation, Batch Execution, Batch Success, Batch Failure. All six carry a link name in `AppLogEntity.Action`, so they are already valid in a log query — `on_approval_start`, `payment_initiated`, `batch_initialisation`, `batch_execution`, `batch_success`, `batch_failure` — they just cannot be picked in the filter yet.

---
## 4. Missed values — options in the dropdown that never return data

Left visible in the filter, but nothing is ever recorded for them, so selecting them gives no results.

### Access Log
- **Payment Callback** — no code writes this action. Removed from the smart-filter combinations.
- **Upsert Record(s)** — Upsert requests are intentionally not saved to the access log.
- **Batch Process Initiation** — registered as an activity, but the batch executor writes `Batch Process Start` instead. Conversely `Batch Process Start` and `Batch Process Completion` are written by the executor but are not registered in `AccessLogActivity`, so they carry no i18n key and are not queryable by token. Same activity, three names — needs one.
- **Viewed Encrypted Data** — registered in `AccessLogActivity` and listed in the Access Log field reference, but no URL in `access-log-resources.xml` and no Java write site sets it.
