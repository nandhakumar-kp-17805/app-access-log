# Logs 2.0 — Smart Filter: Combinations & Open Items

Picking a value in one field narrows the other two to only the combinations that can actually occur.

---
## 1. Access Log — valid combinations

`(no specific component)` = general/API action not tied to a component type. `(any)` = activity has no trigger type in the UI (Admin/Queue actions).

| Component Type | Trigger Type | Activities |
|---|---|---|
| Custom schedule | Schedules | Custom Schedule Execution |
| Form | API | Add Record(s), Delete Record by Id, Delete Record(s), Duplicate Record(s), Edit Record by Id, Edit Record(s), Get Fields Meta, Upload File, Upload File to Subform, Upsert Record(s) |
| Form | User actions in apps | Add Record(s), Add Subform Row, Delete Record(s), Delete Subform Row, Duplicate Record(s), Edit Record(s), Form Load |
| Form schedule | Schedules | Form Schedule Execution |
| _(no specific component)_ | API | AI Skill Execution, Add Event, Custom API Execution, Delete Text Extraction Document, Get Applications Meta, Get Forms Meta, Get Pages Meta, Get Reports Meta, Get Sections Meta, Retry Event, Upload Document For Text Extraction |
| _(no specific component)_ | _(any)_ | Disable Advance Audit, Enable Advance Audit, Event Pipeline Consumer Execution, Export Audit, Remove Capture IP Address, Restore Record, Save Capture IP Address |
| Page | User actions in apps | Execute Page Function, Filter Page Chart Data, Page Button Execution, Print Page, View Page |
| Report | API | Create Bulk Read Job, Download Bulk Read Result, Download File, Download File from Subform, Get Bulk Read Job Status, Get Record by Id, Get Record(s) |
| Report | User actions in apps | Add Comment, Delete Comment, Export Report, Print Report, Save Record, Update Kanban Event, Update Record Event, View Record, View Report |

Component types with **no activities of their own** (picking them keeps the other fields open): Approval workflow, Payment workflow, Report workflow, Blueprint, Batch workflow, Function.

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
| Workflow | Approval | On Approval, On Rejection |
| Workflow | Payment | On Payment Failure, On Payment success |
| Workflow | Report workflow | On Click of Button |
| Workflow | Blueprint | On Transition Trigger |
| Workflow | Page script | Page - HTML Snippet, Page - ZML Snippet, Page Script |
| Task | Web data | Integration Task, Invoke API, Invoke URL |
| Task | Notification | Email, SMS |
| Task | Debug | Info |
| Workflow | Batch workflow | _(no events — picking this locks the Event field)_ |
| Workflow | Function | _(no events — picking this locks the Event field)_ |
| _(AI — no log type in UI)_ | Prompt-based assistance | _(no events — picking this locks the Event field)_ |

---
## 4. Missed values — options in the dropdown that never return data

Left visible in the filter, but nothing is ever recorded for them, so selecting them gives no results.

### Access Log
- **Payment Callback** — no code writes this action. Removed from the smart-filter combinations.
- **Upsert Record(s)** — Upsert requests are intentionally not saved to the access log.

### App Log — events never logged
App Creation using AI, Form Creation using AI, Deluge Assistance with AI, Delete Record.

### App Log — events logged only under an AI category not shown in the Subtype dropdown
Invoke AI Skill, Add Record, Update Record, Get Record.

---
## 5. Needs discussion — dropdown text vs backend name mismatch

The filter sends the **text shown in the dropdown**, but the backend matches by its **official name**. Where they differ by more than capitalisation, the backend rejects it and **quietly returns an empty page (no error)**. This affects the normal filter too, not just the smart-linking work. **Reported, not yet fixed.**

### App Log — Subtype (6 of 13 broken)
| Dropdown shows | Backend expects |
|---|---|
| Schedule | Schedule Workflow |
| Approval | Approval Workflow |
| Payment | Payment Workflow |
| Blueprint | Blueprint Workflow |
| Web data | External Call |
| Prompt-based assistance | AI Thought |

### Access Log — Component Type (1 broken)
| Dropdown shows | Backend expects |
|---|---|
| Blueprint | Blueprint Workflow |

**Suggested fix (safest):** translate the dropdown text to the backend's expected name on the client before sending the filter — no change to what users see, no backend change.
