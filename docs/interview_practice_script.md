# Practice Script - Common Interview Questions
## Practice these out loud 3-5 times each

---

## Question 1: "Tell me about the most complex technical project you've worked on."

### Your Answer (3-4 minutes):

"The most complex project I've worked on is SynthMed, a multi-agent RAG system for medical literature analysis. Let me walk you through the complexity layers.

At the architecture level, I integrated IBM Watsonx Orchestrate with multiple AI models—Granite and Meta-Llama 3.2 90B—coordinating specialized agents for different medical domains like autism, cancer, and dementia. Each agent uses three Python tools I developed from scratch.

The data layer was challenging—processing 82 megabytes across 27 medical research papers. Medical PDFs are notoriously difficult to parse because of complex tables, graphs, and terminology. I used PyMuPDF for its speed, but had to build robust error handling for scanned documents, watermarks, and unusual formatting.

The orchestration layer was probably the most complex part. I needed agents that could:
- Search PubMed for relevant papers
- Retrieve and parse PDFs on the fly  
- Synthesize findings across multiple papers using LLMs
- Maintain medical accuracy while being efficient

One specific challenge: ensuring retrieval accuracy. Initially, my RAG system would sometimes return chunks from cancer papers when queried about autism genetics. I debugged for two days, tried three different approaches—increasing chunk overlap, tuning similarity thresholds, and finally implementing a hybrid search that combines semantic similarity with keyword matching. That hybrid approach improved relevance by about 80%.

The system now processes literature reviews 70% faster than manual methods, which for medical researchers means they can stay current with research instead of drowning in papers.

What made this complex wasn't any one piece—it was the integration: multiple models with different APIs, medical domain accuracy requirements, real-time processing, and making it reliable enough for healthcare use cases."

**Key elements hit:**
- ✅ High-level overview
- ✅ Multiple complexity layers
- ✅ Specific technical details
- ✅ A concrete problem + solution
- ✅ Impact/outcome
- ✅ Why it was hard

---

## Question 2: "Describe a time you made a difficult technical tradeoff."

### Your Answer (2-3 minutes):

"A significant tradeoff I made was choosing in-memory vector storage over a proper vector database for the RAG system.

Here's the context: I needed to store embeddings for 27 medical papers to enable semantic search. The options were:
1. In-memory (NumPy/FAISS) 
2. Vector database (Pinecone, Weaviate)

I chose in-memory, and here's why: With 27 papers, we're talking maybe 10-15,000 chunks, which is maybe 50-100MB in memory. Query latency was under 100 milliseconds, and the simplicity meant no external dependencies, no network calls, no database maintenance.

The tradeoffs I accepted:
- **Lost state on restart** - Every server restart means re-embedding, which takes about 5 minutes
- **Can't scale horizontally** - Each instance has its own copy of vectors
- **Memory limitations** - At maybe 1,000 papers, this approach breaks down

What I gained:
- **Simplicity** - No database to configure, maintain, or pay for
- **Speed** - Sub-100ms retrieval without network overhead  
- **Development velocity** - Prototyped and tested faster

At the time, for an MVP with a limited corpus, this was the right call. But I documented that at around 500-1,000 papers, we'd need to migrate to Pinecone or Weaviate. I even sketched out the migration path—using the same embedding model means we could port vectors with minimal code changes.

If I were building this for production at scale from day one, I'd have chosen differently. But given our constraints—timeline, team size, and corpus size—optimizing for simplicity was the right engineering tradeoff.

This taught me that the 'best' technical solution depends entirely on context: scale, team, timeline, and future plans."

**Key elements hit:**
- ✅ Specific decision with options
- ✅ Clear reasoning
- ✅ Explicit tradeoffs (what you gave up, what you gained)
- ✅ When you'd decide differently
- ✅ Showed reflection and learning

---

## Question 3: "Tell me about a time when something didn't work as expected."

### Your Answer (3 minutes):

"Early in the SynthMed project, I built the agent architecture using a single, general-purpose agent that would handle all tasks—retrieval, synthesis, validation—for all medical domains.

It seemed logical at the time: simpler architecture, one agent to debug, less code duplication. I spent about three days building it out, writing comprehensive prompts, and testing.

The problems started appearing around day four:
- The agent was getting confused between medical domains—sometimes pulling autism research for cancer queries
- Prompts were becoming enormous to handle all edge cases
- I couldn't optimize for specific domains  
- Debugging was a nightmare—was it the retrieval, synthesis, or prompt?

I spent a day trying to fix it with better prompts, more structured outputs, additional validation layers. It got marginally better but was still problematic.

At that point, I had a decision: keep patching or redesign. I talked through it with my team and realized I was fighting the architecture. So I made the call to refactor into specialized agents—one for autism research, one for cancer, one for dementia, etc.

The refactor took two days. I had to:
- Redesign the agent interface
- Split the knowledge base
- Create domain-specific prompts for each agent
- Update the orchestration logic

But it solved the problems immediately:
- Each agent could have domain-optimized prompts
- I could test and debug independently  
- Performance was better—smaller context windows
- Monitoring was clearer—I could track success rates per domain

Looking back, I should have started with specialized agents. The extra complexity is worth it once you hit a certain scale. I learned that sometimes 'simpler' at the architecture level creates complexity at the implementation level.

The silver lining: because I built the single-agent version first, I understood the common patterns, which made the abstraction for specialized agents much cleaner. So it wasn't wasted effort—it informed the final design."

**Key elements hit:**
- ✅ Specific situation that didn't work
- ✅ Why it seemed good initially
- ✅ How problems manifested
- ✅ Multiple attempts to fix
- ✅ The decision to pivot
- ✅ The cost and outcome of change
- ✅ Reflection and learning
- ✅ Found positive in the struggle

---

## Question 4: "How would you scale your system to handle 100x the load?"

### Your Answer (3-4 minutes):

"Great question. Let me think through scaling from 27 papers to 2,700 papers and from single-user to multi-user.

**Data Layer:**
First, I'd migrate from in-memory vectors to a proper vector database—probably Pinecone or Weaviate. At 2,700 papers, we're looking at maybe 500,000 to 1 million chunks, which is beyond in-memory. A vector DB gives us:
- Persistent storage
- Horizontal scaling
- Better query optimization

**Ingestion Pipeline:**
Currently, I embed all papers at startup. At 100x scale, that's infeasible. I'd build an async ingestion pipeline:
- Papers added to a queue (SQS or RabbitMQ)
- Workers process in batches
- Track ingestion status per paper
- Handle failures and retries gracefully

**Query Layer:**
At higher query volume, I'd add:
- **Caching:** Redis for frequent queries—cache the retrieval results and maybe even LLM syntheses for common questions
- **Load balancing:** Multiple inference endpoints for the LLM models
- **Rate limiting:** To prevent runaway costs with large models

**LLM Costs:**
This is critical. At scale, LLM costs could be thousands per month. I'd:
- Use smaller models for simpler queries (routing based on complexity)
- Implement aggressive caching (30-day TTL)
- Maybe batch queries where latency allows
- Monitor cost per query and set alerts

**Infrastructure:**
Move from Docker to Kubernetes for:
- Auto-scaling based on load
- Better resource management
- Rolling deployments
- Health checks and auto-restart

**Monitoring:**
At scale, observability is crucial:
- Centralized logging (ELK stack or Datadog)
- Metrics: latency, error rate, cost per query, cache hit rate
- Alerting on SLAs
- Query performance tracking to identify slow patterns

**Data Quality:**
With 2,700 papers, quality becomes harder:
- Automated checks on PDF parsing quality
- Versioning of embeddings (if we change models)
- A/B testing of retrieval strategies
- User feedback loop to catch bad results

**The Bottlenecks:**
The main bottlenecks would probably be:
1. LLM inference latency and cost
2. Vector similarity search at scale
3. Cold start for new papers

I'd measure everything, find the actual bottleneck, and optimize that first rather than premature optimization.

One thing I learned from building this: scale isn't just about handling more data—it's about handling more failure modes, more edge cases, and more cost management. The architecture needs to be observale and adaptable."

**Key elements hit:**
- ✅ Thought through multiple dimensions
- ✅ Specific technologies and why
- ✅ Cost considerations
- ✅ Identified bottlenecks
- ✅ Showed systematic thinking
- ✅ Acknowledged measurement importance

---

## Question 5: "Why did you choose the Decorator pattern for your tools?"

### Your Answer (2 minutes):

"I chose the Decorator pattern for cross-cutting concerns—things that every tool needs but aren't core to the tool's function.

Specifically, I needed:
- **Logging:** Track when each tool is called, with what parameters
- **Timing:** Measure how long each tool takes (PDF parsing vs. PubMed search)
- **Error handling:** Standardized try-catch and error reporting
- **Monitoring:** Send metrics to our monitoring system

Without decorators, I'd be copy-pasting this logic across three tools. That's error-prone and hard to maintain.

With decorators, my tool code looks like:

```python
@log_call
@measure_time
@handle_errors
def retrieve_pdf(paper_id):
    # Core logic only
    return parsed_content
```

The actual logging, timing, and error handling is abstracted away. If I want to add retry logic later, I add one decorator—not modify three tools.

The tradeoff is abstraction complexity. Someone reading the code needs to understand decorators. But Python developers generally understand them, and the consistency across tools is worth it.

I also considered a base class approach, but decorators are more flexible—I can mix and match decorators per tool if needed. For example, PubMed search has a `@rate_limit` decorator that the others don't need."

**Key elements hit:**
- ✅ Specific reason for the choice
- ✅ What problem it solved
- ✅ Alternative mentioned
- ✅ Code example
- ✅ Tradeoff acknowledged

---

## Question 6: "What's something you learned recently that changed how you approach problems?"

### Your Answer (2 minutes):

"Recently, I dove deep into RAG architectures because I knew I'd be implementing one but had only surface-level knowledge.

I read about 10 papers and studied production implementations from LangChain, LlamaIndex, and some of the research coming out of places like Stanford. What really changed my thinking was understanding the different failure modes of RAG systems.

I had assumed RAG was mostly about embeddings and similarity search. What I learned is that chunking strategy might be more important. How you split documents—respecting semantic boundaries, maintaining context, handling overlap—has a huge impact on retrieval quality.

This changed my approach: instead of starting with model selection, I started with data analysis. I looked at the structure of medical papers—abstract, introduction, methods, results, discussion. I built my chunking to respect those boundaries instead of just splitting every 512 tokens.

Another thing I learned: retrieval is about recall AND precision. Semantic search gives great recall but can miss exact terms. Keyword search gives precision but misses related concepts. The hybrid approach addresses both.

This taught me a meta-lesson: when I encounter a new technique, especially one that's trendy, I should dive deep into the research and production lessons before implementing. The extra day of research saved me a week of debugging.

Now, whenever I'm about to implement something new, I budget time for research first. It feels slower upfront but is faster overall."

**Key elements hit:**
- ✅ Specific learning
- ✅ How you learned it
- ✅ What changed in your approach
- ✅ Concrete example
- ✅ Meta-lesson about learning
- ✅ Shows curiosity

---

## Question 7: "How do you ensure reliability in AI/ML systems?"

### Your Answer (2-3 minutes):

"Reliability in AI systems is tricky because you don't just have traditional software bugs—you have model behaviors that are probabilistic and sometimes unpredictable.

For SynthMed, I thought about reliability at multiple layers:

**1. Input Validation:**
- Validate PDFs before processing (format, size, readability)
- Sanitize queries to prevent prompt injection
- Check that papers are actually medical literature (not random PDFs)

**2. Output Validation:**
- For medical content, I can't just trust the LLM output
- I built a validation pipeline that checks citations are real
- Flag outputs with hallucination indicators (vague language, no citations)
- For critical medical information, I require human review

**3. Monitoring:**
- Track error rates per component (retrieval, embedding, synthesis)
- Monitor latency—if LLM calls suddenly take longer, something's wrong
- Cost monitoring—runaway costs often indicate a bug
- Track user feedback—did the response actually help?

**4. Fallback Strategies:**
- If one LLM fails, fall back to another
- If retrieval returns nothing, widen the search
- If confidence is low, tell the user rather than return garbage
- Graceful degradation: partial results better than complete failure

**5. Testing:**
- Unit tests for each tool
- Integration tests for agent workflows
- Regression tests with known queries and expected results
- I built a test set of 50 medical questions with human-validated answers

**6. Versioning:**
- Track which model version generated which response
- If we update models, we can compare outputs
- Allows rollback if new model performs worse

**The hard part with AI reliability:** There's no 'correct' answer in many cases. A literature review could be synthesized many ways. So reliability is more about consistency, transparency, and giving users confidence in the output.

For medical applications especially, I believe in human-in-the-loop. The system should augment researchers, not replace them. So I focused on making the system's confidence level visible and making it easy to drill into sources."

**Key elements hit:**
- ✅ Multiple layers of reliability
- ✅ Specific techniques
- ✅ Acknowledged AI-specific challenges
- ✅ Testing approach
- ✅ Monitoring and observability
- ✅ Domain-appropriate (medical) considerations

---

## Question 8: "Walk me through how you would debug a production issue where users are getting irrelevant search results."

### Your Answer (3 minutes):

"Great question. Let me walk through this systematically.

**Step 1: Reproduce and Scope (10 minutes)**
- Can I reproduce it with the same query?
- Is it all users or specific users?
- Is it all queries or specific types?
- When did it start? (Helps identify if something changed)

**Step 2: Gather Data (15 minutes)**
- Pull logs for affected queries
- What was the user query?
- What chunks were retrieved?
- What were the similarity scores?
- What did the LLM synthesis look like?
- Check monitoring dashboards for anomalies

**Step 3: Isolate the Layer (20 minutes)**
Test each component independently:
- **Embedding layer:** Is the query embedding correct? Test with known-good queries
- **Retrieval layer:** Is vector search returning reasonable chunks? Check similarity scores
- **LLM layer:** Given good chunks, is synthesis correct?

Let's say I find retrieval is the problem—chunks have low similarity scores.

**Step 4: Dig Into Retrieval (30 minutes)**
- Check if embeddings are stale (maybe papers were updated but not re-embedded)
- Test the query with different retrieval parameters (top-k, similarity threshold)
- Check if vector index is corrupted (restart vector store with fresh data)
- Look at the query—is it too vague? Too domain-specific?

**Step 5: Test Hypothesis**
Let's say I suspect the issue is new papers weren't embedded properly. I'd:
- Check ingestion logs for those papers
- Manually trigger re-embedding for one paper
- Test query again
- If it works, re-embed all recent papers

**Step 6: Fix and Validate**
- Deploy the fix (re-embed papers)
- Test with original failed queries
- Monitor for 24 hours to ensure no regression
- Add alerting for ingestion failures

**Step 7: Prevent Recurrence**
- Add automated tests for embedding pipeline
- Add monitoring for embedding freshness
- Document the incident and root cause
- Consider architectural changes (better ingestion validation)

**Throughout this process:**
- Communicate with users about status
- Document findings in runbook for next time
- Keep stakeholders updated on ETA

The key is being systematic: isolate, test hypotheses, validate fixes, and prevent recurrence. For AI systems especially, I'd resist the urge to immediately tweak model parameters—usually the issue is in data or infrastructure, not the model."

**Key elements hit:**
- ✅ Systematic approach
- ✅ Specific debugging steps
- ✅ Showed thinking process
- ✅ Isolation of components
- ✅ Testing hypotheses
- ✅ Prevention mindset
- ✅ Communication

---

## Practice Tips

**For each answer above:**

1. **Read it once** to understand the structure
2. **Say it out loud** without reading (stumbling is OK)
3. **Record yourself** (phone voice memo)
4. **Listen back** - Are you clear? Too fast? Rambling?
5. **Practice again** - Aim for natural, not memorized

**Timing:**
- Short answer: 1-2 minutes
- Medium answer: 2-3 minutes  
- Long answer: 3-4 minutes

**Remember:**
- ✅ It's OK to pause and think
- ✅ It's OK to ask clarifying questions
- ✅ It's OK to say "let me structure my thoughts"
- ❌ Don't memorize word-for-word (sounds robotic)
- ❌ Don't rush through your answer

**Body Language (even on video):**
- Smile when you start talking
- Make eye contact with camera
- Use hand gestures naturally
- Energy in your voice—show you're excited about your work

---

## You've Got This!

These answers are templates, not scripts. Use your own words. Be authentic. Show your thinking. You built something impressive—just help them see what you see.

🚀 **Good luck!**
