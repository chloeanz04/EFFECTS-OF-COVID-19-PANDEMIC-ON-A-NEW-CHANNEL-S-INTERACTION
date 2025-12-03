# [Python] Effects of COVID-19 Pandemic on a New Channel’s Interaction

## 📖 Overview
This project analyzes how the COVID-19 pandemic affected audience interaction on a new media channel. The goal is to investigate whether the pandemic positively or negatively impacted engagement levels—such as likes, views.

The project covers the full workflow: automated data collection, LLM-based text processing, and statistical analysis with visualizations.

---

## 📂 Project Structure

The project consists of three main notebooks, each representing a stage in the data pipeline:

### 1. `crawl_data.ipynb` — Data Collection
**Objective:** Automatically collect data from the target platform (e.g., YouTube, Facebook, TikTok).

**Main tasks:**
- Used Selenium for web scraping.
- Collected information:
  - Video/post title  
  - Upload date  
  - Likes
- Exported and stored raw data for further processing.

---

### 2. `llm.ipynb` — Text Processing with LLMs
**Objective:** Leverage Large Language Models (LLMs) to analyze unstructured text data.

**Main tasks:**
- Connected to LLM APIs (Gemini).
- Performed: Classification (1 - medical-related, 0 - the opposite).

---

### 3. `analysis.ipynb` — Analysis & Visualization
**Objective:** Derive insights and answer the research question regarding COVID-19’s impact on audience interaction:
1. **Did the proportion of health-related news videos increase during the COVID-19 period?**
2. **Did health-related videos receive higher average view counts during COVID-19?**

**Main tasks:**
- Dataset import and cleaning  
- Labeling health-related content  
- Splitting data into pre-COVID vs COVID periods  
- Statistical hypothesis testing  
- Visualization of content and engagement trends  
