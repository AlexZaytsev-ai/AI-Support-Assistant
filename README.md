# AI Support Assistant

[Русская версия](README_RU.md)

AI-powered customer request classification and routing workflow built with n8n. It receives Telegram messages, extracts structured data using OpenAI, stores every request in Google Sheets, and notifies the responsible department.

---

## Business Problem

Customer support teams spend significant time manually reading, categorizing, and forwarding incoming requests.

This slows down response time, creates routing errors, and makes it difficult to keep a complete history of customer issues.

This workflow automates request classification and routing while sending uncertain cases to a manager for manual review.

---

## Workflow Architecture

```text
Telegram Trigger
      ↓
Normalize Request
      ↓
AI Classifier
      ↓
Strict JSON Schema
      ↓
Prepare Response
      ├── Save to Google Sheets
      └── Route by Department
              ├── Support → Telegram
              ├── Sales → Telegram
              ├── Finance → Telegram
              ├── Other → Manual Review
              └── Fallback → Manual Review
```

---

## Workflow

![AI Support Assistant workflow](workflowSupport.png)

---

## Architecture Principles

* AI analyzes the request, classifies it, and extracts structured information.
* A strict JSON Schema controls field types, allowed values, and required fields.
* The deterministic Router handles the final department routing.
* Every incoming request is stored in Google Sheets independently of its department.
* `Prepare Response` adds Russian labels for department, priority, and topic to make notifications easier for managers to read.
* Unknown or unsupported classifications are sent to Manual Review through the fallback route.
* An AI Agent is intentionally not used because the model does not select tools or decide the sequence of actions.

---

## Structured AI Output

The classifier always returns five fields:

```json
{
  "department": "support",
  "priority": "high",
  "topic": "order_status",
  "order_number": "23232",
  "description": "The customer requests the status of an overdue order."
}
```

Allowed values:

* `department`: `support`, `sales`, `finance`, `other`
* `priority`: `low`, `medium`, `high`
* `topic`: `order_status`, `delivery`, `payment`, `refund`, `account`, `warranty`, `product`, `pricing`, `technical_issue`, `other`

---

## Tech Stack

| Technology             | Purpose                                    |
| ---------------------- | ------------------------------------------ |
| n8n                    | Workflow automation and routing            |
| Telegram Bot API       | Receive requests and notify departments    |
| OpenAI API             | Request classification and data extraction |
| JSON Schema            | Strict structured output validation        |
| Google Sheets API      | Central request log                        |
| JavaScript Expressions | Data mapping and localization              |

---

## Import and Setup

The public workflow export does not contain credentials, Telegram chat IDs, or the Google Sheets document ID.

1. Import `ai-support-assistant-workflow.json` into n8n.
2. Configure Telegram credentials in `Receive Request`.
3. Configure OpenAI credentials in `AI Classifier`.
4. Create a Google Sheet with these columns:

   * `received_at`
   * `source`
   * `customer_name`
   * `customer_contact`
   * `department`
   * `priority`
   * `topic`
   * `message`
   * `order_number`
   * `description`
5. Configure Google Sheets credentials and select the required spreadsheet.
6. Replace the Telegram placeholders in the notification nodes:

   * `YOUR_SUPPORT_CHAT_ID`
   * `YOUR_SALES_CHAT_ID`
   * `YOUR_FINANCE_CHAT_ID`
   * `YOUR_MANUAL_REVIEW_CHAT_ID`
7. Test the workflow with several request types.
8. Activate the workflow after successful testing.

---

## Key Features

* Telegram request intake
* Request data normalization
* AI-powered classification
* Strict structured output
* Department detection
* Priority detection
* Topic detection
* Order number extraction
* Russian manager-friendly labels
* Central Google Sheets request log
* Deterministic department routing
* Telegram department notifications
* Manual Review and fallback handling

---

## Business Rules

* Pre-purchase product, availability, pricing, and delivery questions are routed to Sales.
* Existing order, warranty, account, and technical issues are routed to Support.
* Payment, refund, receipt, and double-charge issues are routed to Finance.
* Unsupported or unclear requests are routed to Manual Review.
* If an order number is present, it is extracted.
* If an order number is missing, an empty string is returned.
* AI does not answer the customer directly.
* AI prepares structured data; workflow rules make the routing decision.
* Every request is logged in Google Sheets before further manual processing.

---

## Tested Scenario

Example customer request:

```text
Hello! Where is my order 23232? It should have arrived yesterday.
```

Result:

* Department: Support
* Priority: High
* Topic: Order status
* Order number: 23232
* The request was stored in Google Sheets.
* The Support department received a Telegram notification.

---

## Future Improvements

* CRM integration
* SLA monitoring
* Request status tracking
* Knowledge base integration
* AI-generated draft replies
* Analytics dashboard
* Multi-language support

---

## Author

Alexander Zaytsev

AI Automation Engineer

* GitHub: https://github.com/AlexZaytsev-ai
* Email: [polonix315@gmail.com](mailto:polonix315@gmail.com)
