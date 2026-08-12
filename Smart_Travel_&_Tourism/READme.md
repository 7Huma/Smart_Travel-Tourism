# 🌍 Smart Tourism & Travel Planning Platform

> **An AI-powered travel automation platform built with n8n to streamline the complete travel-planning journey — from customer registration and destination discovery to itinerary generation, notifications, feedback, and travel analytics.**

**Built by Huma Shadmeen**

---

## ✨ Overview

The **Smart Tourism & Travel Planning Platform** is an automation-based travel management system developed using **n8n**.

The platform combines **AI, APIs, Google services, and workflow automation** to reduce repetitive manual work involved in travel planning.

Instead of handling every customer request manually, the system automates key stages such as:

* 👤 Customer registration
* 🧠 AI-powered destination recommendations
* 💰 Budget planning
* ✈️ Flight and hotel discovery
* 🗺️ Personalized itinerary generation
* 📧 Automated email communication
* 🔔 Travel alerts and reminders
* 📝 Feedback collection
* 📊 AI-powered travel analytics

The system is designed as a collection of **modular n8n workflows**, allowing individual workflows to be executed independently or connected together to create a complete travel automation pipeline.

---

## 🎯 Problem Statement

Travel planning involves a large amount of repetitive work for travel agencies and consultants.

For every customer, staff may need to manually:

* Collect customer information and preferences
* Compare destinations
* Consider budget constraints
* Search for suitable travel options
* Prepare personalized itineraries
* Send confirmation and reminder emails
* Collect customer feedback
* Analyze feedback and customer sentiment

This process becomes difficult to scale as the number of customers increases.

### 💡 The goal

This project aims to **automate these repetitive activities using n8n and AI**, allowing travel professionals to focus more on customer experience and decision-making rather than manual coordination.

---

## 👥 Target Stakeholders

| Stakeholder                     | Benefit                                            |
| ------------------------------- | -------------------------------------------------- |
| 🏢 Travel Agencies              | Reduce repetitive operational work                 |
| 🧑‍💼 Travel Consultants        | Automate recommendations and itinerary preparation |
| 🧳 Travelers                    | Receive personalized travel assistance             |
| 🏨 Hotels & Transport Providers | Improve integration with travel workflows          |

---

# 🔄 System Workflow

The platform consists of **six modular workflows**.

Each workflow follows a general automation pattern:

```text
Trigger
   ↓
Normalize & Validate
   ↓
AI / Business Logic
   ↓
Store Results
   ↓
Notify Customer
   ↓
Response
```

Error handling is incorporated where appropriate so that invalid input or processing failures can be handled without breaking the complete workflow.

---

## 1️⃣ Workflow 1 — Customer Registration & Preference Collection

**Trigger:** Webhook

The registration workflow handles the initial customer onboarding process.

### Process

```text
Customer
   ↓
Registration Webhook
   ↓
Edit Fields
   ↓
Clean Customer Data
   ↓
Validate Data
   ↓
Valid?
 ┌─┴──────────────┐
 │                │
No               Yes
 ↓                ↓
Error         Generate Traveler ID
Response           ↓
               Google Sheets
                   ↓
             Google Calendar
                   ↓
              Gmail Confirmation
                   ↓
              Success Response
```

### Key functions

* Receives customer registration data
* Cleans and validates submitted information
* Checks required fields
* Generates a unique **Traveler ID**
* Stores customer information in **Google Sheets**
* Creates a **Google Calendar event**
* Sends a confirmation email through **Gmail**
* Returns an error response when validation fails

---

## 2️⃣ Workflow 2 — AI Trip Planning Pipeline

**Trigger:** Webhook / workflow execution

This workflow transforms a customer's travel request into a personalized trip plan.

### Process

```text
Trip Request
     ↓
Gemini AI — Normalize Cities
     ↓
Build API Parameters
     ↓
Flight Search
     ↓
Parse Flight Results
     ↓
AI Flight Ranking
     ↓
Hotel Search
     ↓
Parse Hotel Results
     ↓
AI Hotel Ranking
     ↓
Merge Flight + Hotel Rankings
     ↓
AI Itinerary Generator
     ↓
Workflow 3 — Budget Planner
```

### Key functions

* Receives trip requirements
* Normalizes city and destination names using AI
* Builds search parameters
* Retrieves flight information
* Parses and structures flight results
* Uses AI to rank flight options
* Searches for hotels
* Parses hotel results
* Uses AI to rank hotels
* Combines flight and hotel recommendations
* Generates a personalized itinerary
* Passes the generated itinerary to the **Budget Planner workflow**

---

## 3️⃣ Workflow 3 — Budget Planner & Itinerary Processing

**Trigger:** Called by Workflow 2

This workflow handles the next stage of the travel planning pipeline.

### Main purpose

```text
AI Generated Itinerary
        ↓
Budget Calculation
        ↓
Cost Estimation
        ↓
Final Travel Plan
```

The workflow is designed to process the generated itinerary and estimate the expected travel cost.

---

## 4️⃣ Workflow 4 — Travel Alerts & Notifications

**Trigger:** Execute Workflow Trigger

This workflow is designed as an independent notification sub-workflow.

### Main functions

* Validate travel information
* Generate personalized reminder messages
* Retrieve weather information through an HTTP/API request
* Prepare travel alerts
* Send notifications through Gmail
* Maintain an alert log

> **Current status:** Built as a standalone sub-workflow. It is not automatically connected to Workflow 3 yet.

---

## 5️⃣ Workflow 5 — Feedback Collection

**Trigger:** Execute Workflow Trigger

This workflow handles the collection of customer feedback after a trip.

### Main functions

* Validate customer/trip information
* Generate a feedback request
* Prepare feedback communication
* Send the feedback request through Gmail
* Handle workflow errors

> **Current status:** Implemented as a standalone sub-workflow and can be connected to the main travel pipeline later.

---

## 6️⃣ Workflow 6 — Travel Feedback Analytics

**Trigger:** Google Sheets row insertion

This workflow processes newly received feedback.

### Process

```text
New Feedback
     ↓
Google Sheets Trigger
     ↓
AI Sentiment Analysis
     ↓
Data Processing
     ↓
Analytics Report
     ↓
Store Results
     ↓
Thank-you Email
```

### Key functions

* Detects new feedback entries
* Performs AI-based sentiment analysis
* Processes feedback data
* Generates an analytics report
* Stores analytical results
* Sends a thank-you email to the customer

---

# 🔗 Workflow Connectivity

The current implementation has different levels of integration between workflows.

### Currently chained

```text
Workflow 1
    ↓
Workflow 2
    ↓
Workflow 3
```

Workflows 1 → 2 → 3 are connected through n8n's **Execute Workflow** functionality.

### Independent workflows

```text
Workflow 4 — Travel Alerts
Workflow 5 — Feedback Collection
Workflow 6 — Feedback Analytics
```

Workflows 4 and 5 are designed as reusable sub-workflows but are currently not automatically called by the main pipeline.

Workflow 6 operates independently through a **Google Sheets trigger**, processing new feedback responses when they are added.

---

# 🧠 AI Capabilities

AI is used throughout the platform to reduce manual decision-making.

### AI-powered features include:

* 🤖 Destination/city normalization
* ✈️ Flight ranking
* 🏨 Hotel ranking
* 🗺️ Personalized itinerary generation
* 💬 Automated travel communication
* 📊 Feedback sentiment analysis
* 📈 Travel analytics

---

# 🛠️ Technology Stack

| Technology                | Purpose                                                                  |
| ------------------------- | ------------------------------------------------------------------------ |
| **n8n**                   | Workflow automation and orchestration                                    |
| **OpenAI API**            | AI recommendations, ranking, itinerary generation and sentiment analysis |
| **Gemini AI**             | Destination/city normalization                                           |
| **Gmail API**             | Automated customer communication                                         |
| **Google Sheets API**     | Customer records, logs and analytics                                     |
| **Google Calendar API**   | Customer/travel event management                                         |
| **SerpAPI / Flight APIs** | Flight information                                                       |
| **Hotel Search API**      | Hotel information                                                        |
| **Weather API**           | Travel alerts and weather information                                    |
| **JavaScript**            | Data transformation and workflow processing                              |

---

# 📁 Repository Structure

```text
Smart-Tourism-Travel-Planning-Platform/
│
├── workflows/
│   ├── workflow-1-customer-registration.json
│   ├── workflow-2-trip-planning.json
│   ├── workflow-3-budget-planner.json
│   ├── workflow-4-travel-alerts.json
│   ├── workflow-5-feedback-collection.json
│   └── workflow-6-feedback-analytics.json
│
├── screenshots/
│   └── workflow screenshots
│
├── architecture/
│   ├── system-architecture.drawio
│   └── system-architecture.png
│
├── documentation/
│   ├── problem-analysis.md
│   ├── stakeholders.md
│   └── workflow-documentation.md
│
├── README.md
│
└── LICENSE
```

---

# 🚀 Getting Started

### 1. Install n8n

Set up an n8n instance locally or use an available n8n deployment.

### 2. Import the workflows

Navigate to:

```text
n8n → Import from File
```

and import the workflow JSON files from the `workflows/` directory.

### 3. Configure credentials

Configure the required credentials for:

* OpenAI
* Gemini
* Gmail
* Google Sheets
* Google Calendar
* Flight/API provider
* Hotel/API provider
* Weather API

### 4. Configure workflow connections

After importing the workflows, verify the **Execute Workflow** nodes and update the workflow IDs if required.

### 5. Activate workflows

Activate the required workflows in n8n.

### 6. Test

Submit a test customer registration through **Workflow 1** and follow the execution through the connected workflows.

---

# 📊 Current Implementation

### ✅ Implemented

* Customer registration
* Customer data validation
* Traveler ID generation
* Google Sheets integration
* Google Calendar integration
* Gmail notifications
* AI-based destination/city processing
* Flight search and ranking
* Hotel search and ranking
* AI itinerary generation
* Budget-planning workflow
* Travel alert workflow
* Feedback collection workflow
* AI feedback analytics

### 🔄 Future Integration

The following improvements can connect the currently independent workflows into a complete automated lifecycle:

```text
Workflow 1
    ↓
Workflow 2
    ↓
Workflow 3
    ↓
Workflow 4
    ↓
Workflow 5
    ↓
Workflow 6
```

---

# 🗺️ Roadmap

### ✈️ Travel Services

* Flight booking integration
* Hotel booking APIs
* Transport booking
* Payment gateway integration

### 📱 Communication

* WhatsApp notifications
* SMS notifications
* AI travel chatbot
* Voice assistant

### 🤖 AI Enhancements

* More advanced destination personalization
* Dynamic budget optimization
* Real-time itinerary adjustment
* Personalized travel recommendations

### 🔗 Workflow Integration

* Automatically trigger Travel Alerts after itinerary generation
* Automatically initiate Feedback Collection after trip completion
* Connect Feedback Collection with Feedback Analytics
* Create a complete end-to-end automated travel lifecycle

---

# 🌟 Vision

The long-term goal is to transform the platform from a collection of automation workflows into an **intelligent end-to-end travel assistant** capable of understanding traveler preferences, discovering suitable options, creating personalized plans, assisting throughout the journey, and learning from customer feedback.

> **Plan smarter. Automate better. Travel better. 🌍✈️**

---

### 👩‍💻 Author

**Huma Shadmeen**

Built with **n8n + AI + APIs + automation**.
