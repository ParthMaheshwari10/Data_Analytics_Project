# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.


# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.


## Import & Clean Up Data

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

## Filter Indian Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.

```python
df_IN = df[df['job_country'] == 'India']

```


# The Analysis

## 1. What are the most demanded skills for the top 3 most popular Data roles?

To find the most demanded skillls for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here:
[Skills_Count.ipynb](Project\Skills_Count.ipynb)

### Visualize Data
```python 
for i, job_title in enumerate(job_titles):
    df_plot = df_skills_per[df_skills_per['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percentage', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results
![Visualization of Top Skills for Data Nerds](Project\Images\skill_demand.png)

### Insights
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both the roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau). 


## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data
```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_IN_percent.iloc[:, :5] 
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

### Results
![Trending Top Skills for Data Analysts in the US](Project\Images\skill_trend.png)

*Bar Graph visualizing the trending top skills for Data Analysts in India in 2023.* 

### Insights
- SQL remains the most in-demand skill for Data Analyst jobs throughout the year, consistently appearing in over 50% of job postings.
- Python and Excel are both highly valued skills, each appearing in roughly 34–40% of job postings. Python is increasingly sought after for its use in data manipulation and automation, while Excel remains a staple tool for quick analysis and reporting.
- Tableau and Power BI are popular visualization tools, with growing relevance in business intelligence roles. Tableau shows steady demand across listings, while Power BI, although initially less common, demonstrates an upward trend—indicating its increasing adoption, especially within Microsoft-centric organizations.


## 3. How well do jobs and skills pay for Data Analysts?

## Salary Analysis for Data Nerds

### Visualize Data

```python
sns.boxplot(data=df_IN_top6, x='salay_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)K'})
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results
![Salary Distributions of Data Jobs in India](Project\Images\salary_analysis.png)

*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Insights
- Senior Data Engineers have the highest median salary among the roles shown, reflecting their advanced expertise and leadership responsibilities in managing large-scale data systems. Despite this, the role also exhibits a wide spread of lower-end outliers, suggesting variability based on company or region.
- Machine Learning Engineers exhibit the broadest salary range, with compensation stretching beyond $250K. This wide distribution reflects the high demand for AI expertise and the diverse applications of machine learning across industries.
- Data Analysts and Software Engineers have the lowest median salaries, with narrower interquartile ranges, indicating more standardized and entry-level compensation. These roles often serve as stepping stones into more specialized or senior data positions.


## Highest Paid and Most Demanded Skills for Data Nerds

### Visualize Data
```python

fig, ax = plt.subplots(2, 1)

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_top_pay, x='median', y=df_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-demand Skills for Data Analysts
sns.barplot(data=df_top_skills, x='median', y=df_top_skills.index, hue='median', ax=ax[1], palette='dark:b_r')

plt.show()
```

### Results
![The Highest Paid and Most In-demand Skills for Data Analysts in India](Project\Images\salary_analysis_2.png)

*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in India*


### Insights
- Skills like PySpark, Linux, and GitLab command the highest median salaries—all exceeding $150K—indicating that Data Analysts with strong programming, big data, and DevOps-related expertise are particularly well compensated in the market.
- In-demand skills such as Power BI, Tableau, and Excel are more commonly sought after but offer lower median salaries (typically under $100K), showing that while these tools are essential for entry- and mid-level roles, they do not carry the same premium as niche or technical skills.
- There’s a notable gap between the most in-demand and highest-paying skills, suggesting that Data Analysts aiming for higher salaries should consider upskilling in back-end, data engineering, and cloud-related tools like PySpark, MongoDB, and Databricks—skills that are currently less saturated but highly valued.


## 4. What is the most optimal skill to learn for Data Analysts?

### Visualize Data
```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```

### Results
![Most Optimal Skills for Data Analysts in India](Project\Images\optimal_skill.png)

*A scatter plot visualizing the most optimal skills (high paying & high demand) for Data Analysts in India.*


### Insights

- SQL and Excel stand out as the most optimal mainstream skills, offering a strong combination of high job demand (over 55% of postings) and decent median salaries (around $80K+), making them essential and reliable for career growth.
- Skills like Power Automate ("flow") and Microsoft Word offer surprisingly high salaries despite low demand, suggesting they may serve as niche differentiators or be highly valued in specific domains or roles.
- Python and Tableau are moderately in-demand but offer slightly lower median salaries, indicating they are valuable to learn but may need to be paired with higher-paying niche skills for maximizing earning potential.



# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


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