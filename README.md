# Variable Repayments Comparison

Milling calculator comparing:

- A traditional buy now pay later schedule with a calculated fixed monthly payment.
- A variable repayment schedule with a smaller weekly base payment plus a usage-linked amount.

The main visible inputs are the minimum and maximum kilograms milled, entered as
either a weekly or monthly range. The page generates a repeatable random monthly
sample within that range, with a "Generate" button to reshuffle it.

Default inputs are stored in `defaults.json` and loaded when the page starts.
The values embedded in `index.html` are used as a fallback if the JSON file
cannot be fetched, such as when opening the HTML directly from disk.
Serve the page over HTTP, such as with GitHub Pages or a local static server, to
test changes to `defaults.json`.

For local testing, run this from the project folder:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000/` instead of opening `index.html` directly.

Variable repayment is estimated as:

```text
monthly variable repayment = monthly base repayment + monthly grain milled × usage repayment rate
variable balance = price - deposit + variable finance fee
sensor kWh shown = monthly grain milled ÷ kg milled per kWh
fixed BNPL balance = price - deposit + BNPL finance fee
fixed monthly payment = fixed BNPL balance ÷ comparison period
```

The BNPL finance fee and variable finance fee can each be entered as either an
annual percentage of the mill price or as a KES amount. Percentage fees are
prorated by time:

```text
percentage finance fee = price × annual percentage × elapsed months ÷ 12
```

For the variable option, percentage finance fee accrues only until the generated
scenario pays off. If payoff happens before the end of the comparison period,
the variable total uses that shorter payoff period.

## Publish

All CSS and JavaScript are embedded in `index.html`; no build step is required.
Keep `defaults.json` in the same folder as `index.html` when publishing.
