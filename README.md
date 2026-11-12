# Customer Feedback Analyzer

A small AI-powered dashboard for turning customer reviews into structured, actionable signals. Paste reviews into the Streamlit app to see each review's sentiment, score, and main topic, along with a quick summary across the batch.

Built with Python, Streamlit, FastAPI, SQLite, and the Google Gemini API.

## What it does

- Analyzes each review as **positive**, **negative**, or **neutral**.
- Assigns a score from **1 to 5** and identifies a one-word theme, such as `delivery`, `taste`, or `service`.
- Processes up to four reviews concurrently and preserves their input order.
- Summarizes the average score, positive-review percentage, and most-mentioned theme.
- Saves analyses to a local SQLite database so they can be revisited later.
- Reports individual request failures without hiding which reviews could not be analyzed.

## Architecture

```text
Streamlit dashboard  ->  FastAPI service  ->  Google Gemini API
         |                     |
         +------ SQLite -------+
```

The dashboard sends reviews to the backend's `POST /analyze` endpoint. The backend requests structured JSON output from Gemini and returns the sentiment label, score, and theme. The dashboard stores saved results in `feedback.db`.

## Requirements

- Python 3.12+
- [uv](https://docs.astral.sh/uv/)
- A Google Gemini API key

## Run locally

1. Clone the repository and enter the project directory:

   ```bash
   git clone https://github.com/manasi1892/project-feedback-analyzer.git
   cd project-feedback-analyzer
   ```

2. Install the project dependencies:

   ```bash
   uv sync
   ```

3. Create a `.env` file in the project root and add your API key:

   ```env
   GOOGLE_API_KEY=your_google_api_key
   ```

   Keep `.env` private; it is excluded from Git.

4. Start the backend in one terminal:

   ```bash
   uv run fastapi dev api.py
   ```

   The API documentation is available at [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs).

5. In a second terminal, start the dashboard:

   ```bash
   uv run streamlit run app.py
   ```

   Streamlit will print the local URL to open in your browser.

## Project structure

| File | Purpose |
|---|---|
| `app.py` | Streamlit dashboard, concurrent API requests, and summary |
| `api.py` | FastAPI endpoint that calls Gemini |
| `database.py` | SQLite initialization, saving, and history retrieval |
| `sample_reviews.txt` | Example reviews for trying the app |

## Notes

- The backend must be running while using the dashboard.
- Each review makes a Gemini API request. Usage may incur charges based on your Google AI plan.
- The SQLite database is local to the machine running the app; it is ignored by Git and is not shared across deployments.
