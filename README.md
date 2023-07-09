# fucho
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Sat Jul  8 14:15:44 2023

@author: aballados
"""
import pandas as pd
from itertools import groupby
# %% General

data = pd.read_csv("/Users/full_dataframe.csv")


def event_shot(events):
	event_type=events.query("type == 'Shot'")
	event_type=event_type.dropna(axis=1, how="all")
	return event_type

shots = event_shot(data)

columns=shots.columns
#shot_statsbomb_xg
# %% XG
grouped_data = shots.groupby(['match_id', 'player_id']).sum()

# Select the desired columns
selected_columns = ['shot_statsbomb_xg']
result = grouped_data[selected_columns]

result = result.reset_index()


# Export the result to a CSV file
result.to_csv('output.csv', index=False)

# Print the resulting DataFrame
print(result)
# %% Shots
grouped_data2 = shots.groupby(['match_id', 'player_id']).count()

# Select the desired columns
selected_shotcolumns = ['shots']
shotresult = grouped_data[selected_columns]

shotresult = result.reset_index()

# Extract the 'ID' column and rename it
grouped_data2['shots'] = grouped_data2['id']
grouped_data2 = grouped_data2.drop('id', axis=1)

selected_columns2 = ['shots']
shotresult2 = grouped_data2[selected_columns2]

# Print the resulting DataFrame
print(shotresult)

# %% Merge Shots & XG
# Perform the merge based on 'player_id' and 'match_id'
merged_data = pd.merge(result, shotresult2, on=['player_id', 'match_id'], how="outer")

# Print the merged data
print(merged_data)

# %% Export CSV
merged_data.to_csv('XGandShotsMerger2.csv', index=False)
