# TDS Virtual TA Project

This project implements a Retrieval-Augmented Generation (RAG) API for the TDS course, acting as a virtual teaching assistant. It uses FastAPI, a local SQLite knowledge base, and OpenAI-compatible LLMs to answer student questions using course materials and forum discussions.

## Features
- **/query API**: Accepts a question and returns an answer with supporting links from the knowledge base.
- **Knowledge base**: Built from markdown files and downloaded Discourse forum threads.
- **Preprocessing**: `preprocess.py` ingests and chunks content, then generates embeddings for semantic search.
- **RAG pipeline**: Retrieves relevant context and generates answers using an LLM.
- **Promptfoo integration**: Automated evaluation of API responses against test cases.

## Project Structure
```
app.py                  # FastAPI app and RAG logic
preprocess.py           # Script to build and embed the knowledge base
knowledge_base.db       # SQLite database with content and embeddings
markdown_files/         # Course notes and documentation in markdown
downloaded_threads/     # Discourse forum threads (JSON)
project-tds-virtual-ta-promptfoo.yaml # Promptfoo test config
requirements.txt        # Python dependencies
```

## Setup
1. **Clone the repo and install dependencies:**
   ```sh
   git clone <repo-url>
   cd tds-project-v2-full-repo
   uv pip install -r requirements.txt
   ```
2. **Set your API key:**
   - Create a `.env` file with `API_KEY=sk-...` (OpenAI or compatible key)

3. **Prepare the knowledge base:**
   - Place markdown files in `markdown_files/` and Discourse JSON in `downloaded_threads/`.
   - Run:
     ```sh
     uv run preprocess.py
     ```

4. **Start the API server:**
   ```sh
   uvicorn app:app --reload --port 8000
   ```

5. **Test the API:**
   - Use Postman or `curl` to POST to `http://127.0.0.1:8000/query` with `{ "question": "your question" }`.

6. **Run promptfoo tests:**
   ```sh
   npx -y promptfoo eval --config project-tds-virtual-ta-promptfoo.yaml
   ```

## Example API Request
```
POST /query
{
  "question": "How do I submit GA 1?"
}
```

## Troubleshooting
- If answers are not relevant, re-run `preprocess.py` after updating markdown or threads.
- Ensure your API always returns `{ "answer": ..., "links": [...] }` for every request.
- For promptfoo errors, check your YAML and API logs for invalid JSON or missing fields.

## License
MIT
