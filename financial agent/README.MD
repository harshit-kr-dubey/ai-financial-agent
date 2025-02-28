# Agent Team: Web Search and Financial Data Retrieval

## Description

This project demonstrates the creation of a team of agents using the `phi` library. The agents are designed to perform web searches and retrieve financial data, combining their capabilities to handle complex tasks. The team consists of:

1. **Web Agent**: Performs web searches using DuckDuckGo and retrieves the latest news.
2. **Finance Agent**: Fetches financial data (e.g., stock prices, analyst recommendations, and company info) using `YFinanceTools`.
3. **Agent Team**: Combines the capabilities of the Web Agent and Finance Agent to handle tasks that require both web search and financial data retrieval.

---

## Application

The `agent_team` can be used to perform tasks such as:

- Summarizing analyst recommendations for a specific stock.
- Fetching the latest news about a company.
- Combining financial data and news to provide comprehensive insights.

### Example Use Case

To summarize analyst recommendations and fetch the latest news for NVIDIA (NVDA), run the following code:

```python
from phi.agent import Agent
from phi.model.groq import Groq
from phi.tools.duckduckgo import DuckDuckGo
from phi.tools.yfinance import YFinanceTools
from dotenv import load_dotenv

load_dotenv()

# Define the Web Agent
web_agent = Agent(
    name="Web Agent",
    model=Groq(id="llama-3.3-70b-versatile"),
    tools=[DuckDuckGo()],
    instructions=["Always include sources"],
    show_tool_calls=True,
    markdown=True
)

# Define the Finance Agent
finance_agent = Agent(
    name="Finance Agent",
    role="Get financial data",
    model=Groq(id="llama-3.3-70b-versatile"),
    tools=[YFinanceTools(stock_price=True, analyst_recommendations=True, company_info=True)],
    instructions=["Use tables to display data"],
    show_tool_calls=True,
    markdown=True,
)

# Define the Agent Team
agent_team = Agent(
    model=Groq(id="llama-3.3-70b-versatile"),
    team=[web_agent, finance_agent],
    instructions=["Always include sources", "Use tables to display data"],
    show_tool_calls=True,
    markdown=True,
)

# Query the Agent Team
agent_team.print_response("Summarize analyst recommendations and share the latest news for NVDA", stream=True)


#EXAMPLE OUTPUT
### Analyst Recommendations for NVDA

| Metric               | Value          |
|----------------------|----------------|
| Average Recommendation | Buy           |
| Number of Analysts   | 35             |
| Strong Buy           | 20             |
| Buy                  | 10             |
| Hold                 | 5              |
| Sell                 | 0              |
| Strong Sell          | 0              |

### Latest News for NVDA

1. **NVIDIA Announces Breakthrough in AI Chip Technology**  
   Source: [TechCrunch](https://techcrunch.com)  
   Summary: NVIDIA has unveiled a new AI chip that promises to revolutionize machine learning workloads...

2. **NVIDIA Stock Hits All-Time High Amid AI Boom**  
   Source: [Bloomberg](https://www.bloomberg.com)  
   Summary: NVIDIA's stock price surged to a record high as demand for AI-related products continues to grow...

3. **NVIDIA Partners with Major Automaker for Self-Driving Technology**  
   Source: [Reuters](https://www.reuters.com)  
   Summary: NVIDIA has entered into a partnership with a leading automaker to develop next-generation self-driving car technology...