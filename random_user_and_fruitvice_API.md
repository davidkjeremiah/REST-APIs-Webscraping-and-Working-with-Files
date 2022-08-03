# **Simple APIs for Data Analysis Practice**

## **Random User and Fruitvice API Examples**

Created on Wed Jul 29 12:33:46 2022

@author: David K. Jeremiah


## **Objectives**

This lab is aimed at loading and exploring Simple APIs, such as:

*   RandomUser API, using `RandomUser()` Python library
*   Fruitvice API, using `requests` Python library

## **Overview on APIs**

API is the acronym for **Application Programming Interface**. Basically, it is a ***software intermediary*** or ***gateway*** that allows two applications to talk to each other. As a Data Analyst, you might be required to work with API's. Some common API's that you're likely to work with include Browser API, REST API and Sensor-Based API

One of the applications we will use in this notebook is ***Random User Generator***. 

**RandomUser** is a free, [open-source](https://github.com/RandomAPI/Randomuser.me-Node) API that provides developers with randomly generated users data to be used as placeholders for testing purposes. This makes the tool similar to ***Lorem Ipsum***, but is a placeholder for people instead of text. The API can return multiple results, as well as specify generated user details such as gender, email, image, username, address, title, first and last name, and more. More information on [RandomUser](https://randomuser.me/) can be found here.

Another example of simple API that is used in this notebook is ***Fruityvice API***. 

The **Fruityvice** API is a powerful webservice which provides data for all kinds of fruit! You can use Fruityvice to find out interesting information about fruit and educate yourself. The webservice is completely free to use and contribute to. Check out more information about this awesome api here: [Fruityvice](https://www.fruityvice.com).

Now, let's explore our first API...

### **Get Methods**

*   get_cell()
*   get_city()
*   get_dob()
*   get_email()
*   get_first_name()
*   get_full_name()
*   get_gender()
*   get_id()
*   get_id_number()
*   get_id_type()
*   get_info()
*   get_last_name()
*   get_login_md5()
*   get_login_salt()
*   get_login_sha1()
*   get_login_sha256()
*   get_nat()
*   get_password()
*   get_phone()
*   get_picture()
*   get_postcode()
*   get_registered()
*   get_state()
*   get_street()
*   get_username()
*   get_zipcode()

To start using the API you can install the `randomuser` library running the `pip install` command.

```py
#!pip install randomuser
```

Next, we will load the necessary libraries.

```py
# import the necessary libraries
from randomuser import RandomUser
import pandas as pd
import urllib.request as ur
from PIL import Image
```

First, we will create a random user object, let's call it `r`.

```py
# Create a random user object
r = RandomUser()
```

Then, using `generate_users()` function, we generate a list of ten (10) random users.

```py
# Generate a list of 10 random users
some_users = r.generate_users(10)

# print list
print(some_users)
```

The "Get Methods" functions mentioned at the beginning of this notebook, can generate the required parameters to construct a dataset. For example, to get full name, we call get_full_name() function.

```py
name = r.get_full_name()
print(name)
```

Let's say we only need 10 users with full names, gender, email addresses, and their phone numbers. We can write a "for-loop" to print these 10 users.

```py
for user in some_users:
    print(user.get_full_name(), ", ", user.get_gender(), ", ", user.get_phone(), ", ", user.get_email())
```

We can see that the above takes the shape of a Dataset, detailing each users full name, gender, phone number and email address.

Let's try something fun. Let's say we want to generate 5 random user pictures from the same list.

```py
for user in some_users[0:5]:
    print(user.get_picture())
```

When you copy and paste the hyperlinks above (that has been printed out) in the address bar of a web-browser, you'll get a picture of each user. But let's view one of the picture using the `PIL` and `urllib` libraries.

```py
# save image as '19.jpg' in your current working directory
ur.urlretrieve('https://randomuser.me/api/portraits/men/19.jpg', '19.jpg')

# view image
Image.open('19.jpg')
```

To generate a table with information about the users, we can write a function containing all desirable parameters. For example, 'name', 'gender', 'city', etc. The parameters will depend on the requirements of the test to be performed. 

We call the Get Methods, listed at the beginning of this notebook. Then, we return pandas dataframe with the users.

```py
def get_users():
    users =[]
     
    for user in RandomUser.generate_users(10):
        users.append(
            {"Name":user.get_full_name(),
            "Gender":user.get_gender(),
            "City":user.get_city(),
            "State":user.get_state(),
            "Email":user.get_email(), 
            "DOB":user.get_dob(),
            "Picture":user.get_picture()}
            )
      
    return users     
```

```py
# calling function
get_users()
```

```py
# create dataframe
df1 = pd.DataFrame(get_users())  

# print the first 5 rows
print(df1.head())
```

Now we have a pandas dataframe that can be used for any testing and analysis purposes that the tester might have.

## **2. Fruityvice API**

Another common way to use APIs, is through the requests library. The part of the lab will contain more information about requests.

We will start by importing all required libraries.

```py
import requests
import json
```

We will obtain the fruitvice API data using `requests.get("url")` function. The data is in a json format.

```py
data = requests.get("https://www.fruityvice.com/api/fruit/all")
```

**Note**: Currently the webservice consists of two functions: receiving data for a ***specific fruit*** or ***all fruit***, and a function to add your own data. 

To receive needed data, you have to make a HTTP GET call on the resource `/api/fruit/all` or `/api/fruit/{ID}` or `/api/fruit/{name}` of this website's IP. To add data, make a HTTP PUT call on the resource /api/fruit with the data of a fruit in JSON format in the request body. An ID does not have to be provided. A full documentation for the REST API can be found [here](https://www.fruityvice.com/doc/index.html).

An example of what the response body would look like, for receiving data for ***all*** fruit, can be seen below.

We will retrieve results using `json.loads()` function.

```py
results = json.loads(data.text)
```

**Note:** You can get the same result using `data.json()` from the requests library, where data is the variable we created above.

Next, we will convert our json data into pandas data frame.

```py
pd.DataFrame(results)
```

The result is in a nested json format. The 'nutrition' column contains multiple subcolumns, so the data needs to be 'flattened' or normalized.

```py
# normalizing dataframe
df2 = pd.json_normalize(results)

# printing the first 5 rows
print(df2.head())
```

Let's see if we can extract some information from this dataframe. Perhaps, we need to know how many 'calories' are contained in a Banana, plus its 'family' and 'genus'.

```py
banana = df2[df2['name'] == 'Banana'][['name', 'family', 'genus', 'nutritions.calories']]

print(banana)
```

We see than Banana is from the 'Musaceae' family and comes from the 'Musa' genus or species. And we see it contains about 96 calories. 

Awesome!

