### Turning chaotic inboxes into structured, automated order & inquiry workflows

Retail operations look deceptively simple from the outside: customers email, staff respond, orders get booked, stock gets updated. Under the hood, it’s usually an unstructured mess—especially for small or mid-size fashion brands that haven’t fully automated their digital operations. The result is slow responses, lost orders, inconsistent inventory updates, and frustrated customers.

This project is a practical proof-of-concept that shows how LLMs can be deployed as real workers in this workflow. Not abstract magic. Actual operational logic.

#### The goal:  
Automatically read incoming emails, understand whether they are **order requests** or **product inquiries**, extract structured data, check stock, update inventory, and generate responses—end to end.

Everything runs through a modular chain of LLM-based “micro-agents,” each responsible for a small, verifiable task. This keeps the system interpretable instead of becoming a mysterious black box.

## Technical Overview: How the System Works

The design intentionally avoids monolithic “one giant prompt” architectures. Instead, each function is a narrow expert—an independent agent.

Processing happens **sequentially**, not in batches, to maintain inventory correctness. When two emails request the same product, sequential order ensures one stock update happens before the next evaluation.

Everything flows through a deterministic pipeline:

``` mermaid
flowchart TD

    A[llm_classify_email] --> B[email-classification log]

    B --> C{Is Order Request?}

    %% Order Request Path
    C -->|Yes| D[llm_extract_order_items]
    D --> E[process_order_requests_pipeline]
    E --> F[llm_generate_order_response]
    F --> G[order-response log]

    %% Product Inquiry Path
    C -->|No, Product Inquiry| H[answer_inquiry_with_stock - RAG and stock lookup]
    H --> I[inquiry-response log]

    %% Final Step
    G --> J[Write all logs]
    I --> J[Write all logs]
```

This structure makes the system easy to reason about, easy to debug, and easy to extend.
