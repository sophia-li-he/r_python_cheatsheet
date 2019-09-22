# R - Python Cheatsheet

## Shortcuts
- Insert a new code block

```python
# r
Ctrl + Alt + I
# python
Esc + A/B (Above or below)
```
- Fold code

```python
# r
Alt + L
```
- Unfold code

```python
# r
Alt + Ctrl + L
```
- Comment out code

```python
# r
Ctrl + Shift + C
# python
Ctrl + /
```
- Shortcut for the pipe operator in r

```
CTRL + SHIFT + M (or CMD + SHIFT + M for OSX) 
```

## Data Overview
- View a few values in the dataset

```python
# r
glimpse(df)
# python
df.head()
```

## Data Manipulation

- Filter a column is not null

```python
# r
filter(!is.na(COLUMN_A))

# python
df.loc[~isnull(COLUMN_A),]

# sql
WHERE COLUMN_A IS NOT NULL
```

- Sort based on one or more variables

```python
# r
df %>% arrange(desc(colname))

# python
df.sort_values(by='col1', ascending=False, na_position='first')
```
- Filter

```python
# r
df %>% filter(col1%in% c(6,7), col2 < 5)

# python
filter = (df[col1].isin([6,7]) & (df[col2] < 5)
df.loc[filter,]
```

- Add a new column

```python
# r
df %>% mutate(new_col = col1 * col2 / 100)

# python
df['newcol'] = df['col1'] * df['col2'] / 100
```
- Aggregation - Count

```python
# r
df %>% count(col, sort = TRUE)
# Adding weight (multiply counts by weight)
df %>% count(col, wt=weight, sort = TRUE)

# python
df[col1].value_counts()
```
- Aggregation - summarize

```python
# r
df %>%
    group_by(col1, col2) %>%
    summarize(new_col = sum(col3)) %>%
    ungroup %>%
    mutate(col1 = ...) # you won't be able to transform col1 if you don't ungroup
```
`group_by` adds metadata to a data.frame that marks how rows should be grouped. As long as that metadata is there you won't be able to change the factors of the columns involved in the grouping. 

- Top_n (row_number)

```python
# r
df %>%
    group_by(col1) %>%
    top_n(3, col2)
# sql
ROW_NUMBER() OVER(PARTITION BY col1 ORDER BY col2) ROW_NUM 
WHERE ROW_NUM <= 3

# python
df['RN'] = df.sort_values(['col1', 'col2'], ascending=[True, False]).groupby(['key1]).cumcount() + 1
```

- Select multiple columns

```python
# r
df %>% select (col1, col2, col10:col17)
df %>% select (col1, col2, contains("abc")) #select_helpers
df %>% select (col1, col2, starts_with("abc"))
df %>% transmute(col1, col2, col5=col4/col3)

# python
new_col = df.columns # contains "abc"
df[new_col]
```

- Removing columns

```python
# r
df %>% select(-col)
#python
df.drop(['col1'], axis=1)
```

- Rename

```python
# r
df %>% select(col1, col3 = col2)
# python
df.columns
# change to the new column list and reassign to the df.columns
```

- Window functions

```python
# r
df %>%
    group_by(group1) %>%
    mutate(total = sum(col1) %>%
    ungroup() %>%
    mutate(fraction = col1 / total)
# sql
SUM(COL1) OVER(PARTITION BY GROUP1)
# Tableau
{FIXED [GROUP1]: SUM(COL1)}
```
