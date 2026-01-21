# Presentation: Precedent - Institutional Decision Memory

## Slide 1: Title Slide
**Title:** Precedent
**Subtitle:** Never Solve the Same Problem Twice.
**Presenter:** [Your Name]

---

## Slide 2: The Problem
**Header:** Corporate Amnesia
*   **The Issue:** Companies make thousands of decisions (Why AWS? Why Postgres? Why Remote?), but the *context* is lost in forgotten meeting transcripts or Slack threads.
*   **The Consequence:** New employees re-debate settled topics. Teams repeat expensive mistakes (e.g., trying Azure again when it failed last year).
*   **The Gap:** Standard search (Ctrl+F) only finds keywords, not *reasoning*.

---

## Slide 3: The Solution
**Header:** Precedent - An AI Decision Engine
*   **Concept:** A system that reads your unstructured meetings/docs and builds a structured "Memory" of key decisions.
*   **Core Capability:** You don't search for filenames. You search for *questions*.
    *   *Input:* "Why did we choose AWS?"
    *   *Output:* "We rejected Azure because of a steep learning curve and $200k hardware costs."

---

## Slide 4: How It Works (Architecture)
*(Display the Architecture Diagram here)*
1.  **Ingest:** Upload raw PDFs/Text.
2.  **Extract:** We use Groq (Llama 3) to strictly extract the "Rationale" and "Alternatives Considered".
3.  **Vectorize:** We turn that reasoning into math (Embeddings) using a local model.
4.  **Retrieve:** When you search, we find the *semantic match*, not just keyword matches.

---

## Slide 5: Live Demo
**Context:** "Let's pretend I'm a new CTO joining the company."
1.  **Scenario:** I see a huge bill for AWS. I wonder, "Why are we paying this? Should we go On-Prem?"
2.  **Action:** I type "Why use AWS?" into Precedent.
3.  **Result:**
    *   It instantly pulls up the record from `2023_Cloud_Migration_Meeting`.
    *   It tells me: "On-Prem would have cost $200k upfront."
    *   It tells me: "Azure was rejected due to team training time."
4.  **Conclusion:** I assume the context immediately. No meeting required.

---

## Slide 6: Key Features
*   **Extremely Detailed Extraction:** Captures financial metrics ($200k), dates (Q3 Launch), and trade-offs.
*   **Hybrid Search:** Uses Vector Search (Qdrant) to understand intent.
*   **Privacy First:** Embeddings runs locally (`all-MiniLM-L6-v2`); only extraction uses the API.

---

## Slide 7: Future Roadmap
*   **Slack Integration:** Auto-ingest decisions from specific channels.
*   **Conflict Detection:** Warn users if a new decision contradicts an old one.
*   **Timeline View:** Visual history of our tech stack evolution.
