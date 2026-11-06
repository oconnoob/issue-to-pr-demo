# Working on this demo

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
