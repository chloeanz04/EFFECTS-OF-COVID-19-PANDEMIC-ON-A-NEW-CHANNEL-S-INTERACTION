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
<p align="center">
  <strong>Medical-related videos received increased attention during the COVID-19 outbreak.</strong>
</p>
**Workflow**: Data preprocessing -> Hypothesis testing -> Data visualization

**Main tasks:**
- **Data Preprocessing**:
  - Checked and handled missing values, corrupted entries, and duplicates generated during the crawling process.
  - Converted all fields into the correct data types and formats.
  - Identified the intervention period and created a binary label:
    - `0` — outside the COVID-19 period
    - `1` — during the COVID-19 period
  - Processed outliers:
    - Since extreme view counts can heavily distort the mean, we applied min–max capping (performed separately for each period) to ensure the most objective results.

- **Hypothesis Testing**:
  - Performed descriptive statistics to observe changes in the number of medical-related videos over time.
  - Calculated the proportion of medical-related videos across different periods.
  - Computed the Difference-in-Differences (DiD) coefficient to estimate the causal impact of COVID-19 on the *quantity* of medical-related videos.
  - Applied the Difference-in-Differences method using **average view counts** as the dependent variable.
  - Calculated the average views of medical-related videos across all periods.
  - Computed the DiD estimate to quantify the causal effect of COVID-19 on the **views** of medical-related videos.
  - Estimated the coefficients of key variables (medical_field, time_in_covid, interaction term `did`) on view count.
  - Used an OLS regression model to statistically validate the hypothesis based on the final dataset.

- **Data Visualization**:
  - Visualized the changes in the number of medical-related videos and the average view counts to illustrate the influence of COVID-19 across periods.

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
 
---

### **Explanation**

**Scraping Data from Playlists:**  
- YouTube is a dynamic website with infinite scrolling. Crawling videos from 3–4 years ago requires a very large RAM to store the massive information from tens of thousands of videos, making it difficult to process.  
- To get the upload date and view count, each video requires two clicks (video and description), and repeated actions can trigger YouTube’s bot-detection mechanism.  
- If crawling from the main page and the connection is interrupted, it takes time to scroll back to the interrupted video. Using playlists provides a specific index for each video, making it easier to resume crawling.

**Using LLM:**  
- Manual labeling is time-consuming due to the large volume of data. The medical field is also a common category, making classification easier.  

**Not Using “Likes” Attribute:**  
- Although likes reflect user engagement, in this dataset they are negligible compared to view counts. To ensure more accurate hypothesis testing, we did not use this attribute.

**Defining the COVID-19 Period:**  
- **Start:** January 30, 2020 — when the first cases were detected in Vietnam  
- **End:** March 30, 2022 — although the Ministry of Health declared the pandemic over on July 7, 2023, this end date was chosen because society had largely returned to normal operations and vaccines were available to control the outbreak.

**Choosing the Difference-in-Differences (DiD) Model:**  
- Synthetic Control is suitable for hypotheses focusing on a specific group of videos and requires a large number of untreated units (videos not during COVID-19).  
- DiD is more appropriate for datasets with many units and time periods, and it is simpler to implement.
