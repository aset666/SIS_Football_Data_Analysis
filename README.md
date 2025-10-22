# Football Data Analysis - English Premier League

## Description

This project analyzes data for English Premier League teams (2025/2026 season). It utilizes two primary data sources:
1.  **football-data.org API:** To retrieve detailed information about teams, including squad, coach, stadium, and founding year.
2.  **BBC Sport Web Scraping:** To obtain the current league standings table.

The project demonstrates data collection from different sources, cleaning, merging, and basic analysis with visualization.

## Data Sources

1.  **Football Data API:**
    * Endpoint: `https://api.football-data.org/v4/competitions/PL/teams`
    * Requires an API key (X-Auth-Token). The key `983c15f66740402b8a7794f63ebdac49` was used in the code. *Note: API keys should typically be stored securely, not directly in the code.*
2.  **BBC Sport:**
    * URL: `https://www.bbc.com/sport/football/premier-league/table`
    * Web scraping was used to extract the league table data.

## Workflow

1.  **API Data Collection:**
    * Request to the `football-data.org` API to get data about Premier League teams.
    * Extraction of necessary information (name, TLA, founded year, stadium, country) from the JSON response.
    * Creation of a DataFrame (`df`) with this information.
    * Saving the data to `Premier_League_Teams.csv`.
2.  **Web Scraping Data Collection:**
    * Request to the BBC Sport league table page.
    * Parsing the HTML using BeautifulSoup to extract table data.
    * Creation of a DataFrame (`df2`) with table data (position, team, played, won, drawn, lost, goals for/against, difference, points, form).
    * Saving the data to `premier_league_table.csv`.
3.  **Data Cleaning and Preparation:**
    * **DataFrame `df` (API):**
        * Removal of duplicates.
        * Filling missing values (though none were present in this case).
        * Renaming the `ShortName` column to `Team`.
    * **DataFrame `df2` (Scraping):**
        * Removal of duplicates.
        * Filling missing values (replaced with 0).
        * Cleaning team names (removing position numbers from the beginning of the string).
        * Renaming columns `Goals For` -> `GF`, `Goals Against` -> `GA`.
        * Converting numerical columns (Played, Won, Drawn, Lost, GF, GA, Goal Difference, Points) to numeric data type (`int64`).
4.  **Data Merging:**
    * Merging `df2` and `df` using `pd.merge` on the team name (`left_on='Team', right_on='Name'`). A left join (`how='left'`) was used.
    * *Note:* Due to potential discrepancies in team names between the API and BBC, some rows from `df` might not have joined. Missing values in the merged DataFrame (`clean`) were filled with median values for numeric columns and "Unknown" for text columns.
5.  **Analysis and Visualization:**
    * Calculation of descriptive statistics for numerical columns (`describe`).
    * Correlation analysis between numerical metrics (Played, Won, Drawn, Lost, GF, GA, Goal Difference, Points, Founded).
    * Visualization of the correlation matrix using a heatmap (`heatmap`).
    * Creation of box plots (`boxplot`) for key metrics (Points, GF, GA, Goal Difference, Won, Lost).
    * Creation of a scatter plot (`scatterplot`) to visualize the relationship between Goals For (GF) and Goals Against (GA).
    * Creation of a bar chart (`barplot`) to visualize the points for each team.

## Technologies Used

* Python 3
* Jupyter Notebook
* Python Libraries:
    * pandas
    * numpy
    * matplotlib
    * seaborn
    * requests
    * BeautifulSoup (bs4)

## Installation and Setup

1.  **Clone the repository (if applicable):**
    ```bash
    git clone <repository_url>
    cd <repository_directory>
    ```
2.  **Install dependencies:**
    ```bash
    pip install pandas numpy matplotlib seaborn requests beautifulsoup4
    ```
3.  **API Key:** Ensure you have a valid API key from `football-data.org`. It is included in the notebook, but for real projects, it's better to store it in a configuration file or environment variables.
4.  **Run Jupyter Notebook:**
    ```bash
    jupyter notebook Football_data_Analysis.ipynb
    ```
5.  Execute the cells in the notebook sequentially.

## Analysis and Findings

* **Correlation:** The correlation heatmap shows a strong positive relationship between Points (`Points`) and Wins (`Won`), Goals For (`GF`), and Goal Difference (`Goal Difference`). There is also a strong negative correlation between Points and Losses (`Lost`), and Goals Against (`GA`). This confirms the intuitive understanding: teams that win more, score more, and concede less tend to accumulate more points. The founding year of the team (`Founded`) has a weak correlation with current performance metrics.
* **Metric Distribution:** Box plots illustrate the distribution of key performance indicators among the league teams. They confirm that there are no apparent outliers in the data, and the distributions appear consistent.
* **GF vs GA:** The scatter plot visualizes the balance between attack and defense. Teams in the top-left quadrant (high GF, low GA) are typically higher in the league table.
* **League Standings:** The bar chart clearly shows the points distribution among the teams at the time the data was collected.

## Project Structure

* `Football_data_Analysis.ipynb`: The main Jupyter Notebook containing the code and analysis.
* `Premier_League_Teams.csv`: Output file with team data obtained from the API.
* `premier_league_table.csv`: Output file with league table data obtained from BBC Sport.
* `README.md`: This file describing the project.
