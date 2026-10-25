# Hybrid Resume \& Job Matcher

An AI-powered Resume and Job Matching application built in Jupyter Notebook and deployed through a Streamlit interface. The system leverages Ollama, Streamlit, and a Learning-to-Rank (LTR) machine learning pipeline to enable semantic resume parsing, job-description analysis, and intelligent candidate–job matching.

## Project Overview

Traditional resume screening often relies on keyword matching, missing the true context, skills, and experience behind a candidate’s profile.

This project goes beyond simple keyword search by implementing a **Two-Stage Information Retrieval system**. It combines exact-keyword baselines with embeddings, semantic similarity, LightGBM re-ranking, and Generative AI reasoning to evaluate how well a candidate matches a job posting.

## Key Features

* **Resume Parsing**

  * Supports PDF, DOCX, and TXT files.
  * Extracts structured fields: skills, experience, education, certifications, and industries.
  * Initial training being done via strings in a CSV file.
* **Job Description \& Resume Ingestion**

  * Uploads job description files or a resume.
  * Converts job description and resume to raw text.
* **Extraction \& Normalization**

  * Extracts the main information from a resume or job description via Llama3.
  * Normalizes the resume using predefined functions to fix LLM mistakes.
* **Two-Stage Retrieval \& Semantic Embedding**

  * **Stage 1:** Uses `rank\_bm25` for high-speed, exact keyword matching.
  * **Stage 2:** Uses Ollama embeddings (Nomic or BGE) to generate vector embeddings for both resumes and job descriptions.
* **Learning-to-Rank (LTR) \& Similarity**

  * Computes cross-features using Cosine Similarity and BM25 scores.
  * Uses a `LightGBM` Ranker model to dynamically re-order and rank job matches based on contextual relevance (optimized for nDCG).
* **Explainable AI Reasoning**

  * Generates natural-language explanations for why a resume matches a job, showing key strengths and gaps.
  * *Example:* > “This candidate’s experience in data analytics aligns with the Python and SQL requirements of this role.”
* **Streamlit Application**

  * Intuitive UI for uploading resumes and job descriptions.
  * Options to choose embedding or similarity score.
  * Displays match scores and explanations.

## Architecture

**Workflow:**
`User → Streamlit UI → (FastAPI backend) → Agentic workflow`
`↓`
`Stage 1 Retrieval: BM25 (Keywords) + Ollama Models (LLM + embeddings)`
`↓`
`Stage 2 Ranking: LightGBM LTR Matching → LLM Explanation`

**Directory Structure:**

```
project/
│
├── app/
│   └── streamlit\_app.py                 # Functional streamlit app (see video demonstration)
│
├── agents/
│   ├── normalization\_agent.py           # Agentic functions to create normalized prompts
│   └── fixing\_agent.py                  # Fixes LLM output to fit a correct JSON schema
│
├── ingestion/
│   └── preprocess.py                    # Normalize, clean text and convert raw text into structured schema
│
├── embeddings/
│   ├── embedding\_engine.py              # Building and using embedding prompts
│   └── embedding\_format\_conversion.py   # Converting embedding results into ranking agent input format
│
├── match\_engine\_and\_explanation/
│   ├── match\_engine.py                  # Generating matching scores resume-job
│   └── llm\_explanation.py               # Explanation prompt for matching
│
├── notebooks/                           # Experiments
│   ├── smart\_resume\_matcher.ipynb       # Main notebook for transformation \& LightGBM training
│   ├── checkpoint\_test/                 # Checkpoint with the normalized files and embeddings for testing
│   ├── eval.ipynb                       # Evaluation notebook for retrieval metrics
│   └── images/                          # Graphical representation used in the evaluation notebook
│
├── data/
│   ├── resumes/
│   │   └── resumes.csv                  # Resume texts for training
│   ├── jobs/
│   │   └── job\_postings.csv             # Job posting information for training
│   └── test/                            # Resume and job posting examples (txt, docx, csv)
│
├── README.md
├── requirements.txt
├── Smart-Resume-Job-Matcher.pptx        # Presentation, with the link to the demo video
└── Report.docx                          # Report

```

## How to Run

**1. Clone the project**

```
git clone https://github.com/TheSese1/Smart\_Resume\_to\_Job\_Matcher.git
cd Smart\_Resume\_to\_Job\_Matcher

# Create a python environment with correct dependencies (optional)
python -m venv .venv		
source .venv/bin/activate   # or Windows equivalent: .venv\\Scripts\\activate

```

**2. Install Dependencies**

```
pip install -r requirements.txt

```

**3. Install and Setup Ollama**

* Download Ollama from: https://ollama.com
* Start the Ollama service by running `ollama serve` or opening the Ollama application.
* Pull your desired LLM and embedding models:

```
ollama pull llama3
ollama pull nomic-embed-text

```

**4. Run the Streamlit App**

```
streamlit run app/streamlit\_app.py

```

## Technologies Used

|Technology|Purpose|
|-|-|
|**Streamlit**|Front-end UI|
|**Ollama**|Local LLM + embedding models|
|**Python**|Core logic|
|**Jupyter Notebook**|Development \& experimentation|
|**Scikit-learn / LightGBM**|LTR Ranking, Metrics and representation|
|**rank\_bm25**|Keyword retrieval baseline|

## Future Enhancements

* Integration with LinkedIn job scraping
* Recruiter dashboard
* Fine-tuned domain-specific embedding models
* Support for additional file formats (e.g., pictures/images)

## Contributors

* Shreeja Bandi

