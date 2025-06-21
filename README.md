# python_data_projects
This contains basic practice and a project based on job info from the YouTube video 'Python for Data Analytics - Full Course for Beginners' by Luke Barrrouse 'https://www.youtube.com/watch?v=wUSDVGivd-8&t=36353s'
Located in the folder Data_analytics_python

# The Analysis 

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular roles, I filtered out those positions, and got the top 5 skills for these top three roles. This query highlights the most popluar job titles and their top skills, sjowing which skills I should pay attention to depending on which roles im tageting.

View my notebook with detailed steps here: [skills_count.ipynb](Data_Analytics_Python\Final_Data_project\skills_count.ipynb)


```python

fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    #df_plot.plot(kind='barh', x='job_skills', y='skill_percent', ax=ax[i], title=job_title)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    # ax[i].invert_yaxis() -- Not needed in sns
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].legend().remove()
    ax[i].set_xlim(0, 78)

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.2f}%', va='center')

    if i != len(job_titles) - 1:
        ax[i].set_xticks([]) 

fig.suptitle("Likelihood of Skills Requested in Job Postings", fontsize=15)
fig.tight_layout(h_pad=0.5) # Fix the overlap
plt.show()
```

### Results

![Visualization of Top Skills in Data](Data_Analytics_Python\images\Skills_required_in_job_postings.png)


### Insights

- Python is a versatile skill, highly demanded across all three top data roles, but most prominantly for Data Scientists (72%) and Data Engineers (65%).

- SQL is also a highly demanded skill, and the most requested skill in the Data Analyst and Data Scientist, with it in over half of the job postings for bith roles. For Data Engiineers, Python is the most sought-after skill, appearing in 68% of job postings.

- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general management and analysis tools (Excel, Tableau).


## 2. How are in-demand skills trendind for Data Analysis?

```python

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills For Data Analysts in The US')
plt.ylabel('Liklihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1,i], df_plot.columns[i])

plt.show()

```

### Results
![Trending Top Skills for Data Analysts in the US](Data_Analytics_Python\images\top_trending_data_skills.png)

*Bar graph visualizing the top trending skills for data analysts in the US in 2023.*

### Insights:

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.

- Excel experienced a significant increase in demand starting around September, suropassing both Python and Tableau by the end of the year.

- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts.
PowerBI, while less demanded compared to the others, shows a slight upward trend towards the years end.

## 3. How well do jobs and skills pay for data Analysis?

### Salary analysis for Data Nerds

#### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distributions in The United States')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

#### Results

![Salary Distributions in The United States](Data_Analytics_Python\images\salary_distributions.png)


#### Insights

- There is a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to 600K, indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles In contrast, Data analyst roles demonstrate more consistency in salary with fewer oultliers.

- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.