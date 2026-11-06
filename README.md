# Shipping Quote API — issue-to-PR demo

A deliberately small FastAPI service for testing a coding agent on DigitalOcean
Harness Runtime. The initial commit contains one intentional boundary bug.

Orders below $50 pay $5.99 shipping. Orders of **$50 or more** get free shipping.
All API amounts are integer cents.

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
