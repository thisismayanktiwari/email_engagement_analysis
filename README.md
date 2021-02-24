# Email Engagement Analysis

All the details have been clearly laid out in the jupyter notebook. Check out the jupyter notebook [here](https://github.com/thisismayanktiwari/email_engagement_analysis/blob/main/Email%20Engagement%20Analysis.ipynb).

**Note:** The hyperlinks in Table of Contents is note supported on GitHub. To use the notebook with hyperlinks, download and run locally.

Below is a markdown version of the jupyter notebook to view with ease.

# Email Engagement Analysis

## Business case <a name='business_case'/>
Your university is trying to answer questions on email engagement. The school is
worried about the perception of email spam, and want to reduce the number of emails
sent to students.
To focus on email engagement means that you are looking at how students interact with
emails from a school. For example, do they:
- Open the email?
- Read the content?
- Click on links?
- Follow a call to action?


## Table of Contents

- [Business case](#business_case) 
- [Intalling the requirements](#requirements)
- [Exploring the data](#exploring)
- [Analysis](#analysis)
    - [Q1. What are the average number of emails that are being sent per student?](#q1)
    - [Q2. What is the average open rate per student?](#q2)
    - [Q3. How many emails remain unopened (count and %)? ](#q3)
    - [Q4. How many emails does it take (number of delivered) to get students to open an email?](#q4)
    - [Q5. Among students who unsubscribed (emailUnsubscribed), how many emails on average were they sent?](#q5)
    - [Q6. How does email engagement change over the course of enrollment milestones (Prospects→ Applicants→ Admitted) ?](#q6)
    - [Q7. Are there “windows” where applicants/deposited students complete more actions? ](#q7)
    - [Q8. Compute an “engagement score” that combines the actions that lead to higher conversion. ](#q8)

### Installing the requirements  <a name='requirements'/>


```python
#importing packages and requirements
import matplotlib.pyplot as plt
import pandas as pd
import os.path

home = os.path.expanduser('~')
```


```python
#importing datasets
path_users = f'{home}/Projects/Email Engagement/Data sets/users.csv'
path_actions = f'{home}/Projects/Email Engagement/Data sets/actions.csv'

users = pd.read_csv(path_users, low_memory=False)
actions = pd.read_csv(path_actions, low_memory=False)
```

### Exploring the data  <a name="exploring"/>


```python
users.head(5)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Deposit</th>
      <th>Admit</th>
      <th>Apps Started</th>
      <th>active term</th>
      <th>Major*</th>
      <th>Prospects</th>
      <th>Apps Submit</th>
      <th>Address Home Country</th>
      <th>Created At</th>
      <th>Date Of Birth</th>
      <th>...</th>
      <th>Milestone Prospect Date</th>
      <th>Milestone Prospect Source</th>
      <th>Milestone Unsubscribe Email Date</th>
      <th>Milestone Unsubscribe Sms Date</th>
      <th>Milestone Unsubscribe Sms Email</th>
      <th>Milestone Unsubscribe Sms Number</th>
      <th>Milestone Visit Date</th>
      <th>Milestone Waitlist Date</th>
      <th>Milestone Withdraw Date</th>
      <th>Milestone Withdraw Term</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>Fall 2019</td>
      <td>Technion-Cornell Dual Masters Degree in Connec...</td>
      <td>0</td>
      <td>0</td>
      <td>USA</td>
      <td>12/28/17 16:02</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1/7/19 21:07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>Fall 2019</td>
      <td>Master of Engineering in Electrical and Comput...</td>
      <td>0</td>
      <td>0</td>
      <td>USA</td>
      <td>12/28/17 16:03</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>Fall 2019</td>
      <td>Johnson Cornell Tech MBA</td>
      <td>1</td>
      <td>0</td>
      <td>TUR</td>
      <td>12/28/17 16:10</td>
      <td>NaN</td>
      <td>...</td>
      <td>2/8/18 0:25</td>
      <td>workflows</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>Fall 2019</td>
      <td>Master of Engineering in Computer Science</td>
      <td>1</td>
      <td>0</td>
      <td>USA</td>
      <td>12/28/17 16:13</td>
      <td>NaN</td>
      <td>...</td>
      <td>2/13/18 22:00</td>
      <td>NaN</td>
      <td>9/5/18 22:19</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>Fall 2019</td>
      <td>Master of Engineering in Operations Research a...</td>
      <td>1</td>
      <td>0</td>
      <td>IND</td>
      <td>12/28/17 16:17</td>
      <td>NaN</td>
      <td>...</td>
      <td>10/23/18 15:28</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 60 columns</p>
</div>




```python
actions.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Action</th>
      <th>Bounce Categorie</th>
      <th>Browser Family</th>
      <th>Campaign Date</th>
      <th>Campaign Guid</th>
      <th>Campaign Month</th>
      <th>Campaign Population</th>
      <th>Campaign Year</th>
      <th>Device Type</th>
      <th>Dvce Ismobile</th>
      <th>...</th>
      <th>Page Urlhost</th>
      <th>Page Urlpath</th>
      <th>Seed</th>
      <th>Sms Content</th>
      <th>Survey Guid</th>
      <th>Target Url</th>
      <th>Timestamp</th>
      <th>Tracker Ident</th>
      <th>Updated At</th>
      <th>Variant Id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>pageview</td>
      <td>NaN</td>
      <td>Chrome</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Other</td>
      <td>NaN</td>
      <td>Other</td>
      <td>False</td>
      <td>...</td>
      <td>tech.cornell.edu</td>
      <td>/</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6/26/19 17:57</td>
      <td>NaN</td>
      <td>6/26/19 18:20</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>pageview</td>
      <td>NaN</td>
      <td>Chrome</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Other</td>
      <td>NaN</td>
      <td>Other</td>
      <td>False</td>
      <td>...</td>
      <td>tech.cornell.edu</td>
      <td>/programs/phd/phd-studies/</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10/1/19 18:16</td>
      <td>NaN</td>
      <td>10/1/19 18:20</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>pageview</td>
      <td>NaN</td>
      <td>Safari</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Other</td>
      <td>NaN</td>
      <td>Other</td>
      <td>False</td>
      <td>...</td>
      <td>tech.cornell.edu</td>
      <td>/news/category/research/</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6/26/19 17:58</td>
      <td>NaN</td>
      <td>6/26/19 18:20</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>pageview</td>
      <td>NaN</td>
      <td>Chrome</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Other</td>
      <td>NaN</td>
      <td>Other</td>
      <td>False</td>
      <td>...</td>
      <td>tech.cornell.edu</td>
      <td>/admissions/admissions-events/</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10/1/19 18:11</td>
      <td>NaN</td>
      <td>10/1/19 18:20</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>pageview</td>
      <td>NaN</td>
      <td>Chrome</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Other</td>
      <td>NaN</td>
      <td>Other</td>
      <td>False</td>
      <td>...</td>
      <td>tech.cornell.edu</td>
      <td>/research/human-computer-interaction-hci/</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6/26/19 17:59</td>
      <td>NaN</td>
      <td>6/26/19 18:20</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 41 columns</p>
</div>




```python
user_col = users.columns
actions_col =actions.columns
user_col.intersection(actions_col)
```




    Index(['Element Id'], dtype='object')



From the interection of indices of 'users' and 'actions' datasets, we see that 'Element Id' is the common index. We will use this column to join the two datasets. Each observation in the column 'Element Id' refers to a unique user.

**Note:** One thing to keep in mind is that the 'action' dataset contains multiple occurences of the same 'Element Id' as it made observations of the different actions performed by a user.

We need to join the two datasets using 'Element Id' in a way that all the actions performed by a unique 'Element Id' is included in the new dataset. Since we require all the actions performed by the users, we need to perform a **right join** on 'Element Id'.


```python
df = pd.merge(users,actions,how='right',on='Element Id')
```

Removing the rows that contain NaN in the column 'Element Id'


```python
df = df.dropna(subset = ['Element Id'])
```

The new **df** is the dataset that stores the information of all the actions performed by every user. **For example**, we can see below that it stores all the actions performed by the user with 'Element Id' as '5bd00debc2a9d80d47025bb4'.



```python
df.loc[df['Element Id'] =='5bd00debc2a9d80d47025bb4',['Action']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Action</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>324270</th>
      <td>userCreated</td>
    </tr>
    <tr>
      <th>329388</th>
      <td>emailDelivered</td>
    </tr>
    <tr>
      <th>334797</th>
      <td>emailOpened</td>
    </tr>
    <tr>
      <th>339983</th>
      <td>emailDelivered</td>
    </tr>
    <tr>
      <th>340000</th>
      <td>emailOpened</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>1050904</th>
      <td>emailClicked</td>
    </tr>
    <tr>
      <th>1050920</th>
      <td>emailOpened</td>
    </tr>
    <tr>
      <th>1051027</th>
      <td>emailClicked</td>
    </tr>
    <tr>
      <th>1051083</th>
      <td>emailOpened</td>
    </tr>
    <tr>
      <th>1051101</th>
      <td>emailClicked</td>
    </tr>
  </tbody>
</table>
<p>514 rows × 1 columns</p>
</div>



Let's explore what are the different actions that can be performed by a user.


```python
df['Action'].unique().tolist()
```




    ['emailOpened',
     'emailClicked',
     'userLogin',
     'userCreated',
     'emailDelivered',
     'formSaved',
     'emailBounced',
     'emailUnsubscribed',
     'emailAuthComplaint',
     'emailComplaint',
     'applicationRegistrationSuccessful',
     'pageview',
     'linkClick',
     'applicationSubmitted',
     'recommendationReceived',
     'recommendationRequested',
     'applicationPaid',
     'smsDelivered']



We see that a user can perform 18 different actions. The actions are pretty self explanatory, so I will not go into the details here. Let's start answering some important questions for our analysis.

### Answering important question for the analysis <a name='analysis'/>

#### Q1. What are the average number of emails that are being sent per student? <a name='q1'/>

The average number of emails being sent per student can be calculated by the following formula - 

$$
\text{Average number of emails per student } = \frac{\text{Total number of emails delivered}}{\text{Number of unique students}}
$$


```python
total_emails_delivered = sum(df['Action'] == 'emailDelivered')
no_unique_students = df['Element Id'].nunique()

email_per_student = total_emails_delivered/no_unique_students
email_per_student
```




    8.457067963509063



Therefore, the average number of emails sent per student is around **8 emails**.

___

#### Q2. What is the average open rate per student? <a name='q2'/>

The average open rate per student can be calculated as -

$$
\text{Average open rate per student} = \frac{\text{Total number of emails opened}}{\text{Number of unique students}}
$$


```python
total_emails_opened = sum(df['Action'] == 'emailOpened')

open_rate_per_student = total_emails_opened/no_unique_students
open_rate_per_student
```




    4.939555417593598



So on average, students are opening **5 out of the 8 emails** sent to them.

___

#### Q3. How many emails remain unopened (count and %)? <a id='q3'/>

To get the number of unopened emails, we can substract the number of emails opened from the total number of emails delivered.

$$
\text{Number of unopened emails} = \text{Number of emails delivered} - \text{Number of emails opened}
$$

And to get the percentage of unopened emails -

$$
\text{% of unopened emails} = \frac{\text{Number of emails unopened}}{\text{total number of emails delivered}} \times 100
$$


```python
no_unopened_emails = total_emails_delivered - total_emails_opened
no_unopened_emails
```




    203970



**203,970 emails** remain unopened.


```python
percent_emails_unopened = (no_unopened_emails/total_emails_delivered)*100
percent_emails_unopened
```




    41.59257748776509



Therefore, we observe that **41.6%** of emails delivered remain **unopened**.

___

#### Q4.  How many emails does it take (number of delivered) to get students to open an email? <a name='q4'/>

To determine this number, we need to determine the number of emails sent per number of emails opened.

$$
\text{Number of emails sent per number of emails opened} = \frac{\text{Total number of emails delivered}}{\text{Total number of emails opened}}
$$


```python
email_sent_per_open = total_emails_delivered/total_emails_opened
email_sent_per_open
```




    1.7121111615403415



This shows that about **2 emails** were sent before the student opened the email.

___

#### Q5. Among students who unsubscribed (emailUnsubscribed), how many emails on average were they sent? <a name='q5'/>

This can be calculated by creating a subset of the original dataset containing all the 'Element Id' that unsubscribed from emails, and then calculating the average number of emails delivered to the 'Element Id' on this subset.


```python
unsub_list = df.loc[df['Action'] == 'emailUnsubscribed', 'Element Id'].tolist()
unsub_df = df[df['Element Id'].isin(unsub_list)]

unsub_emails_delivered = sum(unsub_df['Action']=='emailDelivered')
unsub_users = unsub_df['Element Id'].nunique()

avg_email_unsub = unsub_emails_delivered/unsub_users
avg_email_unsub
```




    6.266007144820006



So, the students who unsubsribed were sent an average of **6 emails**. 

___

#### Q6. How does email engagement change over the course of enrollment milestones (Prospects→ Applicants→ Admitted) ? <a name='q6'/>

Email engagement in this case includes the following actions performed by the user -

* 'emailOpened'
* 'emailClicked'

Lets get them in a list that we can use later.



```python
engagement = ['emailOpened','emailClicked']
```

The information about student categories is stored in the column 'Campaign Population'-


```python
df['Campaign Population'].unique().tolist()
```




    ['Other',
     'Prospect',
     'Graduate',
     'Deposit',
     'Admit',
     'Admit/Deposit',
     'Event',
     'Applicant',
     'Community Counselor']



Let us divide the students that belong to Prospect, Applicant, and Admit respectively. Then we can measure the engagement based on different milestones.


```python
prospect_df = df.loc[df['Campaign Population']=='Prospect'] 
applicant_df = df.loc[df['Campaign Population']=='Applicant']
admit_df = df.loc[df['Campaign Population']=='Admit']
```

Now we have three datasets containing information of students from the different milestones, namely Prospect, Applicant, Admit. We can now measure engagement based on each milestone.


Engagement can be calculated as a percentage of total email actions per email delivered.

$$
\text{Engagement} = \frac{\text{Total email engagement}}{\text{Total emails delivered}} \times 100
$$


```python
prospect_email_engage = sum(prospect_df['Action'].isin(engagement))
prospect_email_deliver = sum(prospect_df['Action'].isin(['emailDelivered']))
prospect_engage = (prospect_email_engage/prospect_email_deliver)*100
print('Prosepct Engagement' + ' = ' +str(prospect_engage)+' %' )

applicant_email_engage = sum(applicant_df['Action'].isin(engagement))
applicant_email_deliver = sum(applicant_df['Action'].isin(['emailDelivered']))
applicant_engage = (applicant_email_engage/applicant_email_deliver)*100
print('Applicant Engagement' + ' = ' +str(applicant_engage)+' %' )

admit_email_engage = sum(admit_df['Action'].isin(engagement))
admit_email_deliver = sum(admit_df['Action'].isin(['emailDelivered']))
admit_engage = (admit_email_engage/admit_email_deliver)*100
print('Admit Engagement' + ' = ' +str(admit_engage)+' %' )
```

    Prosepct Engagement = 63.46367473894689 %
    Applicant Engagement = 192.6513671875 %
    Admit Engagement = 460.8915906788247 %


Therefore we see that the engagement increases as students move ahead in milestones. 

**Note:** Percentages higher than hundred suggest that students checked their emails more than once. The results are - 

|Milestone|Engagement|
|-|-|
|Prospects|64.5%|
|Applicants|192.7%|
|Admit|460.9%|

___

#### Q7. Are there “windows” where applicants/deposited students complete more actions? <a name='q7'/>
*For example, an admitted student opens 10 emails in 12 weeks, but 50% of them during the 1st week*


Let's start by making a dataset with only applicants/deposited students.


```python
# we alrady made a dataset with applicant students, applicant_df
deposit_df = df.loc[df['Campaign Population']=='Admit/Deposit']
```

Now we have to convert the column 'Timestamp' to ```DatetimeIndex```


```python
applicant_df['Dates'] = pd.to_datetime(applicant_df['Timestamp'])
deposit_df['Dates'] = pd.to_datetime(deposit_df['Timestamp'])
```

    <ipython-input-21-3f882017d97c>:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      applicant_df['Dates'] = pd.to_datetime(applicant_df['Timestamp'])
    <ipython-input-21-3f882017d97c>:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      deposit_df['Dates'] = pd.to_datetime(deposit_df['Timestamp'])


Setting ```'Dates'``` as index so that we can resample them by weeks.


```python
applicant_df.set_index('Dates', inplace=True)
deposit_df.set_index('Dates', inplace=True)
```

Making a new column `'ActionCount'` to keep a count of actions


```python
applicant_df.loc[applicant_df['Action']!='emailDelivered', 'ActionCount'] = 1
applicant_df.loc[applicant_df['Action']=='emailDelivered', 'ActionCount'] = 0

deposit_df.loc[deposit_df['Action']!='emailDelivered', 'ActionCount'] = 1
deposit_df.loc[deposit_df['Action']=='emailDelivered', 'ActionCount'] = 0
```

    /usr/local/lib/python3.8/site-packages/pandas/core/indexing.py:1599: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self.obj[key] = infer_fill_value(value)
    /usr/local/lib/python3.8/site-packages/pandas/core/indexing.py:1720: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self._setitem_single_column(loc, value, pi)


Now we can resample the datasets weekly and calculate the sum of actions taken on a weekly basis.

For **deposited** students - 


```python
deposit_action = deposit_df['ActionCount'].resample('W').sum()
print(deposit_action)
```

    Dates
    2018-12-23    171.0
    2018-12-30    179.0
    2019-01-06    171.0
    2019-01-13    222.0
    2019-01-20    244.0
    2019-01-27    223.0
    2019-02-03    183.0
    2019-02-10    396.0
    2019-02-17    675.0
    2019-02-24    727.0
    2019-03-03    767.0
    2019-03-10    772.0
    2019-03-17    720.0
    2019-03-24    770.0
    2019-03-31    619.0
    2019-04-07    824.0
    2019-04-14    973.0
    2019-04-21    619.0
    2019-04-28    361.0
    2019-05-05    246.0
    2019-05-12     79.0
    2019-05-19     63.0
    2019-05-26     50.0
    2019-06-02     30.0
    2019-06-09     45.0
    2019-06-16     26.0
    2019-06-23     25.0
    2019-06-30     10.0
    2019-07-07      0.0
    2019-07-14      0.0
    2019-07-21      0.0
    2019-07-28      0.0
    2019-08-04      0.0
    2019-08-11      0.0
    2019-08-18      0.0
    2019-08-25      0.0
    2019-09-01      0.0
    2019-09-08      0.0
    2019-09-15      0.0
    2019-09-22      0.0
    2019-09-29      0.0
    2019-10-06      2.0
    Freq: W-SUN, Name: ActionCount, dtype: float64


For **Applicant** students- 


```python
applicant_action = applicant_df['ActionCount'].resample('W').sum()
print(applicant_action)
```

    Dates
    2018-11-25      41.0
    2018-12-02      31.0
    2018-12-09       2.0
    2018-12-16       0.0
    2018-12-23       0.0
    2018-12-30       0.0
    2019-01-06       0.0
    2019-01-13       0.0
    2019-01-20    1786.0
    2019-01-27     192.0
    2019-02-03     117.0
    2019-02-10    2250.0
    2019-02-17    2393.0
    2019-02-24     339.0
    2019-03-03     262.0
    2019-03-10     228.0
    2019-03-17      62.0
    2019-03-24      27.0
    2019-03-31      26.0
    2019-04-07      14.0
    2019-04-14      13.0
    2019-04-21      11.0
    2019-04-28       6.0
    2019-05-05       1.0
    2019-05-12       6.0
    2019-05-19       3.0
    2019-05-26       1.0
    2019-06-02       1.0
    2019-06-09       1.0
    2019-06-16       1.0
    2019-06-23       2.0
    2019-06-30       0.0
    2019-07-07       0.0
    2019-07-14       0.0
    2019-07-21       0.0
    2019-07-28       0.0
    2019-08-04       0.0
    2019-08-11       0.0
    2019-08-18       0.0
    2019-08-25       0.0
    2019-09-01       0.0
    2019-09-08       0.0
    2019-09-15       0.0
    2019-09-22       0.0
    2019-09-29       0.0
    2019-10-06       0.0
    2019-10-13     141.0
    2019-10-20       7.0
    Freq: W-SUN, Name: ActionCount, dtype: float64


We can plot line graphs to visualize the number of actions taken weekly by students.


```python
fig, axs = plt.subplots(1,2)

applicant_action.plot(title='Applicant Sudents',ax=axs[0],figsize=(15,5))
deposit_action.plot(title='Deposited Students',ax=axs[1],figsize=(15,5))
```




    <AxesSubplot:title={'center':'Deposited Students'}, xlabel='Dates'>




    
![png](output_59_1.png)
    


From the plots, we can see that most of the actions taken by **applicant students** was between first week of January to the second week of March, whereas most of the action taken by **Deposited students** spanned from first week of February until the second week of May.

___

#### Q8. Compute an “engagement score” that combines the actions that lead to higher conversion.  <a name='q8'/>
*The “engagement score” is a weighted average from 0-100% that accounts for different actions completed by an student.*

Let's examine the actions taken by deposited students. This will give us an idea of what actions are likely taken by students that end up converting.


```python
deposit_df['Action'].value_counts()
```




    emailOpened          8534
    emailDelivered       2343
    emailClicked         1633
    emailBounced           19
    emailUnsubscribed       6
    Name: Action, dtype: int64



This will help us in determing a formula to calculate a percentage based score, which will help us compute an engagement score for students.

From the study of above data, the following formula for engagmenet score is developed- 

$$
\text{Engagement Score} = \frac{\text{Emails opened} + \text{Emails clicked}}{\text{Email opened} + \text{Emails clicked} + \text{Email delivered}} \times 100
$$


```python
unique_user =  df['Element Id'].unique()
```

We make a datafram only containing `'Element Id'` and `'Action'` columns from the original dataframe and only take the actions `'emailOpened'` `'emailClicked'` `'emailDelivered'`


```python
small_df = df.loc[:,['Element Id','Action']]
filt = ['emailOpened','emailClicked','emailDelivered']
small_df = small_df.loc[df['Action'].isin(filt)]
```

Then we create a new dataframe `'engage_df'` that counts the number of `'emailOpened'` `'emailClicked'` `'emailDelivered'` for each `'Element Id'`


```python
engage_df = small_df.value_counts().unstack().fillna(0)
engage_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Action</th>
      <th>emailClicked</th>
      <th>emailDelivered</th>
      <th>emailOpened</th>
    </tr>
    <tr>
      <th>Element Id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5a45157b1ed0725bc304d120</th>
      <td>0.0</td>
      <td>9.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bc5352b3f</th>
      <td>2.0</td>
      <td>8.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bc86d87fd</th>
      <td>0.0</td>
      <td>7.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bcb0180d2</th>
      <td>0.0</td>
      <td>5.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bcc026d8f</th>
      <td>0.0</td>
      <td>7.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5da536d82ca3f61c984a5b37</th>
      <td>1.0</td>
      <td>3.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>5da53a462ca3f61c6a6a1002</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>5da5cdc72ca3f61e6c0be135</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>5da5d4562ca3f6302f527c57</th>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>5da628e42ca3f61e9c71ade4</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
  </tbody>
</table>
<p>51153 rows × 3 columns</p>
</div>



Then we can calculate the engagement score based on the formula above and add a column containing the results


```python
engage_df['engagement score'] = ((engage_df['emailOpened']+engage_df['emailClicked'])/(engage_df['emailOpened']+engage_df['emailClicked']+engage_df['emailDelivered']))*100
engage_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Action</th>
      <th>emailClicked</th>
      <th>emailDelivered</th>
      <th>emailOpened</th>
      <th>engagement score</th>
    </tr>
    <tr>
      <th>Element Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5a45157b1ed0725bc304d120</th>
      <td>0.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>35.714286</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bc5352b3f</th>
      <td>2.0</td>
      <td>8.0</td>
      <td>6.0</td>
      <td>50.000000</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bc86d87fd</th>
      <td>0.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>12.500000</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bcb0180d2</th>
      <td>0.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5a45157b1ed0725bcc026d8f</th>
      <td>0.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>22.222222</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5da536d82ca3f61c984a5b37</th>
      <td>1.0</td>
      <td>3.0</td>
      <td>5.0</td>
      <td>66.666667</td>
    </tr>
    <tr>
      <th>5da53a462ca3f61c6a6a1002</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5da5cdc72ca3f61e6c0be135</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5da5d4562ca3f6302f527c57</th>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>66.666667</td>
    </tr>
    <tr>
      <th>5da628e42ca3f61e9c71ade4</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
<p>51153 rows × 4 columns</p>
</div>

