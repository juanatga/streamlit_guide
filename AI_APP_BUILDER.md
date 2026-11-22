# Instructions for the AI assistant: build a Streamlit app from this capstone

You are helping a data science student turn the capstone notebooks in this repository into a public Streamlit app. The app will be deployed on **Streamlit Community Cloud** from the **`main` branch** of the student's GitHub repository. The repository is public and must stay tidy: it has **one branch only (`main`)**, and **all app files live in one folder, `app/`**. The student does the Streamlit account, GitHub connection and deployment steps by hand. Your job is everything between those steps: design the app with the student, build it, test it, and hand over the deploy settings.

The student is still learning. They own the design decisions, and you own the engineering. Explain briefly what you do and why, in plain language.

## How to work

- Go through the stages **in order**: A → B → C → D → E, and F only if needed. Don't start a stage until the one before it is finished.
- At every **⏸ STOP** point, end your message and wait for the student's answer.
- In Stage B, ask **one question at a time**. Always give **numbered suggestions** based on this project, and mark the one you recommend. If the student says "I don't know" or "you choose", pick your recommendation, explain it in one sentence, and move on. Never leave the student stuck.
- **One branch only.** Work on `main`. Don't create other branches; if a `streamlit-app` or similar branch exists, tell the student and don't use it. Before you start, run `git pull` so `main` is up to date.
- **One folder only.** Create everything the app needs inside `app/`. See *Where files go* below for the only exceptions. Every push to `main` redeploys the live app, so only push code that passes the tests.
- Ask before every commit and every push.
- Don't change the student's notebooks or their original data files.
- Don't invent results. Every number the app shows must come from the student's data and code.
- Start each stage with a heading like `Stage B: Design (question 2 of 4)`, so the student always knows where they are.
- **Before Stage A, check that you can read files and run commands.** Run `git status` and `python --version` (or `python3 --version`). If you can't run commands, stop and tell the student: "Open this repository folder in VS Code and use the assistant in Agent mode. See Step 0.6 of SETUP_GUIDE.md."
- **Check the student's operating system** and use the right commands for it:
  - **Windows:** `python` or `py`; activate a virtual environment with `.venv\Scripts\activate`.
  - **macOS and Linux:** `python3`; activate with `source .venv/bin/activate`.
- **If a command fails** because Git or Python is missing or too old, point the student to Steps 0.2–0.3 of SETUP_GUIDE.md instead of working around it.

---

## Where files go

The repository already has its own structure (for example `data/`, `notebooks/`, `docs/`, `README.md`). Keep it. Add the app like this:

| What | Where | Why |
| --- | --- | --- |
| All app code, pages, tests, app requirements, design document, saved models | `app/` | Keeps the app in one place |
| Streamlit theme and settings | `.streamlit/config.toml` at the repository root | Streamlit Cloud only reads the config file from the root. It's a hidden folder, so the root still looks tidy |
| Smaller processed data made for the app (if needed) | the repository's existing data folder, for example `data/processed/` | Data stays with data. Don't duplicate files that are already there |
| Link to the app and how to run it | a short **"Interactive app"** section in the existing root `README.md` | Visitors find the app from the front page |
| `.venv/`, `__pycache__/`, `.pytest_cache/`, `.DS_Store` | added to the existing root `.gitignore` (create it if missing) | Keeps generated files out of the repository |

Don't add any other files or folders at the repository root. Don't touch an existing root `requirements.txt`: the app uses its own `app/requirements.txt`, which Streamlit Cloud reads first because it sits next to the entry file.

**Never commit `AI_APP_BUILDER.md`** (this file). The student keeps it at the root while you work and deletes it at the end. If the student asks, add it to `.git/info/exclude` so it never shows up in `git status`.

---

## Stage A: Understand the project and check it can be deployed

1. **Read the project:**
   - the README
   - every notebook, in its intended run order
   - any `.py` exports
   - `requirements.txt`, if there is one
   - the data folder: file names, sizes and formats

   Work out:
   - the question the project answers
   - the data: source, rows, columns, time span
   - the methods and models used, with their parameters and random seeds
   - the **headline results**: the 3–5 numbers or findings that matter most, quoted exactly as the notebooks or README state them

2. **Check for deployment blockers.** For each one you find, plan a fix:

   | Check | Why it matters | Fix |
   | --- | --- | --- |
   | Paths like `/content/`, `drive.mount`, `C:\…`, `/Users/…`, `~` | They don't exist on the server | Paths relative to the repo, using `Path(__file__)` |
   | File-name case differs from the code (`Data.csv` vs `data.csv`) | The server runs Linux, which is case-sensitive, unlike Mac and Windows | Match the case exactly |
   | Any file over 50 MB, or the data needed adds up to over ~200 MB | GitHub rejects files over 100 MB; large data slows start-up and can exceed the app's memory | A smaller processed file: only the needed columns, filtered or aggregated, saved as Parquet or CSV in `data/processed/` by a script in `app/scripts/` |
   | Data downloaded at runtime from Kaggle, an API or Google Drive | Needs keys, may be blocked, slow | Commit a processed copy if the license allows it |
   | Passwords, API keys, tokens, personal data | The repository and app are public | Remove them; tell the student |
   | Data license unclear | Publishing may not be allowed | Ask the student to confirm (see the STOP point below) |
   | Heavy packages (`tensorflow`, `torch`, `prophet`, `pycaret`) | Slow or failed builds | Use them only if the app needs them at runtime; prefer saved predictions or a lighter model artifact |
   | Model training takes longer than ~20 s | Slow cold starts | Train once with a script, save with `joblib` to `app/models/`, and pin the exact library versions |
   | Notebook errors, undefined variables, cells that depend on run order | The extracted code will break | Fix them in the extracted code, not in the notebook, and note the fix |

3. **Report to the student** in this format:
   - **Your project in 5 lines:** the question, the data, the method, and the headline results with exact numbers.
   - **Deployment blockers found**, each with its proposed fix, or "none".
   - **Decisions needed from you:** for example, whether the data may be published, or which notebook is final when two versions exist.

**⏸ STOP:** ask the student to correct the summary and answer the decisions.

---

## Stage B: Design the app with the student

The goal is an app designed for **one real person with a real task**, not a copy of the notebooks. Ask the four questions below, **one at a time**. Base every suggestion on this project's domain, data and results. Generic suggestions don't help.

### Question 1: Who is the app for? (persona)

Explain: *"A persona is the one person the app is built for. Picking one person keeps the app focused."*

Offer three to four personas for this project, each written as:

> **As a** [specific role, in a specific place], **I need to** [the decision or task they face], **because** [what is at stake for them].

Example of the right level of detail: *"As a maritime inspector at the Port of Bar, I need to decide which arriving vessels to inspect first, because I can only inspect a few ships per day."*

Use these four types to build your suggestions:

| Type | Typical need | Example roles |
| --- | --- | --- |
| Decision-maker | The answer and how confident to be in it | ministry analyst, operations manager, NGO programme lead |
| Practitioner | A tool to use in daily work | inspector, engineer, loan officer, doctor |
| Analyst | To explore the data themselves | researcher, data journalist, consultant |
| Informed public | To understand what is going on | journalist, student, citizen |

Rules:
- The instructor, examiner or "anyone" is **not** a persona.
- Allow at most **one** secondary persona, and only if it doesn't change the page order.

**⏸ STOP:** wait for the choice.

### Question 2: What does the persona want to do? (user stories)

Explain: *"A user story is one thing the persona wants to do with the app, and why."* Format:

> **As a** [persona], **I want to** [action in the app] **so that** [outcome in their work].

1. Suggest **6–8 stories** that the project's data and results can actually support.
2. Ask the student to pick **3–5**, and to mark each one **Must** (in the first version) or **Later**.
3. For each Must story, draft one acceptance check:

   > **Done when:** on [page], the user can [do X] and sees [Y].

   Make Y a concrete, checkable item, such as a specific number, chart or table.

Stories that only describe the notebook (*"I want to see the correlation heatmap"*) are a warning sign. Rewrite them around the persona's goal (*"…so that I know which factors to watch"*), or drop them.

**⏸ STOP:** wait for the selection and priorities.

### Question 3: What does the persona see first, next and later? (order)

Explain: *"People decide in about 10 seconds whether an app is useful. The first screen must tell the persona what the app is for and give the main answer."*

Offer these layouts, adapted to the project. Recommend the one that fits the persona type.

```
A. Answer first (best for decision-makers)
   1. Overview: the question, the headline answer, 2–4 key numbers
   2. Evidence: charts that support the answer, with filters
   3. Model / forecast / prediction: how good it is, what it says
   4. Method and limits

B. Tool first (best for practitioners)
   1. Tool: inputs → prediction or score, with a plain-language reading
   2. Why: the main drivers, and how good the model is
   3. Data: context and exploration
   4. Method and limits

C. Story (best for the informed public)
   1. Context: why this matters
   2. What the data shows: 2–3 key charts, each with a one-sentence takeaway
   3. What the model says
   4. What it means and what it can't tell us
```

Then agree the **order within each page**:
1. **Top:** the answer, as one sentence or a metric.
2. **Middle:** the chart that supports it.
3. **Bottom:** details, such as the table, the download, or an expander with the method.

Every page must serve at least one Must story. Anything that serves none goes to **Not in this version**.

**⏸ STOP:** wait for the choice and any changes.

### Question 4: What can the persona change, and what must they not misread?

1. Suggest the **controls** that matter for the stories: filters, date range, model choice, prediction inputs, scenario sliders. Give each a sensible default, so the first view is useful without any clicks.
2. Suggest the **warnings**: the main limits of the data or model that this persona could misread. For example: predictions are not certainties; correlation is not cause; the data covers only a given period or region.
3. Suggest **words to avoid or explain** for this persona, such as RMSE, R², recall or lag. Give the plain wording to use instead.

**⏸ STOP:** wait for confirmation.

### Write `APP_DESIGN.md`

Create `app/APP_DESIGN.md` using this template, then show it to the student.

```markdown
# App design: <app title>

## Persona
As a <role>, I need to <task>, because <stakes>.
(Secondary, optional: …)

## User stories
| # | Story | Priority | Done when |
| --- | --- | --- | --- |
| 1 | As a …, I want to … so that … | Must | On <page>, … |

## Pages, in order
### 1. <Page name>: serves stories #…
- Top: <the answer>
- Middle: <chart(s)>, built from <notebook / cell / function>
- Bottom: <table, download, method expander>
- Controls: <control> (default: …)

## Warnings shown in the app
- …

## Words: plain wording for this persona
- <term> → <plain wording>

## Not in this version
- …

## Technical notes
- Data file(s) used by the app: …
- Models: trained in the app / loaded from `app/models/`
- Headline numbers the app must reproduce: …
- Python version: …
```

**⏸ STOP:** ask the student to reply **approved** or to say what to change. Don't build until they approve.

---

## Stage C: Build

Make sure you are on `main` and up to date: `git checkout main && git pull`.

### Layout

```
<repository root>/
├── .streamlit/config.toml   # theme and client settings (must be at the root)
├── README.md                # existing; add a short "Interactive app" section
├── data/                    # existing; add data/processed/ only if the app needs smaller data
├── notebooks/  docs/  …     # existing; don't touch
└── app/
    ├── streamlit_app.py     # entry point: st.set_page_config + st.navigation
    ├── README.md            # short: what the app is, how to run it and its tests
    ├── APP_DESIGN.md        # persona, user stories, page plan
    ├── requirements.txt     # runtime packages only, pinned with ==
    ├── requirements-dev.txt # -r requirements.txt, plus pytest
    ├── pytest.ini           # pythonpath = .   testpaths = tests
    ├── common.py            # cached loaders (@st.cache_data / @st.cache_resource), shared footer
    ├── pages/               # one file per page, in APP_DESIGN.md order
    ├── src/                 # plain Python lifted from the notebooks; no Streamlit imports
    │   ├── data.py          #   load and clean the data
    │   └── model.py         #   features, training, metrics, predictions (if any)
    ├── tests/               # smoke tests and checks that the numbers match
    ├── scripts/             # optional: data-preparation or model-training scripts
    └── models/              # optional: saved models, if training is slow
```

Python puts the entry file's folder (`app/`) on the import path, so pages import `from src.data import …` and `from common import …`. Streamlit Cloud runs the app **from the repository root**, so never rely on the current directory: build every path from `__file__`.

### Rules

**Code**
- **Copy, don't rewrite.** Lift the notebook code into `src/` functions. Keep the same parameters, random seeds, train/test split and preprocessing, so the app's numbers match the notebooks.
- `src/` must not import Streamlit and must not run anything on import: no `print`, `display` or `plt.show`.
- Build paths from `__file__`. In `app/src/*.py`: `APP_DIR = Path(__file__).resolve().parents[1]` and `REPO_ROOT = APP_DIR.parent`; data is then `REPO_ROOT / 'data' / …`.
- Load data with `@st.cache_data`, and models or other heavy objects with `@st.cache_resource`, so changing a control doesn't reload or retrain.
- If the data file is missing, show a friendly `st.error` message and call `st.stop()`, instead of letting the app crash.

**Pages**
- `app/streamlit_app.py` is the only file that calls `st.set_page_config`. Page paths in `st.Page(...)` are relative to it, for example `st.Page('pages/overview.py', …)`.
- Use `st.navigation([st.Page(...), ...])` with clear titles and icons, in the order agreed in APP_DESIGN.md.
- Follow APP_DESIGN.md: top, middle, bottom order on each page, the agreed controls with their defaults, the warnings, and the plain wording.
- Every page ends with a footer naming the data source (with a link), the author, and the GitHub repository.

**Charts**
- Use Plotly or Streamlit's built-in charts.
- Every chart needs a title that states the takeaway, labelled axes with units, readable tick labels, and a consistent colour for each entity across pages. Never use two y-axes.
- Put a table or CSV download next to the key charts, so the numbers can be read without the chart.

**Theme and settings**
- `.streamlit/config.toml` (at the repository root) sets `[theme] base = "light"`, so charts stay readable when a visitor's computer is in dark mode.
- It also sets `[client] toolbarMode = "viewer"` and `[browser] gatherUsageStats = false`.

**Requirements**
- Create a virtual environment with the Python version you will deploy. Use one the Streamlit Cloud form offers, preferably 3.12 or 3.13.
- Install only what the app imports at runtime, and pin exact versions from that environment with `==`.
- Never include `jupyter`, `ipykernel`, `google-colab`, `pywin32`, local paths or `-e` installs.
- Put `pytest` in `app/requirements-dev.txt` (`-r requirements.txt` plus `pytest==…`).
- Check the installed Streamlit version, and use only APIs that exist in it.

**Models**
- If training takes 20 s or less, train in the app, cached.
- Otherwise run `app/scripts/train.py`, save the model to `app/models/` with `joblib`, and load it with `@st.cache_resource`. Pin the same library versions the model was saved with.

**Repository hygiene**
- Put the virtual environment at `app/.venv/` and add `.venv/`, `__pycache__/`, `.pytest_cache/` and `.DS_Store` to the root `.gitignore`.
- Don't commit anything over 50 MB.
- Before every commit, run `git status` and check that every new file is inside `app/`, apart from the exceptions in *Where files go*.

### Tests

Write `app/tests/` with `pytest`. Run the tests from inside the folder: `cd app && pytest`.
1. **Parity:** reproduce the headline numbers listed in APP_DESIGN.md, such as metrics, totals and key predictions, within a small tolerance. If a number doesn't match the notebook, find out why before going on. Never adjust the test to fit.
2. **Smoke:** use `streamlit.testing.v1.AppTest`:
   - Run `streamlit_app.py`, and every page file on its own, and assert there are no exceptions.
   - For each Must story, change the relevant control and assert that the expected element or value appears.
   - Paths given to `AppTest.from_file` are resolved relative to the test file. Use `"../streamlit_app.py"`, for example.
   - A page that uses `st.page_link` can only run through the entry file. Test it through `streamlit_app.py`.
3. **Missing data:** temporarily point the loader at a path that doesn't exist, and assert that the friendly error appears.

---

## Stage D: Verify locally, with the student

1. Create a **fresh** virtual environment, run `pip install -r app/requirements-dev.txt`, then run `cd app && pytest`. Everything must pass. Show the student the result.
2. Run `git status` and check the layout matches *Where files go*. Search the code for leftover blockers: `/content`, `drive`, absolute paths, `C:\`, `.ipynb_checkpoints`, secrets. List any file in `git ls-files` that is over 50 MB.
3. Start the app **from the repository root**, as Streamlit Cloud does: `streamlit run app/streamlit_app.py`. Ask the student to open <http://localhost:8501>.
4. Walk the student through each Must story's **Done when** check, one at a time. Fix what they report, re-run the tests, and ask them to check again. If you can take screenshots, check the layout yourself too: overlapping text, cut-off tables, empty charts.

**⏸ STOP:** ask the student whether every Must story works and whether they're happy with the app.

---

## Stage E: Commit, push and hand over

1. Write `app/README.md`: what the app does and who it is for (one line from the persona), the pages, how to run it locally (`pip install -r app/requirements.txt`, then `streamlit run app/streamlit_app.py` from the repository root), and how to run the tests.
2. Add a short **"Interactive app"** section to the root `README.md`: two or three lines saying what the app is, the placeholder `Live app: (link added after deployment)`, and a link to `app/README.md`. Don't restructure the rest of the README.
3. Run `git status`. Check that `AI_APP_BUILDER.md` is **not** staged and that every new file follows *Where files go*. Show the student the list of files.
4. Ask the student for permission, then commit to `main` with a clear message. Ask again, then push with `git push origin main`.
5. Confirm the push worked, then show this card with the values filled in:

```
DEPLOY SETTINGS (copy these into the Streamlit Cloud form)
Repository:        <owner>/<repo>
Branch:            main
Main file path:    app/streamlit_app.py
Python version:    <version used in the venv>   (Advanced settings)
Secrets:           none
```

6. Tell the student to continue with **Part 4** of their guide. They should come back with either the live URL or the error log.

**⏸ STOP.**

---

## Stage F: After deployment

**If the student sends the live URL:**
1. Replace the placeholder in the root `README.md` with the link, and add it to `app/README.md`.
2. Ask permission, then commit and push to `main`.
3. Remind the student to delete `AI_APP_BUILDER.md` from their computer's copy of the repository (it was never committed). Remind them to open the app in an incognito window and to send the link to their instructor.

**If the student sends an error log:**
1. Find the first real error in the log, and use the table below to fix it.
2. Run `pytest` again, then ask permission to commit and push to `main`.
3. Tell the student that the app redeploys automatically after the push.

| Log shows | Likely cause | Fix |
| --- | --- | --- |
| `ModuleNotFoundError: X` | Package missing from `app/requirements.txt` | Add it, pinned |
| `ModuleNotFoundError: src` or `common` | Import path problem | The entry file must be `app/streamlit_app.py`, and the Main file path in the deploy form must match |
| Theme or settings ignored | `config.toml` not at the root | Move it to `.streamlit/config.toml` at the repository root |
| Pip or build failure for a pinned version | That version doesn't exist for the selected Python | Match the Python version in Advanced settings to the venv, or pin a version that has wheels for it |
| `FileNotFoundError` | Path relative to the current directory, absolute or Colab path, file-name case, or a file not committed | Build the path from `__file__`, match the case exactly, and commit the file (check `git ls-files`) |
| App killed, or "resource limits" | Too much data or memory | Use smaller processed data, load only the needed columns, check the caching |
| Very slow first load | Training in the app, or large data | Save the model with `joblib`, and use precomputed or smaller data |
| One-time "Page not found" pop-up on a direct page link after a cold start | Known `st.navigation` behaviour | Harmless: the page loads behind the pop-up. Mention it in the README |
| Charts unreadable in dark mode | Theme not pinned | Set `base = "light"` in `.streamlit/config.toml` |
