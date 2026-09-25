# Overview

Welcome to analysis of the data job market, fcusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for Data Analysts.

The data sourced from [Luke Barousse's Python Course](https://www.youtube.com/watch?v=wUSDVGivd-8) which provides the foundation of my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for Data Analysts to learn? (High demand and high paying)

# Tools Used

- Python: This is the backbone of my analysis.I used the following python libraries:
 1. Pandas: This was used to analyse the data
 2. Matplotlib: I used it to visualise the data
 3. Seaborn: Helped me create more advanced visuals.

 - Jupyter Notebooks: The tool I used to run my Python cripts which easily made me include my notes and analysis.
 - Visusal Studio Code: My go-to for executing my Python scripts.
 - Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration. 

 # Data Preparation and Cleanup

 This section outlines the steps taken to preparethe data for analysis, ensuring accuracy and usability.

 ## Import and Clean Up Data

 I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks.


# The Analysis
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here is how I approached each question: 

## 1. What are the most demanded skills for the  top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here:
[2_Skills_Count.ipynb](2_Skills_Count.ipynb)

### Visualize Data
```python
fig,ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    df_plot.plot(kind='barh', x='job_skills', y='skill_percent', ax=ax[i], title=job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].legend().set_visible(False)

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)
plt.show()
```
### Results

![Visualisation of top skills for data nerds](images\Skill_Demand_All_Data_Roles.png)

### Insights

- Python is a versatie skill, highly demanded across all three roles, but prominently for Data Scentists (72%) and Data Engineers(65%).
- SQL is the most requested skill for Data Analyists and Data Engineers, with it in over half the job postings for both roles.
- Data engineers require more specialised technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools.

## 2. How are in-demand skills trending for Data Analysts?

### Visualise Data

```python

df_plot = df_DA_US_percent.iloc[:, :5]

sns.lineplot(data = df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()


from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))



for i in range(5):
    plt.text(11.2, df_plot.iloc[-1,i], df_plot.columns[i])
```
### Results

![Trending skills for Data Analysts in the US](images\Skills_Trend_DA.png)

### Insights

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel experienced  a significant increase in demand starting around September, surpassing both Python and Tableau.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for Data Anlysts.
- Power BI, while less demanded, shows a slight upward trend towards the year end.

## 3. How well do jobs and skills pay for Data Anaysts?

### Salary Analysis

#### Salary Distributions for Data roles in the US

#### Visualise Data
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

#this is all the same
plt.title('Salary Distributions in the United States')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0,600000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
![Salary_Distributions](images\Salary_Distributions.png)
*Box plot visualisation for salary distribution of top 6 data jobs*

#### Highest Paid & Most Demanded Skills for Data Analysts

#### Visualise Data

```python
fig,ax = plt.subplots(2,1)

sns.set_theme(style='ticks')

#Top 10 Highest Paid Skills for Data Analysis
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()

ax[0].set_title('Top 10 Highest Paid Skills for Data Analyists')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/100)}K'))

#Top 10 most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()

ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
fig.tight_layout()
```
![Highest Paid and Most in Demand Skills for Data Analysts in the US](images\Highest_Paid_and_Most_in_Demand_Skills.png)
*Two seperate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US*

#### Insights:
- The top graph shows specialised technical skills like 'dplyr', 'Bitbucket', and 'Gitlab' are associated with higher salaries, meaning advanced technical proficiency can increase earning potential.
- The bottom graph highlights that foundational skills like 'Excel', 'PowerPoint', and 'SQL' are the most in demand even though they may not offer higher salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

- There is a clear distinction between the skills that are the highest paid and those that are the most in demand. Data analysts aiming to maximise their career potential should consider developing a diverse skill set that includes both high-paying specialised skills and widely demanded foundational skills.

## 4. What is the most optimal skill to learn for Data Analysts?

#### Visualise Data
```python
from adjustText import adjust_text

#df_plot.plot(kind='scatter', x='skill_percent', y='median_salary')

sns.scatterplot(
    data= df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
sns.despine()
sns.set_theme(style='ticks')
#Prepare texts f0r adjustment

texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

#Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

#Set axis labels, title and legend
plt.xlabel('Percentage of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary')
plt.title('Most Optimal Skills for Data Analysts in the US')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

#Adjust layout and display
plt.tight_layout()
plt.show()

```

![Most_Optimal_Skills_for_Data Analysts_in_the_US](images\Most_Optimal_SKills_for_Data_Analysts_in_the_US.png)

#### Insights

- The scatter plot shows that most of the programming skills tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer great salary benefits within the data analytics field.

- Analyst tools, including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualisation and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

- The database skills, such as Oracle and SQL server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.

# What I learned

Throughout this project, I deepened my understanding of the Data Analyst job marketand enhanced my technical skills in Python, expecially in data manipulation and visualisation. Here are a few specific things I learned:

- Advanced Python Usage: Utilising libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visusalisation, and other libraries helped me perform complex data analysis tasks more efficiently.
- Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- Strategic Skill Analysis: The project emphasised the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary and job availability allows for more strategic career planning in the tech industry.

# Insights
The project provided several general insights into the data job  marketfor analysts:

- Skill Demand and Salary Correlation: There is clear correlation between the demand for specific skills and the salary that the skills command. Advanced and specialised skills like Python and Oracle often lead to higher salaries.
- Market Trends: There are changing trands in skill demand, highlighting the dynamic nature of the data job market. keeping up with these trends is essential for career growth in data analytics.
- Economic Value of Skills: understanding which skills are both in demand and well compensated can guide data analysts in prioritising learning to maximise their economic returns.

# Challenges

This project was not withut its challenges, but it provided good learning opportunities:

- Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough techniques to ensure integrity of the analysis.
- Complex Data Visusaisation: Designing effective visual represantations of complex datasets was challenging in conveying insights clearly and compellingly.
- Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overall landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion

This exploration into the Data Analysts job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change,ongoing analysis willl be essential to stay ahead in data analytics. This project is a good foundstion for future explorations and underscores the importance of continuous learning and adaptation in the data field.







