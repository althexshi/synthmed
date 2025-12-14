# Technical Interview Questions - SynthMed Project

## Context
These questions are based on your IBM AI Lab experience building SynthMed, a multi-agent RAG system for medical research synthesis. They're designed to explore your technical decisions, problem-solving approach, and learnings from the project.

---

## Question 1: Multi-Agent RAG Architecture

**Question:** You built a multi-agent RAG system integrating IBM Watsonx Orchestrate, Granite, and Meta-Llama 3.2 90B across 5 medical domains. Walk me through your decision-making process for choosing this multi-agent architecture. What other approaches did you consider, and what were the key tradeoffs that led you to this design?

**Follow-ups:**
- How did you decide which model to use for which task (Granite vs. Llama)?
- What problems did the multi-agent approach solve that a single agent wouldn't?
- If you had to rebuild this with a different tech stack (like LangChain or LlamaIndex), what would change?

---

## Question 2: Measuring the 70% Time Reduction

**Question:** You mentioned the system reduced literature review time by 70%. How did you measure this? Walk me through your methodology for establishing the baseline, measuring improvement, and validating that researchers actually achieved this speedup in practice.

**Follow-ups:**
- What were you comparing against (manual review, existing tools)?
- What didn't get captured in that 70% metric?
- How did you account for quality vs. speed tradeoffs?
- Were there any cases where the system was actually slower?

---

## Question 3: Processing 82 MB of Medical Literature

**Question:** You processed 82 MB of medical literature across 27 papers. That's a substantial amount of data. Tell me about the biggest technical challenge you faced in processing these medical PDFs. What didn't work initially, and how did you solve it?

**Follow-ups:**
- How did you handle different PDF formats and quality issues?
- What was your chunking strategy and why?
- Did you run into context window limitations?
- How did you validate that extraction was accurate?

---

## Question 4: Decorator Pattern in Python Tools

**Question:** You mentioned using the Decorator pattern for your 3 Python tools (PDF retrieval, PubMed Search, LLM synthesis). Explain why you chose the Decorator pattern specifically. What problem was it solving, and how did it make your code better compared to other design patterns you could have used?

**Follow-ups:**
- Can you give a specific example of how you applied it?
- What would the code look like without the decorator pattern?
- Were there downsides to this approach?
- How did it help with testing and maintenance?

---

## Question 5: Docker and Deployment Workflows

**Question:** You deployed backend workflows through IBM Orchestrate ADK and Docker with automated validation and monitoring. Walk me through what "automated validation" meant in practice. What were you validating, how did you implement it, and what kinds of failures did you catch before they hit users?

**Follow-ups:**
- How did you structure your Docker containers?
- What monitoring did you set up and why?
- Tell me about a time when your validation caught a real problem
- How did you handle deployment rollbacks or versioning?

---

## Question 6: Citation Tracking Implementation

**Question:** You implemented automated citation tracking across 5 medical domains. This sounds straightforward, but in practice, tracking which claims came from which sources is challenging with LLMs. Describe the technical approach you used to ensure citations were accurate and how you validated that the system wasn't hallucinating references.

**Follow-ups:**
- How did you link generated text back to source passages?
- What happened when the LLM generated conflicting citations?
- How did you handle cases where multiple papers said similar things?
- What was your false positive/negative rate?

---

## Question 7: Evidence-Level Validation

**Question:** You mentioned "evidence-level validation" across medical domains. In medical research, evidence quality varies widely (RCTs vs. case studies, peer-reviewed vs. preprints). How did you approach assigning and validating evidence levels? What framework did you use, and what challenges did you encounter?

**Follow-ups:**
- How did you extract evidence levels from papers?
- What happened when papers didn't clearly state their study design?
- How did you prevent the system from treating all sources equally?
- Did you ever disagree with how the system classified evidence?

---

## Question 8: IBM Design Thinking Application

**Question:** You applied IBM Design Thinking methodologies (Empathy Map, As-Is/To-Be Scenarios, Prototyping). Pick one of these—let's say the Empathy Map or As-Is/To-Be Scenarios—and walk me through how you actually used it. What insights did it reveal that surprised you, and how did it change what you built?

**Follow-ups:**
- Give a specific example of a user need you discovered through this process
- Was there something you thought was important that users didn't care about?
- How did you balance user feedback with technical constraints?
- What would you do differently in the design thinking process next time?

---

## Question 9: Integration Challenges with IBM Watsonx

**Question:** You integrated multiple components: IBM Watsonx Orchestrate, Granite model, Llama 3.2 90B, and external tools like PubMed. What was the hardest integration challenge you faced? Describe a specific technical problem where things weren't working, what debugging process you used, and how you ultimately solved it.

**Follow-ups:**
- How did you handle API rate limits or failures?
- What was your strategy for testing integrations?
- Tell me about a time when you had to work around a platform limitation
- How did you manage different data formats between systems?

---

## Question 10: Reflection and What You'd Do Differently

**Question:** Now that you've completed the 8-week program and built SynthMed, reflect on the project. If you were starting over tomorrow with the same goal but no constraints, what would you do differently? What did you learn that would change your approach?

**Follow-ups:**
- What technical decision do you wish you could revisit?
- What took longer than expected and why?
- What did you over-engineer or under-engineer?
- If you had 8 more weeks, what would be your top 3 priorities?
- What was the most valuable lesson from this project that you'll carry forward?

---

## Interviewer Notes

### How to Use These Questions

These questions are **experience-based** - they're asking about actual work the candidate did. The goal is to:

1. **Verify depth of involvement**: Did they actually do this work or just observe it?
2. **Understand problem-solving process**: How do they approach challenges?
3. **Assess technical judgment**: Can they explain tradeoffs and decisions?
4. **Evaluate growth mindset**: What did they learn? What would they improve?

### Strong Signals to Listen For

Based on the job description criteria:

✅ **Clear explanation of goals, tradeoffs, and reasoning**
- "We chose X because Y, but we knew we'd sacrifice Z"
- "The alternative was A, but that would have meant B"

✅ **Curiosity and thoughtful problem-solving**
- Asking clarifying questions about the interviewer's question
- Discussing multiple approaches they explored
- Showing genuine interest in the problem domain

✅ **Describing the struggle, not just the final answer**
- "Initially we tried X and it failed because..."
- "We spent two days debugging Y before realizing..."
- Honest about dead ends and pivots

✅ **Persistence and resourcefulness**
- "When the documentation wasn't clear, I..."
- "We hit a blocker with X, so we tried Y, then Z..."
- Examples of creative workarounds or learning new tools

✅ **Reflection on what to approach differently**
- "If I could do it again, I'd..."
- "I learned that X doesn't work well for Y because..."
- Shows growth and self-awareness

### Red Flags

❌ Surface-level understanding ("I just used the library")
❌ No discussion of tradeoffs or alternatives
❌ Claiming everything worked perfectly
❌ Unable to explain technical decisions
❌ Blaming others for problems
❌ No acknowledgment of what they'd improve

### Green Flags

✅ Specific technical details (not just buzzwords)
✅ Acknowledges what they didn't know and had to learn
✅ Discusses failure modes and edge cases
✅ Shows ownership of outcomes (good and bad)
✅ Connects technical decisions to user impact
✅ Asks insightful follow-up questions

### Interview Flow Suggestions

1. **Start with Q1 or Q3** (architecture/technical) to get them comfortable
2. **Ask Q2 or Q7** (metrics/validation) to probe analytical thinking
3. **Use Q9** (integration challenges) to understand debugging skills
4. **End with Q10** (reflection) to assess growth mindset

Don't ask all 10 questions - pick 3-5 based on:
- The role's focus areas (backend? ML? full-stack?)
- How the conversation flows
- What you want to dig deeper on

### Question-Specific Guidance

**Q2 (70% metric):** This tests analytical rigor. Watch for hand-waving vs. concrete measurement methodology.

**Q4 (Decorator pattern):** Tests if they understand design patterns or just use them because they're "supposed to."

**Q6 (Citations):** This is a hard problem. Good answers acknowledge the difficulty and discuss imperfect solutions.

**Q8 (Design Thinking):** Tests if they actually engaged with the process or just checked boxes.

**Q10 (Reflection):** The most important question. Reveals maturity, self-awareness, and learning capacity.

---

## Sample Strong vs. Weak Answers

### Q9: Integration Challenges (Strong Answer)

> "The hardest challenge was integrating PubMed API with our RAG pipeline. Initially, we were making synchronous calls during the synthesis step, which added 3-5 seconds of latency per query. We tried caching, but medical research updates frequently, so stale data was a problem.
>
> We ultimately moved to a hybrid approach: pre-populate the vector DB with recent papers from our domains, then only query PubMed for very recent work (last 30 days). We also added async processing so PubMed calls don't block the main synthesis.
>
> The debugging process involved adding detailed logging at each step, using Postman to isolate the PubMed API behavior, and eventually building a mock PubMed service for testing. If I did it again, I'd start with the async approach from day one and set up better monitoring earlier."

**Why it's strong:**
- Specific problem with concrete details (3-5 second latency)
- Describes the evolution of thinking (tried X, then Y)
- Acknowledges tradeoffs (freshness vs. speed)
- Mentions debugging tools and process
- Reflects on what to do differently

### Q9: Integration Challenges (Weak Answer)

> "Integration was pretty smooth. We just used the APIs and followed the documentation. Sometimes we had rate limits but we just added delays. The hardest part was probably just learning the IBM platform, but once we figured it out, everything worked fine."

**Why it's weak:**
- No specific technical details
- No struggle or learning process described
- Claims everything was easy
- No discussion of tradeoffs or alternatives
- No reflection or lessons learned

---

## Additional Deep-Dive Questions (If Time Permits)

If the conversation is going well and you want to probe specific areas:

**Technical Depth:**
- "How did you decide on your chunk size and overlap for the medical papers?"
- "Walk me through how a query flows through your system from user input to final response."
- "What's the difference between how you use Granite vs. Llama 3.2 90B?"

**Problem-Solving:**
- "Tell me about a bug that took you the longest to fix. What made it hard?"
- "When two papers contradicted each other, how did your system handle that?"

**Collaboration & Learning:**
- "How did you divide work across your team?"
- "What was something you had to teach yourself during this project?"
- "How did you get feedback on whether the system was actually helpful?"

**Impact & Product Thinking:**
- "Who was your target user and how did you validate they needed this?"
- "What feature requests did you get that you chose not to build, and why?"
- "How would this scale to 100x more papers or 1000x more users?"
