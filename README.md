# Cisco ESA Mail Policy Hitcount Reporter (Policy Name)

This Python script queries **Cisco ESA (Email Security Appliance) v2 API** endpoints for **incoming and outgoing mail policies** and prints a summary of **policy hit counts** over a configurable time range.  

It is designed for administrators who want a **quick overview of which mail policies were triggered** in a given period.

---

## Features

- Query **incoming** and **outgoing** mail policies.
- Summarizes **policy name → hit count**.
- Configurable **time range** (number of days to query).
- Configurable **number of top policies** to retrieve.
- Handles **Basic Authentication**.
- Optional **SSL verification** for secure ESA setups.
- Clean, easy-to-read tabular output.
- All user parameters are centralized at the top of the script.

---

## Requirements

- Python 3.8+
- `requests` library

Install dependencies via pip:

```
pip install requests
```

---

## Configuration

Edit the user parameters at the top of `esa_policy_hitcount.py`:

```
ESA_IP = "x.x.x.x"                # ESA IP or hostname
ESA_PORT = 6080                  # ESA API port
API_USER = "api_user"            # ESA username
API_PASS = "api_password"        # ESA password
DAYS_TO_QUERY = 1                # Number of past days to query
TOP_N_POLICIES = 10              # Top N policies to return
VERIFY_SSL = False               # Set True if ESA has a valid SSL certificate
```

---

## Usage

Run the script:

```
python esa_policy_hitcount.py
```

Sample output:

```
ESA Report: Incoming Policy Hitcounts Matched - last 1 day(s)
Time Range: 2026-03-30T18:00:00.000Z -> 2026-03-31T18:00:00.000Z

kv                            129
webs50                         7
root after quarantine           6
Test-Sgcweb                     5
postmaster-forwarding           4
Spam-FalsePositive              2
spoofing-allowed                2

ESA Report: Outgoing Policy Hitcounts Matched - last 1 day(s)
Time Range: 2026-03-30T18:00:00.000Z -> 2026-03-31T18:00:00.000Z

gmail-out                       9
```

---

## How it Works

1. The script calculates the **start and end time** based on `DAYS_TO_QUERY`.
2. Constructs ESA API URLs for incoming and outgoing mail policies.
3. Uses **Basic Authentication** to query ESA v2 API.
4. Processes the returned JSON:
   - ESA returns a mapping: `{policy_name: hit_count}`.
5. Prints each policy and the number of hits in a clean table format.

---

## Notes

- The **hit count** represents how many times a policy was triggered (per recipient) in the selected time range.
- ESA API limits results with `top=N`. Increase `TOP_N_POLICIES` if needed.
- Set `VERIFY_SSL = True` if using HTTPS with a valid certificate.
- The script uses **UTC time** for ESA API queries.

---

## License

MIT License. Free to use, modify, and distribute.

---

## Author

Your Name – https://github.com/yourusername

---

## Suggested Improvements

- Export results to **CSV or JSON** for reporting or dashboards.
- Include **automatic pagination** for more than `top=N` results.
- Add **authentication via token** instead of basic auth for better security.
