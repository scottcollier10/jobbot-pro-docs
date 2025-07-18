# JobBot Pro - Match Score Audit Workflow

## 🧠 Overview

This n8n workflow powers JobBot Pro's intelligent job opportunity evaluation system. It takes job data (from Google Sheets), scores the job-resume match using AI, generates audit insights, and logs the final data to Airtable for tracking.

---

## ⚙️ System Architecture

### 🔹 Trigger

- **Node**: `WebhookTrigger`
- **Purpose**: Receives job data payload from a Google Apps Script webhook
- **Format**: JSON POST containing role, company, link, job description, resume summary, and metadata

### 🔹 Core Steps

1. **FlattenMF**: Normalizes the incoming JSON body fields
2. **PrepareFields**: Filters and standardizes key fields
3. **BuildMatchPrompt**: Constructs the prompt for AI to assess role fit
4. **MatchScorerAI**: Uses Claude/GPT to return match score and reasoning
5. **MatchExplanationChain**: Converts AI result into usable JSON fields
6. **SetAuditResults**: Finalizes fields like Match Score, Justification, and Audit Result
7. **BuildPayload**: Merges all fields into one object
8. **Create Record in Airtable**: Sends complete record to the "Job Opportunities" table in Airtable

---

## 📥 Input Payload Example

```json
{
  "Role": "Marketing Director",
  "Company": "Apple",
  "Link": "https://example.com/job1",
  "Job Description": "Seeking a visionary marketing leader...",
  "Resume Summary": "Experienced in leadership, B2B strategy...",
  "Job Source": "LinkedIn"
}
```

## 📤 Final Output (Airtable Record)

```json
{
  "Role": "Marketing Director",
  "Company": "Apple",
  "Link": "https://example.com/job1",
  "Match Score": 92,
  "Is High Match": true,
  "Justification": "The candidate’s experience aligns well with the role requirements.",
  "Audit Result": "Highly recommended to advance.",
  "Status": "New",
  "Workflow Stage": "Score Ready"
}
```

---

## 🧩 Node Documentation (Quick Reference)

| Node Name               | Function                                   |
| ----------------------- | ------------------------------------------ |
| `WebhookTrigger`        | Receives job info from Apps Script webhook |
| `FlattenMF`             | Normalizes JSON fields for later access    |
| `PrepareFields`         | Trims and formats base fields              |
| `BuildMatchPrompt`      | Builds prompt for AI scoring               |
| `MatchScorerAI`         | Calls AI model for match assessment        |
| `MatchExplanationChain` | Parses AI text into structured fields      |
| `SetAuditResults`       | Writes AI results into usable values       |
| `BuildPayload`          | Final merge of all data for output         |
| `Create a Record`       | Sends output to Airtable                   |

---

## 🐛 Known Issues / Debug Notes

- **Duplicate Airtable entries**: Caused by multiple field updates triggering re-execution
- **AI overwrites real-time data**: `MatchExplanationChain` can overwrite values unless properly mapped
- **Case mismatch in fields**: Ensure all field references use consistent casing like `"Job Description"`
- **Webhook batching**: Apps Script may fire webhook multiple times for minor edits

---

## 🗃 Suggested GitHub Repo Structure

```
📁 jobbot-pro-docs/
├── README.md               # This file
├── nodes.md                # Detailed node-by-node description
├── troubleshooting.md      # Known issues and resolutions
├── changelog.md            # Manual version control log
├── payload_examples/
│   ├── input.json
│   └── output.json
```

---

## ✅ Next Steps

-

