# Home Credit Loan Against Property (LAP) Voice Agent

An automated outbound voice qualification system designed for Home Credit to contact existing valued customers, present pre-approved Loan Against Property (LAP) offers of up to ₹75,00,000 (75 Lakhs), and conduct preliminary credit eligibility checks before handoff to senior loan experts.

---

## Overview

This project implements a complete conversational voice agent architecture, conversation state machine, and interactive call testing simulator for financial pre-qualification. The voice agent acts as the primary contact point to verify identity, evaluate eligibility across 7 specific criteria, enforce underwriting boundaries, and ensure customer experience.

---

## Key Features

### 1. Conversational Voice Agent System Prompt
- Built for voice platforms including Retell, Bolna, Vapi, and ElevenLabs.
- Context injection for dynamic variables:
  - `company_name`: Home Credit India
  - `customer_name`: Name of customer being contacted
  - `agent_name`: Voice agent persona
  - `agent_gender`: Voice gender consistency
  - `language_to_speak`: English / Hindi / Hinglish
  - `current_date`, `current_day`, `current_time`
  - `additional_context_from_rag`: Product rules, interest rates, and loan terms

### 2. 7-Point Eligibility Checklist & Underwriting Engine
The voice agent sequentially captures and validates 7 mandatory data points with support for out-of-order responses:
1. **Property Type**: Validates Residential, Commercial, or Industrial. Agricultural land is flagged as ineligible.
2. **Ownership Status**: Checks for Sole or Joint ownership (both eligible).
3. **Document Availability**: Verifies physical availability of original title documents.
4. **Loan Amount**: Evaluates borrowing requirements up to ₹75,00,000. Includes renegotiation flow if amounts exceed the cap.
5. **Occupation & Income Mode**: Evaluates Salaried or Self-Employed status with mandatory bank credit routing. Cash income is flagged as ineligible.
6. **Market Valuation**: Captures estimated market value to assess loan-to-value limits.
7. **Repayment Tenure**: Confirms repayment duration within the allowable 3 to 15-year window.

### 3. Business Logic & Boundary Controls
- **Immediate Disqualification Gate**: Automatically and politely concludes calls whenever an underwriting criteria is breached (such as cash salary, agricultural land, or missing original deeds) without continuing redundant questions.
- **The "Transfer" Logic Switch**: Detects customers requesting Balance Transfers or lower EMIs on existing loans and automatically routes them to the specialized Loan Transfer desk.
- **Out-of-Order Slot Filling**: Extracts multiple criteria from natural conversational sentences and avoids re-asking already answered questions.
- **Busy / Callback Scheduling**: Handles customers who are driving, in meetings, or busy by securing their preferred callback time.
- **Senior Expert Handoff Gate**: Strictly ensures all 7 criteria are met before executing the final handoff to a senior loan specialist.

### 4. Interactive Call Simulator & Testing Suite
- **Web Speech Engine**: Live speech-to-text (microphone input) and text-to-speech voice playback.
- **Real-Time Visual HUD**: Live 7-point checklist displaying extracted parameters and status tags.
- **Preset Test Harness**: 1-click test suites for golden qualification, edge cases, immediate disqualifications, balance transfers, and over-limit renegotiation.
- **Call Recordings & Transcripts**: Turn-by-turn timestamps, audio playback simulation, and exportable logs.

---

## Project Structure

```text
├── src/
│   ├── components/
│   │   ├── CallSimulator.tsx      # Phone dialer, voice engine, and transcript stream
│   │   ├── ChecklistHUD.tsx       # Live 7-point qualification checklist HUD
│   │   ├── PromptViewer.tsx       # System prompt manager & voice platform config
│   │   ├── ScenariosPanel.tsx     # 1-click test scenarios for edge cases
│   │   ├── CallLogsModal.tsx      # Call recording audio player & transcripts
│   │   ├── SubmissionPackage.tsx  # Compiled submission deliverables
│   │   └── Header.tsx             # Navigation and status bar
│   ├── data/
│   │   ├── promptTemplate.ts      # Complete voice prompt & platform JSON configs
│   │   └── scenarios.ts           # Pre-configured test scenarios
│   ├── services/
│   │   ├── voiceAgentEngine.ts    # Dialogue management & slot extraction engine
│   │   └── speechService.ts       # Speech synthesis & recognition integration
│   ├── types/
│   │   └── agent.ts               # State, eligibility, and log data structures
│   ├── App.tsx                    # Main application controller
│   └── main.tsx                   # React entry point
└── package.json
