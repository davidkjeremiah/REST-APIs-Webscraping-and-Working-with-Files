# **HTTP and Requests**
Created on Wed Jul 28 05:50:29 2022

@author: David K. Jeremiah

## **Objectives**

This lab is aimed at:

*   Understanding HTTP
*   Handling HTTP Requests


## **Overview of HTTP**

HTTP stands for **Hypertext Transfer Protocol**. It is the communication protocol or 'rule' you use whenever you browse the web. It is used in transferring web resources such as HTML documents, images, styles, and other files. 

Basically, HTTP is a **request - response** based protocol. A web browser or client (e.g. your laptop) sends an HTTP request to a server, and the webserver sends the HTTP response back to the browser. 


## **Makeup of an HTTP Request**

Next, let's expore the makeup of an HTTP request. 

### **HTTP requests**
Every HTTP request begins with the request line as shown below. This consists of a ***method***, ***path*** or the resource requested, ***version*** or protocol used, and ***headers***.

### HTTP Methods
The HTTP method describes the type of action that the client wishes to perform. The primary or the most commonly used HTTP methods are **GET**, **POST**, **PUT** and **DELETE**.

The GET method is used to retrieve information from the given server. The POST request method is used to send data to the server. The PUT method updates whatever currently exists on the website with something else. And finally, the DELETE method removes the resources. 

### HTTP Path
The **Path** is the ***representation*** of where the resource is stored on the webserver. For example, if you requested an image at <https://example.com/index.html>, the path would be **/index.html**. 

### HTTP Version
There are multiple **Versions** of the HTTP protocol, with the commonly used ones as Version 1.1 and 2.0

### HTTP Headers
Finally, there are the **Headers** which contain additional information about the request, and the client that is making the request. 

**Note:** For certain requests methods, the requests will also contain a **body** of content that the client is sending.


## **Makeup of an HTTP Response**
Now, let's expore some details about the makeup of an HTTP response.

### **HTTP response**
An HTTP response follows a format similar to the request format. Following the header, the response will optionally contain a message body consisting of the response contents such as the HTML document, the image file, and so forth. 

### Status codes
HTTP status codes indicate if the HTTP requests successfully completed. The code values are in the range of a **100 - 599** and are grouped by purpose. The status message, such as 'OK', is a text representation of the status code. We've many times encountered such during our web browsing, where the pages we tried to access displayed 404 (not found) error, or 505 (server not responding) error. These are HTTP status codes with their text representations.

There are five groups of status code:
* **Information** messages are grouped from 100 - 199
* **Successful** messages are grouped from 200 - 299
* **Redirection** messages are grouped from 300 - 399
* **Client error** messages range from 400 - 499, and
* **Server error** messages are grouped from 500 - 599

### Response Headers
Following the status line, there are optional HTTP response headers followed by a line break.

Similar to the request headers, there are many possible HTTP headers that ca be included in the http response.

* The **Date** header specifies the date and time the HTTP response was generated.
* The **Server** header decribes the web server software used to generate the response.
* The **Content-Type** header describes the media type of the resource returned (e.g. HTML document, image, video, etc.)
* The **Content-Length** header described the length of the response.

### Response Body
Following the HTTP response headers is the HTTP response body. This is the main content of the HTTP response.

This can contain images, video, HTML documents and other media types.

## **Python Requests Library**
Requests is a Python Library that allows you to send HTTP/1.1 requests easily. We can import the library as follows:

```Py
import requests 
```

We will also use the following libraries:


```Py
import os 
from PIL import Image 
from IPython.display import IFrame
```

Now, we make an HTTP GET request via the method **get** to <https://www.google.com>

```Py
# creating a url variable
url = 'https://www.google.com'

# using the .get method from the requests library
r = requests.get(url)
```

We have the response object **r**, which has information about the request, such as the status of the request. We can view the status code using the attribute status_code.

```Py
# Check the Status code of request
print(r.status_code)
```

We can view the request headers generated following the get method used on <https://www.google.com>

```Py
# Check the requests header
print(r.request.headers)
```

We can view the request body, in the following line:

```Py
print("request body:", r.request.body)
```

As there is no body for a get request we get a **None**, as shown above. 

We can view the HTTP response header using the attribute ***headers***. This returns a python dictionary of HTTP response headers.

```Py
rep_headers = r.headers
print('response header:', rep_headers)
```

We can obtain the date the request was sent using the key **Date**

```Py
print(rep_headers['Date'])
```

**Content-Type** indicates the type of data:

```Py
print(rep_headers['Content-Type'])
```

We can also check the encoding:
```Py
print(r.encoding)
```

As the Content-Type is text/html we can use the attribute **text** to display the HTML in the body. We can review the first 100 characters:

```Py
print(r.text[0:100])
```

Now, let's try loading other types of data for non-text requests, like images. Consider the URL of the following image: [Flowers Plant](https://cdn.pixabay.com/photo/2022/07/22/18/50/helenium-7338764_960_720.jpg).

```Py
# Generating new url variable for an image file
url_ = 'https://cdn.pixabay.com/photo/2022/07/22/18/50/helenium-7338764_960_720.jpg'

# Making a get request
r_img = requests.get(url_)
```

Let's be sure, we retrieved our image successfully:

```Py
print('Status report:', r_img.status_code)
```

Awesome! Information retrival was successful!

We can look at the response header:

```Py
print(r_img.headers)
```

We can see the 'Content-Type'

```Py
print('Content Type: ', r_img.headers['Content-Type'])
```

An image is a response object that contains the image as a [bytes-like object](https://docs.python.org/3/glossary.html?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2022-01-01#term-bytes-like-object). As a result, we must save it using a file object. First, we specify the file path and name

```Py
path = os.path.join(os.getcwd(), 'image.jpeg')
print(path)
```

Next, we save the file. 

In order to access the body of the response we use the attribute **content** then save it using the open function and write method:

```Py
with open(path, "wb") as file:
    file.write(r_img.content)
```

We can now view the image

```Py
# Viewing Image
Image.open(path)
```

## **Get Request with URL Parameters**

The GET method can be used to influence the result of your query, such as retrieving data from an API. A GET request is sent to the server. We append **/get** to the Route, as we did with the Base URL, to signal that we want to perform a GET request.

The Base URL is for <http://httpbin.org/> is a simple HTTP Request & Response Service. The URL in Python is given by:

```Py
url_get = 'http://httpbin.org/get'
```

A query string is a part of a uniform resource locator (URL), that sends other information to the web server. 

The start of the query is a **?**, followed by a series of parameter and value pairs, like this: `http://httpbin.org/?id='123'&name='David`

The first parameter name is **Name** and the value is **Joseph**. The second parameter name is **ID** and the value is **123**. Each pair, parameter, and value is separated by an equals sign, **=**. The series of pairs is separated by the ampersand **&**.

To create a Query string, add a dictionary. The keys are the parameter names and the values are the value of the Query string.

```Py
payload = {'name':'Joseph', 'ID':'123'}
```

Then passing the dictionary payload to the params parameter of the  get() function:

```Py
r_get = requests.get(url_get, params = payload)
```

We can print out the URL and see the name and values

```Py
print("URL: ", r_get.url)
```

Checking the request body. There is no request body!

```Py
print(r_get.request.body)
```

Let's check the status code

```Py
print(r_get.status_code)
```

We can view the response as text:

```Py
print(r_get.text)
```

We can look at the 'Content-Type':

```Py
print(r_get.headers['Content-Type'])
```

As the content 'Content-Type' is in the **JSON** format we can use the method json(), it returns a Python dict:

```Py
print(r_get.json())
```

The Key 'args' has the names/keys and values.

```Py
print(r_get.json()['args'])
```

## **Post Request**
Like a GET request, a POST is used to send data to a server, but the POST request sends the data in a request body. In order to send the Post Request in Python, in the URL we change the route to POST, like this:

```Py
url_post = 'http://httpbin.org/post'
```

This endpoint will expect data as a file or as a form. A form is convenient way to configure an HTTP request to send data to a server.

To make a POST request we use the post() function, the variable payload is passed to the parameter data:

```Py
r_post = requests.post(url_post,data=payload)
```

Comparing the URL from the response object of the GET and POST request we see the POST request has no name or value pairs.

```Py
print("POST request URL: ",r_post.url)
print("GET request URL: ",r_get.url)
```
