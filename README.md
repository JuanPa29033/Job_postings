# Overview

Welcome to my analysis of data job market, focusing on data analyst roles. 

This project was driven by a desire to both improve my skills and gain a deeper understanding of the data job market. It explores the highest-paying and most in-demand skills to identify the best opportunities in the field of data analysis.

The data sourced from [Luke Barousse's Python Course](https://www.youtube.com/@LukeBarousse) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 mot popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the most optimal skills fod data analysts to learn? (High Demand AND High Paying)

# Tools I Used

* Python: I used Python to handle every part of the analysis—from cleaning and transforming the data to extracting insights and building visualizations. It gave me the tools I needed to deeply explore and understand the job market data.

    * Pandas: This was used to analyze the data. 

    * Matplotlib: I used Matplotlib to create visualizations.

    * Seaborn: Helped me create more advanced visuals.

* Visual Studio Code: My go-to for executing my Python scripts.

* Jupyter Notebook: I relied on Jupyter for exploring the data interactively and documenting each step of my analysis. It made it easy to test code, view results immediately, and build a narrative around my findings.

* GitHub: I used GitHub to track changes and share the project.

# Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market.

## 1. What are the most demanded skills for the top 3 most popular data roles?

To identify the most in-demand skills for the top three most popular data roles, I first filtered job postings to find the most frequently listed positions. Then, I extracted the top five skills associated with each of these roles. This analysis highlights the most sought-after job titles and their key skills, helping me understand which skills to focus on based on the roles I'm targeting.

View my notebook with detailed steps here:

[2_Skills_count.ipynb](2_Skills_count.ipynb)

### Visualize Data

```Python
fig, ax = plt.subplots(len(empleos), 1)

for i, empleo in enumerate(empleos):
    df_plot = df_merge[df_merge['job_title_short'] == empleo].head(5)

    #Usando seaborn
    sns.barplot(data = df_plot,
                x = '%',
                y = 'job_skills',
                ax = ax[i],
                legend = False,
                hue = '%',
                palette = 'dark:b_r')
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].set_title(empleo)
    ax[i].set_xlim(0, 78)

    # Agregar valores de datos para cada barra
    for index, valor in enumerate(df_plot['%']):
        ax[i].text(valor + 1, # + 1 es para alejar el valor de la barra ("valor" es un parametro que igual debe ir)
                    index,
                    f'{round(valor,1)}%',
                    va = 'center') # Para centrar el valor a la barra
    
fig.suptitle('Porcentaje de habilidades requeridas para empleos mas demandados', fontsize = 15)
sns.set_style('ticks')
plt.tight_layout()
plt.show()
```

### Results
![Visualization of Top Skills for Data roles](Images/Skills_Demand.png)

### Insights
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data scientist (72%) and Data Engenierrs (65%).

- SQL is the most requested skills por Data Analyst and Data scientiest, with it in over half the job postings for both roles. For Data Engieneers, Python is the most sought-after skill, appearing in 68% of job postings.

- Data Engienners requiere more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data scientisest who are expected to be proficient in more general data management and analysis tools (Excel, Tableu).


## 2. How are in-demand skills trending for Data Analysts?

### Insights.
- SQL remains the most consistenly demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel experienced  a significal increce in demand starting around september, surpassing both Python and Tableu by the end of the year.
- Both Python and Tableu show relatively stable demand throughout the year with some flutuations but remain essential skills for Data analysts.

View my notebook with detailed steps here:

[3_Skills_Trend.ipynb](3_Skills_Trend.ipynb)

### Visualize Data
```Python
df_plot = df_DA_USA_percent.copy()

sns.lineplot(data = df_plot,
             dashes = False,
             palette = 'tab10',
             legend = False)
plt.title('Tendencia de habilidades para Data Analyst in USA')
plt.xlabel('2023')
plt.ylabel('Porcentaje')

# Especificar para cada linea si habilidad
for i in range(0, 5, 1):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

sns.set_style(style = 'ticks')
sns.despine()
plt.show()
```

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis

#### Insights
- Senior roles like Senior Data Scientist and Senior Data Engineer show the highest median salaries (over $150K), highlighting the value of experience and advanced technical skills in the data field.

- With a median around $90K–$100K, the Data Analyst role offers the lowest pay and the least variability. It's often a starting point but has limited salary growth compared to more technical paths.

- All roles, especially technical ones, include many outliers above $200K, with some reaching $500K+. This indicates exceptional earning opportunities for top performers or those in high-paying industries.

View my notebook with detailed steps here:

[4_Salary_Analysis.ipynb](4_Salary_Analysis.ipynb)

#### Visualize Data

```Python
sns.boxplot(data = df_US_top6,
            x = 'salary_year_avg',
            y = 'job_title_short',
            order = nombres_empleos)
sns.set_style(style = 'ticks')

plt.title('Distribución de salarios en United States')
plt.xlabel('Salarios')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

#### Results
![Distribución de salarios en estados unidos](Images/Salary_Distribution.png)

*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Highest Paid & Most Demanded Skills for Data Analysts

#### Insights
- dplyr stands out as the highest-paying skill, indicating that expertise in R for data manipulation can be highly valuable in niche roles.

- Less common tools like bitbucket, gitlab, solidity, and hugging face offer high salaries, likely due to a limited talent pool and specialized demand.

- High-paying skills are not necessarily in high demand — many of the top-paying technologies do not appear in the most in-demand list, highlighting a gap between specialization and market size.

- Python is the most in-demand skill, confirming its central role in the data analytics ecosystem.

- Business Intelligence and reporting tools (Tableau, Power BI, Excel, PowerPoint) dominate the demand list, showing the importance of communication and visualization in analytics roles.

- SQL remains a foundational skill for data analysts, with both SQL and SQL Server appearing among the top 5, reflecting the ongoing need to work with structured data sources.

#### Visualize Data

```Python
fig, ax = plt.subplots(2,1)

sns.set_theme(style = 'ticks')

# seaborn
sns.barplot(data = df_DA_USA_masPagadas,
            x = 'median',
            y = 'job_skills',
            ax = ax[0],
            hue = 'median',
            palette = 'dark:b_r',
            legend = False)

sns.barplot(data = df_DA_USA_masPopulares,
            x = 'median',
            y = 'job_skills',
            ax = ax[1],
            hue = 'median',
            palette = 'dark:b_r',
            legend = False)

ax[0].set_title('Top 10 habiliades mas pagadas para Data Analyst en USA')
ax[0].set_ylabel('')
ax[0].set_xlabel('')

ax[1].set_title('Top 10 habiliades mas demandadas para Data Analyst en USA')
ax[1].set_ylabel('')
ax[1].set_xlabel('')
ax[1].set_xlim(ax[0].get_xlim()) # Limitar eje x del grafico 2 automaticamente en base al eje x del grafico 1 

plt.tight_layout()
plt.show()
```

#### Results
![Top 10 habiliades mas pagadas y demandadas para Data Analyst en USA](Images/Most_Demanded_and_Popular_Skills.png)

*Two separate  bar graphs visualizing  the highest paid skills and most in-demand skills for Data analysts in the US.*

## 4. How well do jobs and skills pay for Data Analysts?

### Optimal skills Analysis

#### Insights
- Python offers the highest salary (~$98K) and is in high demand (~35%), making it a highly optimal skill.

- SQL is the most in-demand skill (57%) but offers a moderate salary ($91K).

- Oracle and SQL Server provide high salaries (~$97K and ~$93K) but are in low demand (~7–10%).

- Word and PowerPoint have both low demand (<15%) and the lowest salaries (~$81K–$85K).

View my notebook with detailed steps here:

[5_Optimal_Skills.ipynb](5_Optimal_Skills.ipynb)

#### Visualize Data

```Python
from matplotlib.ticker import FuncFormatter

df_DA_skills.plot(kind='scatter',
                  x='skill_count_percent',
                  y='median_salary',
                  color='skyblue',
                  edgecolor='black')

# Agregar etiquetas (sin evitar overlapping)
for i, skill in enumerate(df_DA_skills.index):
    x = df_DA_skills.iloc[i]['skill_count_percent']
    y = df_DA_skills.iloc[i]['median_salary']
    plt.text(x, y, skill, fontsize=8)

ax = plt.gca()
ax.yaxis.set_major_formatter(FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))

plt.xlabel('Percentage of job offers')
plt.ylabel('Salary')
plt.title('Most optimal skills for Data Analyst')
plt.tight_layout()
plt.show()
```

#### Results
![Optimal skills for data analysts](Images/Optimal_Skills.png)

*Scatter plot visualizing the optimal skills for Data Analysts.*

# What I Learned

* Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualizations, and other libraries helped me perform complex data analysis tasks more efficiently.

* Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.

* Strategic Skill Analysis: The project emphasized the importance of alligning one's skills with market demand. 

# Challenges I Faced

* Data inconsistencies: One of the initial challenges was dealing with messy and inconsistent data. Job titles, salaries, and skills were often unstructured, which required a lot of preprocessing and transformation using Pandas before I could analyze them effectively.

* Balancing Readability and Complexity in Visualizations: Creating charts that were both informative and easy to interpret was challenging. I had to fine-tune the styling and choose the right chart types using Matplotlib and Seaborn to make the findings clear without overwhelming the viewer.

* Interpreting Salary Data: Job listings often had a wide salary range or missing values. Making sense of these discrepancies and presenting average or median salaries in a meaningful way required careful handling of outliers and null values.

# Conclusions

This project allowed me to dive deep into the data analyst job market and uncover key trends around skills, salaries, and job locations. It not only helped me understand the current demand but also guided me in identifying the most valuable skills to focus on for career growth.

Working through the data challenges and visualization decisions sharpened my technical abilities and reinforced the importance of clear, insightful communication through data. Overall, this analysis has equipped me with both knowledge and practical experience to better navigate the job market and make informed career choices.

Thi project is a good foundation for future explorations and underscores the importance of continuous learning and adapation in the data fild.