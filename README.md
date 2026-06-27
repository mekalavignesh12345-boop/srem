# Fairness Guardian

**Build Fair AI Before You Build AI.**

Fairness Guardian audits a CSV dataset for fairness and data-quality risks *before*
you train a model. Upload a CSV and instantly get a Fairness Score, a Data Quality
Score, a bias heatmap, class-balance breakdowns, demographic-parity findings, and
actionable, plain-English recommendations — plus a downloadable PDF report.

## Features

- **Fairness Score (0–100)** with a risk level (Low / Medium / High / Critical),
  blending group representation and entropy so under-represented groups are surfaced.
- **Data Quality Score** covering missing values, duplicate rows, outliers (IQR),
  constant columns, and high-cardinality columns.
- **Bias heatmap** ranking sensitive attributes (gender, age, region, race, …) by imbalance.
- **Demographic parity** via [Fairlearn](https://fairlearn.org/) — base/selection rates
  per group for detected prediction targets.
- **Class balance** for detected target columns.
- **AI recommendation engine** (rule-based, offline): turns findings into concrete actions,
  e.g. *"Only 28% of records are Female — collect additional representative samples."*
- **PDF report export** with summary, charts, parity, and recommendations.

## Tech Stack

| Layer     | Tech                                            |
| --------- | ----------------------------------------------- |
| Frontend  | Next.js 14, React 18, Tailwind CSS, Recharts    |
| Backend   | FastAPI, uvicorn                                |
| Analysis  | pandas, NumPy, scikit-learn, Fairlearn          |
| Reporting | reportlab (PDF)                                 |

## Project Structure

```
backend/    FastAPI app (analysis engine, recommendations, PDF report) + tests
frontend/   Next.js dashboard (upload + visualizations)
```

## Getting Started

### Backend

```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

The API serves:

- `GET  /api/health` — health check
- `POST /api/analyze` — upload a CSV, returns the full analysis JSON
- `GET  /api/report/{report_id}/pdf` — download the PDF report

Generate a demo dataset (loan applications with intentional bias):

```bash
python scripts/make_sample.py   # writes sample_data/loan_applications.csv
```

### Frontend

```bash
cd frontend
npm install
npm run dev   # http://localhost:3000
```

Set `NEXT_PUBLIC_API_BASE` if the backend is not on `http://localhost:8000`.

## Development

```bash
# backend
cd backend && ruff check . && pytest

# frontend
cd frontend && npm run lint && npm run typecheck && npm run build
```
