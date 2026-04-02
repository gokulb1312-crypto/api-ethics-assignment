Task 1 — Classify and Handle PII Fields

Field	        - Type	        - Action
full_name     - Direct PII	  - Drop	
email	        - Direct PII	  - Drop	
date_of_birth	- Indirect PII	- Mask (generalize)	
zip_code	    - Indirect PII	- Mask (generalize)
job_title	    - Indirect PII	- Pseudonymize / Generalize	
diagnosis_notes-Sensitive PII - (Health Data)	Pseudonymize + Redact
