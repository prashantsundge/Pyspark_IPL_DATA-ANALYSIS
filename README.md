# Pyspark_IPL_DATA-ANALYSIS
Pyspark_IPL_DATA ANALYSIS

#Databricks link 

https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/1128994819925749/316827284202488/1164930366050986/latest.html

To create a `README.md` file for your Databricks project, you can follow this structure, adding relevant details about your project, its objectives, how to run the project, and any dependencies or libraries you are using. Below is a general template for the `README.md` file that can be used for a Databricks project:

---

# Databricks IPL Analysis Project

## Overview
This project analyzes the Indian Premier League (IPL) dataset using PySpark on Databricks. The goal of the project is to perform exploratory data analysis (EDA) on IPL data and generate insights such as the performance of players, top bowlers, and key statistics related to the matches.

The analysis involves:
- Analyzing match data for different seasons.
- Exploring performance metrics for players and teams.
- Generating insights on runs, wickets, boundaries, and other match statistics.
- Identifying key players and their performance in each season.

## Dataset Description
The project uses the following IPL dataset (till 2017):

- **Ball_by_ball.csv**: Detailed match-level data with information about each ball bowled, runs scored, wickets taken, and player actions.
- **Player_match.csv**: Data about individual players, including batting and bowling statistics, age, and role in the match.
- **Player.csv**: Information about each player such as player ID, name, batting hand, and bowling skill.
- **Match.csv**: Overview of each match, including venue, season, and team information.
- **Team.csv**: Data about IPL teams, their players, and performance.

## Dependencies
- **PySpark**: Used for processing large datasets in a distributed environment.
- **Matplotlib**: For generating visualizations like line charts, bar charts, and scatter plots.
- **Pandas**: For handling data in a tabular format and converting PySpark DataFrame to Pandas for visualization.

## Project Structure
```
├── Ball_by_ball.csv
├── Player_match.csv
├── Player.csv
├── Match.csv
├── Team.csv
└── Notebooks/
    ├── IPL_EDA_Notebook.ipynb
    
```

## Setup and Installation

1. **Clone the repository**:
   Clone this repository to your local machine or Databricks workspace.

   ```bash
   git clone https://github.com/yourusername/ipl-analysis-databricks.git
   ```

2. **Install Dependencies**:
   Ensure the following libraries are installed in your Databricks environment:
   ```python
   !pip install pyspark matplotlib pandas
   ```

3. **Upload Data to Databricks**:
   - Upload the CSV files (`Ball_by_ball.csv`, `Player_match.csv`, `Player.csv`, etc.) to Databricks using the **Data** tab or via **DBFS**.

## Data Loading and Exploration

```python
# Loading the datasets from DBFS
ball_by_ball_df = spark.read.csv("/dbfs/path_to_data/Ball_by_ball.csv", header=True, inferSchema=True)
player_match_df = spark.read.csv("/dbfs/path_to_data/Player_match.csv", header=True, inferSchema=True)
player_df = spark.read.csv("/dbfs/path_to_data/Player.csv", header=True, inferSchema=True)
match_df = spark.read.csv("/dbfs/path_to_data/Match.csv", header=True, inferSchema=True)
team_df = spark.read.csv("/dbfs/path_to_data/Team.csv", header=True, inferSchema=True)
```

## Exploratory Data Analysis (EDA)

### 1. Top Players by Runs Scored
```python
# Example: Grouping runs scored by each player and sorting by top scorers
runs_by_player_df = ball_by_ball_df.groupBy("Player_Name").agg(sum("Runs_Scored").alias("Total_Runs"))
runs_by_player_df.orderBy(col("Total_Runs").desc()).show(10)
```

### 2. Top Bowlers by Wickets
```python
# Example: Identifying top bowlers by wickets taken
top_bowlers_df = ball_by_ball_df.groupBy("Bowler").agg(sum("Bowler_Wicket").alias("Total_Wickets"))
top_bowlers_df.orderBy(col("Total_Wickets").desc()).show(10)
```

### 3. Runs Scored by Batting Position
```python
# Example: Total runs by batting position
runs_by_position_df = ball_by_ball_df.groupBy("Striker_Batting_Position").agg(sum("Runs_Scored").alias("Total_Runs"))
runs_by_position_df.show()
```

### 4. Season-wise Match Count
```python
# Example: Number of matches played in each season
season_count_df = match_df.groupBy("Season_Year").count().orderBy('Season_Year')
season_count_df.show()
```

## Insights
- **Best Performers**: Top players by runs and wickets across all seasons.
- **Venue Analysis**: Understanding how different venues impact player performance.
- **Bowling Strategy**: Identifying bowlers who take the most wickets in crucial situations.

## Visualizations
Here are some examples of visualizations generated in this project:

### 1. Runs Scored by Batting Position
```python
# Plotting total runs scored by batting position
plt.figure(figsize=(8, 5))
plt.bar(runs_by_position_pd["Striker_Batting_Position"], runs_by_position_pd["Total_Runs"], color="purple")
plt.xlabel("Batting Position")
plt.ylabel("Total Runs Scored")
plt.title("Total Runs by Batting Position")
plt.show()
```

### 2. Venue-wise Runs vs Wickets
```python
# Venue analysis with total runs vs wickets
fig, ax1 = plt.subplots(figsize=(10, 6))
ax1.bar(venue_performance_pd["Venue_Name"], venue_performance_pd["Total_Runs"], color="blue", label="Total Runs")
ax1.set_xlabel("Venue")
ax1.set_ylabel("Total Runs", color="blue")
ax1.tick_params(axis='x', rotation=90)

# Line chart for total wickets
ax2 = ax1.twinx()
ax2.plot(venue_performance_pd["Venue_Name"], venue_performance_pd["Total_Wickets"], color="red", marker="o", label="Total Wickets")
ax2.set_ylabel("Total Wickets", color="red")

plt.title("Venue Analysis: Total Runs vs. Wickets")
fig.tight_layout()
plt.show()
```

## Conclusion
This project provides insights into IPL data, showcasing the performance of players, teams, and bowlers across seasons. It can be extended further to include player comparisons, bowling strategy analysis, and match prediction models.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

This `README.md` template includes the project overview, instructions for setup, data loading code, EDA examples, and visualizations. You can further customize it according to your specific project.
