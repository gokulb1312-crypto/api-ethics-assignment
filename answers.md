Task 1 — Classify and Handle PII Fields

Field	        - Type	        - Action
full_name     - Direct PII	  - Drop	
email	        - Direct PII	  - Drop	
date_of_birth	- Indirect PII	- Mask
job_title	    - Indirect PII	- Pseudonymize
diagnosis_notes-Sensitive PII - (Health Data)	Pseudonymize



Task 2 — Audit the API Script for Ethical Compliance

Violation 1: Hardcoded API Key/-

Problem:
API key is exposed in source code
Violates most API provider Terms of Service
Risk of credential leakage and abuse

Fix:
Use environment variables instead of hardcoding.

"""import os
import requests

API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.getenv("HEALTH_API_KEY")  # Secure retrieval

if not API_KEY:
    raise ValueError("API key not found in environment variables")"""


Violation 2: Excessive Data Collection & Permanent Storage/-

"""import time
import requests

records = []

for page in range(1, 21):  # Limit scope based on actual need
    response = requests.get(
        API_URL,
        params={"page": page, "key": API_KEY}
    )
    if response.status_code != 200:
        break
    data = response.json()
    # De-identify BEFORE storing
    for r in data["results"]:
        cleaned_record = {
            "age_group": calculate_age_group(r.get("date_of_birth")),
            "region": r.get("zip_code", "")[:3],
            "job_category": generalize_job(r.get("job_title")),
            "diagnosis_notes": redact_sensitive_info(r.get("diagnosis_notes"))
        }
        records.append(cleaned_record)
     time.sleep(0.5)  # Respect rate limits
    
save_to_database(records)"""
