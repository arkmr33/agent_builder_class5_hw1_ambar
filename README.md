# SpaceX Agent Homework

This project implements an NL-to-SQL + multi-tool SpaceX agent using LangChain and OpenAI/Groq.

The agent can:
- Query a SQLite launches database
- Look up rocket specifications from JSON
- Decide dynamically which tool(s) to use

---

# Setup

## Install dependencies

```bash
uv sync
```

or

```bash
pip install -r requirements.txt
```

## Create environment file

Create `.env`

```env
OPENAI_API_KEY=your_key_here
```

## Run notebook

Open:

```bash
agent.ipynb
```

Run all cells.

---

# Files

- `agent.ipynb` — main notebook
- `spacex_launches.db` — SQLite launch database
- `vehicle_specs.json` — rocket specs
- `runs.md` — saved agent traces

---

# What I Learned

## 1. Pipeline vs Agent

The pipeline worked well for simple SQL-only tasks like counting successful launches because the workflow was predictable and reliable. However, the agent performed much better on multi-source questions. For example, when asked about the thrust of the rocket used for crewed missions, the agent first queried the SQL database to identify the rocket and then used the JSON lookup tool to retrieve the thrust. A pipeline could not easily chain these two data sources together dynamically.

## 2. Where the Agent Surprised Me

The most interesting behavior was when the agent recovered from an incorrect assumption. When I asked how many missions flew on reusable rockets, it initially queried a nonexistent `reusable` column and got an SQL error. Instead of failing completely, it reasoned about which vehicles are reusable and retried with a different query. I did not expect the recovery behavior to work that well.

## 3. What I Changed

I kept most of the workbook structure unchanged but experimented with different questions to observe the tool-calling behavior. I also tested ambiguous prompts and failure scenarios to better understand the agent loop. I additionally observed how increasing the number of tools increases the possibility of unnecessary tool calls.

## 4. What I Would Do Differently

Next time, I would add memory and retrieval support so the agent could answer questions across multiple conversations and use unstructured documents. I would also improve SQL validation because the current validator only blocks dangerous keywords and does not fully validate query correctness.