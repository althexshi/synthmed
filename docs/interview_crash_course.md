# GoFundMe Technical Interview - One Day Crash Course

## Timeline: 8 Hours of Focused Prep

---

## Hour 1: Architecture & Core Decisions (8:00-9:00 AM)

### Task: Create Your Mental Model

**Write out answers to these 5 questions:**

1. **Draw your system architecture** (even if rough)
   - IBM Orchestrate → Agents → Tools → Models
   - Where does data flow?
   - Where are the bottlenecks?

2. **Why RAG instead of fine-tuning?**
   - Cost? Speed? Flexibility? Medical data sensitivity?
   - Be ready to defend this

3. **Why 3 separate tools (PDF, PubMed, LLM)?**
   - Why not one monolithic tool?
   - What does separation buy you?

4. **Why Decorator pattern specifically?**
   - What flexibility does it provide?
   - Alternative patterns considered?

5. **How did you measure "70% reduction"?**
   - Baseline? Methodology? User studies?

**Output: 1-page document you can review tomorrow morning**

---

## Hour 2: Prepare 3 Core Stories (9:00-10:00 AM)

### Story 1: The Biggest Technical Challenge

**Template:**
- **What broke:** "The RAG system was returning irrelevant chunks from medical papers..."
- **How you discovered it:** "I noticed when querying about autism genetics, I'd get cancer research results..."
- **What you tried:** 
  1. First attempt: Increased chunk overlap → didn't help, made it slower
  2. Second attempt: Tuned similarity threshold → better but still issues
  3. Final solution: Implemented hybrid search with keyword + semantic → 80% better relevance
- **Tradeoff:** "Added complexity and 200ms latency, but accuracy was crucial for medical data"
- **What you learned:** "Semantic search alone isn't enough for domain-specific content"
- **What you'd do differently:** "I'd implement evaluation metrics from day 1 instead of relying on manual testing"

### Story 2: A Decision You Changed Course On

**Template:**
- **Initial approach:** "I started with a single agent handling everything..."
- **Why it seemed good:** "Simpler architecture, easier to debug..."
- **When you pivoted:** "After 3 days, I realized the agent was trying to do too much..."
- **New approach:** "Split into specialized agents: retrieval, synthesis, validation..."
- **Why it was better:** "Each agent could be optimized and monitored independently..."
- **Cost of change:** "Took 2 days to refactor, but paid off in maintainability..."

### Story 3: Something You Learned Mid-Project

**Template:**
- **What you didn't know:** "I hadn't worked with Watsonx Orchestrate before..."
- **How you learned:** "Spent 2 days reading docs, built 3 toy examples..."
- **What surprised you:** "The ADK had strong opinions about agent structure..."
- **How it changed your approach:** "Had to redesign my agent interfaces to match their patterns..."
- **What you'd research earlier:** "I'd prototype with the orchestration framework first..."

**Output: Write these 3 stories in detail (1 page each)**

---

## Hour 3: Technical Deep Dive Prep (10:00-11:00 AM)

### For Each Component, Know:
1. **What it does**
2. **Why you chose it**
3. **What could go wrong**
4. **How you'd scale it**

### RAG System Quick Reference

**What:**
- Embedding medical papers → vector store → retrieve relevant chunks → send to LLM

**Why this approach:**
- Can't fine-tune large models (cost, expertise)
- Need up-to-date research (can add new papers)
- Medical accuracy requires source citations

**Problems:**
- Chunk boundaries can split important context
- Similarity search can miss keyword-specific matches
- Cold start problem (initial indexing time)

**Scaling concerns:**
- 27 papers → OK in memory
- 2700 papers → Need vector DB (Pinecone, Weaviate)
- Embedding cost grows linearly
- Query latency grows logarithmically

### Docker Deployment Quick Reference

**What:**
- Containerized agents + tools + dependencies
- Automated validation and monitoring

**Why:**
- Environment consistency (local → production)
- Dependency isolation
- Easy rollback

**Problems:**
- Image size (Python + ML models = big)
- Network between containers
- Secret management

**Scaling concerns:**
- Orchestration (Kubernetes?)
- Log aggregation
- Resource limits per container

### Multi-Model Integration Quick Reference

**What:**
- IBM Watsonx Orchestrate coordinates
- Granite + Meta-Llama 3.2 90B for different tasks

**Why multiple models:**
- Cost/speed tradeoffs (small model for X, large for Y)
- Redundancy/fallback
- Best model for specific tasks

**Problems:**
- Different APIs/formats
- Latency unpredictable
- Error handling across models

**Tradeoffs:**
- Complexity vs. flexibility
- Cost vs. quality
- Latency vs. accuracy

---

## Hour 4: Practice Articulating Tradeoffs (11:00-12:00 PM)

### The Formula: "I chose X over Y because Z, which meant accepting A but gaining B"

**Practice these 10 tradeoffs:**

1. **PyMuPDF vs. other PDF libraries**
   - "I chose PyMuPDF over pdfplumber because it's faster (10x) and handles complex layouts better. The tradeoff is it's less Pythonic and has a steeper learning curve, but for 82MB of medical papers, speed was critical."

2. **Decorator pattern vs. simple functions**
   - "I used decorators for cross-cutting concerns like logging, timing, and error handling. This added abstraction complexity but gave me consistent monitoring across all tools without duplicating code."

3. **In-memory vs. database vector storage**
   - "For 27 papers, I kept vectors in memory for sub-100ms retrieval. I accepted the limitation of server restarts losing state, but gained simplicity. At 1000+ papers, I'd migrate to a proper vector DB."

4. **Pre-processing all PDFs vs. lazy loading**
   - "I chose to preprocess and embed all papers at startup. This meant 5-minute cold start but instant query responses. For users, query speed > startup time."

5. **Synchronous vs. asynchronous tool calls**
   - "Initially synchronous for simplicity. Hit timeouts on PubMed searches. Migrated to async which added complexity (error handling, race conditions) but cut query time 60%."

6. **Generic agents vs. specialized agents**
   - "Started generic, refactored to specialized (autism agent, cancer agent). More code to maintain but each could have domain-specific prompts and retrieval strategies."

7. **Exact match vs. fuzzy search for medical terms**
   - "Medical terms need precision ('ASD' vs. 'ADHD'). Used exact matching with synonym expansion. Missed some results (lower recall) but avoided dangerous mismatches (high precision)."

8. **Caching LLM responses vs. fresh every time**
   - "Cached synthesis results keyed by query + paper IDs. Saved cost but risked stale data. Added TTL and cache invalidation. Reduced API costs 40%."

9. **Automated testing vs. manual validation**
   - "Medical accuracy is critical. Built automated tests for parsing and retrieval, but manual validation for synthesis quality. Balanced speed of development with safety."

10. **Monolithic repo vs. microservices**
   - "Single repo for agents + tools + knowledge bases. Easier to develop and debug. Tradeoff: harder to scale independently and deploy incrementally. Right choice for MVP, would split for production."

**Action: Pick 5 of these and practice saying them out loud**

---

## Lunch Break (12:00-1:00 PM)
- **Review your 3 stories while eating**
- **Practice the architecture explanation out loud**

---

## Hour 5: Curiosity & Learning Examples (1:00-2:00 PM)

### Show You're Not Just a Code Monkey

**Prepare 5 "I wondered..." examples:**

1. **"I wondered why RAG was becoming the standard..."**
   - "So I read 10 papers on RAG architectures, compared to fine-tuning approaches, and realized the key is flexibility with domain-specific data that changes frequently."

2. **"I wondered if our chunk size was optimal..."**
   - "Ran experiments with 256, 512, 1024 token chunks. Found 512 was the sweet spot for medical papers - captured complete thoughts without too much noise."

3. **"I wondered how production RAG systems handle this..."**
   - "Looked at how LangChain and LlamaIndex solve chunking. Adopted their recursive text splitter approach which respects document structure."

4. **"I wondered about the cost implications..."**
   - "Calculated that at 10k queries/month, embedding costs would be $X and LLM costs $Y. Made me think about caching strategies earlier."

5. **"I wondered if there were better orchestration patterns..."**
   - "Researched agent architectures: ReAct, Plan-and-Execute, Reflexion. Found IBM's approach similar to ReAct but with more structured tool calling."

**The pattern: Curiosity → Research → Applied Learning → Better Decision**

---

## Hour 6: Reflection & "What I'd Do Differently" (2:00-3:00 PM)

### The "Learning Mindset" Questions

**Template: "If I were to start over tomorrow, I would..."**

1. **"...set up evaluation metrics on day 1"**
   - "I built the system before figuring out how to measure quality. Spent week 3 retrofitting metrics. Should have defined success criteria first."

2. **"...prototype with smaller models locally"**
   - "I developed against the 90B model which was slow and expensive for iteration. Could have used a smaller model for development, then scaled up."

3. **"...invest in better observability earlier"**
   - "Added monitoring after deployment. Couldn't debug production issues easily. Should have instrumented from the start."

4. **"...write more integration tests"**
   - "Heavy unit tests, light integration tests. Missed issues where tools interacted poorly with the orchestrator."

5. **"...document tradeoffs as I made them"**
   - "Made decisions quickly, didn't write them down. Hard to remember why I chose X over Y months later. Now I keep a decision log."

6. **"...spend more time on data quality"**
   - "Assumed PDFs would be clean. Spent tons of time handling edge cases. Should have analyzed the data quality first."

7. **"...pair program on the architecture"**
   - "Designed in isolation. A second pair of eyes would have caught some design issues earlier."

**Action: Pick 3 and practice explaining WHY each matters**

---

## Hour 7: Mock Interview Questions (3:00-4:00 PM)

### Practice Answering These Out Loud

**Set a 5-minute timer for each. Don't write, just talk.**

1. **"Tell me about the most complex technical project you've worked on."**
   - Lead with SynthMed RAG system
   - Hit: Multi-agent, 27 papers, 3 models, 70% improvement
   - Go into one struggle story

2. **"Describe a time you had to make a difficult technical tradeoff."**
   - Use one from Hour 4
   - Be specific about the decision factors
   - Mention what you'd do at different scales

3. **"Tell me about a time something didn't work as expected."**
   - Use Story 1 from Hour 2
   - Focus on the debugging process
   - Show persistence and learning

4. **"How would you scale your system to 100x the load?"**
   - 27 papers → 2,700 papers
   - Vector DB (Pinecone/Weaviate)
   - Caching layer
   - Async processing
   - Load balancing
   - Cost implications

5. **"What's something you learned recently that changed how you approach problems?"**
   - Use something from Hour 5
   - Show continuous learning
   - Connect to business value

6. **"Tell me about a technical decision you'd make differently now."**
   - Use from Hour 6
   - Show reflection
   - Show growth mindset

7. **"How do you ensure reliability in AI systems?"**
   - Validation pipelines
   - Monitoring (latency, accuracy, cost)
   - Fallback strategies
   - Human-in-the-loop for medical data
   - Automated testing

8. **"Explain your system architecture to me."**
   - Start high level
   - Go deeper based on their questions
   - Use your Hour 1 diagram mentally
   - Focus on data flow

**Action: Record yourself answering 3 of these, listen back**

---

## Hour 8: Final Prep & Cheat Sheet (4:00-5:00 PM)

### Create Your One-Page Cheat Sheet

**On paper or a doc you can glance at pre-interview:**

```
ARCHITECTURE (30 seconds):
IBM Orchestrate → Agents (autism, cancer, etc.) → Tools (PDF, PubMed, LLM) → Models (Granite, Llama 90B)
RAG: 27 papers → embeddings → vector search → LLM synthesis

KEY NUMBERS:
- 3 tools (PDF retriever, PubMed search, LLM synthesizer)
- 82 MB medical literature
- 27 papers across 5 domains
- 70% reduction in lit review time
- 3 AI models integrated

TOP 3 STORIES:
1. [Challenge] - RAG relevance issue → hybrid search solution
2. [Pivot] - Single agent → multi-agent refactor
3. [Learning] - Watsonx Orchestrate learning curve

FAVORITE TRADEOFFS:
1. PyMuPDF speed vs. complexity
2. In-memory vs. DB vectors (scale threshold)
3. Generic vs. specialized agents

CURIOSITY EXAMPLES:
1. Researched RAG architectures
2. Experimented with chunk sizes
3. Calculated cost implications

WHAT I'D CHANGE:
1. Metrics from day 1
2. Better observability earlier
3. Data quality analysis upfront

QUESTIONS TO ASK THEM:
1. How does GoFundMe think about ML/AI reliability?
2. What's your approach to technical tradeoffs?
3. How do you balance innovation vs. stability?
```

### Pre-Interview Ritual (Tomorrow Morning)

**30 minutes before:**
1. Review your cheat sheet (5 min)
2. Read your 3 stories (5 min)
3. Practice your 30-second system explanation out loud (5 min)
4. Take 3 deep breaths (5 min)
5. Get water, adjust lighting/audio (5 min)
6. Remember: They want to see HOW you think, not what you know

---

## Key Phrases to Use

**Show reasoning:**
- "I considered X and Y, and chose X because..."
- "The tradeoff was between A and B..."
- "At our scale, X made sense, but at 100x scale, I'd use Y..."

**Show curiosity:**
- "That made me wonder about..."
- "I researched how companies like..."
- "I experimented with..."

**Show persistence:**
- "When that didn't work, I tried..."
- "After debugging for X hours, I realized..."
- "I tested three approaches..."

**Show reflection:**
- "Looking back, I would have..."
- "If I were starting over, I'd..."
- "I learned that..."

**Show humility:**
- "I didn't know X, so I..."
- "I initially got Y wrong because..."
- "That taught me Z..."

---

## Red Flags to Avoid

❌ "It was easy"
✅ "It took 3 attempts to get right"

❌ "I built everything perfectly"
✅ "I made mistakes in X and learned Y"

❌ "I knew exactly what to do"
✅ "I researched 5 approaches and experimented"

❌ Just describing features
✅ Describing problems, decisions, tradeoffs

❌ Taking credit for team work
✅ "I was responsible for X, collaborated with team on Y"

---

## Emergency Quick Reference

**If your mind goes blank:**

1. **Breathe** - Take 3 seconds
2. **Ask for clarification** - "Are you asking about X or Y specifically?"
3. **Start with structure** - "Let me break that into 3 parts..."
4. **Use STAR** - Situation, Task, Action, Result, Reflection
5. **Be honest** - "I'm not sure about X, but here's how I'd figure it out..."

**If you don't know something:**
- "I haven't worked with that specifically, but I'd approach it by..."
- "That's a great question. Let me think through it..."
- "I'm not familiar with X, but it sounds similar to Y which I used for..."

---

## The Night Before

**DO:**
- Get 7-8 hours sleep
- Review your cheat sheet once
- Set up your space (lighting, audio, water)
- Test your video/audio

**DON'T:**
- Cram new technical details
- Stay up late reviewing
- Over-caffeinate
- Practice until you sound robotic

---

## Remember

**They're evaluating:**
1. ✅ Can you explain complex things clearly?
2. ✅ Do you think about tradeoffs?
3. ✅ Are you curious and keep learning?
4. ✅ Do you persist through challenges?
5. ✅ Do you reflect and improve?

**They're NOT evaluating:**
- Perfect recall of every technical detail
- Whether you knew everything from the start
- Whether you made zero mistakes
- Whether you're the world's best engineer

**Your advantage:**
- You built something real and complex
- You made real tradeoffs under real constraints
- You learned and adapted
- You have specific, concrete examples

---

## You've Got This! 🚀

**Final thought:** The fact that you built a multi-agent RAG system integrating 3 AI models and processing medical literature shows you can handle complex technical challenges. The interview is just telling that story well.

**Be yourself. Be honest. Show your thinking. You'll do great.**
