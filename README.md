# InsightBot: The Conversational BI Assistant

![Language](https://img.shields.io/badge/Language-Python-blue.svg) ![Framework](https://img.shields.io/badge/Framework-LangChain-green.svg) ![Model Provider](https://img.shields.io/badge/Model%20Provider-OpenAI-purple.svg) ![Database](https://img.shields.io/badge/Database-SQL-orange.svg)

An AI-powered agent that empowers non-technical business users to query complex SQL databases using natural language, getting instant answers without writing a single line of code.

This project demonstrates the power of LLM-powered agents to act as a "reasoning engine" for interacting with structured data. Using the LangChain framework, this agent can understand user questions, inspect a database schema, write its own SQL queries, and even self-correct when it makes a mistake.

## The Problem & The Opportunity

Business users need immediate data insights to make timely decisions, but they often lack the technical skills to query databases directly. This creates a dependency on data analytics teams, leading to significant delays and operational friction.

*   **User Persona:** Business Intelligence (BI) Analysts and Sales Managers.
*   **The Problem:** Obtaining answers to ad-hoc business questions (e.g., "Which customers are in the US?") requires filing a ticket with the data team, resulting in a 24-48 hour delay. This bottleneck slows down decision-making and limits the organization's ability to react quickly to new information.
*   **The Hypothesis:** We believe that an LLM-powered agent can democratize data access by translating natural language into SQL. This will reduce the time-to-insight from days to seconds and free up specialized data teams for more strategic, high-impact analysis.

## The Solution

This project is a functional prototype of an autonomous SQL agent that connects to a live SQLite database containing customer, agent, and order information.

### Core Features
*   **Natural Language Querying:** Users can ask complex questions about the business data in plain English.
*   **Autonomous SQL Generation:** The agent intelligently inspects the database schema to understand table structures and relationships before writing a syntactically correct SQL query.
*   **Self-Correction Loop:** When a query fails (e.g., due to a wrong table or column name), the agent analyzes the error message, re-inspects the schema using its tools, and automatically generates a corrected query to find the right answer.
*   **Direct Answer Generation:** The agent interprets the raw SQL output and provides a concise, final answer to the user's original question.

### Tech Stack Highlights
*   **Orchestration Framework:** LangChain Agents
*   **LLM (Reasoning Engine):** OpenAI (`gpt-3.5-turbo`)
*   **Agent Toolkit:** LangChain's `SQLDatabaseToolkit`
*   **Agent Type:** `ZERO_SHOT_REACT_DESCRIPTION`
*   **Database:** SQLite

### Agent Reasoning Loop

The agent uses a ReAct (Reason + Act) framework, allowing it to think step-by-step, choose a tool, observe the outcome, and refine its approach. This is critical for its self-correction ability.

## Results & Impact

This prototype successfully demonstrates the agent's ability to autonomously query a database and self-correct from errors.

*   **Primary KPI (User Value): Query Success Rate.** The agent successfully answered questions requiring table lookups, filtering (`WHERE` clauses), and aggregations (`COUNT(*)`).
*   **Key Outcome: Self-Correction.** When asked a question using the wrong table name (`customers`), the agent received an error, used the `sql_db_list_tables` tool to discover the correct name (`CUSTOMER`), and successfully re-executed the query. This demonstrates robust, autonomous problem-solving.
*   **Identified Limitation:** The agent initially misinterpreted a schema column in one query (mistaking an amount field for a phone number), showing that while powerful, zero-shot agents require well-described schemas or more advanced reasoning models for 100% accuracy.

## Learnings & Future Roadmap (What's Next?)

This project provides a strong foundation for a production-ready conversational BI tool and surfaces important strategic decisions.

### Key Learnings & Strategic Trade-offs
*   **Agent Architecture (Zero-Shot ReAct):** This framework is incredibly flexible and allows for dynamic, multi-step reasoning, which is superior to simple text-to-SQL chains. The trade-off is its reliance on the LLM's raw reasoning ability, which can be improved with more descriptive schemas or by fine-tuning.
*   **LLM Choice (Speed vs. Accuracy):** A model like GPT-3.5 provides fast responses but can make minor logical errors. For a production environment, it would be critical to evaluate more advanced models like GPT-4, which offer superior reasoning and SQL generation accuracy at the cost of higher latency and per-query expense.
*   **Security & Governance:** A production SQL agent would require a robust security layer to handle database authentication, enforce user-specific row-level permissions, and prevent malicious queries (e.g., `DROP TABLE`). This was out of scope for the prototype but is a critical next step.

### Future Roadmap
*   **V2: Connect to Production Databases:** Integrate the agent with enterprise databases like **PostgreSQL, Snowflake, or BigQuery** to prove its value on real-world data.
*   **V3: Few-Shot Prompting:** Improve accuracy for common query types by providing the agent with a few examples of "question-to-SQL" pairs in its prompt, guiding it toward more reliable query structures.
*   **V4: Caching & Performance:** Implement a caching layer to store results of frequently asked questions, reducing redundant queries, lowering costs, and providing instantaneous answers to common requests.

---

## Setup & Installation

To run this project locally, follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd <your-repository-name>
```mermaid
graph TD
    A[User Question] --> B{SQL
    ```

2.  **Create and activate a virtual environment:**
    

3.  **Install the required dependencies:**
    The notebook downloads `requirements.txt` on its own, but it's best practice to include it in the repo.
    ```bash
    pip install -r requirements.txt
    ```

4.  **Set up your OpenAI API Key:**
    You will need to have your OpenAI API key ready. The script will prompt you to enter it securely when you run the notebook cells.

5.  **Run the Jupyter Notebook:**
