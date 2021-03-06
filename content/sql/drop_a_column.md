Title: Drop A Column  
Slug: drop_a_column  
Summary: Delete a column in a table in SQL.   
Date: 2016-05-01 12:00  
Category: SQL  
Tags: Basics  
Authors: Chris Albon  

Note: This tutorial was written using [Catherine Devlin's SQL in Jupyter Notebooks library](https://github.com/catherinedevlin/ipython-sql). If you have not using a Jupyter Notebook, you can ignore the two lines of code below and any line containing `%%sql`. Furthermore, This tutorial uses SQLite's flavor of SQL, your version might have some differences in syntax.

For more, check out [Learning SQL](http://amzn.to/2jRriHj) by Alan Beaulieu.


```python
# Ignore
%load_ext sql
%sql sqlite://
%config SqlMagic.feedback = False
```

## Create Data


```python
%%sql

-- Create a table of criminals
CREATE TABLE criminals (pid, name, age, sex, city, minor);
INSERT INTO criminals VALUES (412, 'James Smith', 15, 'M', 'Santa Rosa', 1);
INSERT INTO criminals VALUES (234, 'Bill James', 22, 'M', 'Santa Rosa', 0);
INSERT INTO criminals VALUES (632, 'Stacy Miller', 23, 'F', 'Santa Rosa', 0);
INSERT INTO criminals VALUES (621, 'Betty Bob', NULL, 'F', 'Petaluma', 1);
INSERT INTO criminals VALUES (162, 'Jaden Ado', 49, 'M', NULL, 0);
INSERT INTO criminals VALUES (901, 'Gordon Ado', 32, 'F', 'Santa Rosa', 0);
INSERT INTO criminals VALUES (512, 'Bill Byson', 21, 'M', 'Santa Rosa', 0);
INSERT INTO criminals VALUES (411, 'Bob Iton', NULL, 'M', 'San Francisco', 0);
```




    []



## View Table


```python
%%sql

-- Select everything
SELECT *

-- From the table 'criminals'
FROM criminals
```




<table>
    <tr>
        <th>pid</th>
        <th>name</th>
        <th>age</th>
        <th>sex</th>
        <th>city</th>
        <th>minor</th>
    </tr>
    <tr>
        <td>412</td>
        <td>James Smith</td>
        <td>15</td>
        <td>M</td>
        <td>Santa Rosa</td>
        <td>1</td>
    </tr>
    <tr>
        <td>234</td>
        <td>Bill James</td>
        <td>22</td>
        <td>M</td>
        <td>Santa Rosa</td>
        <td>0</td>
    </tr>
    <tr>
        <td>632</td>
        <td>Stacy Miller</td>
        <td>23</td>
        <td>F</td>
        <td>Santa Rosa</td>
        <td>0</td>
    </tr>
    <tr>
        <td>621</td>
        <td>Betty Bob</td>
        <td>None</td>
        <td>F</td>
        <td>Petaluma</td>
        <td>1</td>
    </tr>
    <tr>
        <td>162</td>
        <td>Jaden Ado</td>
        <td>49</td>
        <td>M</td>
        <td>None</td>
        <td>0</td>
    </tr>
    <tr>
        <td>901</td>
        <td>Gordon Ado</td>
        <td>32</td>
        <td>F</td>
        <td>Santa Rosa</td>
        <td>0</td>
    </tr>
    <tr>
        <td>512</td>
        <td>Bill Byson</td>
        <td>21</td>
        <td>M</td>
        <td>Santa Rosa</td>
        <td>0</td>
    </tr>
    <tr>
        <td>411</td>
        <td>Bob Iton</td>
        <td>None</td>
        <td>M</td>
        <td>San Francisco</td>
        <td>0</td>
    </tr>
</table>



## Delete Column (Most Common)
%%sql

-- Alter the table called 'criminals'
ALTER TABLE criminals

-- From the table 'criminals'
DROP COLUMN age
## Delete Column (SQLite)

SQLite (the version of SQL used in this tutorial) does not allow you to drop a column. The workaround is to make a new table that contains only the columns you want to keep, then rename the new table to the original template's name.


```python
%%sql

-- Create a table called 'criminals_tamps' with the columns we want to not drop
CREATE TABLE criminals_temp(pid, name, sex);

-- Copy the data from the columns we want to keep to the new table
INSERT INTO criminals_temp SELECT pid, name, sex FROM criminals;

-- Delete the original table
DROP TABLE criminals;

-- Rename the new table to the original table's name
ALTER TABLE criminals_temp RENAME TO criminals;
```




    []



## View Table


```python
%%sql

-- Select everything
SELECT *

-- From the table 'criminals'
FROM criminals
```




<table>
    <tr>
        <th>pid</th>
        <th>name</th>
        <th>sex</th>
    </tr>
    <tr>
        <td>412</td>
        <td>James Smith</td>
        <td>M</td>
    </tr>
    <tr>
        <td>234</td>
        <td>Bill James</td>
        <td>M</td>
    </tr>
    <tr>
        <td>632</td>
        <td>Stacy Miller</td>
        <td>F</td>
    </tr>
    <tr>
        <td>621</td>
        <td>Betty Bob</td>
        <td>F</td>
    </tr>
    <tr>
        <td>162</td>
        <td>Jaden Ado</td>
        <td>M</td>
    </tr>
    <tr>
        <td>901</td>
        <td>Gordon Ado</td>
        <td>F</td>
    </tr>
    <tr>
        <td>512</td>
        <td>Bill Byson</td>
        <td>M</td>
    </tr>
    <tr>
        <td>411</td>
        <td>Bob Iton</td>
        <td>M</td>
    </tr>
</table>
