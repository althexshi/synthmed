# SynthMed Technical Interview Questions

## Context
These questions are designed to assess technical depth, problem-solving approach, and understanding of real-world tradeoffs in building an AI-powered medical research synthesis system using RAG, multi-agent architectures, and LLMs.

---

## Question 1: Multi-Agent Architecture Design

**Question:** You chose a hierarchical multi-agent architecture with a main orchestrator and disease-specific sub-agents, rather than a single monolithic agent with all medical knowledge. Walk me through your reasoning for this design decision. What were the key tradeoffs, and in what scenarios might a monolithic approach have been better?

**What we're looking for:**
- Understanding of agent architecture patterns
- Ability to articulate tradeoffs (complexity vs. modularity, latency vs. accuracy)
- Consideration of maintenance, scalability, and team collaboration
- Discussion of when simpler might be better

---

## Question 2: RAG Chunking Strategy

**Question:** Your current PDF chunking strategy uses fixed character counts (1000 chars, 200 overlap). Medical papers have complex structures—abstracts, methods, results, tables, citations. Walk me through how you would improve this chunking strategy. What problems does naive chunking cause, and what would you do differently?

**What we're looking for:**
- Understanding of semantic chunking vs. fixed-size chunking
- Knowledge of document structure and metadata preservation
- Discussion of specific medical paper challenges (tables, figures, multi-column)
- Awareness of retrieval quality impact
- Concrete technical approaches (sentence splitting, section-aware chunking, metadata)

---

## Question 3: Cross-Domain Synthesis Challenge

**Question:** One of the starter prompts is "How do inflammatory pathways differ between cancer and neurodegenerative disorders?" This requires synthesizing across multiple disease domains. Currently, the system concatenates all passages and makes a single LLM call. What problems could this cause at scale, and how would you architect a more robust cross-domain synthesis pipeline?

**What we're looking for:**
- Recognition of context window limitations
- Understanding of map-reduce patterns for hierarchical synthesis
- Discussion of parallel processing and latency optimization
- Consideration of attribution and provenance tracking
- Thoughtful approach to relevance scoring across domains

---

## Question 4: Prompt Engineering for Consistency

**Question:** The main agent has strict formatting instructions to ensure consistent output (Executive Summary, Synthesis, Disease Domain table, References, Note). Despite this, LLMs can be unpredictable. Describe the challenges with prompt-based formatting and what strategies you'd use to ensure reliable, structured outputs at scale.

**What we're looking for:**
- Experience with prompt brittleness and model non-compliance
- Knowledge of structured outputs (JSON mode, function calling, Pydantic schemas)
- Discussion of validation and retry strategies
- Understanding of few-shot examples and chain-of-thought prompting
- Pragmatic balance between flexibility and reliability

---

## Question 5: Citation Verification and Hallucination

**Question:** Your synthesis includes citations like [Source 1], [Source 2]. However, LLMs can hallucinate or misattribute information. How would you build a citation verification system to ensure that every claim in the synthesis is actually supported by the retrieved passages? Walk through your approach from problem definition to implementation.

**What we're looking for:**
- Recognition of hallucination as a critical issue in medical AI
- Technical approaches (semantic similarity, NLI models, fact-checking)
- Discussion of precision vs. recall tradeoffs
- Consideration of performance impact
- Understanding of when to reject vs. flag suspicious content

---

## Question 6: Evaluation Framework

**Question:** How do you know if your system is working well? If I asked you to build a comprehensive evaluation framework for SynthMed, what metrics would you track, and how would you collect ground truth data for a medical research synthesis task?

**What we're looking for:**
- Understanding of RAG evaluation (retrieval quality, generation quality, end-to-end)
- Knowledge of specific metrics (RAGAS, faithfulness, answer relevance, citation accuracy)
- Discussion of human-in-the-loop evaluation
- Practical approaches to ground truth creation (expert panels, inter-rater reliability)
- Consideration of both online and offline metrics
- Understanding of edge cases and adversarial testing

---

## Question 7: Production Scalability and Cost

**Question:** You're using the Llama 3.2 90B Vision model. In production with 1,000 concurrent researchers, each making 10 queries per day with 5-10 page syntheses, this could get expensive and slow. Walk me through how you'd optimize this system for cost and latency without significantly sacrificing quality.

**What we're looking for:**
- Understanding of LLM economics (tokens, pricing, throughput)
- Caching strategies (query deduplication, semantic caching)
- Discussion of model selection (when to use smaller models)
- Streaming responses for better UX
- Async processing and batching
- Consideration of using fine-tuned smaller models
- Smart retrieval (reducing context size)

---

## Question 8: Handling Medical PDF Complexity

**Question:** Medical research papers often have critical information in tables, figures, and supplementary materials. Your current implementation does basic text extraction. Describe the challenges with medical PDFs and how you'd build a more sophisticated document processing pipeline that preserves this structured information.

**What we're looking for:**
- Awareness of PDF complexity (multi-column, tables, figures, OCR issues)
- Knowledge of specialized tools (LlamaParse, Unstructured, Camelot)
- Discussion of table extraction and structuring
- Understanding of multi-modal embeddings for figures
- Consideration of metadata extraction (study type, sample size, p-values)
- Practical tradeoffs between accuracy and processing time

---

## Question 9: Knowledge Base Freshness and Governance

**Question:** Medical research evolves rapidly—new studies are published daily, and occasionally papers are retracted. Your current knowledge base is static YAML files with local PDFs. How would you design an automated pipeline to keep the knowledge base current while maintaining quality and governance standards?

**What we're looking for:**
- Understanding of data pipeline architecture
- Discussion of automated ingestion (PubMed RSS, arXiv alerts)
- Quality filtering (peer review status, journal impact factor, preprints vs. published)
- Handling retractions and corrections
- Versioning and audit trails
- Incremental indexing vs. full rebuilds
- Consideration of regulatory requirements (FDA, HIPAA)

---

## Question 10: Safety and Responsible AI

**Question:** This system is designed for medical research synthesis with explicit guardrails against giving clinical advice. However, researchers might still use these syntheses to inform patient care decisions. Walk me through how you'd think about the ethical implications, safety measures, and responsible AI practices you'd implement. Where do you draw the line between being helpful and being safe?

**What we're looking for:**
- Deep understanding of healthcare AI ethics
- Discussion of model limitations and uncertainty quantification
- Concrete safety mechanisms (content filtering, disclaimers, human-in-the-loop)
- Understanding of regulatory landscape (FDA, medical device classification)
- Thoughtful discussion of edge cases (urgent queries, conflicting evidence)
- Consideration of bias in medical literature
- Reflection on when AI should defer to human experts
- Understanding that perfect safety is impossible—how to manage risk

---

## Interviewer Notes

### How to Use These Questions

1. **Don't expect perfect answers**: The goal is to understand thinking process, not to get textbook solutions
2. **Ask follow-ups**: "Why did you make that choice?" "What would break in that approach?" "What would you do differently?"
3. **Listen for struggles**: Candidates who discuss dead-ends and pivots often have deeper experience
4. **Watch for curiosity**: Do they ask clarifying questions? Do they want to know more about the problem space?
5. **Assess self-awareness**: Do they acknowledge what they don't know? Do they reflect on past mistakes?

### Red Flags
- Jumping to solutions without understanding constraints
- No discussion of tradeoffs
- Claiming everything is easy/solved
- No consideration of failure modes
- Unable to explain reasoning

### Green Flags
- Asking about requirements, constraints, scale
- Discussing multiple approaches with pros/cons
- Mentioning real-world experience with similar problems
- Acknowledging complexity and uncertainty
- Showing curiosity about the medical domain
- Discussing both technical and operational concerns
