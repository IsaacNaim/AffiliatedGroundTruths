Medical PDF Workflow Testing Repository
Overview
This repository provides a testing structure for a modular medical data processing system that handles 20 PDFs (4 per patient, 5 patients) containing clinical notes, discharge summaries, lab reports, and radiology reports. It includes ground-truth metadata for evaluating four modules: ingestion, extraction, template creation, and report generation. The dataset is assumed to be de-identified and stored securely to comply with HIPAA.
Repository Structure
medical-pdf-workflow-testing/
├── dataset/
│   ├── ground_truth/
│   │   ├── ingestion_metadata.json      # Metadata for 20 PDFs
│   │   ├── extraction_summaries.json    # Full text and summaries for 20 PDFs
│   │   ├── templates.json               # 4 templates for PDF types
│   │   ├── report_metadata.json         # Metadata for 5 ground-truth reports
│   │   └── sample_reports/              # 5 ground-truth report PDFs
├── docs/
│   └── architecture_diagram.png         # Data flow diagram
├── README.md                           # This file
└── LICENSE                             # MIT License

Dataset

PDFs: 20 (4 per patient, 5 patients).
Types: Clinical notes, discharge summaries, lab reports, radiology reports.
Formats: Mix of text-based and scanned PDFs (proportions to be determined).
Storage: PDFs assumed stored in /dataset/patient_<id>/ or a secure cloud (e.g., AWS S3 with AES-256 encryption).
Metadata: De-identified, stored in JSON or MongoDB.

Ground-Truth Metadata

Ingestion Module (ingestion_metadata.json):

Records: 20 (one per PDF).
Schema: patient_id, file_path, pdf_type, date, file_size.
Metrics: Completeness (20/20 PDFs), Accuracy (metadata match), Speed (<10s), Error Rate (0%).
Usage: Compare module output to verify PDF storage and metadata accuracy.


Extraction Module (extraction_summaries.json):

Records: 20 (one per PDF).
Schema: pdf_id, full_text, summary, is_scanned.
Metrics: Text Accuracy (CER/WER), Summary Accuracy (BLEU/ROUGE), Coverage (20/20), Readability (Flesch-Kincaid or expert review).
Usage: Compare extracted text and summaries to ground truth.


Template Creation Module (templates.json):

Records: 4 (one per PDF type).
Schema: template_id, sections, placeholders, sample_pdf_ids.
Metrics: Structural Accuracy (section/placeholder match), Reusability, Completeness (100% sections).
Usage: Compare generated templates to ground truth; test reusability.


Report Generation Module (report_metadata.json, sample_reports/):

Records: 5 (one report per patient, covering different types).
Schema: report_id, template_id, content, guideline_reference, output_file.
Metrics: Accuracy (content match), Compliance (guideline adherence), Formatting Quality, Completeness (100% sections).
Usage: Compare generated reports to ground-truth PDFs and metadata.



Setup Instructions

Clone Repository:git clone https://github.com/<your-username>/medical-pdf-workflow-testing.git


Add PDFs:
Place 20 de-identified PDFs in /dataset/patient_<id>/ (e.g., /dataset/patient_001/lab_report1.pdf).
Alternatively, configure cloud storage (e.g., AWS S3) and update file_path in ingestion_metadata.json.


Populate Ground Truth:
Update ingestion_metadata.json with correct file_path, pdf_type, and date based on your PDFs.
Create extraction_summaries.json by manually extracting text and summarizing 20 PDFs.
Define templates.json by analyzing 1–2 PDFs per type.
Generate 5 ground-truth reports for report_metadata.json and sample_reports/.


Ensure HIPAA Compliance:
Verify PDFs and metadata are de-identified (e.g., use AWS Comprehend Medical).
Use encrypted storage (e.g., AES-256 for files, HTTPS for access).


Test Modules:
Implement modules (not provided here) and compare outputs to ground-truth files.
Use evaluation scripts (e.g., Python with nltk for BLEU/ROUGE) to compute metrics.



Security

De-identification: All PDFs and metadata must exclude PHI (e.g., names, SSNs).
Encryption: Use AES-256 for storage, HTTPS for data transfer.
Access Control: Restrict access via IAM roles or file permissions.
Logging: Enable audit logs (e.g., AWS CloudTrail) for tracking.

License
MIT License (see LICENSE file).
Next Steps

Populate ground-truth files with your dataset’s specifics.
Develop and test the ingestion module against ingestion_metadata.json.
Contact <your-contact> for questions or contributions.
