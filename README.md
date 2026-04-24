# AWS Certification Practice Terminal

A local, offline-capable practice exam app for SAP-C02 (Solutions Architect Professional) and MLA-C01 (Machine Learning Engineer Associate). Built for my own study; extensible via JSON.

## What's in this build

- **90 questions per exam**, each with:
  - Original scenario-based question (not drawn from any specific third-party bank)
  - Four answer choices
  - Explanation of the correct answer AND why the distractors are wrong
  - A "decision pattern" — the generalizable insight behind the question
  - Three reference links (AWS doc + whitepaper/blog/prescriptive guidance) for learning more
- Weighted domain distribution roughly matching the official exam blueprint
- Three modes: timed full simulation, drill with immediate feedback, review-incorrect
- Confidence calibration (1–4 per question) with overconfident-wrong / underconfident-right flagging
- Keyboard shortcuts, navigator grid, flag for review, export results as JSON
- All data stays in your browser (localStorage for session history) — no telemetry

## Running it

**Important:** You cannot just double-click `index.html` to open it. Modern browsers block JavaScript's `fetch()` from loading local files (CORS / `file://` restrictions), and the app loads questions from external JSON files.

You need a local HTTP server. Pick one:

### Python 3 (probably already installed)
```bash
cd exam-prep
python3 -m http.server 8000
```
Then open `http://localhost:8000` in your browser.

### Node
```bash
cd exam-prep
npx http-server -p 8000
```

### Any other static file server
Any tool that serves files from a directory over HTTP. Even `caddy file-server` works.

If you see a message like "⚠ sap-c02.json could not load — serve via local HTTP server" on the exam-select screen, you opened it directly instead of through a server.

## Files

```
exam-prep/
├── index.html       The app (single file, ~900 lines)
├── sap-c02.json     SAP-C02 question bank (40 questions)
├── mla-c01.json     MLA-C01 question bank (40 questions)
└── README.md        This file
```

## Extending the question banks

Add more questions to either JSON file. The schema is:

```json
{
  "id": "sap041",
  "domain": "D1",
  "scenario": "Context paragraph(s) describing the scenario. \\n separates paragraphs.",
  "question": "The actual question being asked.",
  "choices": [
    "Option A text",
    "Option B text",
    "Option C text",
    "Option D text"
  ],
  "correct": 2,
  "explanation": "Why the correct answer is correct, and critically why the distractors are wrong.",
  "pattern": "The generalizable decision pattern — the lesson that transfers to other questions.",
  "note": "Optional — for service-deprecation warnings or other caveats.",
  "references": [
    { "type": "doc", "title": "Service documentation page", "url": "https://docs.aws.amazon.com/..." },
    { "type": "whitepaper", "title": "Relevant whitepaper", "url": "https://docs.aws.amazon.com/whitepapers/..." },
    { "type": "blog", "title": "AWS blog post", "url": "https://aws.amazon.com/blogs/..." }
  ]
}
```

Fields:
- `id`: unique string, any format
- `domain`: `"D1"`, `"D2"`, `"D3"`, or `"D4"` — determines weighting and filtering
- `correct`: zero-indexed into `choices` (so 2 = option C)
- `references[].type`: one of `doc`, `whitepaper`, `blog`, `video` — determines color-coded badge
- `note` is optional; everything else is required

## Caveat on reference links

The reference URLs use stable root patterns (`docs.aws.amazon.com`, `aws.amazon.com/whitepapers`, AWS blog roots) that rarely break. However:

- AWS occasionally restructures deep links to specific documentation sections
- Some blog posts move to `/archive/` URLs over time
- Whitepaper redirect URLs sometimes change when AWS updates the document

If a link 404s, it's usually findable with a quick search of the link title. The canonical service root pages (e.g., `docs.aws.amazon.com/sagemaker/`) are the most durable and are used whenever possible.

## Caveats on the questions themselves

- **Not endorsed by AWS.** These are original questions I built. Any resemblance to actual exam content is coincidence — I don't have exam content, and if I did I wouldn't redistribute it.
- **Some questions cite services that are being deprecated or changed.** Where I know of these, I've added `note` fields (e.g., AWS App Mesh deprecation, Amazon Forecast new-signup restrictions). The AWS service portfolio shifts frequently.
- **40 questions isn't a full bank.** For deeper preparation, combine this with official AWS practice (Skill Builder) and an established provider (Tutorials Dojo, Stephane Maarek practice exams on Udemy, etc.).
- **Questions target the decision patterns I personally want to drill** — multi-account governance, DR tier selection, migration strategy for SAP-C02; SageMaker inference options, Clarify/Model Monitor, feature engineering for MLA-C01. Other exam topics are represented but with less depth.

## Confidence calibration — how to read the results

During questions, click 1 (guessing) through 4 (confident) before moving on. On the results screen, the "Confidence calibration" section flags two patterns:

- **Overconfident and wrong** (high confidence 3–4, wrong answer): your mental model of this topic is actively misleading you. Highest-leverage to review.
- **Underconfident but right** (low confidence 1–2, right answer): you probably know this but don't trust your gut. Not a study priority — just build confidence.

Review the overconfident-wrong set carefully. These are the questions where studying yields the biggest score gains.

## License

MIT. Do what you want with it.
# Exam-Prep
