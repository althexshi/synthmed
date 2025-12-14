# One-Day Interview Prep - Ultra Simplified

## Your 4-Hour Plan

### Hour 1: Memorize Your Pitch (30 sec)
"I built a multi-agent RAG system that processes medical literature. It uses IBM Watsonx Orchestrate to coordinate specialized agents—one for autism, one for cancer, etc. I developed 3 Python tools: PDF retrieval with PyMuPDF, PubMed search, and LLM synthesis. The system integrates Granite and Meta-Llama 3.2 90B models, processes 27 medical papers, and reduced literature review time by 70%."

**Practice this 10 times out loud.**

---

### Hour 2: Prepare 2 Stories

#### Story 1: A Big Challenge
**Problem:** RAG system returned irrelevant results—cancer papers for autism queries  
**What I tried:** 
1. Increased chunk overlap → didn't help
2. Tuned similarity threshold → better
3. Hybrid search (semantic + keyword) → solved it 80%

**Learned:** Semantic search alone isn't enough for specialized domains  
**Would do differently:** Build evaluation metrics from day 1

#### Story 2: A Decision I Changed
**Started:** Single agent doing everything (simpler)  
**Problem:** Got too complex, hard to optimize  
**Changed:** Split into specialized agents per domain  
**Cost:** 2 days refactoring  
**Win:** Better monitoring and performance

**Write these in your own words. Practice saying them.**

---

### Hour 3: Know Your Tradeoffs

Practice saying these out loud:

1. **PyMuPDF vs other libraries**
   "I chose PyMuPDF because it's 10x faster. Tradeoff: less Pythonic, but speed mattered for 82MB of PDFs."

2. **In-memory vs vector database**
   "I used in-memory vectors for 27 papers—sub-100ms retrieval. Accepted restart cost for simplicity. At 1000+ papers, I'd switch to Pinecone."

3. **Specialized vs generic agents**
   "Started generic, refactored to specialized. More code but better performance and easier debugging."

4. **RAG vs fine-tuning**
   "Chose RAG for flexibility—can add new papers easily. Medical data changes frequently, fine-tuning is rigid."

**Pick 2 and practice explaining them.**

---

### Hour 4: Mock Questions

**Set a 3-minute timer. Say your answer out loud.**

#### Q: "Tell me about your most complex project"
Lead with SynthMed → mention multi-agent, 3 models, 27 papers → tell Story 1

#### Q: "Tell me about a technical tradeoff"
Pick one from Hour 3 → explain why, what you gave up, what you gained, when you'd choose differently

#### Q: "How would you scale to 100x?"
"From 27 to 2700 papers:
- Vector DB (Pinecone) instead of in-memory
- Caching layer (Redis) for common queries
- Kubernetes instead of Docker
- Smaller models for simple queries to save cost
- Monitor cost per query"

#### Q: "What would you do differently?"
"Three things:
1. Metrics from day 1—built system then figured out how to measure
2. Observability earlier—couldn't debug production issues
3. Data quality analysis upfront—spent time on PDF edge cases"

---

## Your Cheat Sheet (Print This)

### Numbers
- 3 Python tools
- 27 medical papers  
- 82 MB literature
- 2 AI models (Granite + Llama 3.2 90B)
- 70% time reduction

### Key Decisions
- PyMuPDF: speed over simplicity
- In-memory vectors: simplicity over scale
- Decorator pattern: consistency in logging/monitoring
- Specialized agents: optimization over simplicity
- RAG: flexibility over performance

### Power Phrases
- "I considered X and Y, chose X because..."
- "The tradeoff was..."
- "At our scale X worked, at 100x I'd use Y..."
- "Looking back, I would have..."
- "When that didn't work, I tried..."

### If Mind Goes Blank
1. Breathe 3 seconds
2. "Let me think through that..."
3. Ask for clarification
4. Be honest: "I haven't used X specifically, but I'd approach it by..."

---

## Night Before Checklist

- [ ] Review this doc once (10 min)
- [ ] Get 7-8 hours sleep
- [ ] Test video/audio setup
- [ ] Have water nearby
- [ ] Print cheat sheet (or have it on second screen)

---

## Remember

**They want to see:**
- How you think through problems
- How you make decisions with tradeoffs
- How you learn from mistakes
- How you explain complex things simply

**They DON'T expect:**
- Perfect recall of every detail
- That you never made mistakes
- Immediate answers

**You built something real and complex. Just help them see your thinking process.**

---

## Questions to Ask Them (Pick 1-2)

1. "How does GoFundMe approach technical tradeoffs between speed and quality?"
2. "What are the biggest technical challenges your team is facing?"
3. "What does success look like in the first 6 months?"

---

# That's It. You've Got This! 🚀

**Most important:**
- Practice your 30-second pitch 10 times
- Practice your 2 stories out loud
- Practice 2 tradeoff explanations
- Get good sleep

**Tomorrow:** Breathe, smile, show them how you think. You already did the hard part—you built the thing. Now just tell the story.
