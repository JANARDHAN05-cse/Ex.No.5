# EXP 5: COMPARATIVE ANALYSIS OF DIFFERENT TYPES OF PROMPTING PATTERNS AND EXPLAIN WITH VARIOUS TEST SCENARIOS

# Aim: To test and compare how different pattern models respond to various prompts (broad or unstructured) versus basic prompts (clearer and more refined) across multiple scenarios.  Analyze the quality, accuracy, and depth of the generated responses 

# 🧠 Comparative Analysis of Prompting Patterns

> A structured study of Zero-Shot, Few-Shot, and Chain-of-Thought prompting techniques — with real test scenarios, evaluated responses, and a comparison framework.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Prompting Techniques Explained](#prompting-techniques-explained)
- [Activity Framework](#activity-framework)
- [Scenario 1 — Sentiment Analysis](#scenario-1--sentiment-analysis)
- [Scenario 2 — Math Word Problem](#scenario-2--math-word-problem)
- [Scenario 3 — Text Classification](#scenario-3--text-classification)
- [Scenario 4 — Logical Reasoning](#scenario-4--logical-reasoning)
- [Evaluation Method — ROUGE-Style Rubric](#evaluation-method--rouge-style-rubric)
- [Master Comparison Table](#master-comparison-table)
- [Key Takeaways](#key-takeaways)
-  result

---

## Overview

Prompting is the art of instructing a Large Language Model (LLM). The **structure and specificity** of a prompt dramatically affects the quality, accuracy, and depth of the model's output. This report compares three major prompting patterns:

| Pattern | Description |
|---|---|
| **Basic Prompt** | Broad, unstructured — minimal instruction |
| **Zero-Shot Prompt** | Clear task framing, no examples given |
| **Few-Shot Prompt** | Task framing + 2 or more worked examples |
| **Chain-of-Thought (CoT)** | Step-by-step reasoning explicitly guided |

---

## Prompting Techniques Explained

### 🔹 Basic Prompt
A raw, unrefined instruction. Gives the model maximum freedom but minimum guidance. Results are inconsistent and often shallow.

### 🔸 Zero-Shot Prompt
A well-framed instruction with no examples. The model relies entirely on pre-trained knowledge. Works well for common tasks but may fail on nuanced or domain-specific ones.

### 🔶 Few-Shot Prompt
Provides 2–5 worked examples before the actual task. The model learns the *pattern* from context. Significantly boosts consistency and format compliance.

### 🔷 Chain-of-Thought (CoT) Prompt
Instructs (or demonstrates) the model to reason step-by-step before producing the final answer. Best for multi-step reasoning, math, and logic tasks.

---

## Activity Framework

Each scenario follows this 4-step structure:

```
1. Basic Prompt         → Broad / unstructured
2. Zero-Shot Prompt     → Clear task, no examples
3. Few-Shot Prompt      → Clear task + ≥2 examples
4. Chain-of-Thought     → Step-by-step reasoning shown
```

---

## Scenario 1 — Sentiment Analysis

**Task:** Determine whether a product review is Positive, Negative, or Neutral.

---

### Step 1 — Basic Prompt

```
Tell me if this review is good or bad:
"The battery drains really fast and the screen cracked after one week."
```

**Model Output:**
> Bad.

**Assessment:** One-word answer. No label format. No confidence. Not useful in a pipeline.

---

### Step 2 — Zero-Shot Prompt

```
Classify the sentiment of the following product review as Positive, Negative, or Neutral.

Review: "The battery drains really fast and the screen cracked after one week."

Sentiment:
```

**Model Output:**
> Negative

**Assessment:** Correct label, clean format. But no reasoning or confidence — could misfire on mixed reviews.

---

### Step 3 — Few-Shot Prompt

```
Classify the sentiment of the following product review as Positive, Negative, or Neutral.

Examples:
Review: "Absolutely love this phone! The camera is stunning and battery lasts two days."
Sentiment: Positive

Review: "Decent product, nothing special. Works as expected."
Sentiment: Neutral

Review: "Completely stopped working after 3 days. Very disappointed."
Sentiment: Negative

Now classify:
Review: "The battery drains really fast and the screen cracked after one week."
Sentiment:
```

**Model Output:**
> Negative

**Assessment:** Same correct label, but now consistent format is enforced by pattern. Will generalize better to ambiguous reviews.

---

### Step 4 — Chain-of-Thought Prompt

```
Classify the sentiment of the following product review as Positive, Negative, or Neutral.
Think through your reasoning step by step before giving your final answer.

Review: "The battery drains really fast and the screen cracked after one week."

Step-by-step reasoning:
```

**Model Output:**
> Step 1: Identify key phrases in the review.
> - "battery drains really fast" → indicates dissatisfaction with battery life (Negative signal)
> - "screen cracked after one week" → indicates poor build quality or durability (Negative signal)
>
> Step 2: Check for any positive phrases.
> - None found.
>
> Step 3: Check for neutral or balanced phrases.
> - None found. The review is entirely complaint-based.
>
> Step 4: Determine overall sentiment.
> - Both issues mentioned are significant product failures. There are no redeeming comments.
>
> **Final Sentiment: Negative**

**Assessment:** Full transparency. Reasoning is auditable. Excellent for debugging misclassifications in production.

---

## Scenario 2 — Math Word Problem

**Task:** Solve a multi-step arithmetic word problem.

---

### Step 1 — Basic Prompt

```
A train travels 60 km/h for 2 hours, then 80 km/h for 3 hours. How far did it go?
```

**Model Output:**
> The train traveled 360 km.

**Assessment:** Correct answer but no working shown. Cannot verify or trust output for harder variations.

---

### Step 2 — Zero-Shot Prompt

```
Solve the following math word problem and provide the final answer with units.

Problem: A train travels at 60 km/h for 2 hours, then at 80 km/h for 3 hours. What is the total distance traveled?
```

**Model Output:**
> Total distance = (60 × 2) + (80 × 3) = 120 + 240 = **360 km**

**Assessment:** Better — shows the formula, gives units. Still no step-by-step narration.

---

### Step 3 — Few-Shot Prompt

```
Solve the following math word problems. Show the key formula and the final answer.

Example 1:
Problem: A car travels at 50 km/h for 4 hours. How far does it go?
Solution: Distance = Speed × Time = 50 × 4 = 200 km

Example 2:
Problem: A cyclist rides at 20 km/h for 1.5 hours, then at 30 km/h for 2 hours. What is the total distance?
Solution: Distance = (20 × 1.5) + (30 × 2) = 30 + 60 = 90 km

Now solve:
Problem: A train travels at 60 km/h for 2 hours, then at 80 km/h for 3 hours. What is the total distance traveled?
Solution:
```

**Model Output:**
> Distance = (60 × 2) + (80 × 3) = 120 + 240 = **360 km**

**Assessment:** Format is enforced and consistent. Ready for automated parsing.

---

### Step 4 — Chain-of-Thought Prompt

```
Solve the following math word problem step by step, explaining each step clearly.

Problem: A train travels at 60 km/h for 2 hours, then at 80 km/h for 3 hours. What is the total distance traveled?

Let's solve this step by step:
```

**Model Output:**
> **Step 1: Identify the formula.**
> Distance = Speed × Time
>
> **Step 2: Calculate distance for the first leg.**
> Speed = 60 km/h, Time = 2 hours
> Distance₁ = 60 × 2 = 120 km
>
> **Step 3: Calculate distance for the second leg.**
> Speed = 80 km/h, Time = 3 hours
> Distance₂ = 80 × 3 = 240 km
>
> **Step 4: Add both distances.**
> Total Distance = 120 + 240 = **360 km**
>
> **Answer: The train traveled a total of 360 km.**

**Assessment:** Crystal clear. Each sub-calculation is independently verifiable. Essential for educational tools or high-stakes applications.

---

## Scenario 3 — Text Classification

**Task:** Classify customer support tickets by category (Billing / Technical / General).

---

### Step 1 — Basic Prompt

```
What kind of issue is this? "I was charged twice for my subscription this month."
```

**Model Output:**
> This looks like a billing issue.

**Assessment:** Informal language. No standard label. Inconsistent across runs.

---

### Step 2 — Zero-Shot Prompt

```
Classify the following customer support ticket into one of these categories: Billing, Technical, or General.

Ticket: "I was charged twice for my subscription this month."

Category:
```

**Model Output:**
> Billing

**Assessment:** Correct, standardized label. Suitable for routing systems.

---

### Step 3 — Few-Shot Prompt

```
Classify the following customer support tickets into one of these categories: Billing, Technical, or General.

Examples:
Ticket: "My invoice shows the wrong amount."
Category: Billing

Ticket: "The app crashes every time I try to upload a file."
Category: Technical

Ticket: "Can you tell me what your business hours are?"
Category: General

Ticket: "I was charged twice for my subscription this month."
Category:
```

**Model Output:**
> Billing

**Assessment:** Consistent output. The model has learned the category vocabulary and style from examples.

---

### Step 4 — Chain-of-Thought Prompt

```
Classify the following customer support ticket into one of these categories: Billing, Technical, or General.
Reason through your classification step by step.

Ticket: "I was charged twice for my subscription this month."

Step-by-step reasoning:
```

**Model Output:**
> **Step 1: Read the ticket carefully.**
> The customer says they were "charged twice for my subscription this month."
>
> **Step 2: Identify the key action/complaint.**
> The core complaint is about a financial charge — specifically a duplicate payment.
>
> **Step 3: Match to category definitions.**
> - Billing: Issues related to payments, invoices, charges, refunds → ✅ Matches
> - Technical: Issues with software, hardware, app functionality → ❌ No match
> - General: Questions about services, hours, policies → ❌ No match
>
> **Step 4: Assign category.**
> The ticket is clearly about an incorrect charge.
>
> **Final Category: Billing**

**Assessment:** Provides full audit trail. Ideal for QA systems or training datasets where rationale must be captured.

---

## Scenario 4 — Logical Reasoning

**Task:** Answer a logical deduction problem.

---

### Step 1 — Basic Prompt

```
All roses are flowers. Some flowers fade quickly. Do all roses fade quickly?
```

**Model Output:**
> Not necessarily.

**Assessment:** Correct but with no justification. Unhelpful to a learner.

---

### Step 2 — Zero-Shot Prompt

```
Answer the following logical reasoning question with a clear Yes or No, and briefly explain why.

Premise 1: All roses are flowers.
Premise 2: Some flowers fade quickly.
Question: Do all roses fade quickly?
```

**Model Output:**
> **No.** While all roses are flowers, only *some* flowers fade quickly. This does not mean that roses specifically are among the flowers that fade quickly. The logic does not allow us to conclude that all roses fade quickly.

**Assessment:** Clear and correct. Good for educational use.

---

### Step 3 — Few-Shot Prompt

```
Answer the following logical reasoning questions. State Yes or No and explain the logic.

Example 1:
Premise 1: All dogs are mammals.
Premise 2: Some mammals live in water.
Question: Do all dogs live in water?
Answer: No. While all dogs are mammals, only some mammals live in water. Dogs are not necessarily among those mammals.

Example 2:
Premise 1: All squares are rectangles.
Premise 2: Some rectangles have equal sides.
Question: Do all squares have equal sides?
Answer: Yes. All squares are rectangles by definition, and squares specifically are defined as rectangles with four equal sides, so all squares do have equal sides.

Now answer:
Premise 1: All roses are flowers.
Premise 2: Some flowers fade quickly.
Question: Do all roses fade quickly?
Answer:
```

**Model Output:**
> **No.** Even though all roses are flowers, only some flowers fade quickly. We cannot conclude that roses are among those flowers without additional information. The statement is logically unsupported.

**Assessment:** Consistent format and depth. The second example also teaches the model to handle edge cases where the answer *is* Yes.

---

### Step 4 — Chain-of-Thought Prompt

```
Answer the following logical reasoning question by reasoning through it step by step.

Premise 1: All roses are flowers.
Premise 2: Some flowers fade quickly.
Question: Do all roses fade quickly?

Step-by-step reasoning:
```

**Model Output:**
> **Step 1: Represent the premises using set logic.**
> - Let R = Set of all roses
> - Let F = Set of all flowers
> - Let Q = Set of things that fade quickly
>
> **Step 2: Apply Premise 1.**
> "All roses are flowers" → R ⊆ F (Roses are a subset of Flowers)
>
> **Step 3: Apply Premise 2.**
> "Some flowers fade quickly" → F ∩ Q ≠ ∅ (There is at least one flower that fades quickly, but not necessarily all)
>
> **Step 4: Check if we can conclude R ⊆ Q.**
> We know R ⊆ F and F ∩ Q ≠ ∅.
> However, this does NOT mean R ∩ Q = R (i.e., all roses fade).
> The flowers that fade quickly might not include any roses.
>
> **Step 5: Conclusion.**
> We cannot logically conclude that all roses fade quickly.
>
> **Final Answer: No** — The conclusion does not follow from the premises.

**Assessment:** Exemplary. Uses formal set notation. Demonstrates syllogism failure clearly. Gold standard for logic tutoring or reasoning benchmarks.

---

## Evaluation Method — ROUGE-Style Rubric

> **Selected Method: Qualitative Rubric Scoring (ROUGE-Inspired)**
> Each response is scored across 4 dimensions on a scale of 1–5.

| Dimension | Description |
|---|---|
| **Accuracy** | Is the answer factually/logically correct? |
| **Depth** | How thorough and detailed is the explanation? |
| **Format Compliance** | Does output match the desired structure? |
| **Reasoning Transparency** | Is the reasoning auditable and explainable? |

### Scoring Results Across All Scenarios

| Scenario | Prompt Type | Accuracy | Depth | Format | Reasoning | **Total /20** |
|---|---|:---:|:---:|:---:|:---:|:---:|
| Sentiment Analysis | Basic | 3 | 1 | 1 | 1 | **6** |
| Sentiment Analysis | Zero-Shot | 5 | 2 | 4 | 1 | **12** |
| Sentiment Analysis | Few-Shot | 5 | 3 | 5 | 2 | **15** |
| Sentiment Analysis | Chain-of-Thought | 5 | 5 | 5 | 5 | **20** |
| Math Word Problem | Basic | 4 | 1 | 2 | 1 | **8** |
| Math Word Problem | Zero-Shot | 5 | 3 | 4 | 2 | **14** |
| Math Word Problem | Few-Shot | 5 | 3 | 5 | 2 | **15** |
| Math Word Problem | Chain-of-Thought | 5 | 5 | 5 | 5 | **20** |
| Text Classification | Basic | 3 | 1 | 1 | 1 | **6** |
| Text Classification | Zero-Shot | 5 | 2 | 5 | 1 | **13** |
| Text Classification | Few-Shot | 5 | 3 | 5 | 2 | **15** |
| Text Classification | Chain-of-Thought | 5 | 5 | 5 | 5 | **20** |
| Logical Reasoning | Basic | 4 | 1 | 1 | 1 | **7** |
| Logical Reasoning | Zero-Shot | 5 | 3 | 4 | 2 | **14** |
| Logical Reasoning | Few-Shot | 5 | 4 | 5 | 3 | **17** |
| Logical Reasoning | Chain-of-Thought | 5 | 5 | 5 | 5 | **20** |

---

## Master Comparison Table

| Feature | Basic Prompt | Zero-Shot | Few-Shot | Chain-of-Thought |
|---|:---:|:---:|:---:|:---:|
| **Requires Examples** | ❌ | ❌ | ✅ | Optional |
| **Requires Reasoning Instruction** | ❌ | ❌ | ❌ | ✅ |
| **Output Consistency** | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Accuracy (Simple Tasks)** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Accuracy (Complex Tasks)** | ⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Reasoning Transparency** | ❌ | ❌ | ⚠️ Partial | ✅ Full |
| **Token Cost** | 🟢 Lowest | 🟢 Low | 🟡 Medium | 🔴 Highest |
| **Prompt Engineering Effort** | 🟢 None | 🟡 Low | 🟡 Medium | 🔴 High |
| **Best For** | Quick experiments | Standard tasks | Classification/NLP | Math, Logic, Reasoning |
| **Fails At** | Everything nuanced | Multi-step reasoning | Novel/unseen formats | Token-limited contexts |
| **Use In Production?** | ❌ Rarely | ✅ Simple pipelines | ✅ NLP pipelines | ✅ High-stakes tasks |

---

## Key Takeaways

```
┌─────────────────────────────────────────────────────────────────────┐
│                    PROMPTING PATTERN SUMMARY                        │
├─────────────────────┬───────────────────────────────────────────────┤
│ Basic Prompt        │ Use only for quick, informal experiments.      │
│                     │ Never rely on it for accuracy-critical tasks.  │
├─────────────────────┼───────────────────────────────────────────────┤
│ Zero-Shot           │ The baseline for production. Works well when   │
│                     │ the task is common and the model is capable.   │
├─────────────────────┼───────────────────────────────────────────────┤
│ Few-Shot            │ Best for enforcing output format, style, and   │
│                     │ consistency. Preferred for NLP classification. │
├─────────────────────┼───────────────────────────────────────────────┤
│ Chain-of-Thought    │ Gold standard for reasoning-heavy tasks. Use   │
│                     │ when accuracy and explainability both matter.  │
└─────────────────────┴───────────────────────────────────────────────┘
```

### Progression Rule

> **As task complexity increases → move from Basic → Zero-Shot → Few-Shot → Chain-of-Thought**

### When to Use Each

- **Basic** → Internal prototyping, throwaway tests
- **Zero-Shot** → Well-defined, single-label tasks with standardized outputs
- **Few-Shot** → Multi-class classification, format-sensitive generation, domain-specific tasks
- **Chain-of-Thought** → Math, logic, medical/legal reasoning, any task requiring auditability

---

<div align="center">

# Result
hence all types of prompts from zero shot to chain of thought were researchrd through and verified.
