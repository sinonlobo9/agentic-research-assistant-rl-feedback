# Agentic Research Assistant with RL Feedback (CrewAI)

This project is a **multi-agent research assistant** built with **CrewAI** and the CrewAI Studio visual editor.

Given a natural-language research question, the system:

- Decomposes it into sub-questions (controller agent),
- Searches and scrapes the web for relevant sources,
- Synthesizes and analyses the evidence,
- Generates a **structured Markdown research report** with:
  - Title & abstract  
  - Table of contents  
  - Main sections  
  - Key takeaways  
  - Limitations & future work  
  - References (using a custom citation tool)

It also computes a simple **RL-style reward** per run to help evaluate performance (based on number of sources and answer length).

---

## Project Link (CrewAI Studio)

If your instructor wants to see the visual setup, the CrewAI Studio project is here:

> https://app.crewai.com/studio/v2/projects/d5cffe69-c86f-427d-aee7-525cc89b2104/editor

---

## Architecture Overview

The system uses one controller and three specialist agents:

- **Controller (Research Orchestrator)**  
  - Takes the user’s query  
  - Breaks it into 3–6 sub-questions  
  - Produces a short research plan

- **Web Research Specialist**  
  - Uses web tools to find and scrape sources  
  - Returns a list of sources as JSON (title, URL, snippet)

- **Evidence Synthesizer and Analyst**  
  - Reads the sources + plan  
  - Produces key findings and a synthesis section in Markdown

- **Technical Research Writer**  
  - Takes the analysis + sources  
  - Uses a **custom `CitationBuilderTool`** to format references  
  - Outputs the final Markdown research report + suggested filename

### Tools

**Built-in tools:**

- `SerplyWebSearchTool` – web search (requires `SERPLY_API_KEY`)  
- `ScrapeWebsiteTool` – fetch and extract content from URLs  
- `FileReadTool` – read intermediate files

**Custom tool:**

- `CitationBuilderTool` (in `src/agentic_research_assistant_with_rl_feedback/tools/custom_tool.py`)  
  - Input: list of sources (title, URL, snippet)  
  - Output: formatted, numbered **References** section in Markdown  
  - Has input validation and safe fallback when sources are missing

---

## Requirements

- **Python**: `>= 3.10, < 3.14` (tested with 3.11)
- `pip` (standard Python package manager)
- `uv` (used by CrewAI for dependency management)
- API keys:
  - `OPENAI_API_KEY` (for LLM calls)
  - `SERPLY_API_KEY` (for web search)

---

## Installation

From the project root (the folder with `pyproject.toml` and `src/`):

```bash
# 1. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate    # On Windows: .venv\Scripts\activate

# 2. Install uv (once)
pip install uv

# 3. Install dependencies via CrewAI
crewai install
