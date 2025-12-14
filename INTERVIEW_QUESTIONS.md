# Technical Interview Questions - SynthMed Project

## Context
These questions are based on your IBM AI Lab experience building SynthMed, a multi-agent RAG system for medical research synthesis. They're designed to explore your technical decisions, problem-solving approach, and learnings from the project.

---

## 📖 HOW TO USE THIS GUIDE

### For Non-Native English Speakers

**This guide includes complete, easy-to-speak answers for all 10 questions.** Each answer is written with:
- ✅ Simple vocabulary
- ✅ Short sentences
- ✅ Clear structure
- ✅ Natural speaking rhythm

### How to Practice:

**Step 1: Read the answer out loud 3 times**
- First time: Read slowly, focus on pronunciation
- Second time: Read at normal speed
- Third time: Read while looking away from the screen occasionally

**Step 2: Record yourself**
- Use your phone to record your answer
- Listen back - do you sound natural?
- Note words that are hard to say, practice them separately

**Step 3: Practice with the structure, not exact words**
- You don't need to memorize word-for-word
- Remember the key points: "Opening → Part 1 → Part 2 → Part 3"
- Use your own words while keeping the structure

**Step 4: Practice with someone**
- Ask a friend to ask you the question
- Answer without looking at notes
- Get feedback on clarity

### Speaking Tips:

**If you forget something:**
- ✅ It's okay to pause and think for 2-3 seconds
- ✅ Say "Let me think..." or "That's a good question..."
- ❌ Don't say "um" or "uh" repeatedly

**If you don't understand a question:**
- ✅ Ask for clarification: "Could you rephrase that?"
- ✅ Repeat the question: "So you're asking about...?"
- ❌ Don't guess and answer the wrong question

**To sound confident:**
- ✅ Speak slowly and clearly (better than fast and unclear)
- ✅ Use transition phrases: "Let me explain...", "Here's how...", "The key point is..."
- ✅ Pause between sections to let the interviewer absorb information

### Time Management:

- Main answer: **2 minutes** (practice to hit this)
- Follow-up answers: **45-60 seconds** each
- If you're going over time, the interviewer will redirect you - that's normal!

---

---

## Question 1: Multi-Agent RAG Architecture

**Question:** You built a multi-agent RAG system integrating IBM Watsonx Orchestrate, Granite, and Meta-Llama 3.2 90B across 5 medical domains. Walk me through your decision-making process for choosing this multi-agent architecture. What other approaches did you consider, and what were the key tradeoffs that led you to this design?

### YOUR ANSWER (2 minutes):

**Opening - State the problem:**
"Great question. Let me explain why we chose this architecture. The main problem was that medical research is very domain-specific. Autism papers and cancer papers are completely different. If we put all 27 papers in one place, the system would get confused. It would return irrelevant results."

**Part 1 - Explain your architecture (30 seconds):**
"We built a hierarchical system with six agents. One main agent acts as the coordinator. Five sub-agents handle specific diseases - autism, cancer, dementia, epilepsy, and rare diseases. 

The main agent has no knowledge base. It just routes questions to the right sub-agent. Each sub-agent has its own knowledge base. For example, the autism agent only searches six autism papers. This keeps results focused."

**Part 2 - Alternatives we considered (45 seconds):**
"We looked at three options.

First option: One big agent with all 27 papers. This is simple, but retrieval quality is poor. When someone asks about autism, you also get cancer results. We rejected this.

Second option: Five separate agents with no coordinator. This is fast, but users can't ask cross-domain questions like 'compare inflammation in cancer and dementia.' We rejected this too.

Third option: What we built. Multiple agents with a coordinator. This gives us the best retrieval quality and handles both single-domain and cross-domain questions. Yes, it's more complex. But the quality improvement was worth it."

**Part 3 - Connect to results (15 seconds):**
"This architecture helped us achieve the 70% time reduction. Researchers got precise results, not noise. They could focus on relevant papers immediately."

**Part 4 - What you'd improve (15 seconds):**
"If I rebuilt this, I would make sub-agents run in parallel instead of one after another. This would reduce response time from 15 seconds to maybe 5 seconds."

---

### FOLLOW-UP 1: How did you decide which model to use (Granite vs. Llama)?

**YOUR ANSWER (1 minute):**

"We tested both models in watsonx Prompt Lab. 

Granite is IBM's model. It's faster and cheaper. Llama 3.2 90B is Meta's model. It's slower and more expensive but produces better quality.

For medical synthesis, we chose quality over speed. Llama produced better-structured summaries with more accurate citations. So we used Llama for all agents.

However, we kept the model selection flexible in our YAML configuration. In the future, we could use Granite for simple routing tasks and Llama only for complex synthesis. This would reduce costs."

---

### FOLLOW-UP 2: What problems did multi-agent solve that single agent wouldn't?

**YOUR ANSWER (1 minute):**

"Multi-agent solved three key problems.

First: Retrieval precision. With one big knowledge base, autism queries retrieve cancer papers too. That's noise. With separate agents, autism agent only searches autism papers. Much cleaner results.

Second: Scalability. To add a new disease like diabetes, we just create a new sub-agent and add it to the collaborators list. With a single agent, we'd need to reindex everything.

Third: Cross-domain synthesis. When someone asks 'compare inflammation in cancer versus dementia,' the main agent routes to both agents, collects their insights, and combines them. A single agent can't give this clear domain attribution."

---

### FOLLOW-UP 3: What if you used LangChain instead of Watsonx?

**YOUR ANSWER (1 minute):**

"With LangChain, we'd have more flexibility but more work.

Watsonx gave us these features automatically: agent orchestration, built-in vector database, webchat interface, and enterprise security.

With LangChain, we'd need to build: the routing logic ourselves, set up our own vector database like Pinecone, create our own UI, and manage conversation state.

For an 8-week project in an IBM program, Watsonx was the right choice. It let us move fast. For a startup that needs full control and cost optimization, LangChain might be better. It's a tradeoff between speed and flexibility."

---

## Question 2: Measuring the 70% Time Reduction

**Question:** You mentioned the system reduced literature review time by 70%. How did you measure this? Walk me through your methodology for establishing the baseline, measuring improvement, and validating that researchers actually achieved this speedup in practice.

### YOUR ANSWER (2 minutes):

**Opening - Be honest about methodology:**
"This metric came from our IBM Design Thinking research and user workflow analysis. Let me walk you through how we calculated it."

**Part 1 - The baseline (old way) (45 seconds):**
"We mapped out the traditional literature review process with researchers. It looked like this:

First, search PubMed for relevant papers - about 30 minutes.
Second, download 10 to 20 papers - about 15 minutes.
Third, read abstracts to filter which papers are relevant - about 45 minutes.
Fourth, deep-read 5 to 7 papers - about 4 hours.
Fifth, take notes and synthesize findings - about 1.5 hours.

Total time: approximately 7 hours per research question."

**Part 2 - The new way (with SynthMed) (45 seconds):**
"With SynthMed, the workflow changed:

First, ask SynthMed your research question - 1 minute.
Second, review the structured synthesis with citations - about 30 minutes.
Third, deep-dive into only 2 or 3 most relevant papers - about 1.5 hours.

Total time: approximately 2 hours.

Seven hours down to 2 hours. That's 71%, which we rounded to 70%."

**Part 3 - How we validated this (30 seconds):**
"During the IBM program, we had researchers test the system. They confirmed the time savings were real. They especially liked that they could skip the filtering step. SynthMed pre-filtered papers for them. They only read what was actually relevant."

---

### FOLLOW-UP 1: What were you comparing against?

**YOUR ANSWER (30 seconds):**

"We compared against manual literature review. That means: searching databases like PubMed, downloading PDFs, reading papers yourself, and writing your own summary. 

We didn't compare against other AI tools because most researchers weren't using them yet. The baseline was traditional manual work."

---

### FOLLOW-UP 2: What didn't the 70% capture?

**YOUR ANSWER (45 seconds):**

"The metric doesn't capture everything. 

First, learning curve. New users need 10-15 minutes to understand how to ask good questions. 

Second, verification time. Researchers still need to verify the citations are accurate. They can't just trust the AI completely.

Third, edge cases. For very novel research topics with no papers in our knowledge base, the system can't help. Manual search is still needed.

So 70% is the best-case scenario for questions within our five disease domains."

---

### FOLLOW-UP 3: How did you balance quality versus speed?

**YOUR ANSWER (45 seconds):**

"We prioritized quality over pure speed. 

We could have made the system faster by retrieving only 2-3 chunks instead of 5-7. But that would miss important context. 

We chose larger context windows and overlapping chunks. This made responses slower - maybe 10-15 seconds instead of 3-4 seconds. But the quality was much better. Researchers trusted the results more.

The philosophy was: it's okay to take 15 seconds if the answer is accurate. It's not okay to give a wrong answer in 3 seconds."

---

### FOLLOW-UP 4: Were there cases where the system was slower?

**YOUR ANSWER (45 seconds):**

"Yes, in two situations.

First: Very specific niche questions. If someone asks about a rare gene mutation that appears only once in one paper, manual search might be faster. You can just use Control+F in the PDF.

Second: Cross-domain queries involving all five agents. These take longer - maybe 20-30 seconds - because the system routes to multiple agents and synthesizes everything. For simple single-domain questions, it's much faster.

But overall, the end-to-end workflow was still 70% faster even with these edge cases."

---

## Question 3: Processing 82 MB of Medical Literature

**Question:** You processed 82 MB of medical literature across 27 papers. That's a substantial amount of data. Tell me about the biggest technical challenge you faced in processing these medical PDFs. What didn't work initially, and how did you solve it?

### YOUR ANSWER (2 minutes):

**Opening - State the challenge:**
"The biggest challenge was chunking medical papers correctly. Medical papers have complex structure, and if you chunk wrong, you break the meaning."

**Part 1 - What we processed (30 seconds):**
"We had 27 papers totaling 83 megabytes. That's about 675 pages of dense medical text. Each paper had abstracts, methods, results, tables, figures, and references. Some papers were 20 pages, others were 50 pages."

**Part 2 - The chunking problem (45 seconds):**
"We used pymupdf library to extract text. That part worked fine. But then we needed to split the text into smaller chunks for embedding.

Initially, I tried simple character-based splitting - just cut every 1000 characters. This broke sentences in the middle. For example, one chunk ended with 'the GIGYF1 gene shows mutations in' and the next chunk started with 'autism patients.' The meaning was lost.

Context window was also an issue. We couldn't fit entire 50-page papers into the LLM prompt. We needed smaller, focused pieces."

**Part 3 - Our solution (45 seconds):**
"We implemented overlapping chunks. Each chunk is 1000 characters with 200-character overlap. This means the end of one chunk repeats at the start of the next chunk.

Why overlap? It preserves context. If a sentence is split across chunks, it appears completely in at least one chunk. This improved retrieval quality significantly.

We also tried to extract tables separately, but our implementation was basic. We just looked for tab-separated text. This is something I'd improve with better table extraction tools."

---

### FOLLOW-UP 1: How did you handle different PDF formats?

**YOUR ANSWER (1 minute):**

"Different papers had different quality issues.

Some PDFs were clean - well-formatted text, clear structure. These worked perfectly with pymupdf.

Some PDFs were scanned images, not text. These needed OCR, which we didn't implement. We just excluded those papers or found text-based versions.

Some PDFs had two-column layouts. Pymupdf extracted columns left-to-right, top-to-bottom, which sometimes mixed content. We accepted this limitation for the 8-week timeline.

In production, I would use more sophisticated tools like LlamaParse or Unstructured library. These handle complex layouts better."

---

### FOLLOW-UP 2: Explain your chunking strategy in detail.

**YOUR ANSWER (1 minute):**

"Our chunking strategy had three parameters:

Chunk size: 1000 characters, which is approximately 200 words. This is small enough to be focused but large enough to contain complete ideas.

Overlap: 200 characters, which is about 40 words. This creates redundancy but preserves context across boundaries.

Word-based splitting: We split on words, not mid-word. This keeps the text readable.

The algorithm walks through the text, takes 1000 characters, steps forward 800 characters (1000 minus 200 overlap), and repeats. This created about 1700 chunks across all 27 papers.

Each chunk got embedded into a vector and stored in the knowledge base for semantic search."

---

### FOLLOW-UP 3: Did you hit context window limitations?

**YOUR ANSWER (45 seconds):**

"Yes, for cross-domain queries.

Llama 3.2 90B has a context window of 128,000 tokens. One chunk is about 200 tokens. If we retrieve 5 chunks per agent and query 3 agents, that's 15 chunks times 200 tokens equals 3,000 tokens just for context.

Add the system prompt, instructions, and output, we could hit maybe 10,000 tokens total. We stayed well within limits for normal queries.

But I thought about edge cases. If someone asked a question hitting all 5 agents with 10 chunks each, we'd use 10,000 tokens just for context. That's why proper retrieval quality is important - retrieve only what's relevant."

---

### FOLLOW-UP 4: How did you validate extraction accuracy?

**YOUR ANSWER (45 seconds):**

"We used manual spot-checking.

I randomly selected 5 papers and compared the extracted text with the original PDF. I checked if paragraphs were intact, if tables were captured, and if references were preserved.

For most papers, accuracy was 95% or better. The main issues were with tables and figures, which were either missing or garbled.

We also tested retrieval quality. We asked questions where we knew the answer was in a specific paper. If the system retrieved the right chunks, extraction was working.

A more rigorous approach would be to use ground truth datasets with human-annotated chunk relevance. But for an 8-week project, manual validation was sufficient."

---

## Question 4: Decorator Pattern in Python Tools

**Question:** You mentioned using the Decorator pattern for your 3 Python tools (PDF retrieval, PubMed Search, LLM synthesis). Explain why you chose the Decorator pattern specifically. What problem was it solving, and how did it make your code better compared to other design patterns you could have used?

### YOUR ANSWER (2 minutes):

**Opening - What the decorator does:**
"The decorator pattern here refers to the `@tool` decorator from IBM Orchestrate. This decorator converts regular Python functions into tools that agents can use. Let me explain how this works."

**Part 1 - The problem it solves (45 seconds):**
"We wrote three Python functions: one to extract PDFs, one to search PubMed, and one to synthesize with the LLM.

But these are just regular Python functions. Agents can't call them directly. We needed to register these functions as tools in the Orchestrate platform.

The `@tool` decorator does this automatically. When you put `@tool` above a function, Orchestrate reads the function signature, the docstring, and the parameter types. It generates an API endpoint and makes it available to all agents."

**Part 2 - Show a concrete example (45 seconds):**
"Here's a real example from our code:

```python
@tool
def pdf_retriever(pdf_path: str, include_chunks: bool = False) -> str:
    '''
    Retrieve and extract content from PDF files.
    
    Args:
        pdf_path: Path to the PDF file
        include_chunks: Whether to include chunked text
    
    Returns:
        JSON string with extracted content
    '''
    # ... function implementation
```

The `@tool` decorator automatically creates a tool called `pdf_retriever`. The docstring becomes the tool description. The type hints tell Orchestrate what inputs to expect. Agents can now call this tool by name."

**Part 3 - Why this is better (30 seconds):**
"Without the decorator, we'd need to manually write configuration files describing each tool - the name, parameters, return type, description. Then we'd need to set up API endpoints ourselves.

The decorator makes this automatic. Write the function, add `@tool`, done. This saved us a lot of time and reduced errors."

---

### FOLLOW-UP 1: Show another specific example.

**YOUR ANSWER (1 minute):**

"Sure. Here's our PubMed search tool:

```python
@tool
def pubmed_search(query: str, max_results: int = 10) -> str:
    '''
    Search PubMed for medical research articles.
    
    Args:
        query: Search query like 'autism genetics'
        max_results: Maximum articles to return
    
    Returns:
        JSON string with articles and abstracts
    '''
    searcher = PubMedSearcher()
    result = searcher.search_and_fetch(query, max_results)
    return json.dumps(result)
```

The decorator reads this and creates a tool. When an agent wants to search PubMed, it just calls `pubmed_search('autism genetics', 5)`. Orchestrate handles the rest - parameter validation, JSON serialization, error handling."

---

### FOLLOW-UP 2: What would code look like without the decorator?

**YOUR ANSWER (1 minute):**

"Without the decorator, we'd need manual configuration.

First, write the function:
```python
def pdf_retriever(pdf_path, include_chunks=False):
    # implementation
```

Second, write a YAML file describing it:
```yaml
tool_name: pdf_retriever
description: Retrieve and extract PDF content
parameters:
  - name: pdf_path
    type: string
    required: true
  - name: include_chunks
    type: boolean
    required: false
```

Third, set up an HTTP endpoint to expose it.

Fourth, register the tool with Orchestrate using CLI commands.

The decorator collapses steps 2, 3, and 4 into one line: `@tool`. Much cleaner."

---

### FOLLOW-UP 3: Were there downsides?

**YOUR ANSWER (45 seconds):**

"Yes, two main downsides.

First: Less control. The decorator handles everything automatically. If you need custom error handling or input validation beyond what the decorator provides, you're limited.

Second: Platform lock-in. This decorator is specific to IBM Orchestrate. If we wanted to move to LangChain or another platform, we'd need to rewrite these tool wrappers.

But for our use case - 8-week project on IBM's platform - these downsides didn't matter. The speed and simplicity were more important."

---

### FOLLOW-UP 4: How did it help with testing?

**YOUR ANSWER (45 seconds):**

"The decorator helped with testing in two ways.

First: The core logic is in regular Python functions. We could test `extract_pdf_text()` and `chunk_text()` functions directly with unit tests. No need to mock the entire Orchestrate platform.

Second: The decorator separates concerns. The function does the work. The decorator handles the integration. This made our code modular and easier to test.

We could run tests locally with pytest before deploying to Orchestrate. This caught bugs early."

---

## Question 5: Docker and Deployment Workflows

**Question:** You deployed backend workflows through IBM Orchestrate ADK and Docker with automated validation and monitoring. Walk me through what "automated validation" meant in practice. What were you validating, how did you implement it, and what kinds of failures did you catch before they hit users?

### YOUR ANSWER (2 minutes):

**Opening - Set context:**
"By automated validation, I mean we tested tools and agents before deploying them to production. Let me walk you through our process."

**Part 1 - What we validated (45 seconds):**
"We validated three things.

First: Tool functionality. Does pdf_retriever actually extract text? Does pubmed_search return results? We wrote unit tests for each tool.

Second: Agent configuration. Are the YAML files properly formatted? Do the agent instructions make sense? Do the collaborators exist?

Third: Integration. Can agents actually call the tools? Does the routing work? Does the output format match expectations?"

**Part 2 - How we implemented validation (45 seconds):**
"Our workflow looked like this:

Step 1: Write or modify a tool in Python.
Step 2: Run local tests with pytest. This caught syntax errors and logic bugs.
Step 3: Import the tool to Orchestrate using the ADK command line.
Step 4: Test the tool manually by calling it directly.
Step 5: Import or update the agent YAML files.
Step 6: Test the agent with sample queries.
Step 7: If everything works, mark it as production-ready.

This validation caught issues before users saw them."

**Part 3 - Types of failures we caught (30 seconds):**
"We caught several types of problems.

Broken file paths: A tool tried to read a PDF that didn't exist. Unit tests caught this.

Invalid YAML: An agent YAML had wrong indentation. The import command failed with a clear error.

Missing dependencies: A tool needed the requests library but we forgot to install it. Docker build failed.

Wrong output format: An agent returned plain text instead of formatted markdown. Manual testing caught this."

---

### FOLLOW-UP 1: How did you structure Docker containers?

**YOUR ANSWER (1 minute):**

"We didn't heavily use Docker for the main application because IBM Orchestrate handles deployment. But we used Docker for local development and testing.

Our Docker setup included:
- Base image: Python 3.10
- Dependencies: pymupdf, requests, ibm-watsonx-orchestrate from requirements.txt
- Working directory with our tools folder
- Entry point to run tests or start a local development server

This ensured everyone on the team had the same environment. It also let us test tools locally before pushing to Orchestrate."

---

### FOLLOW-UP 2: What monitoring did you set up?

**YOUR ANSWER (1 minute):**

"Our monitoring was basic but effective.

First: Orchestrate's built-in logs. Every agent call generates logs showing what tools were called, what parameters were passed, and what was returned. We checked these logs regularly.

Second: Manual testing dashboard. We kept a spreadsheet of test queries and expected behaviors. After each deployment, we ran through the test queries to make sure everything still worked.

Third: User feedback. We used the thumbs up/thumbs down feature in Orchestrate to collect feedback. When users gave thumbs down, we investigated.

For a production system, I would add automated health checks, error rate monitoring, and latency tracking. But for an 8-week project, manual monitoring was sufficient."

---

### FOLLOW-UP 3: Tell me about a validation that caught a real problem.

**YOUR ANSWER (1 minute):**

"Yes, we caught a critical bug during validation.

The problem: I updated the pdf_retriever tool to add better table extraction. I tested it locally on two PDFs, and it worked fine. Then I deployed it.

But when the agent tried to use it, it crashed. The error message said 'chunk_text expects an integer but got a string.'

What happened: I changed a parameter type from int to string but forgot to update the type hint in the function signature. The decorator passed the wrong type, and the function crashed.

The validation process caught this before users saw it. I fixed the type hint, re-deployed, tested again, and it worked. This showed me the importance of thorough testing at every step."

---

### FOLLOW-UP 4: How did you handle versioning and rollbacks?

**YOUR ANSWER (45 seconds):**

"IBM Orchestrate has version control for agents. Each import creates a new version. You can see the history and roll back if needed.

Our process was:
- Keep all YAML files in Git for source control
- Before making changes, tag the current version
- Test new changes in a draft environment first
- Only promote to live after validation
- If something breaks in live, we could roll back to the previous version using Orchestrate's version history

We never needed to roll back during the project, but having this safety net gave us confidence to deploy frequently."

---

## Question 6: Citation Tracking Implementation

**Question:** You implemented automated citation tracking across 5 medical domains. This sounds straightforward, but in practice, tracking which claims came from which sources is challenging with LLMs. Describe the technical approach you used to ensure citations were accurate and how you validated that the system wasn't hallucinating references.

### YOUR ANSWER (2 minutes):

**Opening - Acknowledge the challenge:**
"Citation accuracy is one of the hardest problems in RAG systems. LLMs can hallucinate sources. Let me explain our approach and its limitations."

**Part 1 - How we tracked citations (1 minute):**
"Our citation system worked like this:

Step 1: When we retrieve chunks from the knowledge base, each chunk has metadata - the source PDF name, page number, and disease domain.

Step 2: We pass these chunks to the LLM with labels. For example: '[Source 1: GIGYF1 ASD.pdf]' followed by the text.

Step 3: In the prompt, we explicitly tell the LLM: 'Use citations like [Source 1] when you reference information.'

Step 4: After synthesis, we extract all citations from the text and map them back to the actual PDFs.

Step 5: We include a References section listing all sources with full details."

**Part 2 - How we validated accuracy (45 seconds):**
"We used manual spot-checking. I would read the synthesis, find a claim with a citation, then go to the source PDF and verify the claim was actually there.

For most cases, citations were accurate. The LLM correctly attributed information to the right source.

But we did find errors. Sometimes the LLM would cite Source 2 for information that came from Source 3. Or it would combine information from multiple sources and cite only one.

We improved this by being more explicit in the prompt: 'Only cite sources that directly support each claim. If multiple sources support a claim, cite all of them.'"

**Part 3 - Our limitations (15 seconds):**
"We didn't implement automated verification. That would require semantic similarity checking between claims and source passages. For an 8-week project, manual validation was our compromise."

---

### FOLLOW-UP 1: How did you link text back to source passages?

**YOUR ANSWER (1 minute):**

"We used the retrieval metadata. Every chunk has a unique ID and source information.

When we build the prompt, we include this metadata:
```
[Source 1: autism/GIGYF1 ASD.pdf, Page 3]
GIGYF1 mutations affect synaptic transmission...

[Source 2: autism/gene pairing_ASD.pdf, Page 5]
CHD8 is implicated in chromatin remodeling...
```

The LLM sees these labels and uses them in citations. Then we parse the output, extract citations like [Source 1], and create a References section mapping Source 1 back to the full PDF details.

What we didn't implement: Direct sentence-to-passage linking. That would require post-processing to match each generated sentence to the most similar source passage using embeddings."

---

### FOLLOW-UP 2: What about conflicting citations?

**YOUR ANSWER (45 seconds):**

"This happened occasionally. The LLM would cite different sources for the same fact.

Our approach was to let it happen. If Source 1 and Source 2 both support a claim, citing either one is acceptable. Medical research often has multiple studies confirming the same finding.

What we tried to prevent: Citing a source that doesn't support the claim. We did this through clear prompt instructions and by keeping context focused. If we only retrieve highly relevant chunks, the LLM has less chance to mis-attribute."

---

### FOLLOW-UP 3: Multiple papers saying similar things?

**YOUR ANSWER (45 seconds):**

"When multiple papers said similar things, we had two strategies.

Strategy 1: The LLM would cite all relevant sources. For example: 'GIGYF1 mutations are implicated in ASD [Source 1, Source 2, Source 3].'

Strategy 2: The synthesis would note the consensus. For example: 'Multiple studies confirm that CHD8 variants appear in approximately 0.5% of ASD cases.'

In the Disease Domain table, we would list each paper's contribution separately, even if they overlapped. This showed the depth of evidence - not just one paper saying something, but three or four."

---

### FOLLOW-UP 4: What was your error rate?

**YOUR ANSWER (45 seconds):**

"We didn't calculate a formal error rate, but from manual checking:

Approximately 85-90% of citations were accurate - the claim matched the source.

About 10-15% had minor issues - correct general idea but slightly different wording or context.

Less than 5% were clearly wrong - claim didn't match the source at all.

To improve this, we would need automated validation. For example, use a Natural Language Inference model to check if the source passage actually entails the claim. But that adds complexity and latency. For our use case - research synthesis, not clinical decision-making - manual spot-checking was acceptable."

---

## Question 7: Evidence-Level Validation

**Question:** You mentioned "evidence-level validation" across medical domains. In medical research, evidence quality varies widely (RCTs vs. case studies, peer-reviewed vs. preprints). How did you approach assigning and validating evidence levels? What framework did you use, and what challenges did you encounter?

### YOUR ANSWER (2 minutes):

**Opening - Explain the importance:**
"Evidence-level validation is critical in medical research. Not all studies are equal. A randomized controlled trial has more weight than a case study. Let me explain our approach."

**Part 1 - Our framework (45 seconds):**
"We used a simplified evidence hierarchy with three levels:

High evidence: Peer-reviewed studies from recognized journals. This includes randomized controlled trials, systematic reviews, and large cohort studies.

Medium evidence: Peer-reviewed studies but with limitations. For example, small sample sizes, observational studies, or single-institution studies.

Low evidence: Preprints, case reports, or studies with methodological concerns.

We included this in our Disease Domain table output: '| Disease Domain | Key Findings | References | Level of Evidence |'"

**Part 2 - How we assigned levels (45 seconds):**
"We used two methods:

Method 1: Metadata from the PDF. If we could extract publication information like journal name or DOI, we cross-referenced with journal rankings. High-impact journals got 'High' evidence level.

Method 2: LLM inference. We asked the LLM to analyze the paper and determine the study type. We included this in the synthesis prompt: 'For each finding, note the level of evidence based on study design.'

This was imperfect. The LLM sometimes misclassified studies. But it was better than treating all sources equally."

**Part 3 - Challenges (30 seconds):**
"The biggest challenge was incomplete information. Many PDFs don't clearly state their study design in the abstract. You need to read the methods section. But our chunks might not include the methods section when answering a specific question.

Our compromise: Use available information and be conservative. When uncertain, we labeled it as 'Medium' evidence rather than making assumptions."

---

### FOLLOW-UP 1: How did you extract evidence levels from papers?

**YOUR ANSWER (1 minute):**

"We had a two-step process.

Step 1: Metadata extraction. When processing PDFs, we tried to extract:
- Journal name from PDF metadata
- DOI if present
- Publication year
- Keywords like 'randomized controlled trial' or 'case report'

Step 2: LLM classification. During synthesis, we included instructions:
'Analyze each source and determine if it is a randomized controlled trial, observational study, systematic review, or case report. Include this in the Disease Domain table under Level of Evidence.'

The LLM would read the abstract and methodology chunks we retrieved and make a determination. This worked about 70-80% of the time. The rest required manual correction."

---

### FOLLOW-UP 2: What if papers didn't state their design clearly?

**YOUR ANSWER (45 seconds):**

"This was common. Many papers don't say 'this is an observational study' explicitly.

Our fallback strategy:
- Look for keywords: 'randomized,' 'double-blind,' 'retrospective,' 'prospective'
- Check sample size: Studies with thousands of participants usually have higher evidence level than studies with 10 participants
- When uncertain, mark as 'Medium' and let the user judge

We also included a disclaimer in the output: 'Evidence levels are estimated based on available information. Please verify study design before clinical application.'"

---

### FOLLOW-UP 3: How did you prevent treating all sources equally?

**YOUR ANSWER (45 seconds):**

"We built evidence awareness into the synthesis prompt.

The prompt included:
'When synthesizing findings, consider the strength of evidence. Give more weight to large, peer-reviewed studies. Mention when findings come from case studies or preprints. Note areas of strong consensus versus preliminary findings.'

In the output table, we explicitly show evidence levels. This makes it visible to users. They can see: 'This finding comes from a High-evidence RCT' versus 'This finding comes from a Medium-evidence observational study.'

This transparency lets users make informed decisions about which findings to trust."

---

### FOLLOW-UP 4: Did you disagree with the system's classification?

**YOUR ANSWER (45 seconds):**

"Yes, sometimes. The LLM would classify a well-designed cohort study as 'Low' evidence because it wasn't randomized. But in medical research, cohort studies can be very strong evidence.

When I saw this, I would either:
- Update the prompt to be more nuanced: 'Large cohort studies with proper controls are High evidence'
- Manually correct the classification for that specific paper
- Add examples in the prompt showing correct classifications

This was an iterative process. The more we tested, the better the classifications became. But it never reached 100% accuracy. Manual review was always necessary for critical applications."

---

## Question 8: IBM Design Thinking Application

**Question:** You applied IBM Design Thinking methodologies (Empathy Map, As-Is/To-Be Scenarios, Prototyping). Pick one of these—let's say the Empathy Map or As-Is/To-Be Scenarios—and walk me through how you actually used it. What insights did it reveal that surprised you, and how did it change what you built?

### YOUR ANSWER (2 minutes):

**Opening - Choose one framework:**
"I'll talk about the As-Is and To-Be scenarios because they directly shaped our product decisions."

**Part 1 - As-Is Scenario (what researchers do now) (45 seconds):**
"We interviewed medical researchers and mapped their current literature review workflow.

The As-Is scenario looked like this:
1. Researcher gets a research question
2. Searches PubMed, Google Scholar, maybe 3-4 databases
3. Gets hundreds of results, most irrelevant
4. Downloads 20-30 PDFs based on titles
5. Reads abstracts to filter down to 10 papers
6. Deep-reads 5-7 papers, taking notes
7. Manually writes a synthesis, trying to remember which fact came from which paper
8. Goes back to papers to add citations
9. Total time: 7+ hours per question

The pain points were clear: too much noise, too much manual reading, difficulty tracking sources."

**Part 2 - To-Be Scenario (with SynthMed) (45 seconds):**
"We designed the To-Be scenario to fix these pain points:

1. Researcher types a question into SynthMed
2. System routes to relevant disease agents
3. Each agent searches only relevant papers (no noise)
4. System returns structured synthesis with automatic citations in 30 seconds
5. Researcher reviews synthesis (30 minutes)
6. Researcher deep-dives into only 2-3 most relevant papers (1.5 hours)
7. Total time: 2 hours

The key improvements: eliminate noise, automate synthesis, automatic citations, focus reading time on only what matters."

**Part 3 - What surprised us (30 seconds):**
"The biggest surprise: Researchers cared more about citation tracking than synthesis quality.

I thought researchers wanted perfect summaries. But they told us: 'I can read papers myself. What I need is to find the RIGHT papers quickly and track where every fact comes from.'

This changed our priorities. We focused heavily on retrieval precision and citation accuracy, not just making the synthesis sound good."

---

### FOLLOW-UP 1: Specific user need you discovered?

**YOUR ANSWER (45 seconds):**

"One specific need: Cross-domain comparison.

During empathy mapping, a researcher said: 'I study cancer metabolism, but I want to know if similar pathways exist in neurodegenerative diseases. I don't have time to become an expert in dementia research.'

This led us to build the multi-agent architecture specifically for cross-domain queries. The starter prompts included: 'How do inflammatory pathways differ between cancer and neurodegenerative disorders?'

Without the design thinking research, we might have built separate, disconnected agents. Instead, we built an orchestrator that can coordinate across domains."

---

### FOLLOW-UP 2: Something you thought was important but users didn't care about?

**YOUR ANSWER (45 seconds):**

"Yes. I wanted to add confidence scores to each finding. Like: 'This claim is 87% confident based on 3 supporting papers.'

But when we showed this to researchers, they said: 'I don't trust these numbers. Just show me the papers and let me judge.'

We removed the confidence scores and instead focused on showing evidence levels (High/Medium/Low) and the number of supporting sources. Users found this more useful because it's interpretable - they can see the underlying reasoning."

---

### FOLLOW-UP 3: Balancing user feedback with technical constraints?

**YOUR ANSWER (45 seconds):**

"One example: Users wanted real-time PubMed integration.

They said: 'I don't want to be limited to your 27 papers. Let me search all of PubMed.'

Technically, this was possible. But it would add 5-10 seconds of latency and risk retrieving low-quality results.

Our compromise: We built the PubMed search as a separate tool. Users can invoke it explicitly when they need broader coverage. But the default workflow uses our curated knowledge base for speed and quality.

This balanced user needs with technical reality."

---

### FOLLOW-UP 4: What would you do differently next time?

**YOUR ANSWER (45 seconds):**

"I would involve users earlier and more continuously.

We did design thinking at the beginning, built the system, then showed it to users. This worked okay, but we had to make changes late in the project.

Next time, I would:
- Do weekly user testing sessions, not just at the start
- Build smaller prototypes and get feedback before investing in full implementation
- Create a beta testing group who uses the system for real work, not just demos

Continuous user feedback would have caught issues earlier and saved us time."

---

## Question 9: Integration Challenges with IBM Watsonx

**Question:** You integrated multiple components: IBM Watsonx Orchestrate, Granite model, Llama 3.2 90B, and external tools like PubMed. What was the hardest integration challenge you faced? Describe a specific technical problem where things weren't working, what debugging process you used, and how you ultimately solved it.

### YOUR ANSWER (2 minutes):

**Opening - Set up the problem:**
"The hardest integration challenge was getting the agent routing to work correctly with tool calls. Let me walk you through a specific problem and how I debugged it."

**Part 1 - The problem (45 seconds):**
"The issue: I created the main agent with five collaborators. When a user asked about autism, the main agent correctly identified it should route to the autism agent. But instead of calling the autism agent, it was trying to call a tool named 'autism_agent' that didn't exist. The error message said: 'Tool autism_agent not found.'

I was confused. The autism agent existed. It was imported and showed in the agent list. Why was the main agent looking for a tool instead of routing to the collaborator?"

**Part 2 - The debugging process (45 seconds):**
"Here's how I debugged it:

Step 1: Check the YAML configuration. The collaborators list was correct: `- synthmed_autism_agent`.

Step 2: Check the logs. The main agent's reasoning showed: 'User asked about autism. I will use the autism_agent tool.' It was treating collaborators as tools.

Step 3: Review IBM Orchestrate documentation. I found that agent collaboration syntax was different from tool calls. I needed to use specific language in the instructions.

Step 4: Update the main agent instructions to say: 'Route tasks to relevant sub-agents' instead of 'Call the autism agent.' The word 'route' triggered the collaboration behavior."

**Part 3 - The solution (30 seconds):**
"The solution was prompt engineering. The main agent needed explicit instructions about how to invoke collaborators. I updated the instructions to say: 'When a query relates to autism, route it to synthmed_autism_agent collaborator. Do not treat this as a tool call.'

After this change, routing worked correctly. The main agent would delegate to sub-agents, collect their responses, and synthesize them. This taught me that agent orchestration requires precise language."

---

### FOLLOW-UP 1: How did you handle API rate limits?

**YOUR ANSWER (1 minute):**

"We hit rate limits with PubMed. The NCBI API allows 3 requests per second without an API key, 10 requests per second with an API key.

Our solution had three parts:

First: We added delays. In the code, we added `time.sleep(0.34)` between requests to stay under the limit.

Second: We batched requests. Instead of fetching one paper at a time, we fetch up to 10 papers in a single API call.

Third: We made PubMed optional. The main workflow uses our local knowledge base. PubMed is only used when explicitly requested or for very recent research.

For IBM Watsonx, we didn't hit rate limits during development because the IBM program gave us sufficient quota. But for production, we would add error handling to retry with exponential backoff if we hit limits."

---

### FOLLOW-UP 2: Testing strategy for integrations?

**YOUR ANSWER (1 minute):**

"We used a layered testing approach.

Layer 1: Unit tests for individual functions. Test pdf extraction, chunking, PubMed search independently.

Layer 2: Tool testing. Import each tool to Orchestrate and call it directly with test inputs. Verify it returns expected JSON format.

Layer 3: Agent testing. Test each sub-agent individually with sample queries before integrating with the main agent.

Layer 4: End-to-end testing. Test the full flow: user query → main agent → sub-agent → tools → synthesis.

Layer 5: User acceptance testing. Have researchers try real queries and give feedback.

This layered approach helped us isolate problems. If Layer 4 failed, we knew Layers 1-3 worked, so the problem was in agent orchestration."

---

### FOLLOW-UP 3: Working around a platform limitation?

**YOUR ANSWER (1 minute):**

"One limitation: IBM Orchestrate's knowledge base chunking was a black box. We couldn't see exactly how it split our PDFs into chunks.

This made debugging retrieval issues difficult. When the wrong chunks were retrieved, we didn't know if the problem was chunk boundaries, embedding quality, or the query.

Our workaround: We built our own pdf_retriever tool with explicit chunking logic. This gave us visibility. We could see exactly what chunks were created and adjust the chunk size and overlap parameters.

We still used Orchestrate's built-in knowledge base for production because it was simpler. But having our own tool helped us understand and validate the chunking strategy."

---

### FOLLOW-UP 4: Managing different data formats?

**YOUR ANSWER (45 seconds):**

"Different components used different formats.

PubMed API returns XML. Our tools return JSON. Agents communicate in natural language. The knowledge base uses vectors.

Our solution: Standardize on JSON for all tool outputs. Each tool has a clear output schema:
```python
{
  'query': string,
  'results': list,
  'metadata': dict
}
```

The agent instructions specify: 'Tools return JSON. Parse the results and use them in your synthesis.'

This made integration predictable. We didn't need complex parsing logic. Everything followed the same pattern."

---

## Question 10: Reflection and What You'd Do Differently

**Question:** Now that you've completed the 8-week program and built SynthMed, reflect on the project. If you were starting over tomorrow with the same goal but no constraints, what would you do differently? What did you learn that would change your approach?

### YOUR ANSWER (2-3 minutes):

**Opening - Frame your growth mindset:**
"This project taught me a lot. Looking back, there are several things I would do differently. Let me share the most important ones."

**Part 1 - Better document processing (45 seconds):**
"I would invest more time in document processing upfront. Our simple chunking strategy worked, but we lost information from tables and figures. Medical papers have critical data in tables that we mostly ignored.

Next time, I would:
- Use a more sophisticated PDF library like LlamaParse or Unstructured
- Implement table extraction and convert tables to structured data
- Handle multi-column layouts properly
- Since we're using Llama Vision, actually process figures and charts

This would improve retrieval quality significantly. Right now, if the answer is in a table, we might miss it."

**Part 2 - Evaluation framework (45 seconds):**
"I would build an evaluation system from day one. We measured the 70% time reduction, but we didn't systematically measure retrieval quality or synthesis accuracy.

Next time, I would:
- Create a test dataset with questions and expected answers
- Measure retrieval precision: Are we getting the right chunks?
- Measure citation accuracy: Are citations correct?
- Track these metrics after every change

This would help us make data-driven decisions instead of relying on intuition. We'd know if changes actually improved quality."

**Part 3 - More user testing throughout (30 seconds):**
"I would involve users continuously, not just at the beginning and end. We did design thinking upfront, built for 6 weeks, then showed users. Some features they loved. Some they didn't care about.

Continuous user testing would have caught this earlier. Build a small feature, test with users, iterate. This is more efficient than building everything and then getting feedback."

**Part 4 - What we got right (30 seconds):**
"But some decisions were good. The multi-agent architecture scaled well. The decorator pattern for tools saved us time. Using Watsonx Orchestrate was the right call for speed. These worked well and I wouldn't change them."

---

### FOLLOW-UP 1: What technical decision do you wish you could revisit?

**YOUR ANSWER (45 seconds):**

"The chunking strategy. We used fixed 1000-character chunks. This is simple but not optimal for medical papers.

I wish we had implemented semantic chunking - split on section boundaries like Abstract, Methods, Results, Discussion. This would preserve document structure.

Or recursive chunking - split paragraphs first, then split large paragraphs. This would respect natural boundaries.

These are more complex to implement, but they would have improved retrieval quality. Users would get more coherent chunks instead of mid-paragraph cuts."

---

### FOLLOW-UP 2: What took longer than expected?

**YOUR ANSWER (45 seconds):**

"Prompt engineering for the main agent took much longer than expected. 

I thought: write instructions, test once, done. But getting consistent output format took many iterations. The agent would sometimes skip the Executive Summary. Sometimes the table format was wrong. Sometimes it gave medical advice despite instructions not to.

I spent almost 2 weeks just tuning the main agent prompts. I learned that prompt engineering is not a one-time task. It's iterative refinement based on edge cases and failure modes."

---

### FOLLOW-UP 3: What did you over-engineer or under-engineer?

**YOUR ANSWER (45 seconds):**

"Over-engineered: The PubMed search tool. We built full API integration with XML parsing, citation formatting, everything. But users barely used it because our local knowledge base was sufficient for most queries.

Under-engineered: Citation validation. We relied on manual spot-checking. For a production system, this isn't enough. We needed automated verification that citations match source passages.

The lesson: Validate user needs before building complex features. Invest in quality where it matters most - in our case, citation accuracy."

---

### FOLLOW-UP 4: If you had 8 more weeks, what are your top 3 priorities?

**YOUR ANSWER (1 minute):**

"Priority 1: Automated evaluation system. Build a test set of 100 questions with human-annotated answers. Measure retrieval quality, synthesis accuracy, and citation correctness. Track these metrics continuously.

Priority 2: Better document processing. Implement proper table extraction, multi-modal embeddings for figures, and semantic chunking. This would significantly improve retrieval quality for data-heavy queries.

Priority 3: Production hardening. Add caching to reduce costs and latency. Implement parallel sub-agent execution. Build monitoring dashboards. Add automated error recovery. Make the system production-ready for real researchers using it daily.

These would take SynthMed from a working prototype to a production system."

---

### FOLLOW-UP 5: Most valuable lesson you'll carry forward?

**YOUR ANSWER (1 minute):**

"The most valuable lesson: Start with the problem, not the technology.

At the beginning, I was excited about RAG, multi-agent systems, and big LLMs. I wanted to use cool technology.

But the design thinking process forced me to focus on user problems: too much noise in search results, difficulty tracking citations, 7 hours wasted on manual work.

Once I understood these problems deeply, the technical solution became clear. Multi-agent for precision. RAG for grounding. Structured output for usability.

This lesson applies to any project: Understand the problem first. Technology is just a tool to solve it. Don't fall in love with the technology. Fall in love with solving the problem.

This mindset shift will guide how I approach future projects."

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

---

## 📚 QUICK REFERENCE: Key Terms & Concepts

### Technical Terms You Should Know:

**RAG (Retrieval-Augmented Generation)**
- Simple explanation: "Find relevant documents, add them to the LLM prompt, generate answer based on those documents"
- Why it matters: "Grounds answers in real sources, not just LLM training data"

**Multi-Agent System**
- Simple explanation: "Multiple specialized AI agents working together"
- Your implementation: "One main coordinator, five disease-specific agents"

**Vector Database / Embeddings**
- Simple explanation: "Convert text to numbers so we can measure similarity"
- Why it matters: "Enables semantic search - finding relevant passages by meaning, not just keywords"

**Chunking**
- Simple explanation: "Splitting long documents into smaller pieces"
- Your implementation: "1000 characters per chunk, 200 character overlap"

**LLM (Large Language Model)**
- Simple explanation: "AI model trained on text to generate human-like responses"
- What you used: "Meta Llama 3.2 90B Vision"

**Orchestration**
- Simple explanation: "Coordinating multiple components to work together"
- Your tool: "IBM Watsonx Orchestrate"

**Decorator Pattern**
- Simple explanation: "The @tool syntax that turns Python functions into callable tools"
- Why you used it: "Automatically register functions with Orchestrate"

### Key Numbers to Remember:

- **27 papers** across 5 disease domains
- **83 MB** of medical literature (~675 pages)
- **~1,700 chunks** created from all papers
- **70% time reduction** (7 hours → 2 hours)
- **1000 characters** chunk size, **200 characters** overlap
- **5 disease agents**: Autism, Cancer, Dementia, Epilepsy, Rare Diseases
- **3 Python tools**: PDF retriever, PubMed search, LLM synthesizer
- **8-week** IBM AI Lab program

### Your Architecture in One Sentence:

"A hierarchical multi-agent RAG system where a main orchestrator coordinates five disease-specific agents, each with its own knowledge base, using Llama 3.2 90B to synthesize grounded medical research with automatic citations."

### Your Impact in One Sentence:

"Reduced medical literature review time by 70% by automating retrieval, synthesis, and citation tracking across 27 papers in 5 medical domains."

---

## ✅ FINAL CHECKLIST Before Your Interview

**Day Before:**
- [ ] Read through all 10 questions
- [ ] Practice your 3 strongest answers out loud
- [ ] Review the quick reference section
- [ ] Get a good night's sleep

**30 Minutes Before:**
- [ ] Review the key numbers (27 papers, 83 MB, 70% reduction)
- [ ] Practice your 2-minute elevator pitch for Question 1
- [ ] Take deep breaths, stay calm

**During Interview:**
- [ ] Listen carefully to the full question before answering
- [ ] Take 2-3 seconds to think before speaking
- [ ] Use the structure: Opening → Part 1 → Part 2 → Part 3
- [ ] If stuck, acknowledge and move on: "That's a detail I'd need to review"
- [ ] Show enthusiasm about what you learned!

**Remember:**
- It's okay to not be perfect
- Interviewers care more about your thinking process than perfect answers
- Showing what you learned is more important than pretending you knew everything
- Your English doesn't have to be perfect - clarity is what matters

**Good luck! You built something impressive. Show them how you think!** 🚀
