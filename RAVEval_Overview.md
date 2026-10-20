# RAG Evaluation
RAG Evaluation is the systematic process of assessing the performance, accuracy, safety, and efficiency of a RAG system. RAG evaluation ensures that the system provides trustworthy answers grounded in your data, maintains a high-quality user experience, and runs cost-effectively.

## How to evaluate RAG application ?
### Eval Suite
1. Component-Level Evaluation - retreiver on golden labels and generator on hand fed golden context
    1. Retriever Evaluation
         * Recall@k: Measures the proportion of relevant documents successfully retrieved in the top K results out of all truly relevant documents in the database.
         * Precision@K: Measures the proportion of retrieved documents in the top K results that are actually relevant to the user query.
         * Mean Reciprocal Rank (MRR): Evaluates where the first relevant document appears in the retrieved list.
         * Normalized Discounted Cumulative Gain (NDCG): Accounts for both the relevance and the relative position of retrieved documents.
    2. Generator Evaluation
         * Faithfulness (Groundedness): Is the generated answer derived *only* from the provided context? Measures if the generator is hallucinating or using prior parametric knowledge.
         * Answer Relevance: Does the generated response directly address the user's query, regardless of whether it is factually correct?
         * Citation & Attribution: Does the model properly cite the specific source chunks or document segments used to construct each claim?
2. Pipeline-Level Evaluation - RAG Triad
    ```
              [User Query]
             /            \
    Context Relevance   Answer Relevance
           /                \
    [Retrieved Context] <--- Faithfulness ---> [Generated Answer]
    ```
    1.  Context Relevance: Evaluates if the retrieved chunks are relevant to the user query (filters out noise before sending to the LLM).
    2.  Faithfulness: Evaluates if the generated answer is strictly grounded in the retrieved chunks.
    3.  Answer Relevance: Evaluates if the generated answer actually answers the original user query.
3. Application-Level Evaluation
    1. Correctness - Compares the model's generated answer against a ground-truth "golden" answer.
    2. Completeness - Checks if the model addressed all parts of a multi-part query.
    3. Style - Tone rubric, G Eval
    4. Safety
         * Toxicity & Bias: Detecting offensive language or systematic prejudice.
         * PII Leakage: Ensuring no Personally Identifiable Information is leaked in outputs.
         * Jailbreak/Prompt Injection Resistance: Ensuring inputs do not bypass safety systems.
    6. Operations
        * Latency: Time to First Token (TTFT) and End-to-End Latency.
        * Cost & Tokens: Analyzing Input/Output token usage per query to balance performance against budget.
      
### Regression Testing
Running your eval suite on your application
#### How to Implement Regression Testing in RAG:
*   **Golden Dataset:** Maintain a curated set of 50–100+ representative query-context-response triplets.
*   **CI/CD Pipeline Integration:** Automatically trigger your evaluation suite whenever you update:
    *   The retrieval prompt or system instructions.
    *   The embedding model or vector database indexing logic.
    *   The underlying LLM (e.g., migrating from Gemini 1.5 Flash to Gemini 2.5 Flash).
*   **Assertion & Threshold Tests:** Set up hard barriers in your deployment pipelines

### Online Evaluation
1. Capture - latency, cost, escalation
2. computed - faithfulness, relevance on sample
3. correctness by proxy - 
4. Drift

### Recommended Tools and Libraries for RAG Monitoring & Evaluation
#### 1. End-to-End LLM Observability & Telemetry (Capture & Logging)
*   **LangSmith (by LangChain):** Excellent native integration with LangChain. Provides full trace visibility, prompt playground, and cost/latency tracking.
*   **Arize Phoenix / Phoenix:** An open-source AI observability platform that runs locally or in the cloud. It is highly optimized for tracing, visualizing embeddings, and calculating RAG metrics.
*   **Langfuse:** An open-source LLM engineering platform focusing on tracing, prompt management, and analytics.
*   **OpenInference (by Arize):** A standard specification and library to instrument your LLM applications using OpenTelemetry.

#### 2. Evaluation Frameworks (Computed Metrics)
*   **Ragas (Retrieval Augmented Generation Assessment):** The most popular open-source framework dedicated to evaluating RAG pipelines. Offers simple APIs to compute faithfulness, answer relevancy, and context recall.
*   **TruLens (TruEra):** Provides "feedback functions" to evaluate the RAG Triad and run evaluations programmatically in production.
*   **DeepEval (by Confident AI):** A unit-testing framework for LLM applications that operates like Pytest. Great for both CI/CD regression testing and production evaluation.

#### 3. Drift & Data Integrity Monitoring
*   **Evidently AI:** An open-source Python library to evaluate, test, and monitor ML models, now with robust support for text data and LLM system drift analysis.
*   **Whylogs (by WhyLabs):** An open-source library for profiling data in real-time, helpful for identifying shift/drift in embedding distributions at scale.
