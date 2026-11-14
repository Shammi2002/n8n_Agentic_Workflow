
## Built a Multi-Tool AI agentic workflow using n8n !



https://github.com/user-attachments/assets/53b1780f-5773-47b0-9d4b-6ea0a3aafc5e

<img width="1919" height="1033" alt="hosted chat" src="https://github.com/user-attachments/assets/14b03fb6-082b-451a-9abb-91ceaaf1d748" />

A chatbot answers from training knowledge alone — no tools involved. A RAG bot adds a single retrieval step in front of the LLM. An agentic system is different: it has multiple distinct tools, each scoped to a specific task, and the LLM itself decides which one to call based on the question being asked.
That's the distinction I built around here. Instead of one large RAG pipeline handling every query the same way, I gave the agent multiple specialized tools and let it reason about which one to invoke for a given request — semantic search for open-ended questions, structured SQL lookups for precise numeric data, each tool scoped to exactly one job
Here's the design process I followed to get there:

1️⃣ Query scoping —  listed out the realistic user queries could ask, then classified them into distinct groups by data access pattern 

2️⃣ Tool design — One tool one specific job

3️⃣ Prompt engineering for tool descriptions — the hardest part wasn't the tools, it was describing them precisely enough for reliable routing. That meant writing negative guidance and handling empty query results explicitly to prevent hallucinating an answer.

4️⃣ Ensemble retrieval — semantic vector search and SQL querying both feed the same agent, so it can reason across unstructured and structured data depending on what the question actually needs.

From the outside it's a simple chat window. Underneath: a agentic pipeline, a database schema, embeddings, and vector similarity search working together.

The part I didn't expect to enjoy as much: Infrastructure. I containerized the workflow with Docker and deployed it on an EC2 instance (t2.micro) running Ubuntu for continuous operation. Small instance meant real constraints — it hit an out-of-memory (OOM) error under load, and rather than just paying for a bigger box, I set up a swap file to utilize the hard drive as temporary virtual RAM. This provided the necessary headroom to handle the workload spikes, successfully fixing the OOM crashes without touching the hosting cost
