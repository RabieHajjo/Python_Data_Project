# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from Luke Barousse's Python Course which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library** This was used to analyze the data.
    - **Matplotlib Library** I visualized the data.
    - **Seaborn Library** Helped me create more advanced visuals.
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

# Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
# Filter Germany Jobs

To focus my analysis on Germany job market, I apply filters to the dataset, narrowing down to roles based in the United States.

df_US = df[df['job_country'] == 'Germany']

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:



## 1. What are the most demanded skills for the top 3 most popular data roles

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](3_Project\2_Skills_Count.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```
### Results

![Visualization of Top Skills for Data Nerds](3_Project\skill_demand_all_data_roles.png)

### Insights


- Python is the most universally required skill, appearing across all three roles but peaking for Data Scientists.

- SQL is highly demanded across roles, but particularly for Data Analysts and Data Scientist emphasizing that data access remains fundamental.

- Cloud skills (AWS, Azure) are far more important for Data Engineers, aligning them more with DevOps and infrastructure responsibilities.

- Visualization and business intelligence tools (Tableau, Power BI) are exclusive to Data Analysts, highlighting their stakeholder-facing nature.

- The technical depth increases from Analyst → Scientist → Engineer.

## 2. How are in-demand skills tending for Data Analysts?

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_GER_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### Results

![Trending Top Skills for Data Analysts in Germany](3_Project\Images\skill_trend_DA.output.png)

*Bar graph visualizing the trending top skills for data analysts in Germany in 2023.*

### Insights:
- SQL is foundational across Analyst & Scientist roles, but less dominant for Engineers
- Python demand peaks highest for Data Scientists and Engineers, but fluctuates more for Analysts
- Cloud + big-data tech is almost irrelevant for Analysts, but essential for Engineers
- Analyst hiring appears more tied to business cycles → skill trends vary by quarter
- Engineer roles demand long-term infrastructure skills → fewer shifts in required tech

## 3. How well do jobs skills pay for Data Analysts?

### Salary Analysis for Data Nerds

#### Visualize Data

```python
sns.boxplot(data=df_GER_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in Germany](3_Project\Images\salary_Distributions_of_DJ_GER.png)
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights

- Salary ranges vary widely across roles, with Senior Data Scientist positions showing the highest earning potential, reaching well over $200K, highlighting the premium placed on advanced data expertise.

- Senior-level roles (Senior Data Scientist, Senior Data Engineer) show the largest spread and most high-end outliers, meaning top performers or niche experts can earn significantly more than the average. In contrast, Data Analyst salaries are more consistent and capped at a lower range.

- Median salaries clearly increase with seniority and specialization roles involving machine learning, cloud engineering, or leadership responsibilities offer both higher typical pay and greater variation in compensation.


### Highest Paid & Most Demanded Skills for Data Analysts

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```
#### Results

in-demand skills for data analysts in Germany:

![The Highest Paid & Most In_Demand Skills for Data Analysts in Germany](3_Project\Images\highest_paid_skills_da_germany.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

#### Insights: 

- The top graph shows that specialized technical tools such as GitHub, Terraform, and BigQuery are linked to the highest salaries, indicating that deeper expertise in advanced data engineering and cloud systems can significantly boost earnings.

- The bottom graph highlights that foundational analytics skills like Python, SQL, Excel, and Tableau are the most in-demand, emphasizing their importance for securing and maintaining roles in data analysis.

- Overall, the charts show a clear contrast: high-paying skills are more specialized, while most in-demand skills are more fundamental. Data analysts benefit most by mastering core tools first, then adding targeted high-value technical skills to increase salary potential.

## 4. What is the most optimal skill to learn for Data Analysts?

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```
#### Results 

![Most Optimal Skills for Data Analysts in the US](3_Project\Images\optimal_skills_for_DA_GER.png)

*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in Germany.*

#### Insights:

- The scatter plot shows that programming and cloud-related skills such as GitHub, GCP, and Python are positioned at the higher salary range, suggesting that technical depth and cloud fluency can significantly increase earning potential in Germany.

- Core data tools like SQL and Tableau appear with high demand across job postings, demonstrating their essential role in everyday data workflows, even if they don’t always lead to the very highest salaries.

- Skills like Excel, Power BI, and pandas fall in the balanced zon they are widely used and offer solid salary outcomes, making them strong foundational skills for securing and thriving in data analyst roles.

- Overall, the chart highlights a strategy for growth: build strong fundamentals first (SQL, Excel, Tableau) and then layer advanced cloud/programming skills (GCP, GitHub, Python) to maximize both employability and salary potential.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **:Advanced Python Usage:** Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **:Data Cleaning Importance:** I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis** The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.

