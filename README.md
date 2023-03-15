PHASE1PROJECT

**Business** **Understanding** 

The movies dataset can be analyzed from a business perspective to gain insights into the financial performance of movies and the movie industry as a whole.

1. One area of analysis is identifying which studios are most successful at the box office. By examining which studios have the highest total gross across all movies, studios can identify which competitors they need to focus on in terms of marketing and production strategies.

2. Another key area of analysis is understanding the relationship between domestic and foreign gross. Studios can use this information to make strategic decisions about where to allocate their marketing and distribution resources.

3. The dataset can also be analyzed to identify trends in consumer preferences over time. By examining which types of movies perform well in different years, studios can make strategic decisions about which types of movies to produce and market in the future.

4. Finally, the dataset can be used to analyze the financial performance of individual movies. By examining the relationship between a movie's budget and its gross, studios can determine whether a particular movie was profitable or not. This analysis can inform decisions about how much to spend on future movie projects and where to allocate those funds.

Overall, analyzing the movies dataset can provide valuable insights into the financial performance of movies and the movie industry as a whole, and help studios make data-driven decisions about their production, marketing, and distribution strategies.

**Data Understanding**

#importing the necessary libraries

import pandas as pd

import numpy as np

import matplotlib.pyplot as plt 

%matplotlib inline

import seaborn as sns

1.1 Loading the 'bom.movie_gross.csv' data as a dataframe in pandas

df = pd.read_csv('bom.movie_gross.csv')

df

#displaying the first five rows of the dataset

df.head()

#displaying the last five rows of the dataset

df.tail()

From the above output, we can see that the dataset contains missing values 'NAN'

#getting a conscice summary of the data

df.info()

df.shape

The dataframe contains 3387 rows and 5 columns

#summary statistics of the data

df.describe()

The data understanding of this movies dataset is that it contains information about the title, studio, domestic gross, foreign gross, and year for a set of movies. The data is relatively clean, but it contains missing values in the "foreign_gross" variable, and some movies have very low gross earnings. The data includes a mix of categorical and numerical variables.

1.2 Loading and prevewing the tn.movies_budgets.csv data as a dataframe using pandas

df = pd.read_csv('tn.movie_budgets.csv')

df

#displaying the first five rows of the dataset

df.head()

#displaying the last five rows of the dataset

df.tail()

#getting a conscience summary of the data

df.info()

df.shape

the data contains 5782 rows and 6 columns

#summary statistics of the data

df.describe()

The dataset has 5782 rows and 6 columns, with no missing values. The release dates are in a consistent format, and the production budget, domestic gross, and worldwide gross are in US dollars. However, the production budget, domestic gross, and worldwide gross are stored as strings with a "$" sign and commas, so they will need to be cleaned and converted to numerical data before analysis.


**Data Preparation**
1. Check for and handle missing or null values if present.

2. Check for and handle duplicates.

3. Convert data types: Convert the "domestic_gross", "foreign_gross", and "year" columns to their appropriate data types. For example, "domestic_gross" and "foreign_gross" should be converted to integers or floats, and "year" should be converted to a date format.

4. Check for and handle outliers.

5. Handle categorical variables: If there are any categorical variables such as "studio" in the dataset, convert them to numerical values for analysis and modeling.

6. Create new columns from existing ones, such as calculating the total gross by adding the "domestic_gross" and "foreign_gross" columns.

7. Convert the release_date column to datetime format in the 'tn.movie_budgets.csv' dataset.
8. Remove the '$' sign and commas from the production_budget, domestic_gross, and worldwide_gross columns and convert them to numerical format in the 'tn.movie_budgets.csv' dataset.
9. Create any new columns required for analysis, such as profit or ROI.
10. 
#Load the data

df = pd.read_csv('bom.movie_gross.csv')
df

#checking for missing values

df.isna()

#total missing values

df.isna().sum()

#drop any rows with missing values

df.dropna(inplace=True)
df.isna().sum()

#checking for duplicates

df.duplicated().value_counts()

# Remove any rows with zero domestic_gross or foreign_gross
df = df[(df['domestic_gross'] != 0) & (df['foreign_gross'] != 0)]

# Convert foreign_gross to float
df['foreign_gross'] = df['foreign_gross'].str.replace('$', '').str.replace(',', '').astype(float)

# Calculate the total_gross by adding domestic_gross and foreign_gross
df['total_gross'] = df['domestic_gross'] + df['foreign_gross']
cleaned_bom_dataset = df
cleaned_bom_dataset





#loading the tn.movie_budgets.csv data
df = pd.read_csv('tn.movie_budgets.csv')
df

#checking for missing values
df.isna().any()

#checking for duplicates
df.duplicated().value_counts()

# Convert release_date column to datetime format
df['release_date'] = pd.to_datetime(df['release_date'])

# Remove the '$' sign and commas from the budget and revenue columns
df['production_budget'] = df['production_budget'].str.replace('$', '').str.replace(',', '').astype(float)
df['domestic_gross'] = df['domestic_gross'].str.replace('$', '').str.replace(',', '').astype(float)
df['worldwide_gross'] = df['worldwide_gross'].str.replace('$', '').str.replace(',', '').astype(float)

# Check for and remove any outliers
budget_q1 = df['production_budget'].quantile(0.25)
budget_q3 = df['production_budget'].quantile(0.75)
budget_iqr = budget_q3 - budget_q1
df = df[(df['production_budget'] >= budget_q1 - 1.5 * budget_iqr) & (df['production_budget'] <= budget_q3 + 1.5 * budget_iqr)]

revenue_q1 = df['worldwide_gross'].quantile(0.25)
revenue_q3 = df['worldwide_gross'].quantile(0.75)
revenue_iqr = revenue_q3 - revenue_q1
df = df[(df['worldwide_gross'] >= revenue_q1 - 1.5 * revenue_iqr) & (df['worldwide_gross'] <= revenue_q3 + 1.5 * revenue_iqr)]

# Create new columns for profit and ROI
df['profit'] = df['worldwide_gross'] - df['production_budget']
df['ROI'] = (df['profit'] / df['production_budget']) * 100

# Remove any rows with zero domestic_gross or foreign_gross
df = df[(df['domestic_gross'] != 0.0) & (df['worldwide_gross'] != 0.0)]

# Reset the index
df.reset_index(drop=True, inplace=True)
df

cleaned_tn_movie_dataset = df
cleaned_tn_movie_dataset
**Data Analysis** **And Visualization**
#getting an overview of the cleaned dataset
cleaned_bom_dataset.info()

#getting some descriptive statistics about the numerical columns
cleaned_bom_dataset.describe()

#Calculate the total gross for each studio
studio_totals = cleaned_bom_dataset.groupby('studio')['total_gross'].sum().sort_values(ascending=False)
studio_totals

# Calculate the average domestic gross for each year
yearly_averages = cleaned_bom_dataset.groupby('year')['domestic_gross'].mean()
yearly_averages

# Plot a line chart of the average domestic gross for each year
plt.plot(yearly_averages.index, yearly_averages.values)
plt.title('Average Domestic Gross by Year')
plt.xlabel('Year')
plt.ylabel('Average Domestic Gross')
plt.show()
![image](https://user-images.githubusercontent.com/125399280/225433807-c1f7680b-0990-4fce-a660-ecbfd5cbdf02.png)

The line graph above shows increased average domestic gross over the years

# Create a scatter plot of domestic gross vs foreign gross
plt.scatter(cleaned_bom_dataset['domestic_gross'], cleaned_bom_dataset['foreign_gross'])
plt.title('Domestic Gross vs Foreign Gross')
plt.xlabel('Domestic Gross')
plt.ylabel('Foreign Gross')
plt.show()
![image](https://user-images.githubusercontent.com/125399280/225433712-005cf101-8a90-4ca4-8224-a1145059198e.png)

 This scatter plot shows the relationship between domestic gross and foreign gross for each movie, which can help to identify any trends or patterns in how movies perform in different markets.
 
# Create a scatter plot of year vs total gross
plt.scatter(cleaned_bom_dataset['year'], cleaned_bom_dataset['total_gross'])
plt.title('Year vs Total Gross')
plt.xlabel('Year')
plt.ylabel('Total Gross')
plt.show()
![image](https://user-images.githubusercontent.com/125399280/225433581-674bada5-7a06-4982-b6b9-84280a201a04.png)

This scatter plot shows the relationship between year and total gross for each movie, which can help to identify any trends or patterns in how the movie industry has performed over time. 

# Create a bar chart of total gross revenue by year
sns.set_style('whitegrid')
plt.figure(figsize=(12, 6))
sns.barplot(x='year', y='total_gross', data=cleaned_bom_dataset, palette='viridis')
plt.title('Total Gross Revenue by Year')
plt.ylabel('Total Gross Revenue')
plt.xlabel('Year')
plt.show()
![image](https://user-images.githubusercontent.com/125399280/225433487-98375aff-42a7-4f5a-a83e-127b58033cae.png)

#Calculate the correlation between domestic gross and foreign gross
correlation = cleaned_bom_dataset['domestic_gross'].corr(cleaned_bom_dataset['foreign_gross'])

# Print the correlation coefficient
print(f"The correlation between domestic gross and foreign gross is {correlation:.2f}")
A correlation of 0.77 indicates a strong positive relationship between domestic_gross and foreign_gross. This means that as one variable increases, the other variable tends to increase as well. However, it is important to note that correlation does not necessarily imply causation.



#Data analysis and Visualization on the cleaned_tn_movie_dataset

#getting an overview of the cleaned dataset
cleaned_tn_movie_dataset.info()

#getting some descriptive statistics about the numerical columns
cleaned_tn_movie_dataset.describe()

From the output, we can see that the production budget, domestic gross, and worldwide gross columns have a wide range of values, with the minimum value being 0 and the maximum value being in the billions.

# Correlation between columns
print(cleaned_tn_movie_dataset.corr())

# Top 10 highest grossing movies
top_grossing = cleaned_tn_movie_dataset.sort_values('worldwide_gross', ascending=False).head(10)
print(top_grossing[['movie', 'worldwide_gross']])

# Top 10 most profitable movies
most_profitable = cleaned_tn_movie_dataset.sort_values('profit', ascending=False).head(10)
print(most_profitable[['movie', 'profit']])


# Top 10 movies with highest ROI
highest_ROI = cleaned_tn_movie_dataset.sort_values('ROI', ascending=False).head(10)
print(highest_ROI[['movie', 'ROI']])

# Scatterplot of production budget vs worldwide gross
sns.scatterplot(data=cleaned_tn_movie_dataset, x='production_budget', y='worldwide_gross')
plt.show()
![image](https://user-images.githubusercontent.com/125399280/225431887-e13c7c6a-3e8c-4bc9-ba9b-0378b0ab17f8.png)


There is a positive correlation between the production budget and worldwide gross, indicating that spending more on production can lead to higher profits.

# Scatterplot of domestic gross vs worldwide gross
sns.scatterplot(data=cleaned_tn_movie_dataset, x='domestic_gross', y='worldwide_gross')
plt.show()
![image](https://user-images.githubusercontent.com/125399280/225432297-94dbcb96-16bb-4732-bb38-5dbb816bdffc.png)


There's a strong positive correlation between the domestic gross and the worldwide gross
# Visualize the distribution of the production budget using a histogram
plt.hist(cleaned_tn_movie_dataset['production_budget'], bins=30)
plt.title('Distribution of Production Budget')
plt.xlabel('Production Budget (in millions of dollars)')
plt.ylabel('Frequency')
plt.show()
# Visualize the distribution of the production budget using a histogram
plt.hist(cleaned_tn_movie_dataset['production_budget'], bins=30)
plt.title('Distribution of Production Budget')
plt.xlabel('Production Budget (in millions of dollars)')
plt.ylabel('Frequency')
plt.show()
![image](https://user-images.githubusercontent.com/125399280/225432147-37b5747a-f928-4340-897f-4b63f77fb825.png)


**Business recommendations on the 'tn.movie_budgets.csv'**
1. Focus on high ROI: Movies that have a high ROI (Return on Investment) are the most profitable, so it's recommended to focus on producing movies with higher ROI. In the given dataset, some movies have a very high ROI, such as My Date With Drew with a ROI of 16,358.27%.

2. Consider worldwide gross: While domestic gross is important, it is equally important to consider the worldwide gross for a movie. A movie that performs well internationally can significantly increase its overall revenue. 

3. Control production budget: The dataset shows that movies with high production budgets don't necessarily guarantee high returns. Movies like Cutthroat Island and The Alamo had high production budgets but failed to perform well at the box office. It's essential to control production budgets and focus on producing quality content that resonates with the target audience.

**Business recommendations on the bom.movie_gross.csv**
1. Focus on high-grossing movies: The dataset shows that movies that grossed over $500 million worldwide were released by well-established studios like BV (Buena Vista) and WB (Warner Bros). Therefore, it would be wise for studios to focus on producing high-grossing movies that can generate significant revenue for the company.

2.  Expand overseas: The foreign gross revenue for the movies in the dataset was higher than the domestic gross revenue, indicating that it is essential for studios to expand their business overseas to increase their revenue. Studios should aim to distribute their movies in different countries and work with local distributors to increase their global reach.

3. Create franchise movies: Franchise movies like Harry Potter and Toy Story were some of the highest-grossing movies in the dataset. Therefore, studios should consider investing in creating movie franchises that can generate revenue from sequels, spin-offs, merchandise, and theme parks.

4. Focus on quality: The success of movies like Inception and Toy Story 3 shows that the quality of the movie matters. Studios should focus on producing quality content that can attract audiences and generate positive reviews, leading to more significant box office revenue.








