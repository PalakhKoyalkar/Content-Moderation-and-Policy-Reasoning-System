# LLM-Based Content Moderation & Policy Reasoning System

## Overview
This project implements a **policy-driven content moderation system** using a Hugging Face moderation model and explicit safety policies.  
The system focuses on **reasoning and explainability over raw classification**, making every moderation decision transparent, auditable, and safe.

An optional **Gemini AI integration** is used only to generate **human-friendly explanations** of already-decided moderation outcomes. Gemini does **not** influence moderation decisions.

---

## Key Features
- Accepts user-generated text as input
- Classifies content into **ALLOW / RESTRICT / DISALLOW**
- Identifies **violated policy types**
- Provides **clear policy-based reasoning**
- Returns **structured JSON output**
- Uses Gemini AI **only for explanation**, not decision-making
- Safety-aware design with guardrails and fallbacks

---

## System Architecture

User Input
↓
Hugging Face Moderation Model (signal generation)
↓
Policy Mapping & Severity Rules
↓
Deterministic Decision Engine
↓
Policy-Based Reasoning
↓
(Optional) Gemini AI – Explanation Layer


---

## Moderation Policies
The system evaluates content against the following policies:
- Hate Speech
- Harassment
- Violence
- Self-Harm
- Sexual Content
- Safe Content

Each policy has:
- A severity level
- A deterministic explanation template
- Confidence thresholds to prevent false positives

---

## Explainable AI Design
- **Decisions are policy-driven**, not model-driven
- The model is used only as a **signal generator**
- Explanations are generated using:
  - Policy templates (mandatory)
  - Gemini AI (optional, for user-friendly summaries)
- No external API is allowed to override moderation decisions

---

## Gemini AI Usage (Optional)
Gemini AI is used **only to explain** moderation results in simple language.

Important rules:
- Gemini does NOT classify content
- Gemini does NOT choose policies
- Gemini does NOT affect ALLOW/DISALLOW decisions
- If Gemini fails, the system falls back to policy-based explanations

---

## Example Output

```json
{
  "input_text": "I hate all of you and you deserve pain",
  "decision": "DISALLOW",
  "violated_policies": [
    {
      "policy": "VIOLENCE",
      "severity": "HIGH",
      "confidence": 0.24
    }
  ],
  "reasoning": "The content promotes physical harm, which violates the Violence policy.",
  "explainability": {
    "policy_driven": true,
    "model_used_as_signal": true,
    "human_readable_reasoning": true
  }
}
