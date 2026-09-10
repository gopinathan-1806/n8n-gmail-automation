# 📧 Student Progress Email Automation with n8n

An n8n workflow that reads student progress data from Google Sheets, formats it into a clean email, and automatically sends the email through Gmail.

The workflow is event-driven: when a new row is added to the configured Google Sheet, the automation starts automatically.

## 🎯 Objective

Automate student progress notifications without manually creating and sending emails.

```text
New Google Sheet Row
        ↓
Format Email
        ↓
Send Gmail
```

## 🏗️ Workflow Architecture

```text
┌─────────────────────┐
│ Google Sheets       │
│ New Row Added       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Edit Fields         │
│                     │
│ Create:             │
│ • Email Subject     │
│ • Email Message     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Gmail               │
│ Send Email          │
└─────────────────────┘
```

### Nodes Used

1. **Google Sheets Trigger**
2. **Edit Fields**
3. **Gmail**

---

## 🔄 How It Works

### 1. Google Sheets Trigger

The workflow watches the configured Google Sheet.

When a new student record is added, the trigger starts the workflow and passes the new row to the next node.

Example data:

| Name | Course | Progress | Email |
|---|---|---:|---|
| Manoj K | Generative AI | 80% | student@example.com |
| Priya S | Python | 65% | student2@example.com |

---

### 2. Edit Fields

The **Edit Fields** node converts the spreadsheet data into an email subject and readable message.

**Subject:**

```text
Student Progress Update - {{ $json.Name }}
```

**Message:**

```text
Hi {{ $json.Name }},

Here is your latest course progress:

Course: {{ $json.Course }}
Progress: {{ $json.Progress }}

Keep up the good work!

Regards,
Social Eagle AI Academy
```

The workflow uses n8n expressions so the content is generated dynamically for each student.

---

### 3. Gmail

The Gmail node sends the generated email.

| Gmail Field | Expression |
|---|---|
| To | `{{ $json.Email }}` |
| Subject | `{{ $json.subject }}` |
| Message | `{{ $json.message }}` |
| Email Type | Text |

The recipient comes directly from the **Email** column in Google Sheets.

---

## ⚡ Automatic Execution

Once the workflow is activated, no manual execution is required.

```text
User adds a new row
        ↓
Google Sheets Trigger detects it
        ↓
Edit Fields creates the email
        ↓
Gmail sends the email
```

For example, adding:

```text
Name: Rahul
Course: Machine Learning
Progress: 72%
Email: rahul@example.com
```

automatically generates an email such as:

```text
Subject:
Student Progress Update - Rahul

Hi Rahul,

Here is your latest course progress:

Course: Machine Learning
Progress: 72%

Keep up the good work!

Regards,
Social Eagle AI Academy
```

---

## 🧩 Why No Loop Node Is Required

A separate **Loop Over Items** node is not required for this workflow.

n8n passes items through the connected nodes. If multiple rows are returned, each item can flow through the same transformation and Gmail nodes.

```text
Google Sheets
     │
     ├── Student 1 ──→ Edit Fields ──→ Gmail
     ├── Student 2 ──→ Edit Fields ──→ Gmail
     └── Student 3 ──→ Edit Fields ──→ Gmail
```

This keeps the workflow simple.

---

## 🛠️ Technologies Used

- **n8n** — Workflow automation
- **Google Sheets** — Data source
- **Gmail** — Email delivery
- **n8n Expressions** — Dynamic data transformation

---

## 🔐 Credentials

The workflow requires authenticated Google connections for:

### Google Sheets
Used to read the spreadsheet and monitor for new rows.

### Gmail
Used to send the generated emails.

Credentials should be configured securely inside n8n and should not be committed to Git.

---

## 🧪 Testing

Recommended testing process:

1. Add a new test row to the Google Sheet.
2. Confirm the Google Sheets Trigger receives the row.
3. Check the **Edit Fields** output.
4. Verify the generated `subject` and `message`.
5. Confirm the Gmail recipient, subject, and body.
6. Verify the email was delivered.

For initial testing, use your own email address.

---

## 📚 Key Learning

This project demonstrates a simple and useful automation pattern:

> **Trigger → Transform → Action**

```text
Trigger
Google Sheets
     ↓
Transform
Edit Fields
     ↓
Action
Gmail
```

The trigger detects an event, the transformation prepares the data, and the final node performs the required action.

### Concepts Practiced

- Connecting Google Sheets with n8n
- Using Google Sheets Trigger
- Understanding n8n items
- Using n8n expressions
- Dynamically creating email content
- Mapping fields between nodes
- Sending emails with Gmail
- Event-driven workflow automation
- Difference between manual and automatic execution
- Understanding when a loop node is unnecessary

---

## 🚀 Possible Improvements

The workflow can be extended in several ways.

### 1. HTML Emails

Create richer emails with tables, progress indicators, branding, and course details.

### 2. Conditional Notifications

Example:

```text
Progress < 50%
      ↓
Needs Attention Email
```

```text
Progress >= 80%
      ↓
Great Progress Email
```

### 3. Email Status Tracking

Add an `Email_Status` column to the Google Sheet and update it after processing.

### 4. Error Handling

Handle:

- Invalid email addresses
- Missing fields
- Gmail failures
- Google Sheets connectivity issues

### 5. Scheduled Reports

Extend the workflow to generate daily or weekly student progress summaries.

---

## 📂 Suggested Repository Structure

```text
n8n-student-progress-email/
│
├── README.md
├── workflow.json
└── screenshots/
    └── workflow.png
```

Export the n8n workflow as `workflow.json` if you want to version-control the workflow configuration.

## 📸 Workflow Screenshot

Add the completed workflow screenshot here:

```text
screenshots/workflow.png
```

Then reference it in the README:

```markdown
![n8n Workflow](screenshots/workflow.png)
```

---

## 🎓 Project Summary

This project is a practical example of **event-driven automation using n8n**.

It connects:

**Google Sheets → n8n → Gmail**

The key idea is that data can automatically move from a source system, be transformed into a useful format, and trigger an action without manual intervention.

This provides a foundation for more advanced workflows involving AI/LLMs, conditional logic, notifications, data validation, reporting, and multi-step business automation.

---

## 🔗 Skills Demonstrated

`n8n` `Workflow Automation` `Google Sheets` `Gmail` `Event-Driven Automation` `Data Transformation` `n8n Expressions` `Email Automation`

---

### Learning Approach

**Build → Understand → Test → Learn → Improve → Integrate**

## Use the JSON below to create the workflow and import it into n8n.

https://github.com/gopinathan-1806/n8n-gmail-automation/blob/main/Gmail%20Automation.json

## Flow chart representing the execution

<img width="1536" height="1024" alt="email automation flow" src="https://github.com/user-attachments/assets/1d728d0a-8c60-4b1f-bc64-ad7dbf5ed481" />


## n8n Flow

<img width="1357" height="778" alt="Screenshot 2026-09-10 at 9 06 40 PM" src="https://github.com/user-attachments/assets/cf7e24ee-e4eb-4366-8c92-68b7cc928c96" />

## Email received 

<img width="1436" height="639" alt="image" src="https://github.com/user-attachments/assets/d7757921-bc55-4d68-a727-b35222f05076" />

