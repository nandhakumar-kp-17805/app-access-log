# Logs 2.0 — Sample Response JSON

## Access Log — `GET /accesslogs`

```json
{
  "accesslog": [
    {
      "access_log_id": "a1b2c3d4-0000-1111-2222-99887766",
      "timestamp": "01-Jun-2026 14:32:07",
      "search_start_time": "01-06-2026 14:32:07",
      "search_end_time": "01-06-2026 19:32:07",
      "category": "User Action",
      "action": "On Click of Button",
      "time_taken": "850 ms",
      "status_code": 200,
      "error_code": "-",
      "error_message": "-",
      "component_details": {
        "app_name": "order-management",
        "environment": "production",
        "record_ids": [101, 102, 103],
        "component_name": "all_orders",
        "component_type": "Report"
      },
      "user_details": {
        "user_name": "Jane Doe",
        "email": "jane@acme.com",
        "profile": "Administrator",
        "role": "Admin",
        "user_type": "Account Users"
      },
      "request_details": {
        "device_type": "web",
        "ip_address": "10.20.30.40",
        "mode": "User"
      },
      "additional_log_info": [
        { "displayName": "Report Criteria", "displayValue": "Status == \"Open\"" },
        { "displayName": "View Type", "displayValue": "Report" }
      ],
      "tags": [
        { "key": "version", "displayKey": "Version", "value": "1.4.2" },
        { "key": "button", "displayKey": "Button", "value": "Approve" }
      ]
    },
    {
      "access_log_id": "f9e8d7c6-aaaa-bbbb-cccc-112233445566",
      "timestamp": "01-Jun-2026 14:30:12",
      "search_start_time": "01-06-2026 14:30:12",
      "search_end_time": "01-06-2026 19:30:12",
      "category": "Api",
      "action": "Add Record",
      "time_taken": "1.20 s",
      "status_code": 999,
      "error_code": 2930,
      "error_message": "The requested resource is not found. Please verify and try again.",
      "component_details": {
        "app_name": "order-management",
        "environment": "production",
        "record_ids": [],
        "component_name": "orders",
        "component_type": "Form"
      },
      "user_details": {
        "user_name": "API User",
        "email": "api-user@acme.com",
        "profile": "Developer",
        "role": "Developer",
        "user_type": "Account Users"
      },
      "request_details": {
        "device_type": "web",
        "ip_address": "192.168.1.50",
        "mode": "User"
      },
      "additional_log_info": [],
      "tags": [
        { "key": "version", "displayKey": "Version", "value": "1.4.2" }
      ]
    }
  ]
}
```

---

## App Log — `GET /applogs`

```json
{
  "applog": [
    {
      "access_log_id": "a1b2c3d4-0000-1111-2222-99887766",
      "timestamp": "01-Jun-2026 14:32:08",
      "search_start_time": "01-06-2026 09:32:08",
      "search_end_time": "01-06-2026 19:32:08",
      "time_taken": "120 ms",
      "record_type": "Workflow",
      "seq_no": 1,
      "event": "On Click of Button",
      "category": "Form Workflow",
      "component_details": {
        "workflow_name": "approve_order",
        "app_name": "order-management",
        "component_name": "orders"
      },
      "status_code": 200,
      "error_code": "-",
      "message": "-",
      "service_name": "-",
      "additional_log_info": [
        { "displayName": "Info Logs", "displayValue": "Workflow executed successfully" }
      ],
      "record_ids": [101, 102, 103],
      "parent_seq_no": "-"
    },
    {
      "access_log_id": "a1b2c3d4-0000-1111-2222-99887766",
      "timestamp": "01-Jun-2026 14:32:08",
      "search_start_time": "01-06-2026 09:32:08",
      "search_end_time": "01-06-2026 19:32:08",
      "time_taken": "45 ms",
      "record_type": "Task",
      "seq_no": 2,
      "event": "Invoke URL",
      "category": "External Call",
      "component_details": {
        "workflow_name": "approve_order",
        "app_name": "order-management",
        "component_name": "orders"
      },
      "status_code": 200,
      "error_code": "-",
      "message": "-",
      "service_name": "api.acme.com",
      "additional_log_info": [
        { "displayName": "Parameters", "displayValue": "{\"orderId\":101}" },
        { "displayName": "Result", "displayValue": "success" },
        { "displayName": "Info Logs", "displayValue": "[\"Calling external API\",\"Response 200 OK\"]" }
      ],
      "record_ids": [101],
      "parent_seq_no": 1
    },
    {
      "access_log_id": "a1b2c3d4-0000-1111-2222-99887766",
      "timestamp": "01-Jun-2026 14:32:09",
      "search_start_time": "01-06-2026 09:32:09",
      "search_end_time": "01-06-2026 19:32:09",
      "time_taken": "30 ms",
      "record_type": "Task",
      "seq_no": 3,
      "event": "Send Email",
      "category": "Notification",
      "component_details": {
        "workflow_name": "approve_order",
        "app_name": "order-management",
        "component_name": "orders"
      },
      "status_code": 200,
      "error_code": "-",
      "message": "Email sent to approver@acme.com",
      "service_name": "MailService",
      "additional_log_info": [
        { "displayName": "Parameters", "displayValue": "{\"to\":\"approver@acme.com\",\"subject\":\"Order #101 Approval\"}" },
        { "displayName": "Result", "displayValue": "success" }
      ],
      "record_ids": [101],
      "parent_seq_no": 1
    },
    {
      "access_log_id": "a1b2c3d4-0000-1111-2222-99887766",
      "timestamp": "01-Jun-2026 14:32:09",
      "search_start_time": "01-06-2026 09:32:09",
      "search_end_time": "01-06-2026 19:32:09",
      "time_taken": "60 ms",
      "record_type": "Task",
      "seq_no": 4,
      "event": "Invoke URL",
      "category": "External Call",
      "component_details": {
        "workflow_name": "approve_order",
        "app_name": "order-management",
        "component_name": "orders"
      },
      "status_code": 999,
      "error_code": "404",
      "message": "External service returned error",
      "service_name": "api.thirdparty.com",
      "additional_log_info": [
        { "displayName": "Parameters", "displayValue": "{\"endpoint\":\"/webhooks/notify\"}" },
        { "displayName": "Result", "displayValue": "failure" },
        { "displayName": "Info Logs", "displayValue": "[\"Calling webhook\",\"HTTP 404 Not Found\"]" }
      ],
      "record_ids": [101],
      "parent_seq_no": 1
    }
  ]
}
```









