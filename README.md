# [Python] Effects of COVID-19 Pandemic on a New Channel’s Interaction

## 📖 Overview
This project analyzes how the COVID-19 pandemic affected audience interaction on a new media channel. The goal is to investigate whether the pandemic positively or negatively impacted engagement levels—such as likes, views.

The project covers the full workflow: automated data collection, LLM-based text processing, and statistical analysis with visualizations.

---

## 📂 Project Structure

The project consists of **3 main notebooks**, each representing a stage in the data pipeline:

### 1. `crawl_data.ipynb` — Data Collection
**Objective:** Collect structured news data from the Bao Thanh Niên YouTube channel for analysis.

**Main tasks:**
- Selected the Bao Thanh Niên YouTube channel as a consistent and reliable data source due to its steady posting frequency and engagement levels.
- Crawled multiple playlists containing videos across different topics and time periods to control dataset size and coverage.
- Extracted key attributes from each video:
  - **Video title**: Identify the content category
  - **Upload date**: Used to determine the posting period
  - **View count and like count**: Represent user engagement
- Exported and stored the collected data into the initial dataset: `video_data.csv`.

---

### 2. `llm.ipynb` — Text Processing with LLMs
**Objective:** Leveraging LLM to classify videos related to the medical field based on their titles.

**Main tasks:**
- Used an LLM to automatically classify whether each video is related to the medical/health domain.
- Assigned labels by adding a new column `medical_field`:
  - `1` — medical-related content  
  - `0` — other categories
- Exported the labeled dataset as `final_data.csv`, which serves as the primary dataset for hypothesis testing.

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

---

## Result
<p align="center">
  <strong>Distribution of views by years</strong>
</p>

<img width="993" height="610" alt="image" src="https://github.com/user-attachments/assets/0cbb3745-31cc-4b83-9482-ca288ae1c260" />

The average view count of news videos increased during the COVID-19 period.

In particular, at the beginning of the COVID-19 period, the average views of health-related videos surged sharply, reaching a peak nearly **5 times** higher than before and after COVID. This was higher than all other content categories (during the first three months of 2020).

<p align="center">
  <strong>OLS results</strong>
</p>

<img width="678" height="453" alt="image" src="https://github.com/user-attachments/assets/aa7d0a8e-ca93-461f-a7b2-dea958186b89" />

**Intercept**
- **Coefficient:** `4.851e+04`  
- This means that when all independent variables are equal to 0, the average view count is **48,510**.  
- **P-value:** `0.000` — very small, indicating the intercept is statistically significant.

**Medical_field**
- **Coefficient:** `-1.489e+04`  
- This shows that, *outside the COVID-19 period*, health-related videos receive **14,890 fewer views** on average compared to non-health videos.  
- **P-value:** `0.003` — < 0.05, meaning **medical_field** has a statistically significant effect on view counts.

**Time_in_covid**
- **Coefficient:** `5.672e+04`  
- This indicates that during the COVID-19 period, the average view count of **all videos** increased by **56,720 views**.  
- **P-value:** `0.000` — strongly significant, suggesting COVID-19 had a substantial impact on video views.

**DID Interaction Term (`did`)**
- **Coefficient:** `1.245e+04`  
  - This suggests that during COVID-19, **health-related videos gained an additional 12,450 views** compared to non-health videos.  
- **P-value:** `0.019` (< 0.05)  
  - With 95% confidence, we accept the hypothesis that **health-related news received increased attention during the COVID-19 outbreak**.
