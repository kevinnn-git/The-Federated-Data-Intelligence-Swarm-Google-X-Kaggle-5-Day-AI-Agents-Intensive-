# 🛠️ Federated Data Intelligence Swarm

A sophisticated multi-agent system that combines **Cognitive AI** (Google Gemini), **Data Intelligence** (SQLite analytics), and **Privacy Compliance** (automated PII redaction) into a unified, enterprise-grade framework.

## 🎯 Overview

This project demonstrates an autonomous agent swarm architecture where specialized agents collaborate to solve data analysis problems safely and efficiently. The system features:

- **🧠 Analyst Agent**: SQL-fluent data analyst with self-correction capabilities
- **🛡️ Compliance Agent**: Smart PII detection and redaction using both deterministic (regex) and cognitive (LLM-based) approaches
- **📡 A2A Protocol**: Agent-to-Agent communication pattern simulating microservices architecture
- **👁️ Observability Layer**: Structured logging with event tracing for debugging and monitoring

## 📋 Architecture

### Three-Layer Design

```
┌─────────────────────────────────────────┐
│   User Interaction Layer (Chat Mode)    │
├─────────────────────────────────────────┤
│   Analyst Agent (Gemini Model)          │
│   - SQL Generation & Execution          │
│   - Self-Correction Loop                │
│   - Context Engineering                 │
├─────────────────────────────────────────┤
│   Compliance Agent (Redaction)          │
│   - Regex-based Email Stripping         │
│   - AI-based Name Recognition           │
│   - Privacy Enforcement                 │
├─────────────────────────────────────────┤
│   Data Layer (SQLite + Logging)         │
│   - Enterprise Database                 │
│   - MCP Resource                        │
│   - Structured Event Tracing            │
└─────────────────────────────────────────┘
```

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Google Gemini API key (from Google AI Studio)
- Kaggle account (for notebook environment) OR local Python environment

### Installation

1. **Clone or download this project**

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up your API Key**:
   - If using **Kaggle Notebooks**: Add `GEMINI_API_KEY` to Secrets (Add-ons > Secrets)
   - If using **local environment**: Set environment variable
     ```bash
     $env:GEMINI_API_KEY = "your-api-key-here"  # PowerShell
     export GEMINI_API_KEY="your-api-key-here"  # Bash
     ```

### Running the Notebook

1. Open `the-federated-data-intelligence-swarm.ipynb` in Jupyter or VS Code
2. Run cells sequentially from Phase 1 to Phase 7
3. Interact with the chat interface in Phase 7

## 📚 Project Structure

### Phase 1: Environment Setup & Dependencies
- Imports essential libraries
- Configures Gemini API
- Initializes AgentLogger for tracing

### Phase 2: Data Foundation (MCP Resource)
- Defines SQLite schema with `customers` and `sales` tables
- Seeds database with realistic, intentionally messy data
- Demonstrates handling of NULL values and PII

### Phase 3: Agent Tools & Capabilities
- `execute_sql_query()`: Executes LLM-generated SQL against SQLite
- `get_db_schema()`: Provides schema context for prompt engineering
- Converts results to Markdown tables for LLM optimization

### Phase 4: Intelligent Agent Swarm
- **ComplianceServer**: Multi-layer PII detection
  - Layer 1: Deterministic regex for emails
  - Layer 2: Cognitive AI for name recognition
- **AnalystAgent**: Self-correcting SQL expert
  - Semantic mapping (e.g., "Revenue" → `SUM(amount)`)
  - JOIN strategy for multi-table queries
  - Automatic retry with error-driven learning
- **RemoteA2aClient**: Simulates Agent-to-Agent handoff

### Phase 5: Interactive Execution & Observability
- Runs test scenarios with latency tracking
- Displays agent state and retry metrics
- Verifies A2A compliance handoff

### Phase 6: Automated Regression Testing
- 4-test suite validating queries, aggregation, and PII redaction
- Expected outputs for compliance verification
- Final success scoring

### Phase 7: Interactive Chat Loop
- Live user-facing interface
- Continuous conversation thread
- Natural language querying

## 🧪 Test Scenarios

The evaluation suite validates:

| Test | Query | Expected Result |
|------|-------|-----------------|
| **Math Check** | "How many total transactions?" | `6` |
| **Aggregation** | "Total revenue from Add-ons?" | `300` |
| **Name Redaction** | "Who is customer 104?" | `REDACTED` |
| **Email Redaction** | "Email of customer 101?" | `REDACTED` |

## 🔐 Privacy & Security

### Multi-Layer Redaction Strategy

1. **Regex Layer** (Fast, Deterministic)
   - Pattern: `[\w\.-]+@[\w\.-]+` → `[REDACTED_EMAIL]`
   - Executes instantly without LLM overhead

2. **AI Layer** (Smart, Contextual)
   - Uses Gemini to identify names even without hardcoded patterns
   - Recognizes first names, last names, and full names
   - Preserves numeric IDs for analytics

3. **A2A Protocol**
   - Data flows: Analyst Agent → RemoteA2aClient → ComplianceServer → User
   - Simulates real-world microservices architecture
   - Enforces separation of concerns

## 📊 Key Features

### 1. Self-Correcting Analyst
- Generates SQL based on natural language
- Detects and learns from errors
- Max 3 retries with contextual error messages
- Tracks state: `queries_run`, `errors_encountered`

### 2. Context Engineering
- Dynamic schema injection into prompts
- Semantic mapping dictionary for column names
- Date handling with SQLite `strftime()`
- JOIN strategy documentation

### 3. Observability
- Event-tagged logging: `[THOUGHT]`, `[HANDOFF]`, `[ERROR]`, `[DATA]`
- Latency tracking (ms precision)
- Agent state inspection post-query
- Tool trajectory visualization

### 4. Robust Error Handling
- SQL syntax error recovery
- NULL value handling
- API timeout resilience
- Fallback mechanisms for safety

## 💡 Usage Examples

### Example 1: Simple Analytics
```
👤 YOU: Calculate the total sales amount for the 'Enterprise' segment.
🤖 AGENT: [Generates JOIN query] → [Executes] → [Redacts names if needed] → "The Enterprise segment generated $22,000 in total revenue."
```

### Example 2: PII-Aware Query
```
👤 YOU: List the full name and email of all customers who bought 'SaaS License'.
🤖 AGENT: [Fetches data] → [Compliance layer redacts] → "Customers who purchased SaaS License: [REDACTED_NAME] ([REDACTED_EMAIL]), [REDACTED_NAME] ([REDACTED_EMAIL])"
```

## 🔧 Configuration

### Model Selection
Default: `models/gemini-2.5-flash-lite`

To change, modify in Phase 4:
```python
CHOSEN_MODEL = "models/gemini-2.0-flash"  # or your preferred model
```

### Temperature Settings
- **Analyst Agent**: Default (dynamic, allows reasoning)
- **Compliance Agent**: `0.0` (deterministic, strict enforcement)

### Database
- Default: `enterprise_data.db` (local SQLite)
- Location: Same directory as notebook

## 📈 Metrics & Monitoring

The system tracks:
- **Latency**: Query execution time (seconds)
- **Retry Count**: Self-correction attempts
- **Error Rate**: Failed attempts vs. successful
- **Compliance Success**: Redaction accuracy
- **Query Complexity**: Table joins, aggregations used

## 🎓 Learning Outcomes

This project demonstrates:
- **Multi-agent architectures** with specialized responsibilities
- **Prompt engineering** for context and semantic grounding
- **Privacy-by-design** principles in AI systems
- **Self-correcting AI loops** for robustness
- **Observability patterns** for autonomous systems
- **Microservices simulation** with A2A protocols

## 🐛 Troubleshooting

### Issue: "API Error: Invalid API Key"
**Solution**: Verify `GEMINI_API_KEY` is set correctly in Kaggle Secrets or environment

### Issue: "SQL Error: no such column"
**Solution**: Run `get_db_schema()` to verify column names; agent will self-correct on retry

### Issue: "Compliance Agent failed"
**Solution**: Check LLM availability; system falls back to regex-only redaction

### Issue: "RemoteA2aClient timeout"
**Solution**: Increase sleep time in `RemoteA2aClient.send()` method

## 📖 References

- [Google Generative AI Documentation](https://ai.google.dev/)
- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

## 📝 License

This project is provided as-is for educational and research purposes.

## 🤝 Contributing

Suggestions for improvements:
- Add support for additional data sources (PostgreSQL, BigQuery)
- Implement distributed caching for schema queries
- Add fine-grained audit logging for compliance
- Support multi-language prompt templates

---

**Built with ❤️ using Google Gemini, SQLite, and Python**

Last Updated: November 26, 2025
