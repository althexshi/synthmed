# Interview Quick Reference Card
## Keep this visible during your interview

---

## 30-Second System Overview
"I built a multi-agent RAG system that processes medical literature. IBM Watsonx Orchestrate coordinates specialized agents—each focused on a domain like autism or cancer. These agents use three Python tools I developed: PDF retrieval with PyMuPDF, PubMed search, and LLM synthesis. The system integrates Granite and Meta-Llama 3.2 90B models, processes 27 medical papers across 82MB of literature, and reduced literature review time by 70%."

---

## Your 3 Go-To Stories

### Story 1: RAG Retrieval Accuracy Challenge
**Problem:** Getting irrelevant chunks, mixing up medical domains  
**Tried:** Chunk overlap (didn't help) → similarity threshold (better) → hybrid search (solved it)  
**Learned:** Semantic search alone insufficient for specialized domains  
**Differently:** Would implement evaluation metrics from day 1  

### Story 2: Single Agent → Multi-Agent Pivot
**Started:** One agent doing everything (simpler)  
**Problem:** Too complex, hard to optimize  
**Changed:** Split into specialized agents per domain  
**Cost:** 2 days refactoring  
**Win:** Better monitoring, optimization, maintainability  

### Story 3: Learning Watsonx Orchestrate
**Didn't know:** The orchestration framework  
**Learned:** Docs + 3 prototype examples over 2 days  
**Surprised:** Strong opinions about agent structure  
**Adapted:** Redesigned interfaces to match patterns  
**Next time:** Prototype with framework first  

---

## Key Technical Decisions

| Decision | Why | Tradeoff | Scale Consideration |
|----------|-----|----------|---------------------|
| **PyMuPDF** | 10x faster than alternatives | Less Pythonic API | Good to 1000s of PDFs |
| **In-memory vectors** | Sub-100ms retrieval | Lost on restart | Need DB at 1000+ papers |
| **Decorator pattern** | Consistent logging/monitoring | Added abstraction | Scales well |
| **Specialized agents** | Domain-specific optimization | More code to maintain | Better for production |
| **RAG vs fine-tuning** | Flexibility, can add papers | More complex retrieval | Better for changing data |
| **Multi-model** | Cost/quality optimization | Multiple APIs to manage | Gives flexibility |

---

## Numbers to Remember
- **3** Python tools built
- **27** medical papers processed
- **82 MB** of medical literature
- **5** specialized agents (autism, cancer, dementia, epilepsy, rare)
- **2** AI models integrated (Granite + Llama 3.2 90B)
- **70%** reduction in literature review time

---

## "What I'd Do Differently"
1. **Metrics from day 1** - Built system, then figured out how to measure quality
2. **Observability earlier** - Added monitoring after deployment, couldn't debug production
3. **Data quality analysis** - Assumed clean PDFs, spent time on edge cases
4. **Smaller models for dev** - Expensive to iterate on 90B model
5. **Decision documentation** - Made choices quickly, hard to remember why

---

## Curiosity Examples
- Researched 10+ RAG architecture papers to understand best practices
- Experimented with chunk sizes (256, 512, 1024) - found 512 optimal
- Calculated cost implications (embedding + LLM costs at scale)
- Studied how LangChain/LlamaIndex solve similar problems
- Compared agent patterns (ReAct, Plan-and-Execute, Reflexion)

---

## Scaling Answers (27 papers → 2700 papers)
- **Vector storage:** In-memory → Pinecone/Weaviate/Qdrant
- **Embedding:** Batch processing + caching
- **Queries:** Add caching layer, async processing
- **Infrastructure:** Docker → Kubernetes
- **Monitoring:** Centralized logging (ELK/Datadog)
- **Cost:** Embedding costs linear, need budget controls

---

## Question Formulas

### When asked about challenges:
1. Describe the problem specifically
2. What you tried (multiple attempts)
3. How you debugged/investigated
4. The solution and why
5. What you learned

### When asked about decisions:
1. The options you considered
2. Criteria you evaluated against
3. Why you chose X over Y
4. The tradeoffs you accepted
5. When you'd choose differently

### When asked about learning:
1. What you didn't know
2. How you learned it
3. What surprised you
4. How it changed your approach
5. What you'd do next time

---

## Power Phrases

✅ "I considered X and Y, chose X because..."  
✅ "The tradeoff was between A and B..."  
✅ "At our scale X worked, at 100x I'd use Y..."  
✅ "When that didn't work, I tried..."  
✅ "That made me wonder about..."  
✅ "Looking back, I would have..."  
✅ "I didn't know X, so I researched..."  
✅ "Let me think through that..."  

---

## If Your Mind Goes Blank

1. **Breathe** (3 seconds)
2. **Ask for clarification** - "Are you asking about the architecture or the implementation?"
3. **Use structure** - "Let me break that into 3 parts..."
4. **Be honest** - "Great question, let me think through how I'd approach that..."
5. **Bridge to what you know** - "I haven't used X, but it sounds similar to Y..."

---

## Questions to Ask Them

1. "How does GoFundMe approach reliability for user-facing systems?"
2. "What's your philosophy on technical tradeoffs between speed and quality?"
3. "How do you balance innovation with maintaining stable systems?"
4. "What does success look like in the first 6 months for this role?"
5. "What are the most interesting technical challenges your team is facing?"

---

## Remember

**They want to see:**
- How you think through problems
- How you make decisions
- How you learn and adapt
- How you communicate complexity
- How you reflect and improve

**They DON'T expect:**
- Perfect knowledge of everything
- Zero mistakes in your past
- Immediate answers to everything
- You to know their entire stack

---

## Pre-Interview Checklist (10 min before)

- [ ] Water nearby
- [ ] Phone on silent
- [ ] Good lighting on face
- [ ] Background clean/professional
- [ ] Test audio/video
- [ ] Close unnecessary tabs
- [ ] This reference card ready
- [ ] Deep breath, smile, you've got this!

---

## Emergency Mantras

"I built something complex and real"  
"I have concrete examples"  
"I learned and adapted"  
"I can explain my thinking"  
"They want to see how I think, not quiz me"  

**You've got this! 🚀**
