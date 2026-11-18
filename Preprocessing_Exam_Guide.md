# Data Preprocessing Practical Examination Guide

## Complete Exam-Sheet Coverage

| Exam topic | Find the command in this guide |
|---|---|
| Head and tail | Section 2 |
| Dataset dimensions | Section 2 |
| Structure and data types | Section 2 |
| Descriptive statistics | Section 3 |
| Missing value identification and handling | Section 4 |
| Duplicate identification and removal | Section 5 |
| Data filtering using conditions | Section 9 |
| Sorting and top records | Section 9 |
| Unique and distinct values | Section 6 |
| Splitting and renaming columns | Sections 7, 10 |
| Text cleaning and standardization | Section 7 |
| Invalid numeric value checks | Sections 11 and 12 |
| Outlier identification | Section 11 |
| Date conversion and year/month/day extraction | Section 8 |
| Derived columns and calculations | Section 12 |
| Mathematical operations | Section 12 |
| GroupBy and aggregation | Section 13 |
| Dataset merging using common keys | Section 14 |
| Exporting processed data | Section 16 |

Use this file to find the exact pandas/NumPy command for a viva or practical question.

## 1. Imports

| Question | Command | Use |
|---|---|---|
| Import pandas | `import pandas as pd` | Create and process DataFrames. |
| Import NumPy | `import numpy as np` | Perform numeric, array and mathematical operations. |
| Import plotting tools | `import matplotlib.pyplot as plt` | Create charts. |
| Import statistical plotting | `import seaborn as sns` | Create cleaner statistical charts. |
| Display all columns | `pd.set_option("display.max_columns", None)` | Prevent wide DataFrames from hiding columns. |

## 2. Load and Inspect Data

| Question | Command | Use |
|---|---|---|
| Read CSV | `df = pd.read_csv("path/file.csv")` | Load a CSV file into a DataFrame. |
| Show first records | `df.head()` | Display first five rows. |
| Show first N records | `df.head(10)` | Display first ten rows. |
| Show last records | `df.tail()` | Display last five rows. |
| Show last N records | `df.tail(10)` | Display last ten rows. |
| Find rows and columns | `df.shape` | Return `(rows, columns)`. |
| Find row count | `len(df)` or `df.shape[0]` | Return number of rows. |
| Find column count | `df.shape[1]` | Return number of columns. |
| List column names | `df.columns` or `df.columns.tolist()` | Display column names. |
| Show data types and null counts | `df.info()` | Inspect structure and missing values. |
| Show data types only | `df.dtypes` | Identify numeric, text and date-like columns. |
| View one column | `df["Genre"]` | Select a column as a Series. |
| View several columns | `df[["Movie_ID", "Genre"]]` | Select multiple columns. |
| Select rows and columns | `df.loc[0:4, ["Genre", "Rating"]]` | Select by labels or column names. |
| Select by position | `df.iloc[0:5, 0:4]` | Select rows/columns by integer position. |
| Copy a DataFrame safely | `df.copy()` | Prevent changes in the copy from affecting the original. |

## 3. Statistics

| Question | Command | Use |
|---|---|---|
| Summary of numeric columns | `df.describe()` | Get count, mean, standard deviation, quartiles and range. |
| Summary of all columns | `df.describe(include="all")` | Include text and numeric columns. |
| Transpose summary | `df.describe().T` | Put columns as rows for easier reading. |
| Mean | `df["Rating"].mean()` | Find average. |
| Median | `df["Rating"].median()` | Find middle value. |
| Mode | `df["Genre"].mode()[0]` | Find most frequent value. |
| Minimum | `df["Rating"].min()` | Find smallest value. |
| Maximum | `df["Rating"].max()` | Find largest value. |
| Sum | `df["Final_Amount"].sum()` | Calculate total. |
| Count non-null values | `df["Rating"].count()` | Count populated values. |
| Standard deviation | `df["Rating"].std()` | Measure spread. |
| Variance | `df["Rating"].var()` | Measure squared spread. |
| Quantile | `df["Rating"].quantile(0.25)` | Find 25th percentile. |

## 4. Missing Values

| Question | Command | Use |
|---|---|---|
| Count missing per column | `df.isna().sum()` | Find number of missing values. |
| Count missing in table | `df.isna().sum().sum()` | Find total missing values. |
| Find missing percentage | `df.isna().mean() * 100` | Find missing percentage per column. |
| Find rows containing null | `df[df.isna().any(axis=1)]` | Display incomplete records. |
| Find null in one column | `df[df["Genre"].isna()]` | Filter missing values in a column. |
| Remove rows with missing | `df.dropna()` | Remove every row containing at least one null. |
| Remove if selected column is missing | `df.dropna(subset=["Genre"])` | Remove nulls only from selected columns. |
| Fill all numeric nulls with mean | `df[col] = df[col].fillna(df[col].mean())` | Mean replacement. |
| Fill numeric nulls with median | `df[col] = df[col].fillna(df[col].median())` | Median replacement, useful for skewed data. |
| Fill text nulls with mode | `df[col] = df[col].fillna(df[col].mode()[0])` | Replace text nulls with most frequent value. |
| Fill with fixed value | `df["Genre"].fillna("Unknown")` | Replace nulls with a known label. |
| Forward fill | `df.ffill()` | Copy previous value forward. |
| Backward fill | `df.bfill()` | Copy next value backward. |

## 5. Duplicates

| Question | Command | Use |
|---|---|---|
| Count exact duplicate rows | `df.duplicated().sum()` | Check repeated complete records. |
| Display duplicate rows | `df[df.duplicated()]` | Show rows repeated after their first occurrence. |
| Display all duplicate copies | `df[df.duplicated(keep=False)]` | Show the original and every duplicate copy. |
| Remove exact duplicates | `df.drop_duplicates()` | Keep only one copy of each exact row. |
| Count duplicate IDs | `df.duplicated(subset=["Booking_ID"]).sum()` | Check duplicate primary IDs. |
| Remove duplicate IDs | `df.drop_duplicates(subset=["Booking_ID"], keep="first")` | Keep first record for each ID. |

## 6. Data Types and Values

| Question | Command | Use |
|---|---|---|
| Convert column type | `df["Tickets"] = pd.to_numeric(df["Tickets"], errors="coerce")` | Convert text to numeric; invalid values become null. |
| Convert to integer | `df["Tickets"].astype("int")` | Convert to integer after null removal. |
| Convert to string | `df["Genre"].astype("string")` | Convert text column safely. |
| Convert to category | `df["Genre"].astype("category")` | Store text as category. |
| Find invalid text values | `df["Booking_Status"].unique()` | Inspect distinct values. |
| Count category values | `df["Booking_Status"].value_counts()` | Count each category. |
| Include missing in counts | `df["Genre"].value_counts(dropna=False)` | Include null category in frequency count. |
| Find number of unique values | `df["Genre"].nunique()` | Count distinct non-null values. |

## 7. Text Cleaning

| Question | Command | Use |
|---|---|---|
| Remove leading/trailing spaces | `df["Genre"].str.strip()` | Clean category text. |
| Convert uppercase | `df["Genre"].str.upper()` | Standardize uppercase. |
| Convert lowercase | `df["Genre"].str.lower()` | Standardize lowercase. |
| Convert title case | `df["Genre"].str.title()` | Example: `"romance"` becomes `"Romance"`. |
| Replace text | `df["Booking_Status"].str.replace("Canceled", "Cancelled")` | Replace one category by another. |
| Replace with regular expression | `df["Movie_Name"].str.replace(r"[^A-Za-z0-9 ]", "", regex=True)` | Remove special characters. |
| Check text containing word | `df["Genre"].str.contains("Drama", case=False, na=False)` | Create a text match mask. |
| Check prefix | `df["Movie_ID"].str.startswith("M")` | Filter values beginning with text. |
| Check suffix | `df["Booking_ID"].str.endswith("9")` | Filter values ending with text. |
| Split text | `df["Customer_Name"].str.split(" ", expand=True)` | Split text into columns. |
| Extract text pattern | `df["Movie_ID"].str.extract(r"(M\d+)")` | Extract matching characters. |
| Standardize Booking Status | `df["Booking_Status"].str.strip().str.lower().replace({"cancelled":"Cancelled","confirmed":"Confirmed"})` | Fix capitalization and spelling. |

## 8. Date and Time

| Question | Command | Use |
|---|---|---|
| Convert date column | `df["Booking_Date"] = pd.to_datetime(df["Booking_Date"], errors="coerce")` | Convert text to datetime; bad values become null. |
| Parse mixed date formats | `pd.to_datetime(df["Booking_Date"], format="mixed", errors="coerce")` | Parse more than one date format. |
| Extract year | `df["Booking_Date"].dt.year` | Create year column. |
| Extract month number | `df["Booking_Date"].dt.month` | Create month number. |
| Extract month name | `df["Booking_Date"].dt.month_name()` | Create named month. |
| Extract day number | `df["Booking_Date"].dt.day` | Create day of month. |
| Extract weekday | `df["Booking_Date"].dt.day_name()` | Create weekday name. |
| Extract quarter | `df["Booking_Date"].dt.quarter` | Create quarter. |
| Find invalid dates | `df[df["Booking_Date"].isna()]` | Identify dates that could not be parsed. |
| Sort newest/oldest | `df.sort_values("Booking_Date")` | Sort chronologically. |
| Filter date range | `df[(df["Booking_Date"] >= "2025-01-01") & (df["Booking_Date"] <= "2025-12-31")]` | Select a date range. |

## 9. Filtering and Sorting

| Question | Command | Use |
|---|---|---|
| Filter one condition | `df[df["Rating"] > 8]` | Select rows matching a condition. |
| Filter equality | `df[df["Genre"] == "Drama"]` | Select one category. |
| Filter with AND | `df[(df["Genre"] == "Drama") & (df["Rating"] > 8)]` | Both conditions must match. |
| Filter with OR | `df[(df["Genre"] == "Drama") | (df["Genre"] == "Comedy")]` | At least one condition must match. |
| Filter a range | `df[df["Rating"].between(7, 9)]` | Keep values in a range. |
| Filter a list | `df[df["Genre"].isin(["Drama", "Comedy"])]` | Keep selected categories. |
| Exclude a category | `df[~df["Booking_Status"].isin(["Cancelled"])]` | Remove selected categories. |
| Filter using NOT NULL | `df[df["Rating"].notna()]` | Keep populated records. |
| Sort ascending | `df.sort_values("Rating")` | Smallest/earliest first. |
| Sort descending | `df.sort_values("Rating", ascending=False)` | Largest/last first. |
| Sort multiple columns | `df.sort_values(["Genre", "Rating"], ascending=[True, False])` | Primary and secondary sort. |
| Top 5 | `df.nlargest(5, "Rating")` | Highest five values. |
| Bottom 5 | `df.nsmallest(5, "Rating")` | Lowest five values. |
| Sample records | `df.sample(10, random_state=42)` | Random 10-row sample, reproducible. |

## 10. Rename, Select and Reorder

| Question | Command | Use |
|---|---|---|
| Rename one column | `df.rename(columns={"Rating":"Movie_Rating"}, inplace=True)` | Change a column name. |
| Rename several columns | `df.rename(columns={"Tickets":"Ticket_Count","Final_Amount":"Final_Total"})` | Change several names. |
| Select columns | `df[["Booking_ID", "Customer_Age", "Final_Amount"]]` | Create a focused table. |
| Drop column | `df.drop(columns=["UnneededColumn"])` | Remove a column. |
| Drop row | `df.drop(index=[0, 1])` | Remove rows by index. |
| Reorder columns | `df = df[["Booking_ID", "Show_ID", "Final_Amount"]]` | Place selected columns in desired order. |
| Add constant column | `df["Dataset"] = "Bookings"` | Add a fixed value. |

## 11. Outliers

| Question | Command | Use |
|---|---|---|
| Find IQR | `q1 = df["Rating"].quantile(0.25); q3 = df["Rating"].quantile(0.75); iqr = q3 - q1` | Calculate quartile spread. |
| Calculate lower bound | `lower = q1 - 1.5 * iqr` | Values below this are outliers. |
| Calculate upper bound | `upper = q3 + 1.5 * iqr` | Values above this are outliers. |
| Identify outliers | `df[(df["Rating"] < lower) \| (df["Rating"] > upper)]` | Display extreme records. |
| Remove outliers | `df = df[df["Rating"].between(lower, upper)]` | Remove records outside IQR bounds. |
| Keep non-outliers | `df[~df["Rating"].between(lower, upper)]` | Select only outliers. |
| Detect invalid values | `df[(df["Rating"] < 0) \| (df["Rating"] > 10)]` | Check domain-specific invalid range. |

## 12. Derived and Arithmetic Columns

| Question | Command | Use |
|---|---|---|
| Add column | `df["Calculated_Final"] = df["Ticket_Amount"] - df["Discount"] + df["Convenience_Fee"]` | Calculate final amount. |
| Subtract | `df["Net"] = df["Ticket_Amount"] - df["Discount"]` | Find difference. |
| Multiply | `df["Gross"] = df["Tickets"] * df["Ticket_Price"]` | Calculate gross value. |
| Divide safely | `df["Average"] = df["Final_Amount"] / df["Tickets"].replace(0, np.nan)` | Avoid division by zero. |
| Percentage | `df["Discount_Percent"] = df["Discount"] / df["Ticket_Amount"] * 100` | Convert ratio to percentage. |
| Round | `df["Final_Amount"].round(2)` | Round to two decimal places. |
| Absolute value | `df["Final_Amount"].abs()` | Remove negative sign. |
| Bin continuous values | `pd.cut(df["Customer_Age"], bins=[0,18,30,45,60,100], labels=["18-","18-30","31-45","46-60","60+"])` | Create age groups. |
| Conditional column | `np.where(df["Final_Amount"] > 1000, "High", "Low")` | Assign category from condition. |
| Check values match | `np.isclose(df["Calculated_Final"], df["Final_Amount"])` | Compare calculated and stored values. |

## 13. GroupBy and Aggregation

| Question | Command | Use |
|---|---|---|
| Sum by category | `df.groupby("Genre")["Final_Amount"].sum()` | Total amount per genre. |
| Mean by category | `df.groupby("Genre")["Rating"].mean()` | Average rating per genre. |
| Count by category | `df.groupby("Genre")["Movie_ID"].count()` | Movie count per genre. |
| Multiple aggregates | `df.groupby("Genre").agg({"Rating":["mean","max","min"], "Movie_ID":"count"})` | Several summaries together. |
| Named aggregates | `df.groupby("Genre").agg(Average_Rating=("Rating","mean"), Count=("Movie_ID","count"))` | Rename aggregate output columns. |
| Sort grouped result | `df.groupby("Genre")["Rating"].mean().sort_values(ascending=False)` | Highest/lowest group first. |
| Multiple group keys | `df.groupby(["City","Show_Type"])["Final_Amount"].sum()` | Aggregate across two categories. |
| Pivot table | `pd.pivot_table(df, index="Genre", columns="Booking_Status", values="Final_Amount", aggfunc="sum", fill_value=0)` | Create summary cross-table. |

## 14. Merge and Concatenate

| Question | Command | Use |
|---|---|---|
| Inner merge | `pd.merge(left, right, on="Movie_ID", how="inner")` | Keep only matching IDs. |
| Left merge | `pd.merge(left, right, on="Movie_ID", how="left")` | Keep every left-table row. |
| Right merge | `pd.merge(left, right, on="Movie_ID", how="right")` | Keep every right-table row. |
| Outer merge | `pd.merge(left, right, on="Movie_ID", how="outer")` | Keep all IDs from both tables. |
| Merge with different key names | `pd.merge(left, right, left_on="Movie_ID", right_on="Film_ID", how="left")` | Join when key names differ. |
| Merge three tables | `bookings.merge(shows, on="Show_ID").merge(movies, on="Movie_ID")` | Create one complete movie-booking table. |
| Combine rows vertically | `pd.concat([df1, df2], axis=0, ignore_index=True)` | Stack compatible tables. |
| Combine columns horizontally | `pd.concat([df1, df2], axis=1)` | Add columns side-by-side. |

## 15. Encoding

| Question | Command | Use |
|---|---|---|
| Label encode manually | `df["Status_Code"] = df["Booking_Status"].map({"Cancelled":0,"Confirmed":1})` | Convert two categories to numbers. |
| One-hot encode | `pd.get_dummies(df, columns=["Genre"], drop_first=True)` | Create 0/1 columns for categories. |
| Replace text categories | `df["Payment_Mode"].replace({"UPI":"UPI","Card":"Card"})` | Standardize category values. |

## 16. Export

| Question | Command | Use |
|---|---|---|
| Save CSV | `df.to_csv("Output/processed.csv", index=False)` | Export cleaned data without pandas index. |
| Save Excel | `df.to_excel("Output/processed.xlsx", index=False)` | Export Excel file. |
| Save JSON | `df.to_json("Output/processed.json", orient="records")` | Export JSON records. |

## 17. Charts

| Question | Command | Use |
|---|---|---|
| Bar count chart | `sns.countplot(data=df, x="Genre")` | Count rows in each category. |
| Histogram | `sns.histplot(df["Rating"], bins=20, kde=True)` | View numeric distribution. |
| Box plot | `sns.boxplot(data=df, x="Booking_Status", y="Final_Amount")` | Compare distributions and identify outliers. |
| Scatter plot | `plt.scatter(df["Rating"], df["Final_Amount"])` | Show relationship between two numeric columns. |
| Line chart | `df.groupby("Month")["Final_Amount"].sum().plot()` | Show values over time or sequence. |
| Pie chart | `df["Booking_Status"].value_counts().plot.pie(autopct="%1.1f%%")` | Show category percentages. |
| Correlation heatmap | `sns.heatmap(df.select_dtypes(include=np.number).corr(), annot=True, cmap="coolwarm")` | Visualize numeric relationships. |

## 18. Probable Viva Answers

- **Preprocessing:** Cleaning and changing raw data so it is suitable for analysis or machine learning.
- **Missing value:** No value is recorded; repair using drop or fill only after considering the column.
- **Outlier:** An extreme value; detect with IQR or domain rules.
- **Duplicate:** Repeated record; remove exact duplicates or duplicate IDs.
- **Feature:** Input column used to make a prediction.
- **Encoding:** Converting text categories into numeric format.
- **Mean vs median:** Mean is sensitive to extreme values; median is more robust.
- **One-hot encoding:** Creates separate 0/1 columns for categories.
- **Inner vs left join:** Inner keeps matches only; left keeps every row from the left table.
