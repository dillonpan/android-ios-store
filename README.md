# android-ios-store
Project using Reader within the CSV package for Python

# Project Details:
Let's pretend we work for a company that specializes in creating profitable Android & iOS apps. Assume that the company business model focuses on free to download & install apps but generates income via in-app purchases. This means that one of our primary focuses is on developing apps which attract as many users as possible. Let's gather data on both the App Store on iOS and the Google Play Store on Android.

Since there are millions of apps within each store, we will be using a small sample size of data for this project. Details are as follows and both data sets have been uploaded/links provided below:  
1. 'googleplaystore.csv' containing data about approximately ten thousand Android apps from Google Play  
[Google Play Store Data](https://www.kaggle.com/lava18/google-play-store-apps)  
2. ''AppleStore.csv''containing data about approximately seven thousand iOS apps from the App Store  
[iOS App Store Data](https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps)  
Note: All of the code below was run using Jupyter Notebook


# Opening and Exploring the Data:
Import the csv package
```python
import csv
```

# The Google Play data set
First we open the file and assign it to a variable(opened_file). Afterwards, we can use the Reader function within the CSV package to appoint the file as a CSV. Lastly, we assign the CSV as a list of rows and seperate the first row (the column headers) from the rest of the data.
```python
opened_file = open('googleplaystore.csv')
read_file = csv.reader(opened_file)
android = list(read_file)
android_header = android[0]
android = android[1:]
```

# The App Store data set
We do the same as above for the iOS data
```python
opened_file = open('AppleStore.csv')
read_file = csv.reader(opened_file)
ios = list(read_file)
ios_header = ios[0]
ios = ios[1:]
```

# Exploring the data
I've created a function titled "explore_data" that we can use to view specific data whenever we want. This function takes in a dataset and prints out the specific rows per user request. It also prints out the total number of rows and columns in the dataset at the end.

```python
def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n')
        
    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))
```
We can test out the "explore_data" function by passing the android data set and printing the first 3 rows. Note: The index for Python lists start at 0 and list slicing does not include the final number listed. In this case, we passed 0 & 3 as starting and ending variables. However, the printouts will be for rows 0, 1, & 2. Let's also print the column headers to better view the data:
:
```python
print(android_header)
explore_data(android, 0, 3, True)
```
['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']


['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']


['Coloring book moana', 'ART_AND_DESIGN', '3.9', '967', '14M', '500,000+', 'Free', '0', 'Everyone', 'Art & Design;Pretend Play', 'January 15, 2018', '2.0.0', '4.0.3 and up']


['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']


Number of rows: 10841
Number of columns: 13


Let's look at the App Store Data Set as well:
```python
print(ios_header)
explore_data(ios, 0, 3, True)
```
['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']


['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']


['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']


['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']


Number of rows: 7197
Number of columns: 16

# Deleting Wrong Data
In one of the [discussion boards](https://www.kaggle.com/lava18/google-play-store-apps/discussion/66015) for the Google play data, someone noticed an error for row 10472. Let's take a look at what could be wrong:
```python
print(android_header)  # header
print('\n')
print(android[10472])  # row with mistake
print('\n')
print(android[0])      # correct row for reference
```
['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']

['Life Made WI-Fi Touchscreen Photo Frame', '1.9', '19', '3.0M', '1,000+', 'Free', '0', 'Everyone', '', 'February 11, 2018', '1.0.19', '4.0 and up']

['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']

The row 10472 corresponds to the app Life Made WI-Fi Touchscreen Photo Frame, and we can see that the rating is 19. The maximum rating for an app under the Google Play Store is 5, thus a mistake was made. We will just delete the entire row as a response in this case.

```python
print(len(android))
del android[10472]  # don't run this more than once
print(len(android))
```
10841  
10840

# Removing Duplicate Entries
# Part One: Finding the Duplicates
Let's pretend after looking over the Google Play data, we noticed that Instagram seems to have multiple entries. Let's take a closer look:

```android
print(android_header)
for app in android:
    name = app[0]
    if name == 'Instagram':
        print(app)
```
['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']

['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']

['Instagram', 'SOCIAL', '4.5', '66577446', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']

['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']

['Instagram', 'SOCIAL', '4.5', '66509917', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']

It looks like the only difference between these entries is the # of reviews(Column 4). However, it's highly unlikely that Instagram is the only duplicated app. We can take a look and see how many apps are duplicated, at least by title:

```python
duplicate_apps = []
unique_apps = []

for app in android:
    name = app[0] # first column in each app is the name of the app
    if name in unique_apps:
        duplicate_apps.append(name)
    else:
        unique_apps.append(name)
    
print('Number of duplicate apps:', len(duplicate_apps))
print('\n')
print('Examples of duplicate apps:', duplicate_apps[:15]) # only print the first 15 as examples
```
Number of duplicate apps: 1181

Examples of duplicate apps: ['Quick PDF Scanner + OCR FREE', 'Box', 'Google My Business', 'ZOOM Cloud Meetings', 'join.me - Simple Meetings', 'Box', 'Zenefits', 'Google Ads', 'Google My Business', 'Slack', 'FreshBooks Classic', 'Insightly CRM', 'QuickBooks Accounting: Invoicing & Expenses', 'HipChat - Chat Built for Teams', 'Xero Accounting Software']

Note: 'Google My Business' shows up twice in the examples list, meaning there is more than just one duplicate app with that name  

# Part Two: Fixing the Data Set
To fix the issue of duplicate apps, let's only keep the most reviewed version and hold it in a dictionary (Ex. name: highest # of reviews). Afterwards, we can use the dictionary to rebuild the data set without duplicates:

```python
reviews_max = {}

for app in android:
    name = app[0] # take the name
    n_reviews = float(app[3]) # take the # of reviews
    
    if name in reviews_max and reviews_max[name] < n_reviews: # if the name is in the dictionary and there are more reviews
        reviews_max[name] = n_reviews # then update the # of reviews in the dictionary to the higher number
    elif name not in reviews_max:
        reviews_max[name] = n_reviews
```

Let's do a quick double check. If our dictionary was created correctly, it should be the same size as the original data set minus the duplicates:
```python
print('Expected length:', len(android) - 1181)
print('Actual length:', len(reviews_max))
```
Expected length: 9659
Actual length: 9659

Perfect! Now let's recreate the dataset but without duplicates:

```python
android_clean = []
already_added = [] # seperate list as a double check on exact duplicate mistakes

for app in android:
    name = app[0]
    n_reviews = float(app[3])
    
# If the amt of reviews matches the amount listed in the dictionary, we can add it to our clean dataset. We also added a check in case
# theres two rows with the same name and the same amt of reviews.
    if (reviews_max[name] == n_reviews) and (name not in already_added):
        android_clean.append(app)
        already_added.append(name)
```

Now we can double check the new dataset matches the expected length using our explore_data function
```python
explore_data(android_clean, 0, 3, True)
```

['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']

['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']

['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']

Number of rows: 9659  
Number of columns: 13  
We have 9659 rows, just as expected.
