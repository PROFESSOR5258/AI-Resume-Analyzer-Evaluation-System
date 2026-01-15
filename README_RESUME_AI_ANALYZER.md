# 📄 Resume AI Analyzer (n8n Workflow)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![n8n](https://img.shields.io/badge/n8n-Automation-orange)](https://n8n.io/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4.1--mini-blue)](https://openai.com/)
[![AI Powered](https://img.shields.io/badge/AI-Resume%20Screening-brightgreen)](https://github.com)

> **Automatically receives PDF resumes, extracts candidate details, evaluates job relevance using AI, and appends results into Google Sheets.**

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [Output Data Fields](#-output-data-fields)
- [Troubleshooting](#-troubleshooting)
- [Best Practices](#-best-practices)
- [License](#-license)

---

## 🎯 Overview

**Resume AI Analyzer** is an automated candidate screening workflow built in **n8n**.  
It monitors your Gmail inbox for new **PDF resume attachments**, uploads them to Google Drive, extracts text from the PDF, and uses an **OpenAI-powered agent** to:

- Extract structured candidate contact details (name, email, phone)
- Summarize education, job history, and skill set
- Score how relevant the candidate is for a target role (1–10)
- Save everything into a **Google Sheet** for hiring review

This workflow is ideal for recruiters, founders, and hiring managers who want quick candidate shortlisting.

---

## ✨ Key Features

- 📩 **Gmail Auto Intake**: Triggers whenever a PDF resume attachment arrives
- ☁️ **Resume Storage**: Uploads PDFs into a specific Google Drive folder
- 🧾 **PDF Text Extraction**: Converts resumes into raw text for AI processing
- 🧠 **AI Candidate Evaluation**: Summarizes + scores suitability
- 🧩 **Structured Parsing**: Uses JavaScript parsing to split AI output into clean fields
- 📊 **Google Sheets Export**: Automatically appends one row per candidate

---

## 🔄 How It Works

### User Experience

1. A candidate emails a **PDF resume**
2. Workflow automatically processes it
3. The candidate profile is added into a **Google Sheet** with:
   - Contact info
   - Skills summary
   - Job history summary
   - Relevance score + justification

---

## 🏗️ System Architecture

```
Gmail Trigger (PDF attachments)
        ↓
Upload resume to Google Drive folder
        ↓
Download the same file from Drive
        ↓
Extract text from PDF
        ↓
AI Information Extractor (Name / Email / Phone)
        ↓
AI Agent: CV summarization + relevance scoring
        ↓
Set node (store AI output)
        ↓
Code node (parse sections + score)
        ↓
Append row into Google Sheets
```

---

## 🛠️ Tech Stack

| Category | Technology |
|---------|------------|
| Automation | n8n |
| Intake | Gmail Trigger node |
| Storage | Google Drive |
| Parsing | Extract From File (PDF) |
| AI | OpenAI Chat Model (`gpt-4.1-mini`) |
| Output | Google Sheets |

---

## 📦 Prerequisites

### Required Accounts / Access

- ✅ n8n instance (cloud or self-hosted)
- ✅ Gmail account connected via OAuth2
- ✅ Google Drive OAuth2
- ✅ Google Sheets OAuth2
- ✅ OpenAI API key

### Resume Intake Requirement

- Resumes must arrive as **PDF attachment** in Gmail.
- Gmail Trigger filter: `has:attachment filename:pdf`

---

## 🚀 Installation

1. Import the workflow JSON into n8n:
   - `RESUME AI ANALYZER.json`
2. Create or select:
   - A Google Drive folder where resumes are stored
   - A Google Sheet to store candidates
3. Configure credentials (OpenAI + Google) in n8n
4. Activate the workflow

---

## ⚙️ Configuration

### 1) Gmail Trigger

**Node:** `Gmail Trigger`  
Filter used:
```
has:attachment filename:pdf
```

Recommended: use a dedicated hiring mailbox (e.g., `careers@...`) or a Gmail label filter.

---

### 2) Google Drive Upload Folder

**Node:** `Upload file`

Set:
- Drive: `My Drive`
- Folder: your **JOB RESUMES** folder (or create one)

---

### 3) OpenAI Model

**Node:** `OpenAI Chat Model`  
Model configured:
- `gpt-4.1-mini`

---

### 4) AI Agent Prompt

**Node:** `AI Agent`

The system message instructs the AI to:
- Extract education, job history, skill set
- Provide relevance score (1–10)
- Provide short justification

⚠️ Tip: include your **Job Description** inside the prompt text if you want role-based scoring.

---

### 5) Google Sheet Destination

**Node:** `Append row in sheet`

Create headers in Google Sheets:
```
Name | Email | Number | Skill Set | Qualifications | Job History | Score | Justification
```

---

## 📖 Usage

### Option A: Gmail-based intake (default)

Send any resume as a **PDF attachment** to the Gmail inbox connected with the workflow.

### Option B: Add additional triggers (optional)

The JSON also contains:
- `On form submission` (file upload form trigger)
- `Slack Trigger` (file_share event)

You can connect them into the same pipeline if you want multi-channel resume intake.

---

## 📊 Output Data Fields

| Field | Description |
|------|-------------|
| Name | Candidate name |
| Email | Candidate email |
| Number | Candidate phone number |
| Skill Set | Comma-separated skills extracted from CV |
| Qualifications | Education summary |
| Job History | Work history summary |
| Score | 1–10 relevance score |
| Justification | Short reasoning for the score |

---

## 🐛 Troubleshooting

### Issue: No workflow trigger

- Ensure the workflow is **Active**
- Confirm the Gmail filter matches your emails (`filename:pdf`)
- Check Gmail OAuth token validity

### Issue: Resume uploads but text extraction fails

- Confirm PDF is not scanned-only
- If scanned PDFs are common, add OCR (e.g., Google Vision / OCR node)

### Issue: AI output not parsed into fields

- The Code node expects headings like:
  - `Educational Qualifications:`
  - `Job History:`
  - `Skill Set:`
  - `Evaluation ...`
- Ensure your AI prompt keeps this exact structure

### Issue: Google Sheet columns mismatch

- Confirm column names match workflow mapping:
  - Name, Email, Number, Skill Set, Qualifications, Job History, Score, Justification

---

## 💡 Best Practices

- Use a dedicated Drive folder for tracking resumes
- Add job description context for better scoring
- Include a "Status" column in Sheets for recruiter review (Shortlisted / Rejected / Hold)
- Consider adding deduplication (match by Email)

---

## 📄 License

This workflow documentation is provided under the **MIT License**.
