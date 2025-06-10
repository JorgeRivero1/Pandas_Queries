# 🐼 Pandas Exercises: Querying Data




## Apple Product Counts


We’re analyzing user data to understand how popular Apple devices are among users who have performed at least one event on the platform. Specifically, we want to measure this popularity across different languages. Count the number of distinct users using Apple devices —limited to "macbook pro", "iphone 5s", and "ipad air" — and compare it to the total number of users per language.

Present the results with the language, the number of Apple users, and the total number of users for each language. Finally, sort the results so that languages with the highest total user count appear first.

playbook_events

| Columna      | Tipo de dato       |
|--------------|--------------------|
| user_id      | int64              |
| occurred_at  | datetime64[ns]     |
| event_type   | object             |
| event_name   | object             |
| location     | object             |
| device       | object             |


playbook_users

| Columna      | Tipo de dato       |
|--------------|--------------------|
| user_id      | int64              |
| created_at   | datetime64[ns]     |
| company_id   | int64              |
| language     | object             |
| activated_at | datetime64[ns]     |
| state        | object             |


 ```python
import pandas as pd

# Start writing code
df = pd.merge(playbook_events, playbook_users, on = 'user_id', how = 'inner')
productos = df.groupby('language').user_id.nunique().reset_index(name= 'total')

dispositivos = ['macbook pro', 'iphone 5s', 'ipad air']
apple = df[df['device'].str.lower().isin(dispositivos)]
productos_apple = apple.groupby('language').user_id.nunique().reset_index(name='apple')

merged = pd.merge(productos_apple, productos, on= 'language', how= 'right')
merged['apple'] = merged['apple'].fillna(0)
merged.sort_values(by='total', ascending=False)

 ```



## Titanic Survivors and Non-Survivors


Make a report showing the number of survivors and non-survivors by passenger class. Classes are categorized based on the pclass value as:

•	First class: pclass = 1

•	Second class: pclass = 2

•	Third class: pclass = 3

Output the number of survivors and non-survivors by each class.
| Columna       | Tipo de dato |
|---------------|--------------|
| passengerid   | int64        |
| survived      | int64        |
| pclass        | int64        |
| name          | object       |
| sex           | object       |
| age           | float64      |
| sibsp         | int64        |
| parch         | int64        |
| ticket        | object       |
| fare          | float64      |
| cabin         | object       |
| embarked      | object       |

 
 ```python
import pandas as pd

df = titanic

dic = {
    1:'first_class',
    2:'second_class',
    3:'third_class'
}

df['pclass'] = df['pclass'].map(dic)

df.pivot_table(values='passengerid', index = 'survived', columns='pclass', aggfunc = 'count').reset_index()

 ```

## Processed Ticket Rate By Type


Find the processed rate of tickets for each type. The processed rate is defined as the number of processed tickets divided by the total number of tickets for that type. Round this result to two decimal places.

| Columna      | Tipo de dato |
|--------------|--------------|
| complaint_id | int64        |
| type         | int64        |
| processed    | bool         |


 ```python

import pandas as pd

df = facebook_complaints
df['processed'] = df['processed'].astype('int')

grouped = df.groupby('type').agg({
    'processed':'sum', 'complaint_id':'size'
}).reset_index()

result = (grouped['processed'] / grouped['complaint_id']).round(2).reset_index()

 ```

 ## Second Highest Salary

 Find the second highest salary of employees.

 | Columna         | Tipo de dato |
|-----------------|--------------|
| id              | int64        |
| first_name      | object       |
| last_name       | object       |
| age             | int64        |
| sex             | object       |
| employee_title  | object       |
| department      | object       |
| salary          | int64        |
| target          | int64        |
| bonus           | int64        |
| email           | object       |
| city            | object       |
| address         | object       |
| manager_id      | int64        |


 ```python

import pandas as pd
df = employee
df['rank'] = df['salary'].rank(method='max', ascending = False)
df[df['rank'] == 2]['salary']

 ```

## Largest Olympics

Find the Olympics with the highest number of unique athletes. The Olympics game is a combination of the year and the season, and is found in the games column. Output the Olympics along with the corresponding number of athletes. The id column uniquely identifies an athlete.

| Columna  | Tipo de dato |
|----------|--------------|
| id       | int64        |
| name     | object       |
| sex      | object       |
| age      | float64      |
| height   | float64      |
| weight   | float64      |
| team     | object       |
| noc      | object       |
| games    | object       |
| year     | int64        |
| season   | object       |
| city     | object       |
| sport    | object       |
| event    | object       |
| medal    | object       |

 ```python
import pandas as pd

# Start writing code
df = olympics_athletes_events

df = df.groupby('games').id.nunique().reset_index()
df['rango'] = df['id'].rank(method = 'min', ascending = False)
df[df['rango'] == 1][['games', 'id']]
```

## Reviews of Categories

Calculate number of reviews for every business category. Output the category along with the total number of reviews. Order by total reviews in descending order.

| Columna       | Tipo de dato |
|---------------|--------------|
| business_id   | object       |
| name          | object       |
| neighborhood  | object       |
| address       | object       |
| city          | object       |
| state         | object       |
| postal_code   | object       |
| latitude      | float64      |
| longitude     | float64      |
| stars         | float64      |
| review_count  | int64        |
| is_open       | int64        |
| categories    | object       |


 ```python

import pandas as pd

df = yelp_business
df['categories'] = df['categories'].astype(str)
df['categories'] = df['categories'].str.split(';')
df = df.explode('categories').reset_index(drop=True)

result = df.groupby('categories').review_count.sum().reset_index()
result.sort_values(by='review_count', ascending=False, inplace=True)
result
 ```

## Income By Title and Gender

Find the average total compensation based on employee titles and gender. Total compensation is calculated by adding both the salary and bonus of each employee. However, not every employee receives a bonus so disregard employees without bonuses in your calculation. Employee can receive more than one bonus.

Output the employee title, gender (i.e., sex), along with the average total compensation.

sf_employee

| Columna         | Tipo de dato |
|-----------------|--------------|
| id              | int64        |
| first_name      | object       |
| last_name       | object       |
| age             | int64        |
| sex             | object       |
| employee_title  | object       |
| department      | object       |
| salary          | int64        |
| target          | int64        |
| email           | object       |
| city            | object       |
| address         | object       |
| manager_id      | int64        |

sf_bonus

| Columna       | Tipo de dato |
|---------------|--------------|
| worker_ref_id | int64        |
| bonus         | int64        |


 ```python
import pandas as pd

df = sf_bonus.groupby('worker_ref_id').bonus.sum().reset_index()

merged = pd.merge(sf_employee, df, left_on = 'id', right_on = 'worker_ref_id')
merged['total'] = merged['salary'] + merged['bonus']

merged.groupby(['employee_title', 'sex']).total.mean().reset_index()

 ```


## Highest Cost Orders

Find the customer with the highest daily total order cost between 2019-02-01 to 2019-05-01. If customer had more than one order on a certain day, sum the order costs on daily basis. Output customer's first name, total cost of their items, and the date.

For simplicity, you can assume that every first name in the dataset is unique.

customers

| Columna      | Tipo de dato |
|--------------|--------------|
| id           | int64        |
| first_name   | object       |
| last_name    | object       |
| city         | object       |
| address      | object       |
| phone_number | object       |

orders

| Columna           | Tipo de dato       |
|-------------------|--------------------|
| id                | int64              |
| cust_id           | int64              |
| order_date        | datetime64[ns]     |
| order_details     | object             |
| total_order_cost  | int64              |


 ```python

import pandas as pd

df1 = customers.copy()
df2 = orders.copy()

df = df1.merge(df2, left_on = 'id', right_on = 'cust_id', how = 'inner')
df = df[df['order_date'].between('2019-02-01', '2019-05-01')]

df = df.groupby(['order_date', 'first_name']).total_order_cost.sum().reset_index()
df[df['total_order_cost'] == df['total_order_cost'].max()]

 ```


## Employee and Manager Salaries

Find employees who are earning more than their managers. Output the employee's first name along with the corresponding salary.

| Columna         | Tipo de dato |
|-----------------|--------------|
| id              | int64        |
| first_name      | object       |
| last_name       | object       |
| age             | int64        |
| sex             | object       |
| employee_title  | object       |
| department      | object       |
| salary          | int64        |
| target          | int64        |
| bonus           | int64        |
| email           | object       |
| city            | object       |
| address         | object       |
| manager_id      | int64        |





 ```python
import pandas as pd

emp = employee.copy()
mng = employee.copy()

mng = mng.rename(columns={
    'id':'mng_id',
    'salary':'manager_salary'
})

merged = emp.merge(mng[['mng_id', 'manager_salary']], left_on='manager_id', right_on = 'mng_id', how='inner')

merged[merged['salary'] > merged['manager_salary']][['first_name', 'salary']]


 ```

## User with Most Approved Flags

Which user flagged the most distinct videos that ended up approved by YouTube? Output, in one column, their full name or names in case of a tie. In the user's full name, include a space between the first and the last name.

user_flags

| Columna       | Tipo de dato |
|---------------|--------------|
| user_firstname| object       |
| user_lastname | object       |
| video_id      | object       |
| flag_id       | object       |


flag_review

| Columna           | Tipo de dato       |
|-------------------|--------------------|
| flag_id           | object             |
| reviewed_by_yt    | bool               |
| reviewed_date     | datetime64[ns]     |
| reviewed_outcome  | object             |




 ```python
import pandas as pd

df = pd.merge(user_flags, flag_review, on = 'flag_id', how = 'inner')

df = df[df['reviewed_outcome'].str.lower() == 'approved']

df['nombre_completo'] = df['user_firstname'].fillna('') + ' ' + df['user_lastname'].fillna('')
df

 ```

## Number of Streets Per Zip Code

Count the number of unique street names for each postal code in the business dataset. Use only the first word of the street name, case insensitive (e.g., "FOLSOM" and "Folsom" are the same). If the structure is reversed (e.g., "Pier 39" and "39 Pier"), count them as the same street. Output the results with postal codes, ordered by the number of streets (descending) and postal code (ascending).

| Columna               | Tipo de dato       |
|-----------------------|--------------------|
| business_id           | int64              |
| business_name         | object             |
| business_address      | object             |
| business_city         | object             |
| business_state        | object             |
| business_postal_code  | float64            |
| business_latitude     | float64            |
| business_longitude    | float64            |
| business_location     | object             |
| business_phone_number | float64            |
| inspection_id         | object             |
| inspection_date       | datetime64[ns]     |
| inspection_score      | float64            |
| inspection_type       | object             |
| violation_id          | object             |
| violation_description | object             |
| risk_category         | object             |



 ```python
import pandas as pd

df = sf_restaurant_health_violations

def normalizar_calle(direccion):
    partes = str(direccion).split()
    
    if partes[0].isdigit() and len(partes)>1:
        return partes[1].lower()
    return partes[0].lower()

df['calle_normalizada'] = df['business_address'].apply(normalizar_calle)
result = df.groupby('business_postal_code').calle_normalizada.nunique().reset_index()
result.sort_values(by=['calle_normalizada', 'business_postal_code'], ascending =[False, True])

 ```

## New Products
Calculate the net change in the number of products launched by companies in 2020 compared to 2019. Your output should include the company names and the net difference.

(Net difference = Number of products launched in 2020 - The number launched in 2019.)

| Columna       | Tipo de dato |
|---------------|--------------|
| year          | int64        |
| company_name  | object       |
| product_name  | object       |



 ```python

import pandas as pd

df = car_launches
df = car_launches.groupby(['company_name', 'year']).count().reset_index()
df = pd.pivot_table(df, values = 'product_name', index='company_name', columns='year', aggfunc='sum').reset_index()

df['variación_neta'] = df[2020] - df[2019]
df[['company_name', 'variación_neta']]


 ```

## Finding User Purchases

Identify returning active users by finding users who made a second purchase happen within 7 days of the first. Output a list of these user_ids.

| Columna     | Tipo de dato       |
|-------------|--------------------|
| id          | int64              |
| user_id     | int64              |
| item        | object             |
| created_at  | datetime64[ns]     |
| revenue     | int64              |



 ```python

import pandas as pd
import numpy as np

df = amazon_transactions

df.sort_values(['user_id', 'created_at'], inplace = True)
df['prev_date'] = df.groupby('user_id').created_at.shift(1)
df['diff'] = (df['created_at'] - df['prev_date']).dt.days
a = df[(df['diff']<7)][['user_id']].drop_duplicates()


 ```

## Activity Rank

Find the email activity rank for each user. Email activity rank is defined by the total number of emails sent. The user with the highest number of emails sent will have a rank of 1, and so on. Output the user, total emails, and their activity rank.

•	Order records first by the total emails in descending order.

•	Then, sort users with the same number of emails in alphabetical order by their username.

•	In your rankings, return a unique value (i.e., a unique rank) even if multiple users have the same number of emails.


| Columna    | Tipo de dato |
|------------|--------------|
| id         | int64        |
| from_user  | object       |
| to_user    | object       |
| day        | int64        |



 ```python

import pandas as pd

df = google_gmail_emails

grupo = df.groupby('from_user').to_user.size().reset_index(name='conteo')

grupo['rank'] = grupo['conteo'].rank(method = 'first', ascending = False)
grupo.sort_values(by='rank')


 ```

## Election Results

The election is conducted in a city and everyone can vote for one or more candidates, or choose not to vote at all. Each person has 1 vote so if they vote for multiple candidates, their vote gets equally split across these candidates. For example, if a person votes for 2 candidates, these candidates receive an equivalent of 0.5 vote each. Some voters have chosen not to vote, which explains the blank entries in the dataset.

Find out who got the most votes and won the election. Output the name of the candidate or multiple names in case of a tie.

To avoid issues with a floating-point error you can round the number of votes received by a candidate to 3 decimal places.

| Columna   | Tipo de dato |
|-----------|--------------|
| voter     | object       |
| candidate | object       |



 ```python

import pandas as pd
voting_results = voting_results.dropna()
df = voting_results

df['vote_value'] = df['voter'].apply(lambda x: 1/(df['voter'] == x).sum())

result = df.groupby('candidate').vote_value.sum().reset_index().sort_values(by='vote_value', ascending = False)
result[result['vote_value'] == result['vote_value'].max()]['candidate']

 ```



## Flags per Video

For each video, find how many unique users flagged it. A unique user can be identified using the combination of their first name and last name. Do not consider rows in which there is no flag ID.

| Columna        | Tipo de dato |
|----------------|--------------|
| user_firstname | object       |
| user_lastname  | object       |
| video_id       | object       |
| flag_id        | object       |


 ```python

import pandas as pd

df = user_flags
df = df.dropna(subset= 'flag_id')
df['nombre completo'] = df['user_firstname'].fillna('') + ' ' + df['user_lastname'].fillna('')


df.groupby('video_id')['nombre completo'].nunique().reset_index()

 ```
