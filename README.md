# CI/CD Demo

A minimal project to learn CI/CD hands-on. Push it to GitHub and watch the
pipeline run in the **Actions** tab.

## What's here
- `src/calculator.py` — the code
- `tests/test_calculator.py` — unit tests
- `.github/workflows/ci.yml` — the pipeline (lint → test → deploy)

## The pipeline (what CI/CD actually means)
1. **CI (Continuous Integration)** — every push/PR triggers:
   - **Lint** (`flake8`): code-quality gate
   - **Test** (`pytest`): correctness gate
2. **CD (Continuous Deployment)** — only after tests pass **and** only on `main`:
   - Build an artifact, then deploy (placeholder step)

Note the gate logic: `deploy` has `needs: test`, so a failed test blocks
deployment. PRs run tests but do **not** deploy — that only happens when code
lands on `main`.

## Run the checks locally (before pushing)
```bash
pip install pytest flake8
flake8 src tests --max-line-length=100   # lint
pytest -v                                # test
```

## Push to GitHub
```bash
cd cicd-demo
git init
git add .
git commit -m "Initial CI/CD demo"
git branch -M main
git remote add origin https://github.com/<your-username>/cicd-demo.git
git push -u origin main
```
Then open the repo's **Actions** tab to watch it run.

## Try the feedback loop
1. Break a test on purpose (e.g. change `assert add(2, 3) == 5` to `== 6`).
2. Commit on a branch, open a PR → CI fails, PR is blocked. **This is the gate working.**
3. Fix it, push again → CI goes green → merge → deploy runs.
