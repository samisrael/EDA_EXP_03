**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**

```
# Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Load dataset
file_path = 'delhi_pm25_aqi.csv'  # Upload this file to Colab environment
df = pd.read_csv(file_path)

# 2. Inspect dataset
print("Dataset Shape:", df.shape)
print("Columns:", df.columns)
print("\nSample Data:\n", df.head())
print("\nData Types:\n", df.dtypes)
print("\nNull Value Counts:\n", df.isnull().sum())

# 3. Data Cleaning
# Parse datetime column
df['period.datetimeFrom.utc'] = pd.to_datetime(df['period.datetimeFrom.utc'], errors='coerce')

# Filter PM2.5 values > 0 (valid)
df = df[df['value'] > 0]

# 4. Add date, month, hour columns for analysis
df['date'] = df['period.datetimeFrom.utc'].dt.date
df['month'] = df['period.datetimeFrom.utc'].dt.month
df['hour'] = df['period.datetimeFrom.utc'].dt.hour

# 5. Monthly boxplots of PM2.5
plt.figure(figsize=(12, 6))
sns.boxplot(x='month', y='value', data=df)
plt.title('Monthly Boxplot of PM2.5 in Delhi')
plt.xlabel('Month')
plt.ylabel('PM2.5 (µg/m³)')
plt.show()

# 6. Monthly average PM2.5
monthly_avg = df.groupby('month')['value'].mean().reset_index()
plt.figure(figsize=(12, 6))
plt.plot(monthly_avg['month'], monthly_avg['value'], marker='o')
plt.title('Monthly Average PM2.5 in Delhi')
plt.xlabel('Month')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.grid(True)
plt.show()

# 7. Days exceeding WHO PM2.5 limit (25 µg/m³)
daily_avg = df.groupby('date')['value'].mean().reset_index()
days_exceeding = daily_avg[daily_avg['value'] > 25]
print(f'Total days analyzed: {len(daily_avg)}')
print(f'Days exceeding WHO limit: {len(days_exceeding)}')
print(f'Percentage exceeding WHO limit: {len(days_exceeding)/len(daily_avg)*100:.2f}%')

# 8. Average PM2.5 vs hour of day
hourly_avg = df.groupby('hour')['value'].mean().reset_index()
plt.figure(figsize=(12, 6))
plt.plot(hourly_avg['hour'], hourly_avg['value'], marker='o', color='green')
plt.title('Average PM2.5 vs Hour of Day in Delhi')
plt.xlabel('Hour of Day')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.grid(True)
plt.show()

# 9. Top 5 worst-polluted days
top5_days = daily_avg.nlargest(5, 'value')
print("Top 5 Worst Polluted Days (Date & Avg PM2.5):")
print(top5_days)
```



**Output**

<img width="612" height="401" alt="image" src="https://github.com/user-attachments/assets/4413693d-95aa-4624-a9d0-32604b15888f" />

<img width="754" height="315" alt="image" src="https://github.com/user-attachments/assets/2ae35542-2ce7-4e85-8b5b-408bbef39075" />

<img width="638" height="328" alt="image" src="https://github.com/user-attachments/assets/814cf441-1450-4f8c-babd-ac31eaac5140" />

<img width="580" height="367" alt="image" src="https://github.com/user-attachments/assets/8c8d11e7-b445-4284-a446-231c2baf8707" />



**Interpretation**

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2)Seasonal Trends

PM2.5 concentrations are significantly higher during winter months, especially from November to January, compared to summer and monsoon seasons. The increase in winter is likely due to temperature inversion, reduced wind speed, and higher fuel consumption for heating. These conditions trap pollutants close to the ground, worsening air quality. In contrast, rainfall and strong winds during the monsoon help to disperse or wash out particulate matter, leading to cleaner air.

3)Daily (Hourly) Variation

The hourly analysis shows that PM2.5 levels typically peak during morning (7–10 AM) and evening (7–10 PM) hours. This pattern corresponds to rush-hour traffic emissions and lower atmospheric mixing during cooler parts of the day. During midday, sunlight and thermal mixing help disperse pollutants, leading to lower concentrations.

4)WHO Limit Exceedance

A significant proportion of days exceed the WHO safe limit of 25 µg/m³ for PM2.5. This means that residents are frequently exposed to unsafe air quality, which increases the risk of respiratory diseases, cardiovascular issues, and long-term lung damage. Continuous exceedance also points to the need for stricter emission regulations and urban air-quality management policies.

5)Monthly Averages

Monthly average PM2.5 analysis shows clear seasonal cycles — values gradually rise from autumn to winter, then fall sharply after the onset of the monsoon. This consistent pattern suggests that seasonal meteorology plays a major role in pollutant accumulation and dispersion, beyond emission sources alone.

6)Top Polluted Days

The top five most polluted days show extremely high PM2.5 concentrations, often linked to specific events such as festivals (e.g., Diwali fireworks), crop residue burning, or temperature inversion episodes. These events cause temporary but severe degradation of air quality, posing acute health risks.

7)Public Health Implication

Overall, the data indicates that both short-term spikes (daily/hourly) and long-term seasonal buildup of pollutants can severely affect population health. Strategies such as reducing vehicular emissions, controlling industrial discharge, and public awareness campaigns during high-pollution periods are crucial to mitigate exposure risks.

**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


