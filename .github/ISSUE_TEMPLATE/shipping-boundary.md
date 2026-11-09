---
name: Shipping boundary bug
about: Reproduce the intentional exact-$50 shipping bug in your own demo copy
title: Free shipping is incorrectly charged at exactly $50
labels: ''
assignees: ''
---

## Bug

Orders of 5000 cents ($50) or more should receive free shipping. The API currently
charges 599 cents at exactly 5000 cents.

## Reproduce

Run the API using the README, then request:

```bash
curl 'http://127.0.0.1:8001/quote?subtotal_cents=5000'
```

Actual: `{"subtotal_cents":5000,"shipping_cents":599,"total_cents":5599}`.

Expected: `{"subtotal_cents":5000,"shipping_cents":0,"total_cents":5000}`.

## Acceptance criteria

- Make the smallest fix, preserving the API, prices and test expectations.
- Add no dependencies or CI workflows.
- Run `python3 -m unittest discover -s tests -v`; all four tests must pass.
- Open a PR with a standalone `Fixes #N`, using this issue's number, and report
  the exact test command/result.
- Send the final summary through the configured Slack output. Do not post issue
  comments or merge the PR.

Configure your own webhook and OAuth connection before adding the `agent` label.
