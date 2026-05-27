# 🧬 Python for Visualization

---

# 📌 What This Training Focuses On

We focus on:

- 📂 Metadata exploration
- 📊 Visualization (bar chart, pie chart, heatmap, line graph, tables)
- 🧬 SNP analysis
- 🌳 Phylogenetic tree visualization

All work is done in Jupyter Notebooks only.

---

# 📦 STEP 1 — Install and Import Libraries

```python
pip install pandas matplotlib seaborn biopython scikit-learn
```

<details>

<summary>📦 pip install — What this does</summary>

## 📘 What this does

This command installs all required Python libraries for data analysis and visualization:

- pandas → data handling and tables (DataFrames)  
- matplotlib → basic plotting and graphing  
- seaborn → advanced statistical visualization  
- biopython → biological data tools (including phylogenetic trees)  
- scikit-learn → machine learning tools and SNP distance calculations  

---

## ❌ Common mistakes beginners make

- Missing `!` when running inside Jupyter (`!pip install ...`)

</details>

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from Bio import Phylo
from sklearn.metrics import pairwise_distances
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

Loads all tools needed for:

- data handling (`pandas`)  
- visualization (`matplotlib`, `seaborn`)  
- SNP analysis (`scikit-learn`)  
- phylogenetic trees (`Biopython`)  

---

## 💡 Tip: 

Always run this cell before anything else.

</details>

---

# METADATA + BASIC VISUALIZATION

---

## 📂 1.1. Load Your Metadata (Real Dataset)
Before creating plots, data must be properly imported, inspected, and sanitized. Hand-collected clinical datasets often contain hidden issues like trailing spaces or incorrect data types.

```
import pandas as pd

# 1. Load the data into Python
df = pd.read_csv("metadata.csv")

# 2. Show the first few lines to make sure it looks correct
df.head()
# 3. View the last 5 rows of the dataset
df.tail()
```

<details>

<summary>📂 Loading CSV — What this does</summary>

## 📘 What this does

- `import pandas as pd` → imports the Pandas library for handling tabular data  
- `pd.read_csv("metadata.csv")` → reads the CSV file and loads it into a DataFrame called `df`  
- `df.head()` → displays the first 5 rows of the dataset to quickly check if the file loaded correctly
- `df.tail()` → prints out the final 5 entries of the DataFrame.

---

## ❌ Common mistakes 

- File not found error because `metadata.csv` is not in the working directory  
- Typing the wrong filename or missing `.csv` extension  
- Using `read_csv()` for Excel files instead of `read_excel()`  
- Forgetting to assign the data to a variable like `df`  

---

## 💡 Tip

Always run at least :

```python
df.head()
```

</details>

### more techniques
```python
# to view a specific number of rows from the bottom (for example, the last 10 rows), just put the number inside the parentheses:
df.tail(10)
```
Sometimes, checking only the top (head) or the bottom (tail) isn't enough because a data entry mistake might be hiding right in the middle of the spreadsheet. Taking a random sample lets you spot-check the data blindly.

```python
# 1. Look at 5 completely random rows from your table
df.sample(5)
```
The Quick Structural Health Check (df.info()),  It acts like an X-ray scan of your entire dataset, telling you your table's structure, storage size, data types, and missing cells all in one glance.
```python
# 1. View a complete structural summary of the dataset
df.info()
```
<details>
<summary>📘 What this does</summary>

`df.info()` prints out a technical summary sheet showing:

- The exact total number of entries (rows)
- A clean list of all column headers
- The number of non-null (not empty) cells in each column
- The data types (numbers vs. text objects)
- How much computer memory (RAM) the table is using

</details>


## 1.2. Check Table Size and Columns
```python
# 1. See how many rows and columns you have (Rows, Columns)
print(df.shape)

# 2. See a simple list of all your column titles
print(df.columns)

# 3. Ask Python to print out the data type of every single column
print(df.dtypes)
```
<details>

<summary>📊 Dataset Structure — What this does</summary>

## 📘 What this does

- `df.shape` acts like a tape measure. It tells you exactly how many patients (rows) and characteristics (columns) are in your file.  
- `df.columns` prints out a clean list of every column header so you know exactly how they are spelled.
- `print(df.dtypes)`gives
- - `int64`: Whole integers (for example, Age if there are no decimals)
- `float64`: Continuous numbers with decimals (for example, sequence quality scores or missing values converted to numbers)
- `object`: Text strings or mixed data types (for example, Sex, Region, CultureResult)
---

## ❌ Common mistakes beginners make

- Adding parentheses: writing `df.shape()` instead of `df.shape`.  
  - `shape` is a property, not a function, so it does not need `()`.

---

</details>

### more techniques
Check the Total Number of Rows Only (len()): While df.shape gives both rows and columns together as a pair, sometimes you only need the total number of entries (like the total number of patients) as a single integer for a report or loop.

```python
# 1. Get just the total number of rows (patients) in the table
total_rows = len(df)

# 2. Print out the number in a clean message
print("Total number of patients in this dataset:", total_rows)
```
<details>
<summary>📘 What this does</summary>

`len(df)` acts like a counter that tells you exactly how many horizontal rows are in your DataFrame.

It is a built-in Python tool that works very quickly even on massive genomic metadata datasets.

<summary>❌ Common mistakes beginners make</summary>

Confusing `len(df)` with `df.size`.

- `len(df)` counts the rows (for example, 88 rows)
- `df.size` multiplies rows by columns to give the total number of individual data cells in the entire dataset, which is usually not what a beginner wants

</details>


## 1.3. Remove Hidden Blank Spaces
```python
# 1. Clean the column names (remove hidden spaces at the start/end)
df.columns = df.columns.str.strip()

# 2. Clean the 'Sex' column entries specifically
df["Sex"] = df["Sex"].str.strip()

# 3. Clean the 'CultureResult' column entries specifically
df["CultureResult"] = df["CultureResult"].str.strip()
```

<details>

<summary>🧹 Cleaning Text — What this does</summary>

## 📘 What this does

Human typists often accidentally leave blank spaces in spreadsheets (for example, typing `"Male "` instead of `"Male"` or naming a column `"Sex "` with an invisible trailing space).

`.str.strip()` acts like an eraser that automatically removes invisible spaces from the edges of your text.

---

- `df.columns.str.strip()` → removes hidden spaces from all column names (e.g., `"Sex "` → `"Sex"`)  
- `df["Sex"].str.strip()` → removes extra spaces from values in the `Sex` column (e.g., `"Male "` → `"Male"`)  
- `df["CultureResult"].str.strip()` → removes extra spaces from values in the `CultureResult` column  
---

This helps ensure that Python treats values consistently without errors caused by invisible spaces.

---

## 💡 Tip

If Python says a column doesn't exist, always check your raw spreadsheet for hidden spaces!

</details>


## More techniques
### Bulk Cleaning Text Across Multiple Columns
```python
# 1. List all the text columns you want to strip of hidden spaces
text_columns = ["TwoWeeksCough", "ChestPain", "LowFeverGrade", "NightSweating", "AppetiteLoss", "WeightLoss", "CultureResult"]

# 2. Loop through each column name and strip whitespace automatically
for col in text_columns:
    df[col] = df[col].str.strip()

print("All listed columns successfully stripped of trailing spaces!")
```
<details>
<summary>📘 What this does</summary>

`for col in text_columns:` takes one column name from your list at a time.

`df[col] = df[col].str.strip()` temporarily targets that specific column and removes invisible spaces from the edges of its text fields.

<summary>💡 Tip</summary>

Using loops to clean multiple metadata columns:

- Saves screen space
- Prevents copy-paste typing errors
- Makes your bioinformatics code look professional and highly reproducible

</details>

### Cleaning Invisible Spaces from the Whole Table at Once (df.map())
```python
# 1. Apply the text-stripping eraser to every cell in the entire table using the modern map() function
# (Only targets cells that are read as text strings)
df = df.map(lambda x: x.strip() if isinstance(x, str) else x)

# 2. Print a success message
print("The entire dataset has been successfully stripped of hidden spaces!")
```
<details>
<summary>📘 What this does</summary>

`df.map(...)` loops through every single cell across every row and column in your spreadsheet automatically behind the scenes.

`lambda x: x.strip() if isinstance(x, str) else x` examines each individual data cell (`x`):

- If the cell contains a text string (`isinstance(x, str)`), it applies `.strip()` to remove blank spaces from both ends.
- If the cell contains a number (e.g., Age or MonthsStay), it leaves it unchanged so the code does not crash.

❌ Common mistakes 

Using `.map()` on an individual column vs. the whole table:

- If used on a single column (e.g., `df["Sex"].map(...)`), it transforms only that column.
- If used on the whole DataFrame (e.g., `df.map(...)`), it applies the transformation across every cell in the grid (in newer Pandas versions).

</details>

## 1.4. Count Missing (Empty) Cells

```python
# 1. Count how many empty/blank cells are in each column
print(df.isnull().sum())
```
<details>

<summary>🧹 Missing Values Check — What this does</summary>

## 📘 What this does

- `df.isnull()` → checks every cell in the dataset to see whether it is empty (NaN) or not  
- `.sum()` → counts how many missing values exist in each column  
- `print(...)` → displays the results clearly in the notebook or terminal  

This helps identify columns that may need cleaning before analysis or visualization.

---

## ❌ Common mistakes 

- Assuming datasets are complete without checking for missing values  
- Confusing empty strings (`""`) with true missing values (`NaN`)  
- Ignoring missing data before plotting or statistical analysis  
- Forgetting that missing values can break some calculations and visualizations  

---

## 💡 Tip

Always check for missing values early in your workflow before performing analysis.

</details>

### more codes 
View Rows with Missing Values `(df.isna())`
Counting missing cells with `.sum()` tells you how many exist, but sometimes scientists need to look at the exact rows where data is missing to understand why it wasn't recorded.

```python
# 1. Filter out and view only the rows where the 'zone' column is missing/empty
missing_zones = df[df["zone"].isna()]

# 2. Display the rows with missing data
missing_zones
```
<details>
<summary>📘 What this does</summary>

`df["zone"].isna()` scans every cell in the `zone` column and flags it as `True` if it is empty (NaN) and `False` if it has data.

`df[...]` then uses those Boolean flags like a filter window, showing only the rows where the condition is `True`—in this case, rows where the `zone` value is missing.

</details>

Safely Drop Rows with Missing Data `(df.dropna())`
If critical clinical data (like `CultureResult`) is missing for a sample, you might not be able to use it in your analysis. You can drop rows with empty cells, but you must teach students how to do it safely so they don't erase their entire spreadsheet.

```python
# 1. Safely remove rows ONLY if they are missing critical data in 'CultureResult'
df_cleaned = df.dropna(subset=["CultureResult"])

# 2. Compare the old size vs new size to see how many rows were dropped
print("Original size:", df.shape)
print("Size after dropping missing values:", df_cleaned.shape)
```

<details>
<summary>📘 What this does</summary>

`df.dropna(subset=["CultureResult"])` removes rows only if the missing value is inside the specified column(s).

This means it specifically checks `CultureResult`, and only deletes rows where that column is empty, while keeping rows that may have missing data in other columns.

<summary>❌ Common mistakes </summary>

Running `df.dropna()` without a `subset`:

If a beginner just types `df.dropna()`, Pandas deletes an entire row if even one single cell in any column is empty.

For example, if a patient is only missing a zone name, a plain `dropna()` can remove all their other valuable clinical data.

Using `subset=["CultureResult"]` makes the cleaning much safer and more controlled.

</details>


## 1.5 Handling Empty/Missing Cells (fillna)

```python
# 1. Check how many missing values are in the 'zone' column
print("Missing before:", df["zone"].isnull().sum())

# 2. Fill empty 'zone' cells with the text "Unknown" so the table is complete
df["zone"] = df["zone"].fillna("Unknown")

# 3. Double check to ensure 0 missing values remain in that column
print("Missing after:", df["zone"].isnull().sum())
```

<details>
<summary>📘 What this does</summary>

`.fillna("Unknown")` looks at every cell containing `NaN` (Not a Number / empty) within that specific column and replaces it with the word `"Unknown"`.

This prevents errors when downstream scripts expect every cell to contain text.

<summary>❌ Common mistakes beginners make</summary>

Dropping everything by accident:

Beginners often use `df.dropna()`, which deletes the entire row if even one single cell is empty.

For example, if a patient is missing just a zone name, using `dropna()` throws away all their other valuable data.

- Filling missing cells with `"Unknown"` or `"Missing"` is usually much safer.

</details>



## 1.6 Count How Many Times Each Category Appears
```python
# 1. Count how many patients come from each region
print(df["region"].value_counts())

# 2. Count how many patients are smokers vs non-smokers
print(df["Smoking"].value_counts())
```
<details>

<summary>📊 Category Counts — What this does</summary>

## 📘 What this does

- `df["region"].value_counts()` → counts how many patients come from each region and sorts them from highest to lowest  
- `df["Smoking"].value_counts()` → counts how many patients are smokers vs non-smokers  

- `print(...)` → displays the results clearly in the notebook or terminal  

These functions are used to quickly understand how data is distributed across categories.

---

## ❌ Common mistakes

- Forgetting that `value_counts()` automatically sorts results  
- Misspelling column names (`Smoking` vs `smoking`)  
- Not noticing inconsistent labels (e.g., `Yes`, `yes`, `YES`)  
- Assuming categories are already clean without checking  
---

## 💡 Tip

Always run `value_counts()` before visualization to understand your dataset structure.

</details>

If you want to see these counts as percentages instead of raw numbers, add normalize=True and multiply by $100$:]
```python
df["region"].value_counts(normalize=True) * 100
```
## 1.7  Viewing Only Specific Columns
```python
# 1. Choose a few specific columns to look at
mini_df = df[["Age", "Sex", "region"]]

# 2. Preview the new smaller table
mini_df.head()
```
<details>

<summary>📋 Selecting Specific Columns — What this does</summary>

## 📘 What this does

- `df[["Age", "Sex", "region"]]` → selects only the specified columns from the full dataset  
- `mini_df = ...` → creates a new smaller DataFrame containing only those columns  
- `mini_df.head()` → displays the first 5 rows of the reduced dataset for preview  

This is useful when you want to focus only on important variables and simplify your analysis.

---

## ❌ Common mistakes

- Forgetting double brackets `[[ ]]` when selecting multiple columns  
- Misspelling column names (`region` vs `Region`)  
- Expecting the original dataset (`df`) to change (it does not)  
- Trying to select columns that do not exist in the dataset  

---

## 💡 Tip

Creating a smaller dataset helps make analysis faster and easier to manage, especially with large clinical data.

</details>

## 1.8 Summary Statistics for Numbers (Quick Overview)
```python
# 1. Look at the summary statistics for all numeric columns
df.describe()
```
<details>
<summary>📘 What this does</summary>

`df.describe()` automatically finds all numerical columns and calculates:

- **count**: Total number of non-empty cells.
- **mean**: The average value.
- **std**: Standard deviation (how spread out the values are).
- **min / max**: The lowest and highest values in the dataset.
- **25%, 50%, 75%**: Percentiles (the 50% mark is your absolute median).

</details>

<details>
<summary>❌ Common mistakes beginners make</summary>

Running this before converting data types:  
If your `"Age"` column is still being read as text/strings, `df.describe()` will completely ignore it.

Always run `pd.to_numeric()` first!

</details>

## 1.9 Finding and Dropping Duplicate Rows

```python
# 1. Count how many completely identical duplicate rows exist
print("Number of duplicate rows:", df.duplicated().sum())

# 2. Remove the duplicate rows and keep only the first occurrence
df = df.drop_duplicates()

# 3. Verify the new shape of the table
print("Table size after removing duplicates:", df.shape)
```
<details>
<summary>📘 What this does</summary>

`df.duplicated().sum()` scans the whole table, looks for rows where every single column matches another row perfectly, and counts them.

`df.drop_duplicates()` deletes the duplicate rows and leaves you with unique records.

</details>

<details>
<summary>💡 Tip</summary>

If you only want to look for duplicates based on a specific column (like checking if a `Sample_ID` was reused accidentally), you can add a `subset` argument.
```python
df.duplicated(subset=["Sample_ID"]).sum()
```
</details>

## 1.10 Advanced Category Standardization (Mapping Clean Labels)
As you discovered, human data entry can result in many different ways to spell the same category (e.g., "Male ", "M", "male", "Male"). Instead of writing multiple separate lines of code, we can clean up everything at once using a single, unified mapping dictionary.
```python
# 1. See what messy categories exist right now
print("Before cleaning:", df["Sex"].unique())

# 2. Use ONE clean dictionary to catch all variations at the same time
clean_map = {
    "Male ": "Male", "M": "Male", "male": "Male",
    "Female ": "Female", "F": "Female", "female": "Female"
}

# 3. Apply the dictionary to standardise the column
df["Sex"] = df["Sex"].replace(clean_map)

# 4. Verify that it worked perfectly
print("After cleaning:", df["Sex"].unique())
```
<details>
<summary>📘 What this does</summary>

`clean_map = {...}` creates a clear translation guide.  
The messy text on the left (**key**) gets transformed into the clean text on the right (**value**).

`df["Sex"].replace(clean_map)` makes Pandas look at every single cell in the column.  
If a cell matches any key in your translation guide, it immediately switches it to your clean standard name.

</details>

<details>
<summary>❌ Common mistakes beginners make</summary>

Accidentally overwriting correct data:  
If you miss a category or misspell your destination label (for example, typing `"Mlae"` instead of `"Male"`), Python will save your mistake into the whole dataset.

Always run `.unique()` before and after to verify the changes.

</details>



## 1.11 Filtering Rows (Finding Specific Patients)

```python
# 1. Filter out and view only patients who are older than 30 years old
older_patients = df[df["Age"] > 30]

# 2. Check how many patients matched this condition (Rows, Columns)
print(older_patients.shape)
```
<details>

<summary>🔎 Filtering Data — Patients Older Than 30</summary>

## 📘 What this does

- `df[df["Age"] > 30]` → filters the dataset to include only patients whose age is greater than 30  
- `older_patients = ...` → stores the filtered dataset in a new variable  
- `older_patients.shape` → shows how many rows (patients) and columns are in the filtered dataset  
- `print(...)` → displays the result in the notebook or terminal  

This helps you focus on a specific subgroup of patients for analysis.

---

## ❌ Common mistakes

- Forgetting to ensure `Age` is numeric before filtering  
- Using incorrect comparison operators (`=` instead of `>`)  
- Thinking the original dataset (`df`) is modified (it is not)  
- Missing parentheses or incorrect column names (`age` vs `Age`)  

---

## 💡 Tip

Filtering is one of the most powerful tools in data analysis—always double-check your conditions before applying them.

</details>

## 1.12. Convert Age Into Numbers

```python
# 1. Force the 'Age' column to be read as math numbers
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")

# 2. Check the column's average to prove it is numeric now
print(df["Age"].mean())
```
<details>

<summary>🔢 Converting to Numbers — What this does</summary>

## 📘 What this does

Sometimes numbers get imported as "text" instead of real numeric values. This happens if a column contains words like `"Unknown"` or has typing errors.

- `pd.to_numeric()` converts the column into proper numeric values.  
- `errors="coerce"` tells Python: if a value cannot be converted into a number, turn it into `NaN` (empty value) instead of crashing.

---

## ❌ Common mistakes

- Trying to calculate averages or build plots while the column is still stored as text instead of numbers.  

---
💡 Tip

Always verify conversion using:
```python
df["Age"].dtype
```

before doing statistical analysis.
</details>

## 1.13. Renaming Columns for Cleanliness
```python
# 1. Fix capitalization and remove question marks from column headers
df = df.rename(columns={"region": "Region", "ChewKhat?": "ChewKhat"})

# 2. Print columns to verify the change
print(df.columns)
```
<details>

<summary>🏷️ Renaming Columns — What this does</summary>

## 📘 What this does

- `df.rename(columns={"region": "Region", "ChewKhat?": "ChewKhat"})`  
  → renames columns to fix inconsistencies in naming  
  → changes:
  - `region` → `Region`  
  - `ChewKhat?` → `ChewKhat`  

- `df = ...` → saves the updated column names back into the DataFrame  
- `print(df.columns)` → displays all column names to confirm the changes worked  

This helps standardize column names so analysis and plotting work correctly.

---

## ❌ Common mistakes

- Forgetting to assign back to `df` (so changes are not saved)  
- Misspelling column names in the rename dictionary  
- Assuming renaming changes data instead of just labels  
- Not verifying changes using `df.columns`  

---

## 💡 Tip

Always print `df.columns` after renaming to confirm everything was updated correctly.

</details>

## 1.14. List Unique Categories
```python
# 1. See all unique categories inside the 'Sex' column
print(df["Sex"].unique())

# 2. See all unique categories inside the 'CultureResult' column
print(df["CultureResult"].unique())
```

<details>

<summary>🔍 Unique Values Check — Data Consistency</summary>

## 📘 What this does

- `df["Sex"].unique()` → shows all distinct values in the `Sex` column (e.g., Male, Female, etc.)  
- `df["CultureResult"].unique()` → shows all distinct values in the `CultureResult` column (e.g., Positive, Negative, etc.)  

This helps you detect inconsistencies like:

- `"Male"` vs `"male"`  
- `"Positive"` vs `"positive"`  

---

## ❌ Common mistakes

- Skipping this check and later getting duplicate categories in plots  
- Assuming data is clean without verifying spelling and formatting  
- Not noticing case differences (upper vs lower case)  

---

## 💡 Tip

Always run `.unique()` before visualization to ensure your categories are clean and consistent.

</details>

---


# 📊 2. AGE DISTRIBUTION 
## Approach A: The Quick & Simple "Pandas-Only" Way
### 2.1. The Built-in Pandas Histogram Shortcut
```python
# Generate a quick histogram using ONLY pandas
df["Age"].plot(kind="hist", bins=10, color="orange", edgecolor="black")

plt.title("Quick Age Distribution Chart")
plt.show()
```
<details>

<summary>📊 Pandas Histogram — What this does</summary>

## 📘 What this does

- `df["Age"].plot(kind="hist")` → creates a histogram directly using Pandas without Seaborn  
- `bins=10` → splits the data into 10 age ranges  
- `color="orange"` → changes the bar color to orange  
- `edgecolor="black"` → adds black borders to bars for better visibility  
- `plt.title()` → adds a title to the chart  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `Age` must be numeric before plotting  
- Confusing Pandas plotting with Seaborn plotting  
- Missing `plt.show()` so nothing appears  
- Not adding labels or title, making the chart unclear  

---

## 💡 Tip

Pandas plotting is useful for quick and simple visualizations, but Seaborn is better for publication-quality plots.

</details>

## Approach B: The Step-by-Step Breakdown matplotlib and seaborn
### 2.1. Draw the Base Histogram
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Draw just the plain bars
sns.histplot(df["Age"])

# Always put this at the very end to show the plot
plt.show()
```
<details>

<summary>📊 Histogram Code — Line-by-Line Explanation</summary>

## 📘 Line-by-line explanation

### `import matplotlib.pyplot as plt`

This imports Matplotlib’s plotting module, which is responsible for displaying figures and graphs in Python.

We shorten it to `plt` so we can easily call plotting functions.

---

### `import seaborn as sns`

This imports Seaborn, a high-level visualization library built on top of Matplotlib.

We shorten it to `sns` so we can quickly call visualization functions like histograms, bar plots, and heatmaps.

---

### `sns.histplot(df["Age"])`

This creates a histogram of the `Age` column from your dataset.

- It groups ages into bins (age ranges)  
- Then counts how many patients fall into each bin  
- Helps you understand the distribution of ages  

---

### `plt.show()`

This command displays the plot on the screen.

Without it, the graph may not appear in some environments (especially scripts or some Jupyter setups).

---

## ❌ Common mistakes beginners make

- Forgetting to import `seaborn` or `matplotlib`  
- Using a column name that does not exist (e.g., `"age"` vs `"Age"`)  
- Not checking if `Age` contains text instead of numbers  
- Forgetting `plt.show()` so the plot does not display  

---

</details>

### 2.2. Adding Titles and Axis Labels
```python
# 1. Draw the histogram bars
sns.histplot(df["Age"])

# 2. Add titles and labels so humans can read it
plt.title("Age Distribution of TB Patients")
plt.xlabel("Age of Patients (Years)")
plt.ylabel("Number of Patients")

plt.show()
```

<details>

<summary>📊 Age Histogram — What this does</summary>

## 📘 What this does

- `sns.histplot(df["Age"])` → draws a histogram showing how patient ages are distributed  
- `plt.title()` → adds a clear title to the graph  
- `plt.xlabel()` → labels the X-axis (Age in years)  
- `plt.ylabel()` → labels the Y-axis (number of patients)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to check if `Age` is numeric before plotting  
- Missing `plt.show()` so the graph does not appear  
- Using wrong column names (`age` vs `Age`)  
- Forgetting labels, making graphs hard to understand  

---

## 💡 Tip

Always label your plots clearly so others can understand your results instantly.

</details>

### 🎨 2.3. Customizing Bins, Colors, and Trend Lines
```python
# Make the chart larger, change color, and add a smooth trend line (kde)
sns.histplot(df["Age"], bins=10, color="teal", kde=True)

plt.title("Age Distribution of TB Patients")
plt.xlabel("Age (Years)")
plt.ylabel("Patient Count")

plt.show()
```

<details>

<summary>📊 Enhanced Age Histogram — What this does</summary>

## 📘 What this does

- `sns.histplot(df["Age"], bins=10, color="teal", kde=True)`  
  → Creates a histogram of patient ages  
  → `bins=10` groups ages into 10 ranges  
  → `color="teal"` changes the bar color  
  → `kde=True` adds a smooth curve showing the overall distribution trend  

- `plt.title()` → adds a title to the plot  
- `plt.xlabel()` → labels the X-axis (Age in years)  
- `plt.ylabel()` → labels the Y-axis (number of patients)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to convert `Age` to numeric before plotting  
- Using too many bins, making the plot noisy  
- Forgetting `kde=True` effect is optional and may confuse beginners  
- Missing labels, making interpretation difficult  

---

## 💡 Tip

Use `kde=True` when you want to see the **overall trend shape** of your data, not just raw counts.

</details>
---





# 📊 4. SEX DISTRIBUTION (BAR CHART)

```python
sns.countplot(data=df, x="Sex")

plt.title("Gender Distribution")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Counts male vs female patients  
- Shows simple distribution  

---

## ❌ Common mistakes beginners make

- Column written as `sex` vs `Sex`  
- Missing values or inconsistent labels (M, Male, male)  

---

## 💡 Tip

Standardize values first:

```python
df["Sex"] = df["Sex"].str.lower()
```
</details> 

---

# 📊 5. REGION DISTRIBUTION

## 📊 5.1. Basic Count Plot with Label Rotation
```python
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Create the vertical bar chart
sns.countplot(data=df, x="region", palette="pastel")

# 2. Rotate the region names so they don't overlap!
plt.xticks(rotation=45)

# 3. Add titles
plt.title("Number of TB Cases by Region")
plt.xlabel("Region of Origin")
plt.ylabel("Patient Count")
plt.show()
```

<details>

<summary>📊 Region Bar Chart — What this does</summary>

## 📘 What this does

- `sns.countplot(data=df, x="region", palette="pastel")` → creates a bar chart that counts how many patients come from each region  
- `plt.xticks(rotation=45)` → rotates region labels so they don’t overlap and become unreadable  
- `plt.title()` → adds a clear title to the chart  
- `plt.xlabel()` → labels the X-axis (region names)  
- `plt.ylabel()` → labels the Y-axis (number of patients)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Misspelled column name (`region` vs `Region`)  
- Overlapping x-axis labels (forgetting `rotation=45`)  
- Using palette without understanding color grouping  
- Forgetting `plt.show()` so the plot does not display  

---

## 💡 Tip

Use `countplot()` when you want to quickly compare category counts across groups.

</details>

## 5.2. Sorting the Bars from Highest to Lowest
```python
# 1. Find the order of regions from most frequent to least frequent
highest_to_lowest = df["region"].value_counts().index

# 2. Tell Seaborn to plot the bars in that exact order
sns.countplot(data=df, x="region", order=highest_to_lowest, palette="muted")

plt.xticks(rotation=45)
plt.title("Ordered TB Cases by Region")

plt.show()
```
<details>

<summary>📊 Ordered Region Bar Chart — What this does</summary>

## 📘 What this does

- `df["region"].value_counts().index` → finds regions sorted from most common to least common  
- `order=highest_to_lowest` → forces Seaborn to display bars in that sorted order  
- `sns.countplot(...)` → creates a bar chart showing number of TB cases per region  
- `plt.xticks(rotation=45)` → rotates region names so they don’t overlap  
- `plt.title()` → adds a clear title to the plot  
- `plt.show()` → displays the final graph  

---

## ❌ Common mistakes beginners make

- Forgetting to define `order` so bars appear in random order  
- Misunderstanding `.value_counts().index`  
- Misspelled column name (`region` vs `Region`)  
- Overlapping x-axis labels without rotation  

---

## 💡 Tip

Always sort categorical data when order matters — it makes patterns much easier to see.

</details>



## 🗺️ 5.3. The Horizontal Bar Chart (Best for Long Names)
```python
# Switch x and y! Put region on the y-axis to lay the bars flat
sns.countplot(data=df, y="region", palette="Set3")

plt.title("TB Cases by Region (Horizontal View)")
plt.xlabel("Number of Patients")
plt.ylabel("Region")

plt.show()
```

<details>

<summary>📊 Horizontal Region Bar Chart — What this does</summary>

## 📘 What this does

- `sns.countplot(data=df, y="region")` → creates a horizontal bar chart showing counts of TB cases per region  
- `palette="Set3"` → applies a color palette to make bars visually distinct  
- `plt.title()` → adds a title to the chart  
- `plt.xlabel()` → labels the X-axis (number of patients)  
- `plt.ylabel()` → labels the Y-axis (region names)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Confusing `x` and `y` in Seaborn countplot  
- Missing column name consistency (`region` vs `Region`)  
- Forgetting axis labels after switching orientation  
- Using too many categories, making the chart long  

---

## 💡 Tip

Horizontal bar charts are very useful when category names are long or many.

</details>

## 📈 5.4.  The Simplified "Pandas-Only" Way Sorted Pandas Bar Chart Shortcut
```python
# 1. Count the regions (Pandas automatically sorts them highest to lowest!)
region_counts = df["region"].value_counts()

# 2. Plot as a bar chart with custom colors and clean black edges
region_counts.plot(kind="bar", color="cornflowerblue", edgecolor="black")

plt.title("TB Cases by Region")
plt.xlabel("Region")
plt.ylabel("Count")
plt.xticks(rotation=45)

plt.show()
```
<details>

<summary>📊 Region Bar Chart (Pandas Version) — What this does</summary>

## 📘 What this does

- `df["region"].value_counts()` → counts how many TB cases exist in each region and sorts them from highest to lowest  
- `region_counts.plot(kind="bar")` → creates a bar chart using Pandas plotting  
- `color="cornflowerblue"` → sets bar color for better visual clarity  
- `edgecolor="black"` → adds borders around bars for better separation  
- `plt.title()` → adds a chart title  
- `plt.xlabel()` → labels the X-axis (region names)  
- `plt.ylabel()` → labels the Y-axis (number of cases)  
- `plt.xticks(rotation=45)` → rotates region names to prevent overlap  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `value_counts()` already sorts data automatically  
- Confusing Pandas plotting with Seaborn plotting  
- Misspelling column names (`region` vs `Region`)  
- Forgetting `plt.show()` so the chart does not appear  
- Overlapping labels when not rotating x-axis text  

---

## 💡 Tip

Pandas plotting is faster for quick analysis, while Seaborn is better for polished visualizations.

</details>

## 5.5 If you want both the Count AND the Percentage: Count (Percentage%)
```python
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Create the chart
ax = sns.countplot(data=df, x="region", palette="pastel")
total = len(df)

# 2. Add percentages on top of each bar (compact look)
for p in ax.patches:
    if p.get_height() > 0:
        ax.annotate(f'{100 * p.get_height() / total:.1f}%', 
                    (p.get_x() + p.get_width() / 2, p.get_height()), 
                    ha='center', va='bottom', xytext=(0, 4), textcoords='offset points')

# 3. Titles and clean layout
plt.xticks(rotation=45)
plt.title("Number of TB Cases by Region")
plt.xlabel("Region of Origin")
plt.ylabel("Patient Count")
plt.show()
```
<details>

<summary>📊 Region Bar Chart with Percentages — What this does</summary>

## 📘 What this does

- `sns.countplot(...)` → creates a bar chart showing number of TB cases per region  
- `total = len(df)` → calculates total number of patients in the dataset  
- `ax.patches` → loops through each bar in the chart  
- `ax.annotate(...)` → writes percentage values on top of each bar  
- `100 * p.get_height() / total` → converts counts into percentages  
- `plt.xticks(rotation=45)` → rotates region labels to avoid overlap  
- `plt.title()` → adds chart title  
- `plt.xlabel()` → labels X-axis (region names)  
- `plt.ylabel()` → labels Y-axis (patient count)  
- `plt.show()` → displays final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to calculate `total = len(df)` before percentage calculation  
- Using wrong column name (`region` vs `Region`)  
- Not understanding `ax.patches` loop structure  
- Overlapping labels when not rotating x-axis  
- Incorrect percentage formula or integer division mistakes  

---

## 💡 Tip

Adding percentages makes bar charts more informative and publication-ready, especially for epidemiology data.

</details>

---

# 🥧 6. CULTURE RESULT (PIE CHART)
## Option A: The Enhanced Pie Chart (Custom Colors & Edge Borders)
This option upgrades the standard pie chart by adding custom colors and distinct borders between slices, which makes it much easier to read.

```python
import matplotlib.pyplot as plt

# 1. Count the values
culture_counts = df["CultureResult"].value_counts()

# 2. Draw the pie chart (Fixed by placing edgecolor inside wedgeprops)
culture_counts.plot(
    kind="pie", 
    autopct="%1.1f%%", 
    colors=["tomato", "lightskyblue"], 
    wedgeprops=dict(edgecolor="white", linewidth=2), # Fixed here!
    startangle=140
)

# 3. Add title and clear out the default vertical label
plt.title("TB Culture Results Breakdown", pad=20)
plt.ylabel("")

plt.show()
```
<details>

<summary>🥧 Pie Chart — What this does</summary>

## 📘 What this does

- `df["CultureResult"].value_counts()` → counts how many positive and negative TB results exist and sorts them automatically  
- `culture_counts.plot(kind="pie")` → creates a pie chart from the counted values  
- `autopct="%1.1f%%"` → shows percentage values on each slice  
- `colors=["tomato", "lightskyblue"]` → sets custom colors for the slices  
- `wedgeprops=dict(edgecolor="white", linewidth=2)` → adds white borders between slices for clarity  
- `startangle=140` → rotates the pie chart for better visual orientation  
- `plt.title()` → adds a title to the chart  
- `plt.ylabel("")` → removes the default y-axis label (which is unnecessary in pie charts)  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `value_counts()` automatically sorts values  
- Not setting `autopct`, making percentages invisible  
- Overcrowding pie charts with too many categories  
- Forgetting to remove the default y-label (`plt.ylabel("")`)  
- Using unclear or similar colors for slices  

---

## 💡 Tip

Pie charts work best when you have **only a few categories (2–5 max)** and want to show proportions clearly.

</details>

## 🍩 Option B: The Modern Donut Chart (Highly Recommended)
A donut chart is simply a pie chart with a hollow center. It looks highly modern and professional, and it is incredibly easy to create in a single line using wedgeprops.
```python
import matplotlib.pyplot as plt

# 1. Count the values
culture_counts = df["CultureResult"].value_counts()

# 2. Use 'wedgeprops' to hollow out the center and turn it into a donut
culture_counts.plot(
    kind="pie", 
    autopct="%1.1f%%", 
    colors=["mediumseagreen", "gold"],
    wedgeprops=dict(width=0.4, edgecolor="white", linewidth=2),
    startangle=90
)

plt.title("TB Culture Results (Donut View)")
plt.ylabel("")

plt.show()
```
<details>

<summary>🍩 Donut Chart — What this does</summary>

## 📘 What this does

- `df["CultureResult"].value_counts()` → counts how many TB positive and negative results exist in the dataset  
- `culture_counts.plot(kind="pie")` → creates a pie chart from the counts  
- `autopct="%1.1f%%"` → displays percentage values on each slice  
- `colors=["mediumseagreen", "gold"]` → assigns custom colors to each category  
- `wedgeprops=dict(width=0.4, edgecolor="white", linewidth=2)` → transforms the pie chart into a donut chart by creating a hole in the center  
- `startangle=90` → rotates the chart for better visual alignment  
- `plt.title()` → adds a title to the chart  
- `plt.ylabel("")` → removes the default y-axis label  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting that `width` in `wedgeprops` controls donut hole size  
- Using too many categories (donut charts become unclear)  
- Not setting `startangle`, making slices look misaligned  
- Forgetting to remove y-label (`plt.ylabel("")`)  
- Using poor color contrast, making slices hard to distinguish  

---

## 💡 Tip

Donut charts are a cleaner version of pie charts and are better for presentations when you want a modern visual style.

</details>


## 🗂️ Option C: Pie Chart with a Side Legend (Best for Long Text Labels)
When category names are long, printing them directly on top of the pie slices makes a huge mess. This approach hides the text labels on the slices and places them into a clean side legend box instead.

```python
import matplotlib.pyplot as plt

# 1. Count the values
culture_counts = df["CultureResult"].value_counts()

# 2. Draw the pie chart but turn labels OFF on the slices
culture_counts.plot(
    kind="pie", 
    labels=None, 
    autopct="%1.1f%%", 
    colors=["coral", "silver"],
    startangle=90
)

# 3. Add a separate, clean legend box on the side
plt.legend(
    labels=culture_counts.index, 
    loc="center left", 
    bbox_to_anchor=(1, 0.5)
)

plt.title("TB Culture Results Summary")
plt.ylabel("")

plt.show()
```
<details>

<summary>📊 Pie Chart with Legend — What this does</summary>

## 📘 What this does

- `df["CultureResult"].value_counts()` → counts how many TB positive and negative results exist in the dataset  
- `culture_counts.plot(kind="pie")` → creates a pie chart from the counts  
- `labels=None` → removes text labels directly from the pie slices for a cleaner look  
- `autopct="%1.1f%%"` → shows percentage values on each slice  
- `colors=["coral", "silver"]` → assigns custom colors to the slices  
- `startangle=90` → rotates the chart for better visual alignment  
- `plt.legend(...)` → adds a separate legend box showing category names  
  - `labels=culture_counts.index` → uses category names for the legend  
  - `loc="center left"` → positions legend on the left side  
  - `bbox_to_anchor=(1, 0.5)` → moves legend outside the chart area  
- `plt.title()` → adds a title to the chart  
- `plt.ylabel("")` → removes the default y-axis label  
- `plt.show()` → displays the final plot  

---

## ❌ Common mistakes beginners make

- Forgetting to set `labels=None` and getting cluttered slices  
- Misplacing legend outside the figure (bad `bbox_to_anchor` values)  
- Using mismatched number of colors vs categories  
- Forgetting `plt.ylabel("")` leading to unnecessary labels  
- Overcrowding pie charts with many categories  

---

## 💡 Tip

Using a legend instead of slice labels makes pie charts **much cleaner and publication-ready**, especially when category names are long.

</details>
---

# 7 REGION vs CULTURE RESULT (HEATMAP OPTIONS)
Option A: Percentages by Region (Row-Wise Proportions)If one region has 100 patients and another has only 5, comparing raw numbers can be misleading. This code converts the data into percentages along each row ($100\%$ per region) so you can compare ratios fairly.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 1. Clean invisible trailing spaces from text values first
df["region"] = df["region"].str.strip()
df["CultureResult"] = df["CultureResult"].str.strip()

# 2. Create the crosstab but add normalize="index" to calculate row percentages
cross_pct = pd.crosstab(df["region"], df["CultureResult"], normalize="index") * 100

# 3. Plot using a modern Yellow-Green-Blue (YlGnBu) palette and format numbers with 1 decimal (.1f)
sns.heatmap(cross_pct, annot=True, cmap="YlGnBu", fmt=".1f")

plt.title("Percentage of Culture Results within Each Region")
plt.xlabel("Culture Result")
plt.ylabel("Region")

plt.show()
```

<details>

<summary>🌡️ Heatmap with Percentages — What this does</summary>

## 📘 What this does

- `df["region"].str.strip()` → removes hidden spaces from region names  
- `df["CultureResult"].str.strip()` → removes hidden spaces from culture result values  
- `pd.crosstab(df["region"], df["CultureResult"], normalize="index")`  
  → creates a table comparing regions against culture results  
  → `normalize="index"` converts counts into row percentages  
- `* 100` → converts proportions into percentage values  
- `sns.heatmap(...)` → visualizes the table as a heatmap  
- `annot=True` → writes the numeric values inside each heatmap box  
- `cmap="YlGnBu"` → applies a Yellow-Green-Blue color palette  
- `fmt=".1f"` → formats displayed numbers with 1 decimal place  
- `plt.title()` → adds a chart title  
- `plt.xlabel()` → labels the X-axis (Culture Result)  
- `plt.ylabel()` → labels the Y-axis (Region)  
- `plt.show()` → displays the final heatmap  

---

## ❌ Common mistakes beginners make

- Forgetting to clean hidden spaces before analysis  
- Misunderstanding `normalize="index"` percentages  
- Forgetting `* 100`, resulting in decimal proportions instead of percentages  
- Using too many categories, making the heatmap crowded  
- Confusing rows and columns in `pd.crosstab()`  

---

## 💡 Tip

Heatmaps are excellent for spotting **patterns and trends across groups**, especially in epidemiology and genomics datasets.

</details>


🔠 Option B: Sorted Raw Counts (Highest to Lowest Concentration)
By default, the heatmap lists categories alphabetically. This option sorts the regions based on total volume, placing the busiest regions at the very top of your matrix grid.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 1. Generate the base cross table counts
cross = pd.crosstab(df["region"], df["CultureResult"])

# 2. Sort the table rows based on which region has the highest number of Positive results
cross_sorted = cross.sort_values(by="Positive", ascending=False)

# 3. Draw the heatmap with crisp whole numbers (fmt="d") and an orange/red color scheme
sns.heatmap(cross_sorted, annot=True, cmap="OrRd", fmt="d", linewidths=1, linecolor="white")

plt.title("Sorted Grid: Region vs TB Outcome Concentration")
plt.xlabel("Culture Result")
plt.ylabel("Region (Sorted Highest to Lowest)")

plt.show()
```

<details>

<summary>🌡️ Sorted Heatmap — What this does</summary>

## 📘 What this does

- `pd.crosstab(df["region"], df["CultureResult"])`  
  → creates a table counting TB culture results within each region  

- `cross.sort_values(by="Positive", ascending=False)`  
  → sorts regions from highest to lowest number of positive TB cases  

- `sns.heatmap(...)` → visualizes the table as a heatmap  
- `annot=True` → displays the actual count values inside each box  
- `cmap="OrRd"` → applies an Orange-Red color palette  
- `fmt="d"` → formats numbers as whole integers  
- `linewidths=1` → adds spacing lines between boxes  
- `linecolor="white"` → colors the separating lines white for a cleaner appearance  

- `plt.title()` → adds a title to the heatmap  
- `plt.xlabel()` → labels the X-axis (Culture Result categories)  
- `plt.ylabel()` → labels the Y-axis (sorted regions)  
- `plt.show()` → displays the final heatmap  

---

## ❌ Common mistakes beginners make

- Sorting using a column name that does not exist (`"Positive"` spelling mismatch)  
- Confusing raw counts with percentages  
- Forgetting `fmt="d"` and getting decimal numbers for integer counts  
- Using too many regions, making the heatmap difficult to read  
- Misunderstanding which axis represents rows vs columns  

---

## 💡 Tip

Sorting heatmaps helps important patterns stand out immediately, especially when comparing disease burden across regions.

</details>



Option C: The Total Grid Dataset Percentage (Grand Total Share)
Instead of looking at rows or columns individually, this configuration shows what percentage of the entire patient registry falls into each block.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# 1. Use normalize="all" to compute cell value over total dataset size
cross_total_pct = pd.crosstab(df["region"], df["CultureResult"], normalize="all") * 100

# 2. Draw the heatmap with a Cool-Warm theme (coolwarm)
sns.heatmap(cross_total_pct, annot=True, cmap="coolwarm", fmt=".1f")

plt.title("Share of Total Dataset Contribution (%)")
plt.xlabel("Culture Result")
plt.ylabel("Region")

plt.show()
```
<details>

<summary>🌡️ Percentage Heatmap — Total Dataset Contribution</summary>

## 📘 What this does

- `pd.crosstab(df["region"], df["CultureResult"], normalize="all")`  
  → creates a cross table and calculates each cell as a proportion of the entire dataset  

- `* 100` → converts proportions into percentage values  

- `sns.heatmap(...)` → visualizes the percentages as a heatmap  
- `annot=True` → displays percentage values inside each cell  
- `cmap="coolwarm"` → applies a blue-to-red color palette  
- `fmt=".1f"` → formats displayed numbers with 1 decimal place  

- `plt.title()` → adds a title to the heatmap  
- `plt.xlabel()` → labels the X-axis (Culture Result categories)  
- `plt.ylabel()` → labels the Y-axis (Region names)  
- `plt.show()` → displays the final heatmap  

---

## ❌ Common mistakes beginners make

- Confusing `normalize="all"` with `normalize="index"`  
- Forgetting `* 100`, resulting in decimal proportions instead of percentages  
- Misinterpreting colors without checking actual values  
- Overcrowding heatmaps with too many categories  
- Using inconsistent category labels (`Positive` vs `positive`)  

---

## 💡 Tip

`normalize="all"` is useful when you want to understand how much each group contributes to the entire dataset overall.

</details>

---

# 📊 8. SMOKING vs CULTURE RESULT

```python
sns.countplot(data=df, x="Smoking", hue="CultureResult")

plt.title("Smoking vs TB Outcome")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Compares smoking status with TB infection outcome  

---

## ❌ Common mistakes beginners make

- Yes/No not standardized  
- Missing values in categorical columns  

---

## 💡 Tip

Clean values:

```python
df["Smoking"] = df["Smoking"].str.lower()
```
</details>

---

# 9. AGE vs CULTURE RESULT (LINE GRAPH STYLE)

```python
df.groupby("Age")["CultureResult"].value_counts().unstack().plot()

plt.title("Age vs TB Outcome")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows how TB positivity varies by age  

---

## ❌ Common mistakes beginners make

- Age treated as categorical instead of numeric  
- No sorting of age values  

---

## 💡 Tip

Sort age if graph looks messy.

</details>

---

# 🌍 10. ORIGIN (ComeFrom) ANALYSIS

```python
df["ComeFrom"].value_counts().plot.bar()

plt.title("Patient Origin Distribution")

plt.show()
```

<details>
<summary>📘 Explanation + Common Mistakes</summary>

## What this does

- Shows where patients came from (migration/travel patterns)  

---

## ❌ Common mistakes beginners make

- Different spelling of same location (Saudi Arabia vs SaudiArabia)  

---

## 💡 Tip

Standardize location names first.

</details>
---


---
