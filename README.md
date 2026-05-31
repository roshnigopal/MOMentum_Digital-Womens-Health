# MOMentum — A Digital Women's Health Intervention for Gestational Diabetes Mellitus (GDM)

> A research-driven digital health prototype developed at the **Centre for Digital Health Interventions**, a joint initiative of the University of Zurich, ETH Zurich, and the University of St. Gallen.

---

## 🌐 Live Demo

**[→ Open the App on Lovable](https://gdm-guide.lovable.app)**

> The prototype is fully hosted on Lovable. No local setup is required.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Background & Motivation](#background--motivation)
- [Key Features](#key-features)
- [GINI — The AI Chatbot](#gini--the-ai-chatbot)
- [App Architecture](#app-architecture)
- [Tech Stack](#tech-stack)
- [Research Foundation](#research-foundation)
- [Long-term Vision](#long-term-vision)
- [Team](#team)

---

## Overview

**MOMentum** is a web-based digital health prototype designed to support women diagnosed with Gestational Diabetes Mellitus (GDM) during pregnancy. The application provides personalised nutritional guidance, GDM education, emotional wellbeing tracking, a community space, and an AI-powered conversational assistant — all in a single, low-friction interface optimised for pregnant women across diverse cultural and literacy backgrounds.

The project was developed over two semesters as part of the *Digital Transformation Challenges* program, under the supervision of **Prof. Dr. Marcia Nißen**.

---

## Background & Motivation

GDM is a growing public health issue:

- **~10%** of pregnancies in Switzerland are affected
- **~17%** globally (International Diabetes Federation, 2021)
- Women with prior GDM face a **17× increased risk** of developing Type 2 diabetes within 3–6 years postpartum
- Existing digital tools are inaccessible, overly complex, or clinic-gated — leaving the majority of women without meaningful support

User research (surveys with 58 participants, exploratory interviews) revealed that:

- **64%** of women with GDM used *no digital tools at all*
- The primary barriers were cognitive load, time pressure, lack of cultural relevance, and poor usability — not resistance to technology
- Women prioritised **ease of use**, **reliable dietary guidance**, and **privacy** above all else

MOMentum was designed in direct response to these findings.

---

## Key Features

| Module | Description |
|---|---|
| **Dashboard (Home)** | Personalised daily overview with tasks, recommendations, reminders, and quick access to all modules |
| **Nutrition Guide** | Low-GI meal suggestions, cultural food preferences, a "Build My Plate" tool, recipe browser with daily carb distribution guidance |
| **GINI Chatbot** | AI-powered conversational assistant for dietary advice, GDM education, and emotional support (see section below) |
| **Wellbeing Tracker** | Optional logging for blood glucose, mood, sleep, and weight — designed to remain useful even with minimal input |
| **Learn (Blogs)** | Curated educational articles and "Did You Know" content covering GDM pathology, nutrition, lifestyle, and postpartum health |
| **Community** | Peer discussion space and forum for women to share experiences, strategies, and support |
| **Profile** | User preferences, pregnancy stage, cultural/dietary settings, doctor information, and medical report upload |

---

## GINI — The AI Chatbot

**GINI** is the conversational AI assistant embedded in the MOMentum dashboard. It serves as the primary interaction point for users who need guidance, reassurance, or quick answers — without the need to navigate menus or search through static content.

### How GINI Works

GINI is built on a **Retrieval-Augmented Generation (RAG)** architecture, combining a curated medical knowledge base with a large language model to produce responses that are both contextually accurate and empathetically phrased.

The pipeline operates as follows:

1. **Knowledge Base** — Medical PDFs and clinical guidelines from reliable sources are ingested and stored
2. **Text Extraction & Chunking** — Documents are processed using Datalab Chandra and split into overlapping semantic chunks for fine-grained retrieval
3. **Embedding Generation** — Chunks are converted to vector embeddings using `all-MiniLM-L6-v2`
4. **FAISS Vector Store** — Embeddings are indexed in a FAISS vector database for efficient similarity search
5. **User Query** — The user's message is embedded and used to query the vector store
6. **Semantic Similarity Search** — Top-k relevant chunks are retrieved based on cosine similarity
7. **Prompt Construction** — A structured prompt is assembled combining the system instructions (safety control), retrieved context chunks, and the user's query
8. **LLM Response Generation** — The assembled prompt is passed to `Qwen2.5-7B-Instruct` to generate a contextually grounded, safe, and empathetic response
9. **Final Answer** — The response is returned to the user in the chat interface

### What GINI Can Help With

- **Dietary guidance** — what to eat, carb distribution across meals, safe snacks, foods to avoid
- **GDM education** — explaining insulin resistance, blood sugar targets, what test results mean
- **Recipe support** — suggesting meal options aligned with the user's cultural preferences and glycaemic needs
- **Emotional reassurance** — responding empathetically to anxiety, overwhelm, or confusion about the diagnosis
- **Quick facts** — answering common questions without requiring the user to read full articles

### Safety & Design Principles

GINI is designed to complement — not replace — clinical care. All responses are grounded in retrieved medical content from reliable sources, and the system prompt enforces safety boundaries (no diagnosis, no medication dosage advice, clear referral to healthcare providers where appropriate). The tone is warm, non-clinical, and culturally sensitive.

---

## App Architecture

```
MOMentum
├── Dashboard (Home)
│   ├── Daily task & recommendation engine
│   ├── Pregnancy-stage-aware content
│   └── GINI chatbot entry point
├── Nutrition
│   ├── Rule-based meal suggestion engine (low GI focus)
│   ├── Cultural preference filtering
│   ├── Recipe display templates
│   └── "Build My Plate" interactive tool
├── Wellbeing
│   ├── Glucose, mood, sleep & weight logging
│   └── Trend mapping for recommendations
├── Community
│   ├── Blogs & educational articles
│   └── Discussion / forum interface
├── Profile
│   ├── User & pregnancy data
│   ├── Cultural & dietary preferences
│   └── Medical report upload
└── GINI (AI Chatbot)
    ├── RAG pipeline (FAISS + Qwen2.5-7B-Instruct)
    ├── Safety-controlled prompt construction
    └── Empathetic response generation
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React (low-code environment via Lovable) |
| Styling | Tailwind-inspired design system, soft UI principles |
| State & Logic | Built-in workflow logic for dynamic recommendations |
| Data Handling | Structured JSON datasets (meals, recipes, user inputs) |
| AI Chatbot (GINI) | RAG pipeline — FAISS vector store + Qwen2.5-7B-Instruct |
| Embeddings | `all-MiniLM-L6-v2` |
| Deployment | Lovable (web-hosted, no installation required) |

---

## Research Foundation

MOMentum is grounded in a two-semester mixed-method research process:

- **Literature review** — GDM pathology, mHealth adherence, digital health equity, intersectionality in healthcare
- **Market analysis** — Review of four commercial GDM applications (GDM-DH, Gestational Diabetes Tracker, My GDM TeleHealth, Pregnant with Diabetes), identifying key gaps in accessibility and usability
- **Exploratory interview** — In-depth qualitative interview with an experienced GDM mother (≈1 hour)
- **Survey** — 58 respondents with prior GDM experience across German-speaking Europe, covering food insecurity (adapted HFIAS scale) and digital tool usage patterns

Key findings that shaped the design:
- Diet management is the most difficult and most important daily challenge
- Food insecurity is driven by time, stress, and caregiving responsibilities — not individual choice
- Users want simple, trustworthy, culturally relevant guidance — not feature-rich platforms
- Privacy and autonomy are non-negotiable

---

## Long-term Vision

The current prototype is an MVP. The full-featured roadmap includes:

- **Emotional Support Ecosystem** — Peer community, enhanced empathetic chatbot, optional access to remote psychological support
- **Postpartum Follow-up System** — Automated push reminders at 6 weeks, 6 months, and 1 year postpartum for glucose screening and lifestyle guidance (OGTT reminders, Type 2 diabetes prevention)
- **Full Cross-Platform Deployment** — Native mobile app (iOS & Android) + web dashboard for professionals
- **Recipe & Retail Collaboration** — Strategic integration with Swiss retailers (e.g., Migros/Migusto) to bridge GDM-friendly recipe recommendations with real ingredient ordering and pickup

---

## Team

| Name | Affiliation |
|---|---|
| Arjun Singh Bhadoria | University of Zurich |
| Roshini Gopal | University of Zurich |
| Qiqi Li | University of Zurich |
| Yue Zhang | University of Zurich |
| Leyi Hu | University of Zurich |
| Wan-Yu Sung | University of Zurich |
| **Prof. Dr. Marcia Nißen** | University of Zurich / ETH Zurich / University of St. Gallen (Supervisor) |

**Centre for Digital Health Interventions (C4DHI)**
University of Zurich · ETH Zurich · University of St. Gallen
[www.c4dhi.org](https://www.c4dhi.org)

---

## References

Selected key references underpinning the research:

- Aubry et al. (2021). GDM in Switzerland: national coverage of screening and diagnosis. *Swiss Medical Weekly*
- International Diabetes Federation (2021). *IDF Diabetes Atlas*, 10th ed.
- Kramer et al. (2019). GDM and cardiovascular disease risk in women. *Diabetologia*
- Lowe et al. (2018). Gestational maternal blood glucose and childhood BMI. *JAMA*
- McIntyre et al. (2019). Gestational diabetes mellitus. *Nature Reviews Disease Primers*
- Song et al. (2018). Long-term risk of diabetes after GDM. *Obesity Reviews*

---

*MOMentum is a research prototype developed for academic and public health purposes. It does not constitute medical advice. Always consult a qualified healthcare provider for GDM management.*
