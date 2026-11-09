# Shipping Quote API — intentional bug for an issue-to-PR demo

A deliberately small FastAPI service for testing a coding agent on DigitalOcean
Harness Runtime. The initial commit contains one intentional boundary bug.

Orders below $50 pay $5.99 shipping. Orders of **$50 or more** get free shipping.
All API amounts are integer cents.

## Start with your own copy

This repository is the runnable target for the
[issue-to-PR setup tutorial](https://github.com/oconnoob/issue-to-pr).
It is source code to copy, not a shared agent service. Use your own DO account,
GitHub authorization and Slack webhook; do not relabel upstream example issues
expecting access to the author's automation.

Choose **Use this template → Create a new repository** and include **only the
default branch**. Start with a private copy while testing the direct webhook
path. The default branch deliberately contains the bug; the example fix branches
and PRs show earlier runs and are not the starting code for your experiment.

Or create your own copy with GitHub CLI:

```bash
gh repo create YOUR_OWNER/YOUR_DEMO_REPO \
  --template oconnoob/issue-to-pr-demo --private --clone
cd YOUR_DEMO_REPO
```

Then follow the main tutorial's
[credential and Slack setup](https://github.com/oconnoob/issue-to-pr/blob/main/docs/environment.md).
For a complete technical demo with before/after curl output and PR verification,
use the [demo walkthrough](https://github.com/oconnoob/issue-to-pr/blob/main/docs/demo.md).

## Run the API

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
uvicorn app.main:app --port 8001
```

Open `/docs`, or reproduce the bug:

```bash
curl 'http://127.0.0.1:8001/quote?subtotal_cents=5000'
```

Expected: `{"subtotal_cents":5000,"shipping_cents":0,"total_cents":5000}`.
The initial version charges 599 cents for shipping at exactly 5000 cents.
Amounts below and above the threshold work as intended; negative input gets 422.

## Run regression tests

```bash
python3 -m unittest discover -s tests -v
```

The tests need only Python's standard library. The initial commit intentionally
fails the exact-threshold test. The agent's PR should make all four tests pass
without changing their expected values.

## Demo flow

Connect this repository's **Issues** webhook to the direct v0 trigger or the relay
from the parent tutorial. Open the shipping boundary bug issue, then add the
`agent` label after the webhook is ready. The agent should create a branch named
`agent/issue-N`, fix the comparison, run the tests and open a PR with `Fixes #N`.
Its summary goes to Slack. A person reviews the PR; no automated merge is needed.

In your own repository, create a new issue from the **Shipping boundary bug**
issue template. Connect your webhook first, then add `agent` using GitHub's label
picker. The issue template does not apply the label automatically.

Stop the API before checking out the agent's PR and restart it afterward; a
running Python process can otherwise serve the old code. Check out the actual
PR branch, rerun all four tests and repeat the same curl request. Checking out a
branch locally does not merge its PR.

Do not merge example fix PRs into this upstream template's `main`: keeping the
bug there makes fresh copies reproducible. Merge reviewed fixes in your own
experiment if desired. Re-labeling the same issue should return its existing
agent PR, including a closed PR. Use a fresh copy for another independent trial;
deleting the old branch is not a reliable reset.

## License

MIT. See [LICENSE](LICENSE).
