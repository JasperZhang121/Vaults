```python
# Load datasets
current_data = pd.read_csv('train.csv')

# Descriptive statistics
print(current_data.describe())

# Target variable analysis
sns.histplot(current_data['variable'], kde=True)
plt.show()

# Feature correlation
correlation_matrix = current_data.corr()
sns.heatmap(correlation_matrix, annot=True)
plt.show()
```
