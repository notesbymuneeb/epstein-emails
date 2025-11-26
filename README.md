# Epstein Email Threads Dataset

[![Hugging Face](https://img.shields.io/badge/🤗%20Hugging%20Face-Dataset-blue)](https://huggingface.co/datasets/notesbymuneeb/epstein-emails)

A structured, machine-readable dataset of **5,082 parsed email threads** (16,447 individual messages) extracted from OCR'd documents released by the U.S. House Oversight Committee.

## Dataset Overview

- **5,082 email threads** with structured metadata
- **16,447 individual messages** across all threads
- **Average of 3.24 messages per thread**
- **Chronologically ordered** messages within threads
- **OCR errors corrected** and footers removed
- **Format**: Parquet (4.34 MB compressed)

## Quick Start

### Using Hugging Face Datasets

```python
from datasets import load_dataset

# Load the thread-level dataset
dataset = load_dataset("notesbymuneeb/epstein-emails", "default")

# Access a thread
thread = dataset["train"][0]
print(f"Subject: {thread['subject']}")
print(f"Messages: {thread['message_count']}")

# Parse messages JSON
import json
messages = json.loads(thread['messages'])
for msg in messages:
    print(f"From: {msg['sender']}")
    print(f"Body: {msg['body'][:100]}...")
```

### Using Pandas

```python
import pandas as pd

# Load from Hugging Face (or download locally)
df = pd.read_parquet("epstein_email_threads.parquet")

# Access messages
import json
for idx, row in df.head().iterrows():
    messages = json.loads(row['messages'])
    print(f"Thread: {row['subject']}")
    print(f"Messages: {len(messages)}")
```

## Dataset Structure

### Thread-level Schema

| Field | Description |
|-------|-------------|
| `thread_id` | Unique identifier (e.g., "TEXT-001-HOUSE_OVERSIGHT_031683.txt_2") |
| `source_file` | Original source filename from House Oversight release |
| `subject` | Thread subject line |
| `messages` | JSON string containing list of messages in the thread |
| `message_count` | Number of messages in the thread |

### Message Structure

Each message in the `messages` JSON array contains:

- `sender`: Sender name and/or email address
- `recipients`: Array of recipient strings
- `timestamp`: Message timestamp (various formats preserved)
- `subject`: Message subject line
- `body`: Message body content (quoted text and footers removed)

## Download

**Hugging Face:** [notesbymuneeb/epstein-emails](https://huggingface.co/datasets/notesbymuneeb/epstein-emails)

The dataset is available in Parquet format and can be loaded directly using the Hugging Face `datasets` library or pandas.

## Processing Details

This dataset was created by processing the [tensonaut/EPSTEIN_FILES_20K](https://huggingface.co/datasets/tensonaut/EPSTEIN_FILES_20K) CSV file:

1. **Email Thread Parsing** (xAI Grok 4.1 Fast via OpenRouter API)
   - Identified email threads vs. non-email content
   - Separated individual messages within threads
   - Extracted headers (From, To, Sent, Subject)
   - Separated quoted replies from message bodies
   - Ordered messages chronologically (oldest first)
   - Removed footers, disclaimers, and signature blocks

2. **Quality Improvements**
   - OCR error correction
   - Footer/disclaimer removal
   - Quoted text cleanup
   - Recipient extraction from message bodies

3. **Quality Assurance**
   - Automated structure validation
   - Manual sample verification (no hallucinations or data loss detected)
   - Data integrity checks

## Data Quality

**Extraction Accuracy:** Sample verification showed accurate extraction with no hallucinations or data loss. Message boundaries, headers, and bodies were correctly identified and separated.

**Known Limitations:**
- Some original OCR errors from source material may remain (inherited from source, not introduced during extraction)
- Timestamp formats preserved as-is (not normalized)
- Some email addresses may be incomplete/malformed due to source OCR

**Note:** While sample verification found no issues, errors may exist in the full dataset. Please [report any issues](https://huggingface.co/datasets/notesbymuneeb/epstein-emails/discussions) you encounter.

## Legal & Ethical Considerations

**Copyright:** Original documents were created by various private individuals/entities and are sourced from U.S. House Committee on Oversight and Government Reform releases. This dataset does not assert ownership or grant licenses beyond what may be permitted by law (e.g., fair use). Users are solely responsible for compliance with applicable laws and policies.

**Content Warning:** Documents contain material related to sexual abuse, exploitation, trafficking, violence, and other highly sensitive topics, as well as unverified allegations. Handle content appropriately in research and analysis.

**Intended Use:** This dataset is for **research and exploratory analysis** only. Not intended for fine-tuning language models, harassment, doxing, or making unverified allegations as factual claims.

See the [full dataset card](https://huggingface.co/datasets/notesbymuneeb/epstein-emails) for complete legal and ethical guidelines.

## Citation

If you use this dataset in your research, please cite:

```bibtex
@dataset{epstein_emails_2025,
  title={Epstein Email Threads Dataset},
  author={notesbymuneeb},
  year={2025},
  url={https://huggingface.co/datasets/notesbymuneeb/epstein-emails},
  note={Derived from tensonaut/EPSTEIN_FILES_20K, which contains OCR'd text from U.S. House Oversight Committee public release}
}
```

## Acknowledgments

**Original Source:** U.S. House Committee on Oversight and Government Reform  
[Oversight Committee Releases Additional Epstein Estate Documents](https://oversight.house.gov/release/oversight-committee-releases-additional-epstein-estate-documents/)

**Intermediate Source:** [tensonaut/EPSTEIN_FILES_20K](https://huggingface.co/datasets/tensonaut/EPSTEIN_FILES_20K)  
Pre-processed CSV containing OCR'd text files from the House Oversight release.

## Contact & Issues

- **Hugging Face Dataset:** [notesbymuneeb/epstein-emails](https://huggingface.co/datasets/notesbymuneeb/epstein-emails)
- **Report Issues:** Open a discussion on the Hugging Face dataset repository

---

**Remember:** This dataset contains sensitive content. Use responsibly and in accordance with all applicable laws and ethical guidelines.

