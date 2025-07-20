import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv('/content/factory_log_machines.csv')

df.fillna("No Remarks", inplace=True)

summary = df.groupby(['Machine ID', 'Shift']).agg({
    'Output Units': 'sum',
    'Downtime (min)': 'sum'
}).reset_index()

print(" Production Summary:")
display(summary)

pivot = summary.pivot(index='Machine ID', columns='Shift', values='Output Units')
pivot.plot(kind='bar', figsize=(20, 10))

plt.title('Total Output per Machine per Shift')
plt.xlabel('Machine ID')
plt.ylabel('Total Output Units')
plt.xticks(rotation=0)
plt.tight_layout()
plt.show()




summary.to_excel('summary_output.xlsx', index=False)

# Download it
from google.colab import files
files.download('summary_output.xlsx')
