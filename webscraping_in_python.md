## **Web Scraping in Python**

Created on Fri Jul 29 08:37:28 2022

@author: David K. Jeremiah

<h2><b>Table of Contents</b></h2>
    <ul>
        <li>Overview of Webscraping</li>
        <li>Beautiful Soup Objects</li>
        <ul>
            <li>Tag</li>
            <li>Parents, Children, and Siblings</li>
            <li>HTML Attributes</li>
            <li>Navigable String</li>
        </ul>
    </ul>
    <ul>
        <li>Filter</li>
        <ul>
            <li>find All</li>
            <li>find</li>
            <li>HTML Attributes</li>
            <li>Navigable String</li>
        </ul>
    </ul>
    <ul>
        <li>Downloading and Scraping a Web Page Content</li>
    </ul>

## **Overview of Webscraping**
Let's say you want some information from a website. For instance, you would like to know more about your favourite cuisine! What do you do? Well, you can decide to copy and paste the information from Wikipedia to your own file. But what if, as you read through Wikipedia about your favourite meal, you realize there are large amounts of information from a website that you want, and you want it as quickly as possible? In such a situation, copying and pasting will not work and, quite frankly, will be tedious work! This is where Web Scraping comes in handy!

Web Scraping is a process that can be used to automatically extract information from a website, and can easily be accomplished within a matter of minutes and not hours. Most of the data on a website are unstructured data in an HTML format which, when web-scraped properly, is then converted into structured data in a spreadsheet or a database so that it can be used in various applications. 

To get started we just need a little Python code and the help of two modules named `Requests` and `Beautiful Soup`. 

First, we import the required modules and functions...

```py
# Import the required modules and functions
from bs4 import BeautifulSoup # this module helps in web scrapping.
import requests # this module helps us to download a web page
```

## **Beautiful Soup Objects**

Beautiful Soup is a Python package for extracting data from HTML and XML files. 

Here, we'll concentrate on HTML files. This is accomplished by representing the HTML as a collection of objects that contain methods for parsing the HTML. We can navigate the HTML as a tree and/or filter for what we want.

Consider the following HTML:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <h3><b id='boldest'>Lebron James</b></h3>
        <p> Salary: $ 92,000,000 </p>
        <h3> Stephen Curry</h3>
        <p> Salary: $85,000, 000 </p>
        <h3> Kevin Durant </h3>
        <p> Salary: $73,200, 000</p>
    </body>
</html>
```

<!DOCTYPE html>
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <h3><b id='boldest'>Lebron James</b></h3>
        <p> Salary: $ 92,000,000 </p>
        <h3> Stephen Curry</h3>
        <p> Salary: $85,000, 000 </p>
        <h3> Kevin Durant </h3>
        <p> Salary: $73,200, 000</p>
    </body>
</html>

We can store it as a string in the variable. Let's call the variable 'html'...

```py
html = "<!DOCTYPE html><html><head><title>Page Title</title></head><body><h3><b id='boldest'>Lebron James</b></h3><p> Salary: $ 92,000,000 </p><h3> Stephen Curry</h3><p> Salary: $85,000, 000 </p><h3> Kevin Durant </h3><p> Salary: $73,200, 000</p></body></html>"
```

To parse or extract parts of this string, we pass it into the BeautifulSoup constructor, the BeautifulSoup object, which represents the document as a nested data structure

```py
soup = BeautifulSoup(html, 'html.parser')

# View output
print(soup)
```

Beautiful Soup transforms a complex HTML document into a complex tree of Python objects.

Next, we can use the method <code>prettify()</code> to display the HTML in the nested structure:

```py
print(soup.prettify())
```

### Tags
Let's say we want the  title of the page and the name of the top paid player we can use the <code>Tag</code>. The <code>Tag</code> object corresponds to an HTML tag in the original document, for example, the tag <code>title</code>.

```py
# extracting the 'title' tag
tag_object = soup.title

# print result
print(tag_object)
```

We can confirm that what we have stored in the tag_object variable is truly a tag with the `type` function:

```py
print('tag object type: ', type(soup.title))
```

Let's try extracting another tag. For example, let's get the most paid player, 'Lebron James', which we know is in the first 'h3' tag:

```py
tag_object1 = soup.h3
print(tag_object1)
```

From the above result, we see so two tags enclosing the name 'Lebron James'. What if I just want to extract the name without the tags? We can do this with the concept of, `Parents`, `Children` and `Siblings`, which helps us navigate easily through html code.

### Parents, Children, and Siblings
Each HTML document can actually be referred to as a document tree. 

Tags may contain strings as well as other tags. These elements are the tag’s children. We can represent this as a family tree. Each nested tag is a level in the tree. 

The tag `HTML tag` contains the head and body tag. In other words, the `Head` and `body tag` are the descendants of the html tag. In particular, they are the children of the HTML tag. HTML tag is their parent. The head and body tag are siblings as they are on the same level. 

`Title tag` is the child of the head tag and thus, its parent is the head tag. The title tag is a descendant of the HTML tag but not its child. 

The `heading and paragraph tags` are the children of the body tag; and as they are all children of the body tag they are siblings of each other. The `bold tag` is a child of the heading tag


```py
tag_child = tag_object1.b
print(tag_child)
```

You can access back the parent this way:

```py
parent_tag = tag_child.parent
print(parent_tag)
```

the above result is identical to `tag_object1`

```py
print(tag_object1)
```

the tag_object1 parent is the body element.

```py
print(tag_object1.parent)
```

tag_object1 sibling is the paragraph element

```py
# First sibling in the body element
sibling_1 = tag_object1.next_sibling
print(sibling_1)
```

sibling_2 is the header element which is also a sibling of both sibling_1 and tag_object

```py
# Second sibling
sibling_2 = sibling_1.next_sibling
print(sibling_2)
```

Using the object sibling_2 and the property `.next_sibling` and `.string` we find the salary of Stephen Curry:

```py
# Finding the salary of sibling_2
sibling_3 = sibling_2.next_sibling.string
print('Stephen Curry\'s', sibling_3.strip())
```

### HTML Attributes

All HTML elements have attributes, which are basically special words that give additional information about the HTML elements as well as serve as a modifier of an HTML element type.

Attributes are always specified in the start or opening tag, and usually come in name/value pairs like: name="value".
For example, the `<img>` tag used to embed an image in an HTML page, has an `src attribute` which specifies the path to or source of the image to be displayed: `<img src='image.jpg'>`

In our case, the bold tag, represented as `<b>`, has an attribute `id` whose value is `boldest`. You can access a tag’s attributes by treating the tag like a dictionary. Recall that we have the bold tag, stored in the `tag_child` variable:

```py
# print the tag_child variable containing the bold tag
print(tag_child)
```

We see that it has an `id` attribute. Now, let's obtain it's value. We can access a tag’s attribute's value by treating the tag like a dictionary:

```py
# Get the id attribute of the bold tag
print(tag_child['id'])
```

You can access that dictionary directly as `attrs`:

```py
print(tag_child.attrs)
```

We can also obtain the content of the attribute of the tag using the Python `get()` method.

```py
print(tag_child.get('id'))
```

### Navigable String
Earlier, we saw that we were able to get Stephen Curry's salary, from the `h3 tag` using the following line of code:

```py
sibling_3 = sibling_2.next_sibling.string
print('Stephen Curry\'s', sibling_3.strip())
```

Noticed we used a BeautifulSoup attribute called `string`. Although, Beautiful soup calls it differently as ***Navigable String***

```py
# verify the type is Navigable String
print(type(sibling_3))
```

A **NavigableString** is just like a Python string or Unicode string, to be more precise. The main difference is that it also supports some BeautifulSoup features. We can covert it to sting object in Python:

```py
unicode_string = str(sibling_3)
print(type(unicode_string))
print(unicode_string)
```

## **Filter**
Filters allow you to find complex patterns, the simplest filter is a string. 

In this section we will pass a string to a different filter method and Beautiful Soup will perform a match against that exact string. Consider the following HTML of rocket launchs:

```html
<!DOCTYPE html>
<html>
    <table>
        <tr>
            <td>Flight no</td>
            <td>Launch site</td>
            <td>Payload mass</td>
        </tr>
        <tr>
            <td>1</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>300</td>
        </tr>
        <tr>
            <td>2</td>
            <td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td>
            <td>94</td>
        </tr>
        <tr>
            <td>3</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>80</td>
        </tr>
    </table>
</html>        
```

<!DOCTYPE html>
<html>
    <table>
        <tr>
            <td>Flight no</td>
            <td>Launch site</td>
            <td>Payload mass</td>
        </tr>
        <tr>
            <td>1</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>300</td>
        </tr>
        <tr>
            <td>2</td>
            <td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td>
            <td>94</td>
        </tr>
        <tr>
            <td>3</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>80</td>
        </tr>
    </table>
</html>        

We can store all that line of code as a string in the variable `table`:

```html
table = """<html>
    <table>
        <tr>
            <td>Flight no</td>
            <td>Launch site</td>
            <td>Payload mass</td>
        </tr>
        <tr>
            <td>1</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>300</td>
        </tr>
        <tr>
            <td>2</td>
            <td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td>
            <td>94</td>
        </tr>
        <tr>
            <td>3</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>80</td>
        </tr>
    </table>
</html>"""
```

table = """<html>
    <table>
        <tr>
            <td>Flight no</td>
            <td>Launch site</td>
            <td>Payload mass</td>
        </tr>
        <tr>
            <td>1</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>300</td>
        </tr>
        <tr>
            <td>2</td>
            <td><a href="https://en.wikipedia.org/wiki/Texas">Texas</a></td>
            <td>94</td>
        </tr>
        <tr>
            <td>3</td>
            <td><a href="https://en.wikipedia.org/wiki/Florida">Florida</a></td>
            <td>80</td>
        </tr>
    </table>
</html>"""

Now, we pass table into the BeautifulSoup constructor:

```py
# creating a BeautifulSoup object
table_bs = BeautifulSoup(table, 'html.parser')
```

### find_all
The find_all() method looks through a tag’s descendants and retrieves all descendants that match your filters.

The Method signature for `find_all(name, attrs, recursive, string, limit, **kwargs)`

<h4><i>name</i></h4>
When we set the name parameter to a tag name, the method will extract all the tags with that name and its children.

```py
# find all table rows tag
table_rows = table_bs.find_all('tr')
print(table_rows)
```

The result is a Python Iterable just like a list, each element is a tag object:

```py
# print the first element of the list
# print the first tables row
print(table_rows[0])
```

```py
# print the second table row
print(table_rows[1])
```

The type is `tag`

```py
print(type(table_rows[1]))
```

we can obtain the child of the first table row, let's assign it to a variable, `table_row_1`

```py
table_row_1 = table_rows[0]
print(table_row_1.td)
```

If we iterate through the list, each element corresponds to a row in the table:

```py
for i, row in enumerate(table_rows):
    print("row", i, ":", row)
```

As row is a cell object, we can apply the method find_all to it and extract table cells in the object cells using the tag td, this is all the children with the name td. The result is a list, each element corresponds to a cell and is a Tag object, we can iterate through this list as well. We can extract the content using the string attribute.

```py
for i,row in enumerate(table_rows):
    print("row",i)
    cells=row.find_all('td')
    for j,cell in enumerate(cells):
        print('colunm',j,"cell",cell)
```

If we use a list with the name parameter, we can match against any item in that list.

```py
list_input = table_bs.find_all(name = ['tr', 'td'])
print(list_input)
```

<h4><i>Attributes</i></h4>

We've established that HTML tag’s have attributes. For example the `href` argument, Beautiful Soup will filter against each tag’s href attribute.

```py
list_input_ = table_bs.find_all(href="https://en.wikipedia.org/wiki/Florida")
print(list_input_)
```

If we set the href attribute to `True`, regardless of what the value is, the code finds all tags with href value:

```py
list_input_ = table_bs.find_all(href=True)
print(list_input_)
```

How about finding all the elements without href value?

```py
list_input_ = table_bs.find_all(href=False)
print(list_input_)
```

<h4><i>string</i><h4>
With string you can search for strings instead of tags, where we find all the elments with Florida:

```py
table_bs.find_all(string="Texas")
```

### find
The `find()` method looks through a tag’s descendants and retrieves the first descendants that match your filters.

This method is used if you are looking for one element. Basically, it finds the first element in the document. 

Consider the following two table:

```html
<!DOCTYPE html>
<html>
    <body>
        <h3>Rocket Launch</h3>
        <p></p>
        <table class='rocket'>
            <tr>
                <td><b>Flight No</b></td>
                <td><b>Launch site</b></td>
                <td><b>Payload mass</b></td>
            </tr>
            <tr>
                <td>1</td>
                <td>Florida</td>
                <td>300 kg</td>
            </tr>
            <tr>
                <td>2</td>
                <td>Texas</td>
                <td>94 kg</td>
            </tr>
            <tr>
                <td>3</td>
                <td>Florida</td>
                <td>80 kg</td>
            </tr>
        </table>
        <p></p>
        <h3>Pizza Party</h3>
        <table class='pizza'>
            <tr>
                <td><b>Pizza</b></td>
                <td><b>Place Orders</b></td>
                <td><b>Slices</b></td>
            </tr>
            <tr>
                <td>Domino's Pizza</td>
                <td>10</td>
                <td>100</td>
            </tr>
            <tr>
                <td>Little Caesars</td>
                <td>12</td>
                <td>144</td>
            </tr>
            <tr>
                <td>Papa John's</td>
                <td>15</td>
                <td>165</td>
            </tr>
        </table>
    </body>
</html>
```


<!DOCTYPE html>
<html>
    <body>
        <h3>Rocket Launch</h3>
        <p></p>
        <table class='rocket'>
            <tr>
                <td><b>Flight No</b></td>
                <td><b>Launch site</b></td>
                <td><b>Payload mass</b></td>
            </tr>
            <tr>
                <td>1</td>
                <td>Florida</td>
                <td>300 kg</td>
            </tr>
            <tr>
                <td>2</td>
                <td>Texas</td>
                <td>94 kg</td>
            </tr>
            <tr>
                <td>3</td>
                <td>Florida</td>
                <td>80 kg</td>
            </tr>
        </table>
        <p></p>
        <h3>Pizza Party</h3>
        <table class='pizza'>
            <tr>
                <td><b>Pizza</b></td>
                <td><b>Place Orders</b></td>
                <td><b>Slices</b></td>
            </tr>
            <tr>
                <td>Domino's Pizza</td>
                <td>10</td>
                <td>100</td>
            </tr>
            <tr>
                <td>Little Caesars</td>
                <td>12</td>
                <td>144</td>
            </tr>
            <tr>
                <td>Papa John's</td>
                <td>15</td>
                <td>165</td>
            </tr>
        </table>
    </body>
</html>

We store the HTML as a Python string - assigning it to `table_new`

```py
table_new = "<html><body><h3>Rocket Launch</h3><p></p><table class='rocket'><tr><td><b>Flight No</b></td><td><b>Launch site</b></td><td><b>Payload mass</b></td></tr><tr><td>1</td><td>Florida</td><td>300 kg</td></tr><tr><td>2</td><td>Texas</td><td>94 kg</td></tr><tr><td>3</td><td>Florida</td><td>80 kg</td></tr></table><p></p><h3>Pizza Party</h3><table class='pizza'><tr><td><b>Pizza</b></td><td><b>Place Orders</b></td><td><b>Slices</b></td></tr><tr><td>Domino's Pizza</td><td>10</td><td>100</td></tr><tr><td>Little Caesars</td><td>12</td><td>144</td></tr><tr><td>Papa John's</td><td>15</td><td>165</td></tr></table></body></html>"
```

```py
table_new_bs = BeautifulSoup(table_new, "html.parser")
print(table_new_bs)
```

We can find the first table using the tag name <code>table</code>

```py
print(table_new_bs.find(name='table'))
```

We can filter on the `class` attribute to find the second table; the pizza table.

**Note** however, because class is a keyword in Python, we add an underscore.

```py
print(table_new_bs.find(name="table", class_="pizza"))
```

## **Downloading And Scraping a Web Page Content**
We Download the contents of the web page:

```py
url = "http://www.google.com"
```

We use get to download the contents of the webpage in text format and store in a variable called `data`:

```py
# Downloading and store the contents of the webpage
data = requests.get(url)

# Checking the Success data of download
print('Status code: ', data.status_code)

# getting the html file in text format
data = data.text
```

We create a BeautifulSoup object using the BeautifulSoup constructor

```py
# create a soup object using the variable 'data'
soup_new = BeautifulSoup(data, 'html.parser') 
```

### **Scrape all links**

```py
for link in soup_new.find_all('a', href=True): 
    # in html anchor/link is represented by the tag <a>
    print(link['href'])
```

### **Scrape  all images  Tags**

```py
for img_tag in soup_new.find_all('img', src=True):
    # in html image is represented by the tag <img>
    print(img_tag)
    print(" ")
    print(img_tag.get('src'))
```

### **Scrape data from HTML tables**

```py
# The below url contains an html table with data about colors and color codes.
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/labs/datasets/HTMLColorCodes.html"
```

Before proceeding to scrape a web site, let's examine the contents, and the way data is organized on the website. So, open the above url in your browser and check how many rows and columns are there in the color table.

```py
# get the contents of the webpage in text format and store in a variable called data_new
data_new = requests.get(url).text
```

Next, parse with Beautiful Soup constructor

```py
soup_ = BeautifulSoup(data_new,"html.parser")
```

```py
# find a html table in the web page
table = soup_.find(name='table') # in html table is represented by the tag <table>
```

```py
# Get all rows from the table
for i, row in enumerate(table.find_all('tr')):
    # Get all columns in each row
    cols  = row.find_all('td')
    print("row", i, ":") # print each row
    print(cols[2].string, ":", str(cols[3].string)) # get the colors, and hex code in each row
```

### **Scrape data from HTML tables into a DataFrame using BeautifulSoup and Pandas**

```py
# import pandas
import pandas as pd
```

```py
# The below url contains html tables with data about world population.
url = "https://en.wikipedia.org/wiki/World_population"
```

Before proceeding to scrape a web site, we need to examine the contents, and the way data is organized on the website. 

Therefore, open the above url in your browser and check the tables on the webpage.

```py
# get the contents of the webpage and store in a variable called data_new_2
data_new_2 = requests.get(url)

# Status check
print(data_new_2.status_code)

# Convert contents from data_new_2 to text format
data_new_2 = data_new_2.text
```

```py
# Parse data using Beautiful Soup constructor
soup_new_1 = BeautifulSoup(data_new_2,"html.parser")
```

```py
# find all html tables in the web page
table_list = soup_new_1.find_all(name="table")

# we can see how many tables were found by checking the length of the tables list
print(len(table_list))
```

We have 25 tables in this website.

Now, assume that we are looking for the `10 most densely populated countries` table, we can look through the tables list and find the right one we are look for based on the data in each table or we can search for the table name if it is in the table but this option might not always work.

```py
for index, table in enumerate(table_list):
    if "10 most densely populated countries" in str(table):
        table_index = index

print(table_index)
```

Let's locate, using the table_index, the name of the table, '10 most densely populated countries', below.

```py
pop_table = table_list[table_index] # this is the table we need

print(pop_table.prettify())
```

```py
population_data = pd.DataFrame(columns=["Rank", "Country", "Population", "Area", "Density"])

for row in pop_table.tbody.find_all("tr"):
    col = row.find_all("td")
    if (col != []):
        rank = col[0].text
        country = col[1].text
        population = col[2].text.strip()
        area = col[3].text.strip()
        density = col[4].text.strip()
        population_data = population_data.append({"Rank":rank, 
                                                "Country":country, 
                                                "Population":population, 
                                                "Area":area, 
                                                "Density":density}, 
                                                ignore_index=True)

population_data
```

### **Scrape data from HTML tables into a DataFrame using BeautifulSoup and read_html**
Using the same `url`, `data_new_2`, `soup_new_1`, and `table_list` object as in the last section we can use the Pandas `read_html` function to create a DataFrame.

Remember the table we need is located in `pop_table`

We can now use the pandas function `read_html` and give it the string version of the table as well as the `flavor` which is the parsing engine bs4.

```py
pd.read_html(str(pop_table), flavor="bs4")
```

The function `read_html` always returns a list of DataFrames. 

Thus, since we have only one list of dataframe, we must pick the one we want out of the list, so as to represent it as a proper dataframe.

```py
pd.read_html(str(pop_table), flavor="bs4")[0]
```

### **Scrape data from HTML tables into a DataFrame using read_html**

We can also use the read_html function to directly get DataFrames from a url.

```py
dataframe_list = pd.read_html(url, flavor="bs4")

# print the number of table found in the url
print(len(dataframe_list))
```

We see that we get the same length of table just like when we used find_all on the soup_new_1 object.

Finally we can pick the DataFrame we need out of the list.

```py
print(dataframe_list[table_index])
```

Let's get the `Global annual population growth` located as the 7th index

```py
print(dataframe_list[7])
```

We can also use the `match` parameter to select the specific table we want. If the table contains a string matching the text it will be read.

```py
pd.read_html(url, match="10 most densely populated countries", flavor='bs4')[0]
```