# Working on this demo

## If the user asks to set up the cloud agent

This repository is the demo target. Open the separate
[issue-to-PR setup repository](https://github.com/oconnoob/issue-to-pr) and
follow its `AGENTS.md` and [demo walkthrough](https://github.com/oconnoob/issue-to-pr/blob/main/docs/demo.md).
Use the user's own private copy of this demo as the GitHub target. Leave the
intentional shipping bug in place for the first hosted run.

## If you are here to fix an issue

Work on the assigned issue in this repository. Do not run the setup steps or
configure cloud resources as part of an issue fix.

This is a small FastAPI shipping quote service. Monetary amounts are integer cents.
Shipping is 599 cents for orders below 5000 cents and free for orders of 5000 cents
or more. Negative subtotals are invalid.

Run the regression suite with:

```bash
python3 -m unittest discover -s tests -v
```

These tests use only the standard library; installing the API dependencies is not
required to run them. Install `requirements.txt` only to serve the API locally.
Keep fixes small. Do not change the shipping prices, threshold, tests' expected
values or API contract to hide a failure. Do not add dependencies or CI workflows.
