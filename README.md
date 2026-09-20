# AI Mentor Learning Analyzer

An AI-powered learning analysis system that helps mentors quickly understand a student's learning progress without manually reviewing large amounts of handwritten notes, documents, code, and project work.

The system collects learning evidence over time, analyzes it using LLMs and RAG, identifies concepts learned and implemented, highlights potential knowledge gaps, and generates a concise mentor-ready progress report.

---

## Problem

When learning AI Engineering under a mentor, most of the actual learning happens between mentor meetings.

A student may accumulate:

* Handwritten note
* PDFs and DOCX file
* Daily learning log
* Code experiment
* GitHub repositories
* Project
* Research material
* Implementation results

Reviewing all of this manually can take a significant amount of time.

This creates a problem for both sides:

**Student**

* Difficult to communicate everything learned
* Easy to forget what was studied weeks earlier
* Difficult to identify gaps objectively

**Mentor**

* Limited time to inspect every document and project
* Hard to track progress across multiple weeks
* Difficult to distinguish between "studied", "understood", and "implemented"

### Goal

Build an AI system that converts scattered learning evidence into a structured, evidence-backed learning report that a mentor can review in a few minutes.

---

# Core Idea

The system follows an:

**Evidence → Analysis → Verification → Mentor Review**

pipeline.

```text
Learning Evidence
       │
       ├── Handwritten Notes
       ├── PDF / DOCX
       ├── Learning Logs
       ├── Code
       └── Projects
              │
              ↓
       Document Processing
              │
              ↓
       Learning Knowledge Base
              │
              ↓
        AI Analysis Layer
              │
       ┌──────┼───────┐
       ↓      ↓       ↓
   Concepts  Gaps   Progress
       │      │       │
       └──────┼───────┘
              ↓
       Verification
              │
              ↓
      Mentor Report
```

---

# Key Principle

The system should not simply ask an LLM:

> "What did the student learn?"

Instead, it should provide **evidence-backed conclusions**.

For example:

```text
Concept: Semantic Chunking

Evidence:
✓ Learning notes found
✓ Implementation found
✓ Experiment results found
✓ Student explanation verified

Status:
Strong evidence
```

Whereas:

```text
Concept: Hybrid Search

Evidence:
✓ Mentioned in notes
✗ No implementation found
✗ No experiment found
✗ No verification answer

Status:
Unverified
```

This prevents the system from confusing **mentioning a concept** with **actually understanding or implementing it**.

---

# Objectives

The system aims to:

1. Collect learning evidence from multiple sources.
2. Extract and organize the concepts studied.
3. Track implementations and experiments.
4. Build a searchable learning knowledge base.
5. Identify potential knowledge gaps.
6. Generate questions to verify understanding.
7. Compare current progress with previous learning periods.
8. Generate concise monthly mentor reports.
9. Preserve links between conclusions and their underlying evidence.
10. Reduce the amount of time a mentor needs to review a student's progress.

---

# Planned Features

## 1. Document Ingestion

Support learning material such as:

* PDF
* DOCX
* Markdown
* TXT
* Images of handwritten notes

Future:

* GitHub repositories
* Code files
* Project documentation

---

## 2. Document Processing

Documents will be processed into structured learning information.

```text
Document
   ↓
Text Extraction / OCR
   ↓
Cleaning
   ↓
Chunking
   ↓
Metadata
   ↓
Embeddings
```

Each chunk will contain useful metadata such as:

```json
{
  "document_id": "doc_123",
  "topic": "RAG",
  "source_type": "learning_notes",
  "date": "2026-09-15",
  "page": 4
}
```

---

# 3. Learning Knowledge Base

The system will maintain a searchable representation of the student's learning history.

It will contain information about:

* Topics
* Concepts
* Learning sessions
* Implementations
* Projects
* Experiments
* Questions
* Mentor feedback
* Progress over time

The knowledge base will combine:

**Structured data + semantic search**

---

# 4. RAG-Based Learning Search

The system should be able to answer questions such as:

> What did I learn about RAG last month?

> Which topics have I implemented?

> Where did I study MMR?

> Have I previously worked with hybrid search?

> What concepts related to embeddings have I covered?

The system will retrieve relevant learning evidence before generating an answer.

---

# 5. Learning Analysis

The AI analysis layer will identify:

### Concepts Learned

What concepts appear in the student's learning material.

### Concepts Implemented

Which concepts have supporting code or project evidence.

### Concepts Repeated

Topics that appear across multiple learning sessions.

### Potential Gaps

Concepts that are mentioned but lack sufficient evidence of understanding or implementation.

### Progress

How the student's learning changes over time.

---

# 6. Understanding Verification

The system can generate questions based on the student's own learning material.

Example:

```text
Topic: RAG

Question:
Why can increasing chunk size negatively affect
retrieval precision?
```

The student answers the question.

The system evaluates the answer using the relevant learning material and evidence.

This creates an additional distinction:

```text
Mentioned
    ↓
Studied
    ↓
Implemented
    ↓
Explained
    ↓
Verified
```

---

# 7. Mentor Dashboard

The mentor should not need to read every document.

Instead, the dashboard provides a high-level overview.

Example:

```text
========================================
        MONTHLY LEARNING REVIEW
========================================

Period: September 2026

Topics Studied:              14
Concepts Identified:         47
Implementations:              8
Experiments:                  5

----------------------------------------
MAJOR AREAS
----------------------------------------

RAG                    Strong Evidence
LLM Fundamentals       Strong Evidence
Embeddings             Moderate Evidence
Agents                 Early
Deployment             Early

----------------------------------------
NEEDS VERIFICATION
----------------------------------------

• Hybrid Search
• Agent Architecture
• Production Deployment

----------------------------------------
SUGGESTED MENTOR QUESTIONS
----------------------------------------

1. Why would hybrid search be useful?
2. How would you evaluate a retriever?
3. How would you deploy this system?
```

---

# 8. Monthly Mentor Report

The system will generate a concise report containing:

### Learning Summary

What was studied during the period.

### Implementation Summary

What was actually built.

### Evidence

Where the conclusions came from.

### Understanding Status

What has been verified versus what is only mentioned.

### Knowledge Gaps

Areas that may require additional study.

### Suggested Questions

Questions the mentor can ask during the next meeting.

### Previous Goals

Progress against goals from the previous mentor meeting.

---

# Technology Stack

## Frontend

* React
* JavaScript / TypeScript
* Tailwind CSS

## Backend

* Python
* FastAPI

## AI / LLM

* LangChain
* LLM provider abstraction
* Structured output

Potential LLM providers:

* OpenAI
* Gemini
* Claude
* OpenRouter

## Embeddings

Configurable embedding model.

## Database

* PostgreSQL

## Vector Search

* pgvector

## Document Processing

* PyMuPDF
* python-docx
* OCR solution for handwritten notes

## Deployment

Planned:

* Docker
* Cloud deployment

---

# Planned Architecture

```text
                       FRONTEND
                          │
                          ↓
                     FastAPI API
                          │
              ┌───────────┴───────────┐
              │                       │
              ↓                       ↓
       Document Service          Learning Service
              │                       │
              ↓                       ↓
       Parsing / OCR             AI Analysis
              │                       │
              └───────────┬───────────┘
                          ↓
                    LangChain Layer
                          │
              ┌───────────┼───────────┐
              ↓           ↓           ↓
           LLM         Retriever    Tools
              │           │
              │           ↓
              │       pgvector
              │           │
              └─────┬─────┘
                    ↓
              PostgreSQL
                    │
                    ↓
             Mentor Dashboard
```

---

# LangChain Usage

LangChain will be used as an application framework around the core AI workflow.

Planned concepts:

* Document loaders
* Text splitters
* Prompt templates
* Chat models
* Embeddings
* Vector stores
* Retrievers
* Chains
* Structured output
* Tool calling
* Agents

The project will initially avoid hiding important concepts behind high-level abstractions.

Where useful, core RAG components will first be understood and experimented with independently before using their LangChain equivalents.

---

# Development Roadmap

## Phase 1 — LangChain Fundamentals

Learn and experiment with:

* Models
* Prompts
* Messages
* Document loaders
* Text splitters
* Embeddings
* Vector stores
* Retrievers
* Chains
* Structured output

---

## Phase 2 — Document Ingestion

Implement:

* PDF ingestion
* DOCX ingestion
* Markdown ingestion
* Metadata extraction
* Chunking
* Basic OCR

---

## Phase 3 — Learning Knowledge Base

Implement:

* PostgreSQL schema
* pgvector
* Document storage
* Chunk storage
* Embeddings
* Semantic search

---

## Phase 4 — RAG

Implement:

```text
User Query
    ↓
Query Processing
    ↓
Retriever
    ↓
Relevant Learning Evidence
    ↓
LLM
    ↓
Evidence-backed Answer
```

---

## Phase 5 — Learning Analyzer

Implement:

* Topic extraction
* Concept extraction
* Implementation detection
* Learning summaries
* Gap detection
* Evidence mapping

---

## Phase 6 — Verification

Implement:

* Automatic question generation
* Student answers
* Answer evaluation
* Concept-level verification
* Confidence/evidence tracking

---

## Phase 7 — Mentor Dashboard

Implement:

* Monthly learning overview
* Topic progress
* Evidence explorer
* Knowledge gaps
* Suggested questions
* Learning timeline

---

## Phase 8 — Mentor Feedback Loop

Allow mentors to provide:

* Feedback
* New learning goals
* Topics requiring deeper study
* Questions to revisit

The system will use this information in future monthly reports.

---

# Example End-to-End Workflow

```text
Day 1
Student studies embeddings
        ↓
Uploads notes

Day 2
Student studies RAG
        ↓
Uploads DOCX

Day 5
Student implements semantic chunking
        ↓
Adds project/code evidence

Day 10
Student evaluates retrieval
        ↓
Adds experiment results

        ...

End of Month
        ↓
AI analyzes all evidence
        ↓
Generates learning profile
        ↓
Identifies gaps
        ↓
Generates verification questions
        ↓
Creates mentor report
        ↓
Mentor reviews in minutes
        ↓
Mentor provides feedback
        ↓
Next month's goals
```

---

# Future Extensions

Potential future capabilities:

* GitHub repository analysis
* Automatic code/project inspection
* Learning timeline visualization
* Knowledge graph
* Concept dependency graph
* Adaptive quizzes
* Mentor-specific dashboards
* Voice-based learning logs
* Automatic daily learning summaries
* Multi-modal handwritten note analysis
* Agent-based project inspection
* Learning recommendations
* Automated evaluation of project implementations

---

# Design Principles

### 1. Evidence First

AI-generated conclusions should be traceable to actual student evidence.

### 2. Separate Evidence from Inference

The system should distinguish between:

```text
What the student wrote
        ↓
What the student implemented
        ↓
What the AI inferred
```

### 3. Don't Equate Exposure With Mastery

Mentioning a concept does not mean the student understands it.

### 4. Human-in-the-Loop

The mentor remains the final reviewer.

### 5. Incremental Development

Start with a simple working system and progressively add:

```text
Documents
   ↓
RAG
   ↓
Analysis
   ↓
Verification
   ↓
Mentor feedback
   ↓
Agents / advanced features
```

### 6. Evaluate the AI

The system itself should eventually be evaluated for:

* Retrieval quality
* Factual consistency
* Evidence grounding
* Question quality
* Answer evaluation accuracy
* Report usefulness

---

# Project Status

🚧 **Currently in development**

Initial focus:

**LangChain fundamentals → document ingestion → RAG → learning analysis**

---

# Long-Term Vision

The goal is to build an AI-powered personal learning system that acts as a bridge between a student's daily learning and periodic mentor guidance.

Instead of a mentor spending hours reviewing everything a student has done, the system should provide:

> **"Here is what I learned, here is the evidence, here is what I implemented, here is what I can explain, here are the gaps, and here are the things you should discuss with me."**

The mentor remains the decision-maker; the AI handles the repetitive analysis and organization.
