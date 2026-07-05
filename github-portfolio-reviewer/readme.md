# GitHub Portfolio Reviewer

A fast, lightweight portfolio auditor for GitHub profiles. This FastAPI application scans a GitHub account, evaluates repositories for best practices, generates improvement recommendations, and can create professional README content for individual projects.

## Key Features

- Scan GitHub profiles and public repositories
- Detect missing files like `README.md`, `LICENSE`, `.gitignore`, `CONTRIBUTING.md`
- Identify repository quality signals such as tests, CI configuration, and Docker support
- Score portfolio quality using heuristic rules
- Provide structured improvement recommendations and project ideas
- Generate polished repository README content using Gemini AI when available
- Cache scan results locally using SQLite

## Technology Stack

- Python 3.x
- FastAPI
- SQLAlchemy
- Pydantic
- HTTPX
- Google Gemini generative AI (`google-generativeai`) optional
- SQLite for persistence

## Installation

1. Clone the repository

```bash
git clone <repo-url>
cd github-portfolio-reviewer
```

2. Create and activate a Python virtual environment

```bash
python -m venv venv
venv\Scripts\activate
```

3. Install dependencies

```bash
pip install -r requirements.txt
```

## Configuration

Create a `.env` file in the project root to set optional environment values:

```env
DATABASE_URL=sqlite:///./portfolio_reviewer.db
GEMINI_API_KEY=
GITHUB_TOKEN=
PORT=8000
```

- `GEMINI_API_KEY` enables AI-powered portfolio auditing and README generation.
- `GITHUB_TOKEN` improves GitHub API rate limits and reliability.

## Running the Application

Start the FastAPI server with Uvicorn:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

Then open the API documentation at:

```text
http://127.0.0.1:8000/docs
```

## API Endpoints

### Scan GitHub Portfolio

- `POST /api/portfolio/scan`
- Request body:
  - `username`: GitHub username to scan
  - `github_token` (optional)
  - `gemini_api_key` (optional)

This endpoint scans the GitHub profile, audits repository structure, and saves a portfolio analysis to the local database.

### Retrieve Cached Analysis

- `GET /api/portfolio/{username}`

Fetches a cached portfolio analysis for the specified GitHub username.

### Get Scan History

- `GET /api/history`

Returns the latest scanned profiles and summary metrics.

### Generate README for a Repository

- `POST /api/projects/readme`
- Request body:
  - `username`
  - `repo_name`
  - `custom_instructions` (optional)
  - `gemini_api_key` (optional)

Returns a generated README markdown for the specified repository.

## Database

This project uses SQLite by default. The local database file is named `portfolio_reviewer.db`.

## Project Structure

- `app/main.py` — FastAPI application entry point and API routes
- `app/github.py` — GitHub data retrieval and repository auditing code
- `app/heuristics.py` — Rule-based scoring and recommendation engine
- `app/llm.py` — Optional Gemini AI integration for auditing and README generation
- `app/config.py` — Configuration with environment variables
- `app/database.py` — SQLAlchemy database configuration
- `app/models.py` — SQLAlchemy ORM models
- `app/schemas.py` — Pydantic request and response schemas
- `static/` — Frontend assets and UI files

## Notes

- Gemini AI support is optional; if no API key is provided, the application uses local heuristics.
- The app scans up to 12 repositories per profile to maintain performance and avoid API rate limits.
- Use a GitHub personal access token when scanning larger portfolios or if the API returns rate limit errors.

## License

This project does not include an explicit license file.

## Contact

For questions or improvements, update the repository README or reach out through the project source.
