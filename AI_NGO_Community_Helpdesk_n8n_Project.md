# AI NGO/Community Helpdesk Agent

## AI Agent Development Using n8n – Group Activity

---

# 1. Project Overview

## Project Title

**AI NGO/Community Helpdesk Agent**

## Project Idea

The AI NGO/Community Helpdesk Agent receives help requests from people in a community, uses an AI model to understand and categorize the request, determines its priority, identifies the appropriate volunteer category, and automatically routes the request to a suitable volunteer.

High-risk, urgent, or uncertain requests are sent for human review instead of being handled completely automatically.

## Overall Workflow

```text
User submits Help Request
          |
          v
    n8n Form Trigger
          |
          v
    Prepare Request Data
          |
          v
       AI Agent
          |
          v
   Structured AI Output
          |
          v
   Human Review Required?
       /          \
     YES           NO
      |             |
      v             v
 Human Review   Find Volunteer
      |             |
      |             v
      |       Assign Volunteer
      |             |
      \______ ______/
             |
             v
      Save to Google Sheets
             |
             v
       Send Email
             |
             v
        Final Status
```

---

# 2. Problem Definition

## Problem Statement

NGOs and community organizations receive different types of requests for assistance, such as medical assistance, education support, food and essential supplies, elderly support, and women and child support.

Manually reading every request, understanding its category, determining its priority, and assigning it to the appropriate volunteer can be time-consuming. There is also a possibility that requests may be incorrectly categorized or that urgent requests may not receive appropriate attention.

The proposed solution is an AI-powered community helpdesk built using n8n. The system receives a request, uses an AI model to analyze it, categorizes it, determines its priority, identifies an appropriate volunteer category, and automatically sends the request to a suitable volunteer. Requests requiring additional attention are sent for human review.

---

# 3. Objectives

The objectives of the project are:

1. Automatically receive community help requests.
2. Use AI to understand natural-language requests.
3. Categorize requests into predefined categories.
4. Determine the priority of requests.
5. Identify the appropriate volunteer category.
6. Automatically assign requests to available volunteers.
7. Send notifications to assigned volunteers.
8. Store requests and results in Google Sheets.
9. Route high-risk or uncertain cases to a human reviewer.
10. Demonstrate an agentic workflow using n8n.

---

# 4. Target Users

The system can be used by:

* NGOs
* Community organizations
* NGO coordinators
* Volunteers
* Community members seeking assistance

---

# 5. Technology Stack

| Technology               | Purpose                                  |
| ------------------------ | ---------------------------------------- |
| n8n                      | Workflow automation                      |
| AI Model                 | Request understanding and classification |
| Google Forms/n8n Form    | User input                               |
| Google Sheets            | Volunteer database and request storage   |
| Gmail/Email              | Volunteer notification                   |
| IF/Switch nodes          | Decision making                          |
| Structured Output Parser | Convert AI output into structured fields |

---

# 6. Google Sheets Database

Create one Google Spreadsheet named:

```text
AI_NGO_Community_Helpdesk
```

Create the following seven sheets.

---

## Sheet 1 — Volunteers

Sheet name:

```text
Volunteers
```

Columns:

| Volunteer_ID | Name           | Email      | Phone      | Category              | Location | Availability | Status |
| ------------ | -------------- | ---------- | ---------- | --------------------- | -------- | ------------ | ------ |
| V001         | Aditi Sharma   | YOUR_EMAIL | 9876543201 | Medical Assistance    | Pune     | Available    | Active |
| V002         | Rahul Patil    | YOUR_EMAIL | 9876543202 | Education             | Pune     | Available    | Active |
| V003         | Sneha Joshi    | YOUR_EMAIL | 9876543203 | Food & Essentials     | Pune     | Available    | Active |
| V004         | Priya Deshmukh | YOUR_EMAIL | 9876543204 | Elderly Support       | Pune     | Available    | Active |
| V005         | Neha Verma     | YOUR_EMAIL | 9876543205 | Women & Child Support | Pune     | Available    | Active |
| V006         | Aman Shah      | YOUR_EMAIL | 9876543206 | Other                 | Pune     | Available    | Active |

Replace `YOUR_EMAIL` with an email address that you control for testing.

---

# 7. Sheet 2 — Help Requests

Sheet name:

```text
Help_Requests
```

Columns:

| Request_ID | Timestamp | Name | Contact | Location | Request | Category | Priority | Summary | Volunteer_Type | Assigned_Volunteer | Human_Review | Status | Notes |
| ---------- | --------- | ---- | ------- | -------- | ------- | -------- | -------- | ------- | -------------- | ------------------ | ------------ | ------ | ----- |

This sheet will be populated automatically by n8n.

---

# 8. Sheet 3 — Categories

Sheet name:

```text
Categories
```

Add:

| Category              | Description                                    | Volunteer_Type        | Default_Priority |
| --------------------- | ---------------------------------------------- | --------------------- | ---------------- |
| Medical Assistance    | Medical or medicine-related assistance         | Medical Assistance    | Medium           |
| Education             | Education, tutoring, books or school support   | Education             | Low              |
| Food & Essentials     | Food, groceries, clothes or essential supplies | Food & Essentials     | Medium           |
| Elderly Support       | Assistance for elderly people                  | Elderly Support       | Medium           |
| Women & Child Support | Support involving women or children            | Women & Child Support | High             |
| Other                 | Requests that do not fit another category      | Other                 | Medium           |

---

# 9. Sheet 4 — Test Cases

Sheet name:

```text
Test_Cases
```

Add:

| Test_ID | Input                                            | Expected_Category     | Expected_Priority | Expected_Human_Review | Actual_Category | Actual_Priority | Actual_Human_Review | Result |
| ------- | ------------------------------------------------ | --------------------- | ----------------- | --------------------- | --------------- | --------------- | ------------------- | ------ |
| TC1     | Student needs textbooks and mathematics tutoring | Education             | Low               | No                    |                 |                 |                     |        |
| TC2     | Elderly person needs help collecting medicines   | Elderly Support       | Medium            | No                    |                 |                 |                     |        |
| TC3     | Person has collapsed and is not responding       | Medical Assistance    | High              | Yes                   |                 |                 |                     |        |
| TC4     | Family needs food and groceries                  | Food & Essentials     | Medium            | No                    |                 |                 |                     |        |
| TC5     | Child needs educational support                  | Women & Child Support | High              | Yes                   |                 |                 |                     |        |

---

# 10. Sheet 5 — AI Logs

Sheet name:

```text
AI_Logs
```

Columns:

| Log_ID | Request_ID | AI_Category | AI_Priority | AI_Summary | Volunteer_Type | Human_Review | AI_Timestamp |
| ------ | ---------- | ----------- | ----------- | ---------- | -------------- | ------------ | ------------ |

---

# 11. Sheet 6 — Human Review

Sheet name:

```text
Human_Review
```

Columns:

| Review_ID | Request_ID | Reason | AI_Decision | Reviewer | Review_Status | Reviewer_Decision | Comments | Review_Timestamp |
| --------- | ---------- | ------ | ----------- | -------- | ------------- | ----------------- | -------- | ---------------- |

Possible review statuses:

```text
Pending
Approved
Rejected
Needs More Information
```

---

# 12. Sheet 7 — Workflow Config

Sheet name:

```text
Workflow_Config
```

Add:

| Setting                 | Value                           |
| ----------------------- | ------------------------------- |
| Project_Name            | AI NGO Community Helpdesk Agent |
| Default_Status          | New                             |
| High_Priority_Review    | Yes                             |
| Medical_Human_Review    | Yes                             |
| Unknown_Category_Review | Yes                             |
| Notification_Method     | Email                           |
| Database                | Google Sheets                   |

---

# 13. n8n Workflow

Create a new workflow in n8n.

Workflow name:

```text
AI NGO Community Helpdesk Agent
```

The final workflow should contain approximately these nodes:

```text
1. Form Trigger
       |
       v
2. Edit Fields
       |
       v
3. AI Agent
       |
       v
4. Structured Output Parser
       |
       v
5. IF - Human Review?
      / \
    YES  NO
     |    |
     |    v
     |  6. Google Sheets
     |     Find Volunteer
     |       |
     |       v
     |  7. Save Request
     |       |
     |       v
     |  8. Send Email
     |       |
     \-------/
          |
          v
    9. Final Status
```

Depending on the exact n8n version, some nodes can be combined or implemented with a Switch node.

---

# 14. Node 1 — Form Trigger

## Node Name

```text
Community Help Request Form
```

Add:

```text
n8n Form Trigger
```

## Form Title

```text
Community Help Request
```

## Form Description

```text
Please provide the details of the help you need.
Your request will be analyzed and routed to an appropriate community volunteer.
```

## Fields

### Field 1

```text
Name
Type: Text
Required: Yes
```

### Field 2

```text
Contact
Type: Text
Required: Yes
```

### Field 3

```text
Location
Type: Text
Required: Yes
```

### Field 4

```text
Help Request
Type: Textarea
Required: Yes
```

---

# 15. Test Node 1

Use this sample:

```text
Name:
Riya Sharma

Contact:
9876543210

Location:
Pune

Help Request:
My elderly grandmother needs someone to help collect her medicines from the pharmacy.
```

Execute the workflow.

Confirm that n8n receives the four fields.

---

# 16. Node 2 — Edit Fields

Add:

```text
Edit Fields
```

You can also see this as:

```text
Set
```

depending on the n8n version.

## Purpose

This node prepares clean data before sending it to the AI.

Create:

```text
request_id
name
contact
location
request
status
```

Example values:

```text
request_id:
REQ-{{$now.toMillis()}}

name:
{{$json.Name}}

contact:
{{$json.Contact}}

location:
{{$json.Location}}

request:
{{$json["Help Request"]}}

status:
New
```

The exact field references may differ slightly depending on your form field names. Use the expression selector in n8n rather than typing field paths manually if necessary.

---

# 17. Node 3 — AI Agent

Add:

```text
AI Agent
```

This is the main intelligence of the workflow.

The AI Agent receives the user's request and determines:

* Category
* Priority
* Summary
* Volunteer type
* Human review requirement

---

# 18. AI System Prompt

Use the following as the AI Agent's system instruction:

```text
You are an AI Community Helpdesk Assistant for an NGO.

Your job is to analyze incoming community help requests and classify them accurately.

You must classify every request into exactly one of these categories:

1. Medical Assistance
2. Education
3. Food & Essentials
4. Elderly Support
5. Women & Child Support
6. Other

You must also assign one priority:

- High
- Medium
- Low

You must create a short summary of the request.

You must identify the appropriate volunteer type.

Use these mappings:

Medical Assistance -> Medical Assistance
Education -> Education
Food & Essentials -> Food & Essentials
Elderly Support -> Elderly Support
Women & Child Support -> Women & Child Support
Other -> Other

Human review is required when:

1. The request appears urgent or potentially dangerous.
2. The request describes a serious medical situation.
3. The request involves violence, abuse, or child safety.
4. The information is unclear or insufficient.
5. The AI is not confident about the category.

Do not invent information that is not provided by the user.

Do not provide medical, legal, or emergency advice.

If a request appears to describe an immediate emergency, mark human_review as true and priority as High.

Return only structured information matching the required output fields.
```

---

# 19. AI Input

Pass the request from the previous Edit Fields node.

The AI should receive something similar to:

```text
Name: {{$json.name}}

Location: {{$json.location}}

Help Request: {{$json.request}}
```

---

# 20. Required AI Output

Configure the AI to produce:

```json
{
  "category": "Elderly Support",
  "priority": "Medium",
  "summary": "Elderly person needs assistance collecting medicines.",
  "volunteer_type": "Elderly Support",
  "human_review": false,
  "review_reason": ""
}
```

---

# 21. Structured Output Parser

If your n8n AI Agent setup supports a Structured Output Parser, add it.

Expected schema:

```json
{
  "category": "string",
  "priority": "string",
  "summary": "string",
  "volunteer_type": "string",
  "human_review": "boolean",
  "review_reason": "string"
}
```

This is important because later n8n nodes can use these values.

---

# 22. Expected AI Results

For this input:

```text
My elderly grandmother needs someone to help collect her medicines from the pharmacy.
```

Expected:

```json
{
  "category": "Elderly Support",
  "priority": "Medium",
  "summary": "Elderly person needs assistance collecting medicines.",
  "volunteer_type": "Elderly Support",
  "human_review": false,
  "review_reason": ""
}
```

---

# 23. Node 4 — Human Review IF Node

Add an:

```text
IF
```

node.

Name it:

```text
Human Review Required?
```

Condition:

```text
human_review
is equal to
true
```

This creates two paths:

```text
TRUE
 |
 v
Human Review

FALSE
 |
 v
Continue Automatic Routing
```

---

# 24. Human Review Path

For the TRUE branch, add:

```text
Google Sheets
```

Connect it to:

```text
Human_Review
```

Create a row containing:

```text
Review_ID
Request_ID
Reason
AI_Decision
Review_Status
Review_Timestamp
```

Example:

```text
Review_ID:
REV-001

Request_ID:
REQ-001

Reason:
Possible emergency medical situation

AI_Decision:
Medical Assistance / High Priority

Review_Status:
Pending
```

---

# 25. Human Review Notification

For a stronger project, add an email notification after the Human Review Google Sheets node.

Subject:

```text
ACTION REQUIRED - Community Help Request
```

Body:

```text
A community help request requires human review.

Request ID:
{{request_id}}

Category:
{{category}}

Priority:
{{priority}}

Reason:
{{review_reason}}

Please review the request through the NGO helpdesk.
```

---

# 26. Automatic Routing Path

For the FALSE branch of the IF node, continue to volunteer matching.

Add:

```text
Google Sheets
```

Node name:

```text
Find Available Volunteer
```

Select the:

```text
Volunteers
```

sheet.

Use the AI's:

```text
volunteer_type
```

to identify the appropriate category.

Also check:

```text
Availability = Available
Status = Active
```

The goal is to find something like:

```text
AI Output:
Elderly Support

Google Sheet:
Priya Deshmukh
Elderly Support
Available
Active
```

---

# 27. Volunteer Matching Logic

The logic should be:

```text
AI volunteer_type
       |
       v
Find volunteer with matching Category
       |
       v
Availability = Available?
       |
      YES
       |
       v
Assign Volunteer
```

If no volunteer is available, route the request to human review.

---

# 28. Node — Save Request

Add another:

```text
Google Sheets
```

node.

Select:

```text
Help_Requests
```

Operation:

```text
Append Row
```

Save:

| Column             | Value                        |
| ------------------ | ---------------------------- |
| Request_ID         | Request ID                   |
| Timestamp          | Current timestamp            |
| Name               | User name                    |
| Contact            | User contact                 |
| Location           | User location                |
| Request            | Original request             |
| Category           | AI category                  |
| Priority           | AI priority                  |
| Summary            | AI summary                   |
| Volunteer_Type     | AI volunteer type            |
| Assigned_Volunteer | Matched volunteer            |
| Human_Review       | No                           |
| Status             | Assigned                     |
| Notes              | AI generated notes if needed |

---

# 29. Node — AI Logs

Add another Google Sheets node.

Sheet:

```text
AI_Logs
```

Save:

```text
Log_ID
Request_ID
AI_Category
AI_Priority
AI_Summary
Volunteer_Type
Human_Review
AI_Timestamp
```

This allows you to demonstrate that AI decisions are being recorded.

---

# 30. Node — Send Email

Add:

```text
Gmail
```

or an appropriate email node available in your n8n setup.

## Recipient

Use the email returned from the volunteer database.

## Subject

```text
New Community Help Request Assigned
```

## Body

```text
Hello {{volunteer_name}},

A new community help request has been assigned to you.

Request ID:
{{request_id}}

Category:
{{category}}

Priority:
{{priority}}

Location:
{{location}}

Request:
{{request}}

Summary:
{{summary}}

Please review the request and take appropriate action.

Thank you,
NGO Community Helpdesk
```

---

# 31. Node — Update Status

After sending the email, update the request status.

For example:

```text
Assigned & Notified
```

If email fails:

```text
Notification Failed
```

If human review is required:

```text
Pending Human Review
```

---

# 32. Complete Workflow

Your final n8n workflow should approximately look like:

```text
                     ┌─────────────────────┐
                     │    FORM TRIGGER     │
                     │ Name                │
                     │ Contact             │
                     │ Location            │
                     │ Help Request        │
                     └──────────┬──────────┘
                                |
                                v
                     ┌─────────────────────┐
                     │    EDIT FIELDS      │
                     │ Request ID          │
                     │ Clean Input         │
                     └──────────┬──────────┘
                                |
                                v
                     ┌─────────────────────┐
                     │      AI AGENT       │
                     │ Category            │
                     │ Priority            │
                     │ Summary             │
                     │ Volunteer Type      │
                     │ Human Review        │
                     └──────────┬──────────┘
                                |
                                v
                     ┌─────────────────────┐
                     │ STRUCTURED OUTPUT   │
                     └──────────┬──────────┘
                                |
                                v
                     ┌─────────────────────┐
                     │ HUMAN REVIEW?       │
                     └───────┬───────┬─────┘
                             |       |
                          TRUE       FALSE
                           |           |
                           v           v
                  ┌────────────┐  ┌────────────────┐
                  │ Human      │  │ Find Volunteer │
                  │ Review     │  └───────┬────────┘
                  └─────┬──────┘          |
                        |                 v
                        |        ┌─────────────────┐
                        |        │ Save Help       │
                        |        │ Request         │
                        |        └────────┬────────┘
                        |                 |
                        |                 v
                        |        ┌─────────────────┐
                        |        │ Send Volunteer  │
                        |        │ Email           │
                        |        └────────┬────────┘
                        |                 |
                        |                 v
                        |        ┌─────────────────┐
                        |        │ Update Status   │
                        |        └─────────────────┘
                        |
                        v
                ┌──────────────────┐
                │ Human Review     │
                │ Sheet + Email    │
                └──────────────────┘
```

---

# 33. Recommended n8n Node List

For the report, list the nodes as follows:

| No. | Node                     | Purpose                                     |
| --- | ------------------------ | ------------------------------------------- |
| 1   | Form Trigger             | Receives user request                       |
| 2   | Edit Fields              | Cleans and prepares input                   |
| 3   | AI Agent                 | Understands and categorizes request         |
| 4   | Structured Output Parser | Produces structured AI data                 |
| 5   | IF                       | Determines whether human review is required |
| 6   | Google Sheets            | Finds suitable volunteer                    |
| 7   | Google Sheets            | Stores help request                         |
| 8   | Google Sheets            | Stores AI log                               |
| 9   | Gmail/Email              | Notifies volunteer                          |
| 10  | Google Sheets            | Records human-review cases                  |

---

# 34. Test Case 1 — Education

## Input

```text
A student from a low-income family needs textbooks and someone who can help with mathematics.
```

## Expected Output

```text
Category:
Education

Priority:
Low

Volunteer Type:
Education

Human Review:
No
```

## Expected Routing

```text
Education
     ↓
Rahul Patil
     ↓
Email Notification
```

---

# 35. Test Case 2 — Elderly Support

## Input

```text
My elderly grandmother cannot go outside and needs help collecting her medicines from the pharmacy.
```

## Expected Output

```text
Category:
Elderly Support

Priority:
Medium

Volunteer Type:
Elderly Support

Human Review:
No
```

## Expected Routing

```text
Elderly Support
       ↓
Priya Deshmukh
       ↓
Email Notification
```

---

# 36. Test Case 3 — Emergency/High-Risk

## Input

```text
Someone has collapsed and is not responding. We need immediate help.
```

## Expected Output

```text
Category:
Medical Assistance

Priority:
High

Volunteer Type:
Medical Assistance

Human Review:
Yes
```

## Expected Routing

```text
Medical Assistance
       ↓
High Priority
       ↓
Human Review
       ↓
NGO Coordinator
```

The system should NOT attempt to independently provide emergency medical instructions.

---

# 37. Test Case 4 — Food

## Input

```text
A family in our neighborhood has no food for the next few days and needs groceries.
```

Expected:

```text
Category:
Food & Essentials

Priority:
Medium

Volunteer Type:
Food & Essentials

Human Review:
No
```

---

# 38. Test Case 5 — Child Support

## Input

```text
A child is facing a difficult situation at home and needs support.
```

Expected:

```text
Category:
Women & Child Support

Priority:
High

Human Review:
Yes
```

This demonstrates the human-review mechanism.

---

# 39. Testing Table for PDF

Use this in the final report:

| Test Case | Input                                | Expected Output                             | Actual Output        | Result    |
| --------- | ------------------------------------ | ------------------------------------------- | -------------------- | --------- |
| TC1       | Student needs textbooks and tutoring | Education / Low / No Review                 | Fill after execution | Pass/Fail |
| TC2       | Elderly person needs medicine help   | Elderly Support / Medium / No Review        | Fill after execution | Pass/Fail |
| TC3       | Person has collapsed                 | Medical / High / Human Review               | Fill after execution | Pass/Fail |
| TC4       | Family needs groceries               | Food & Essentials / Medium / No Review      | Fill after execution | Pass/Fail |
| TC5       | Child needs support                  | Women & Child Support / High / Human Review | Fill after execution | Pass/Fail |

---

# 40. Screenshots You Need

Take screenshots during implementation.

## Screenshot 1

Google Sheets workbook showing:

```text
Volunteers
Categories
Help_Requests
```

## Screenshot 2

n8n Form Trigger.

## Screenshot 3

AI Agent configuration.

## Screenshot 4

AI system prompt.

## Screenshot 5

Structured AI output.

## Screenshot 6

IF Human Review node.

## Screenshot 7

Google Sheets volunteer lookup.

## Screenshot 8

Email node.

## Screenshot 9

Complete n8n workflow.

## Screenshot 10

Successful execution showing all nodes completed.

## Screenshot 11

Google Sheets showing automatically inserted request.

## Screenshot 12

Received volunteer notification email.

---

# 41. Limitations

Include the following limitations in the report:

1. AI classification may occasionally be incorrect.
2. AI may misunderstand ambiguous requests.
3. The system depends on the availability of the AI service.
4. Volunteer availability data may become outdated.
5. Email delivery may fail.
6. The system does not replace human judgment.
7. High-risk and sensitive cases require human review.
8. The prototype uses dummy volunteer data.
9. The system does not independently verify whether a request is genuine.
10. The current prototype supports a limited number of categories.

---

# 42. Human Review Requirement

Human review is particularly important for:

```text
Medical emergencies
Violence
Abuse
Child safety
Unclear requests
High-priority requests
Sensitive situations
Unknown categories
No available volunteer
```

The AI should assist the coordinator rather than completely replace the coordinator.

---

# 43. Pre-Reflection

Use this in the final report:

> Before developing the workflow, we expected AI integration with n8n to mainly involve sending a prompt to an AI model and receiving a response. We were not initially familiar with how triggers, structured outputs, decision nodes, external data sources and automated actions could be combined to create an agentic workflow.

---

# 44. Post-Reflection

Use this:

> After implementing the workflow, we understood that an AI agent becomes more useful when it is connected to tools and actions rather than only generating text. We learned how n8n can receive real-world inputs, use an AI model to make structured decisions, route information using conditions, access Google Sheets and perform automated actions. We also learned that human review is important for uncertain or high-risk cases because AI decisions may not always be reliable.

---

# 45. Future Scope

## 1. WhatsApp Integration

The system could receive help requests through WhatsApp instead of only using a web form.

## 2. Location-Based Volunteer Matching

The system could identify volunteers based on the requester's location.

For example:

```text
User Location
     ↓
Find nearby volunteers
     ↓
Check availability
     ↓
Assign closest suitable volunteer
```

## 3. Multilingual Support

The system could support:

```text
English
Hindi
Marathi
```

This would make the system more accessible to local communities.

Additional future improvements could include:

* Volunteer mobile application
* SMS notifications
* Real-time volunteer availability
* Request status tracking
* Admin dashboard
* Analytics
* Duplicate-request detection

---

# 46. Final Report Structure

Your PDF should follow this structure.

```text
TITLE PAGE

AI NGO/Community Helpdesk Agent

AI Agent Development Using n8n – Group Activity

Group Members:
1. __________
2. __________
3. __________
4. __________


SECTION 1 – PROBLEM DEFINITION

1.1 Title
1.2 Problem Statement
1.3 Objective
1.4 Target Users


SECTION 2 – AGENT DESIGN

2.1 Input
2.2 AI Processing
2.3 Tools/Data Sources
2.4 Decision Logic
2.5 Output/Action
2.6 Human Review Point


SECTION 3 – n8n IMPLEMENTATION

3.1 Workflow Diagram
3.2 n8n Workflow Screenshot
3.3 Node-by-Node Explanation
3.4 AI Prompt/System Instruction
3.5 Tools/APIs Used


SECTION 4 – TESTING

4.1 Test Case 1
4.2 Test Case 2
4.3 Test Case 3
4.4 Testing Table
4.5 Execution Screenshots


SECTION 5 – REFLECTION

5.1 Pre-Reflection
5.2 Post-Reflection


SECTION 6 – LIMITATIONS

6.1 Limitations


SECTION 7 – FUTURE SCOPE

7.1 Future Improvement 1
7.2 Future Improvement 2
7.3 Future Improvement 3


CONCLUSION
```

---

# 47. Tools/APIs Section

For the report:

| Tool/API        | Purpose                                           |
| --------------- | ------------------------------------------------- |
| n8n             | Workflow automation                               |
| AI API/Model    | Natural language understanding and classification |
| Google Sheets   | Volunteer database and request storage            |
| Gmail/Email API | Volunteer notification                            |
| n8n Form        | Request collection                                |

If you use OpenAI, Gemini, or another specific model, mention the exact model used in the final report.

---

# 48. Demonstration Flow

During your presentation, demonstrate the following.

## Demo 1 — Normal Request

Submit:

```text
My elderly grandmother needs help collecting medicines.
```

Show:

```text
Form
 ↓
AI
 ↓
Elderly Support
 ↓
Priya
 ↓
Email
 ↓
Google Sheet
```

---

## Demo 2 — Education Request

Submit:

```text
A student needs books and mathematics tutoring.
```

Show:

```text
Form
 ↓
AI
 ↓
Education
 ↓
Rahul
 ↓
Email
 ↓
Google Sheet
```

---

## Demo 3 — High-Risk Request

Submit:

```text
Someone has collapsed and is not responding.
```

Show:

```text
Form
 ↓
AI
 ↓
Medical Assistance
 ↓
High Priority
 ↓
Human Review
```

This demonstrates why your system is not simply an AI chatbot.

---

# 49. What Makes This an AI Agent?

In your presentation, explain:

> Our system is an AI-powered agent because it does more than generate text. It receives an input, interprets the request using an AI model, produces structured decisions, uses external data to identify an appropriate volunteer, applies conditional logic, performs an automated action by sending a notification, stores the result, and routes selected cases to a human for review.

The key agentic flow is:

```text
INPUT
  ↓
AI REASONING / CLASSIFICATION
  ↓
DECISION
  ↓
TOOL USE
  ↓
ACTION
  ↓
RESULT
```

---

# 50. Final Implementation Checklist

Before submission, verify all of these:

## Google Sheets

* [ ] Volunteers sheet created
* [ ] Help_Requests sheet created
* [ ] Categories sheet created
* [ ] Test_Cases sheet created
* [ ] AI_Logs sheet created
* [ ] Human_Review sheet created
* [ ] Workflow_Config sheet created

## n8n

* [ ] Form Trigger works
* [ ] Edit Fields works
* [ ] AI Agent works
* [ ] AI prompt added
* [ ] Structured output works
* [ ] Human Review IF works
* [ ] Volunteer lookup works
* [ ] Request is saved
* [ ] AI log is saved
* [ ] Email is sent
* [ ] Status is updated

## Testing

* [ ] Education request tested
* [ ] Elderly request tested
* [ ] Medical/high-risk request tested
* [ ] Food request tested
* [ ] Child-support request tested
* [ ] Screenshots taken
* [ ] Actual outputs recorded

## PDF

* [ ] Problem statement
* [ ] Objective
* [ ] Target users
* [ ] Agent design
* [ ] Workflow diagram
* [ ] n8n screenshot
* [ ] Node explanation
* [ ] AI prompt
* [ ] Tools/APIs
* [ ] Test cases
* [ ] Screenshots
* [ ] Limitations
* [ ] Pre-reflection
* [ ] Post-reflection
* [ ] Future scope

---

# 51. Final Architecture

```text
                     COMMUNITY USER
                           |
                           v
                 ┌──────────────────┐
                 │   n8n FORM       │
                 │                  │
                 │ Name             │
                 │ Contact          │
                 │ Location         │
                 │ Help Request     │
                 └────────┬─────────┘
                          |
                          v
                 ┌──────────────────┐
                 │  DATA PREP       │
                 │  Edit Fields     │
                 └────────┬─────────┘
                          |
                          v
                 ┌──────────────────┐
                 │    AI AGENT      │
                 │                  │
                 │ Understand       │
                 │ Categorize       │
                 │ Prioritize       │
                 │ Summarize        │
                 │ Select Volunteer │
                 └────────┬─────────┘
                          |
                          v
                 ┌──────────────────┐
                 │ STRUCTURED       │
                 │ OUTPUT           │
                 └────────┬─────────┘
                          |
                          v
                 ┌──────────────────┐
                 │ HUMAN REVIEW?    │
                 └──────┬─────┬─────┘
                        |     |
                      YES      NO
                       |       |
                       v       v
                 ┌────────┐ ┌──────────────┐
                 │ HUMAN  │ │   GOOGLE     │
                 │ REVIEW │ │   SHEETS     │
                 └───┬────┘ │ Volunteers   │
                     |      └──────┬───────┘
                     |             |
                     |             v
                     |      ┌──────────────┐
                     |      │   ASSIGN     │
                     |      │  VOLUNTEER   │
                     |      └──────┬───────┘
                     |             |
                     |             v
                     |      ┌──────────────┐
                     |      │ SEND EMAIL   │
                     |      └──────┬───────┘
                     |             |
                     └──────┬──────┘
                            |
                            v
                   ┌──────────────────┐
                   │ GOOGLE SHEETS    │
                   │ Help Requests    │
                   │ AI Logs          │
                   │ Human Review     │
                   └──────────────────┘
```

# 52. Conclusion

The AI NGO/Community Helpdesk Agent demonstrates how n8n can combine an AI model, structured data, decision logic, external tools and human oversight into a practical automation workflow.

The system helps categorize community requests, determine their priority, identify suitable volunteers, automatically send notifications and maintain records. Human review is included for high-risk, sensitive or uncertain cases, making the workflow more appropriate for a real-world NGO environment.

---
