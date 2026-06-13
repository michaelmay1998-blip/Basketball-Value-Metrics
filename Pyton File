pip install pandas openpyxl
import pandas as pd
file_path = 'Mayscore_Seasons.xlsx'
#Create a library of each sheet
excel_file = pd.ExcelFile(file_path)
sheet_names = excel_file.sheet_names
dfs = {sheet: pd.read_excel(file_path, sheet_name=sheet) for sheet in sheet_names}
#Check for duplicate player entries
for season, df in dfs.items():
    # Find rows where 'Player' appears more than once
    duplicates = df[df.duplicated(subset=['Player'], keep=False)]
    
    if not duplicates.empty:
        print(f"Duplicates found in {season}:")
        # Print the player names and the rows
        print(duplicates[['Player']]) 
    else:
        print(f"No duplicates found in {season}.")
#Keep the first player entry as that will be the entry with the total for their multiple teams.
for season, df in dfs.items():
    df.drop_duplicates(subset=['Player'], keep='first', inplace=True)
    print(f"Duplicates removed from {season}")
#Lebron James season by season scores with his overall rank
import matplotlib.pyplot as plt

# 1. Initialize a list to hold data from each season
plot_data = []

# 1. Process all seasons to get LeBron's Rank and Score
for season, df in dfs.items():
    # Calculate rank for everyone in the league for this season
    df['Rank'] = df['Mayscore to 100'].rank(method='dense', ascending=False)
    
    # Filter for LeBron
    lebron = df[df['Player'].str.contains('LeBron James', case=False, na=False)]
    
    if not lebron.empty:
        score = lebron['Mayscore to 100'].iloc[0]
        rank = int(lebron['Rank'].iloc[0])
        plot_data.append({'Season': season, 'Score': score, 'Rank': rank})

# 2. Create DataFrame and sort by Season
df_plot = pd.DataFrame(plot_data).sort_values('Season')

# 3. Plotting
fig, ax = plt.subplots(figsize=(12, 6))

# Plot the main line of mayscore to 100
ax.plot(df_plot['Season'], df_plot['Score'], marker='o', linestyle='-', color='blue', label='Mayscore')

# 4. Add Overall Player Rank
for i, row in df_plot.iterrows():
    ax.text(row['Season'], row['Score'] + 1, f"Rank: {row['Rank']}", 
            ha='center', va='bottom', fontsize=9, fontweight='bold', color='darkred')

ax.set_title('LeBron James: Mayscore to 100 & League Rank by Season')
ax.set_ylabel('Mayscore to 100')
ax.set_xlabel('Season')
ax.grid(True, linestyle='--', alpha=0.6)

plt.tight_layout()
plt.show()

#Players who did not qualify by games played each season. Also will show health of the league.
import seaborn as sns
ineligible_counts = []

for season, df in dfs.items():
    # Count rows where the column equals 'Not Enough Games Played'
    if 'Enough Games Played?' in df.columns:
        count = df[df['Enough Games Played?'] == 'Not Enough Games Played'].shape[0]
        ineligible_counts.append({'Season': season, 'Count': count})
    else:
        print(f"Column 'Enough Games Played?' not found in {season}")

# 2. Create a DataFrame for plotting
df_counts = pd.DataFrame(ineligible_counts).sort_values('Season')

# 3. Plotting the results
plt.figure(figsize=(10, 6))
plt.bar(df_counts['Season'], df_counts['Count'], color='orange', edgecolor='black')

plt.title('Number of Players Not Meeting 25 Game Requirements per Season')
plt.xlabel('Season')
plt.ylabel('Number of Players')
plt.xticks(rotation=45)
plt.grid(axis='y', linestyle='--', alpha=0.7)

plt.tight_layout()
plt.show()

#This step I am inmporting the last 11 MVPs into the data frame and comparing the actual MVP vs the Mayscore MVP

mvp_data = {
    'Season': ['2015-16', '2016-17', '2017-18', '2018-19', '2019-20', '2020-21', 
               '2021-22', '2022-23', '2023-24', '2024-25', '2025-26'],
    'Actual_MVP': ['Stephen Curry', 'Russell Westbrook', 'James Harden', 'Giannis Antetokounmpo', 
                   'Giannis Antetokounmpo', 'Nikola Jokic', 'Nikola Jokic', 'Joel Embiid', 
                   'Nikola Jokic', 'Shai Gilgeous-Alexander', 'Shai Gilgeous-Alexander']
}
df_mvp = pd.DataFrame(mvp_data)

# 2. Plotting 
plt.figure(figsize=(14, 6))

# Plot the Mayscore Leaders 
plt.scatter(df_top['Season'], [100] * len(df_top), color='blue', s=100, label='Mayscore Leader')

# Plot the Actual MVPs 
plt.scatter(df_mvp['Season'], [99.8] * len(df_mvp), color='red', marker='X', s=100, label='Actual MVP')

# Add Labels
for i, row in df_top.iterrows():
    plt.text(row['Season'], 100.2, f"Mayscore: {row['Player']}", 
             rotation=45, ha='left', va='bottom', fontsize=9, color='blue')

for i, row in df_mvp.iterrows():
    plt.text(row['Season'], 99.6, f"MVP: {row['Actual_MVP']}", 
             rotation=-45, ha='left', va='top', fontsize=9, color='red')

plt.title('Mayscore Leader vs. Actual NBA MVP')
plt.ylim(99, 101)
plt.yticks([]) # Hide Y-axis as it's just a timeline
plt.legend()
plt.tight_layout()
plt.show()
