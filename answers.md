# Task 1 - PII Classification

| Field          | Type          | Handling Method | Justification                                                                 |
|----------------|---------------|-----------------|-----------------------------------------------------------------------------|
| full_name      | Direct PII    | Drop            | Directly identifies individuals; cannot be anonymized without losing utility  |
| email          | Direct PII    | Drop            | Direct identifier; requires explicit consent for sharing                    |
| date_of_birth  | Direct PII    | Pseudonymize    | Can be used to re-identify individuals; requires hashing or tokenization      |
| zip_code       | Indirect PII  | Mask            | Can be combined with other data to identify individuals; partial masking     |
| job_title      | Indirect PII  | Mask            | Can indicate demographic information; partial masking recommended           |
| diagnosis_notes| Indirect PII  | Pseudonymize    | Contains sensitive health information; requires de-identification            |

# Task 2 - Ethical Compliance Audit

# Violation 1: Unauthorized API Key Usage
- Problem: The script uses a public free-tier API key without proper authentication or rate limiting. This violates the API provider's Terms of Service and could lead to account suspension or data breaches.
- Corrected Code: Implement proper authentication and rate limiting:
```python
import requests

API_URL = "https://healthstats-api.example.com/records"
API_KEY = "your_authorized_key"  # Use a properly authenticated key

# Add rate limiting to prevent abuse
MAX_REQUESTS = 10
request_count = 0

records = []
for page in range(1, 101):
    if request_count >= MAX_REQUESTS:
        print("Rate limit reached; pausing for 1 hour")
        time.sleep(3600)
        request_count = 0
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    data = response.json()
    records.extend(data["results"])
    request_count += 1

# Violation 2: Permanent Storage Without Consent
-Problem: The script permanently stores all collected records in the company database without proper consent mechanisms or data minimization practices.
-Corrected Code: Implement data minimization and consent management:
```python
# Replace permanent storage with temporary processing
processed_records = []  # Anonymized/aggregated data
for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    data = response.json()
    # Anonymize data before processing
    for record in data["results"]:
        anonymized_record = {
            "anonymized_id": hash(record["full_name"] + record["date_of_birth"]),
            "zip_code_masked": record["zip_code"][:3] + "***",
            "diagnosis_notes_aggregated": "aggregated_health_data"
        }
        processed_records.append(anonymized_record)
