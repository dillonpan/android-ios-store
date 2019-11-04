# android-ios-store
Project using Reader within the CSV package for Python

# Project Details:
Let's pretend we work for a company that specializes in creating profitable Android & iOS apps. Assume that the company business model focuses on free to download & install apps but generates income via in-app purchases. This means that one of our primary focuses is on developing apps which attract as many users as possible. Let's gather data on both the App Store on iOS and the Google Play Store on Android.

Since there are millions of apps within each store, we will be using a small sample size of data for this project. Details are as follows and both data sets have been uploaded as well:
1. 'googleplaystore.csv' containing data about approximately ten thousand Android apps from Google Play
2. ''AppleStore.csv''containing data about approximately seven thousand iOS apps from the App Store
Note: All of the code below was run using Jupyter Notebook


# Opening and Exploring the Data:


# The Google Play data set
```
opened_file = open('googleplaystore.csv')
read_file = csv.reader(opened_file)
android = list(read_file)
android_header = android[0]
android = android[1:]
```

# The App Store data set
```
opened_file = open('AppleStore.csv')
read_file = csv.reader(opened_file)
ios = list(read_file)
ios_header = ios[0]
ios = ios[1:]
```
