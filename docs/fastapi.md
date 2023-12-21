## FastAPI
FastAPI is a modern Python web framework, very efficient in building APIs.It is one of the fastest web frameworks of Python.

#### FastAPI - EnvironmentSetup

To install FastAPI  
```
pip install "fastapi[all]"
```
FastAPI depends on Starlette and Pydantic libraries, hence they also get installed.

To run FastAPI app, you need an ASGI server called uvicorn, so install it.
```
pip install uvicorn
```
#### Getting started
The first step in creating a FastAPI app is to declare the application object of FastAPI class.
```
from fastapi import FastAPI
app=FastAPI()
```
The next step is to create path operation.Path is a URL which when visited by the client invokes visits a mapped URL to one of the HTTP methods, an associated function is to be executed.. For example, the index() function corresponds to ‘/’ path with ‘get’ operation.
```
@app.get("/")
async def root():
    return {"message":"Hello World"}
```
Start the uvicorn server<br>
Unlike the Flask framework, FastAPI doesn’t contain any built-in development server. Hence we need Uvicorn. It implements ASGI standards and is lightning fast. ASGI stands for Asynchronous Server Gateway Interface.

```
uvicorn main:app --reload
```
### Path Parameters

Modern web frameworks use routes or endpoints as a part of URL instead of file-based URLs. This helps the user to remember the application URLs more effectively. In FastAPI, it is termed a path. A path or route is the part of the URL trailing after the first ‘/’.

For example, in the following URL,
```
http://localhost:8000/hello/welcomeNepal
```
the path or the route would be 
```
/hello/welcomeNepal
```
In FastAPI, such a path string is given as a parameter to the operation decorator. The operation here refers to the HTTP verb used by the browser to send the data. These operations include GET, PUT, etc. The operation decorator (for example, @app.get("/")) is immediately followed by a function that is executed when the specified URL is visited. In the below example −

```
from fastapi import FastAPI
app = FastAPI()
@app.get("/")
async def index():
   return {"message": "Hello World"}
```
Here, ("/") is the path, get is the operation, @app.get("/") is the path operation decorator, and the index() function just below it is termed as path operation function.

**Method and Description**<br>
1. **POST:**  to create data. Used to send HTML form data to the server. Data received by the POST method is not cached by the server.        
2. **GET:** to read data.Sends data in unencrypted form to the server. Most common method.
3. **PUT:** to update data. Replaces all current representations of the target resource with the uploaded content.
4. **DELETE:** to delete data. Removes all current representations of the target resource given by a URL.

#### Path Parameters with Types
You can declare the type of a path parameter in the function, using standard Python type annotations.
```
@app.get("/hello/{name}/{age}")
async def hello(name:str,age:int):
        return {"name":name,"age":age}
```
### Query Parameters
When you declare other function parameters that are not part of the path parameters, they are automatically interpreted as "query" parameters.The query is the set of key-value pairs that go after the ? in a URL, separated by & characters.
```
http://127.0.0.1:8000/home?name=ram&age=20
```
The trailing part of the URL, after (?), is the query string, which is then parsed by the server-side script for further processing.
```
@app.get("/home")
async def hello(name:str,age:int):
    return {"name":name,"age":age}
```
You can use Python’s type hints for the parameters of the function to be decorated. In this case, define name as str and age as int.

```
@app.get("/hello/{name}")
def display_info(name:str,age:int):
    return{"name":name,"age":age}
```
Here,The parameter 'name' is a path parameter and 'age' is a query parameter.

### Parameter Validation

It is possible to apply validation conditions on path parameters as well as query parameters of the URL. **In order to apply the validation conditions on a path parameter, you need to import the Path class**. In addition to the default value of the parameter, you can specify the maximum and minimum length in the case of a string parameter.

```
@app.get("/hello/{name}/")
def helo(name:str=Path(...,min_length=3,max_length=10)):
    return{"name":name}
```
Validation rules can be applied on numeric parameters too, using the operators as given below −

- gt − greater than
- ge − greater than or equal
- lt − less than
- le − less than or equal

```
@app.get("/hello/{name}/{age}")
def hello(*,name:str=Path(...,min_length=3,max_length=10),age:int=Path(...,ge=1,le=100)):
    return {"name":name,"age":age}

```
In this case, validation rules are applied for both the parameters name and age.

**Query validatiion** 
     
The query parameters can also have the validation rules applied to them. **You have to specify them as the part of arguments of Query class constructor.**
```
@app.get("/hello/{name}/{age}")
def hello(*,name:str=Path(...,min_length=3,max_length=10),
          age:int= Path(...,ge=1,le=100),
          percent:float=Query(...,ge=0,le=100)):
    return {"name":name,"age":age}
```
#### FastAPI - Pydantic
Pydantic is a Python library for data parsing and validation. It uses the type hinting mechanism of the newer versions of Python (version 3.6 onwards) and validates the types during the runtime. Pydantic defines **BaseModel** class. It acts as the base class for creating user defined models.

```
from typing import List
from pydantic import BaseModel
class Student(BaseModel):
   id: int
   name :str
   subjects: List[str] = []
```

Pydantic also contains a **Field class** to declare metadata and validation rules for the model attributes.
```
from typing import List
from pydantic import BaseModel, Field
class Student(BaseModel):
   id: int
   name :str = Field(None, title="The description of the item", max_length=10)
   subjects: List[str] = []

```
#### Request Body
We shall now use the Pydantic model object as a request body of the client’s request. As mentioned earlier, we need to use **POST** operation decorator for the purpose.
```

class Student(BaseModel):
   id: int
   name :str = Field(None, title="name of student", max_length=10)
   subjects: List[str] = []
@app.post("/students/")
async def student_data(s1: Student):
   return s1

```
## Templates
By default, FastAPI renders a JSON response to the client. However, it can be cast to a HTML response. For this purpose, FastAPI has **HTMLResponse** class defined in **fastapi.responses** module. We need to add **response_class** as an additional parameter to operation decorator, with HTMLResponse object as its value.

In the following example, the @app.get() decorator has "/hello/" endpoint and the HTMLResponse as response_class. Inside the hello() function, we have a string representation of a HTML code of Hello World message. The string is returned in the form of HTML response.
```
from fastapi.responses import HTMLResponse
from fastapi import FastAPI
app = FastAPI()
@app.get("/hello/")
async def hello():
   ret='''
<html>
<body>
<h2>Hello World!</h2>
</body>
</html>
'''
   return HTMLResponse(content=ret)

```
Web template library has a template engine that merges a static web page having place holder variables. Data from any source such as database is merged to dynamically generate and render the web page. FastAPI doesn’t have any prepackaged template library. So one is free to use any one that suits his needs. In this tutorial, we shall be using jinja2, a very popular web template library. Let us install it first using pip installer.
```
pip3 install jinja2
```
FastAPI’s support for Jinja templates comes in the form of jinja2Templates class defined in **fastapi.templates module**.

```
from fastapi.templating import Jinja2Templates
```
To declare a template object, the folder in which the html **templates** are stored, should be provided as parameter. Inside the current working directory, we shall create a ‘**templates**’ directory.
```
templates = Jinja2Templates(directory="templates")
```
A simple web page ‘hello.html’ to render Hello World message is also put in ‘**templates**’ folder.

We are now going to render html code from this page as HTMLResponse. Let us write the hello() function as follows −
```
from fastapi.responses import HTMLResponse
from fastapi.templating import Jinja2Templates
from fastapi import FastAPI, Request
app = FastAPI()
templates = Jinja2Templates(directory="templates")
@app.get("/hello/", response_class=HTMLResponse)
async def hello(request: Request):
   return templates.TemplateResponse("hello.html", {"request": request})
```
Here, **templateResponse()** method of template object collects the template code and the request context to render the http response.

## Static files
Often it is required to include in the template response some resources that remain unchanged even if there is a certain dynamic data. Such resources are called static assets. Media files (.png, .jpg etc), JavaScript files to be used for executing some front end code, or stylesheets for formatting HTML (.CSS files) are the examples of static files.

In order to handle static files, you need a library called **aiofiles**
```
pip3 install aiofiles
```
Next, import **StaticFiles** class from the **fastapi.staticfiles** module. Its object is one of the parameters for the **mount()** method of the FastAPI application object to assign "static" subfolder in the current application folder to store and serve all the static assets of the application.
```
app.mount("/static", StaticFiles(directory="static"), name="static")
```
Lion image is to be rendered in the hello.html template. 
```
templates = Jinja2Templates(directory="templates")
app.mount("/static", StaticFiles(directory="static"), name="static")
@app.get("/hello/{name}", response_class=HTMLResponse)
async def hello(request: Request, name:str):
   return templates.TemplateResponse("hello.html", {"request": request, "name":name})

```
## HTML Form Templates

Let us add another route "/login" to our application which renders a html template having a simple login form. The HTML code for login page is as follows:
```
<html>
   <body>
      <form action="/submit" method="POST">
         <h3>Enter User name</h3>
         <p><input type='text' name='nm'/></p>
         <h3>Enter Password</h3>
         <p><input type='password' name='pwd'/></p>
         <p><input type='submit' value='Login'/></p>
      </form>
   </body>
</html>
```
Add login() function in the main.py file as :
```
templates=Jinja2Templates(directory="templates")
@app.get("/login",response_class=HTMLResponse)
async def login(request:Request):
    return templates.TemplateResponse("login.html",{"request":request})
```
## Accessing Form Data

Now we shall see how the HTML form data can be accessed in a FastAPI operation function. In the above example, the /login route renders a login form. The data entered by the user is submitted to /**submit** URL with POST as the request method. Now we have to provide a view function to process the data submitted by the user.

FastAPI has a Form class to process the data received as a request by submitting an HTML form. However, you need to install the **python-multipart** module. It is a streaming multipart form parser for Python.
```
pip3 install python-multipart
```
Add **Form** class to the imported resources from FastAPI
```
from fastapi import Form
```
Let us define a submit() function to be decorated by @app.post(). In order to receive the form data, declare two parameters of Form type, having the same name as the form attributes.
```
@app.post("/submit/")
async def submit(nm: str = Form(...), pwd: str = Form(...)):
   return {"username": nm}
```
It is even possible to populate and return Pydantic model with HTML form data. In the following code, we declare User class as a Pydantic model and send its object as the server’ response.
```
from pydantic import BaseModel
class User(BaseModel):
   username:str
   password:str
@app.post("/submit/", response_model=User)
async def submit(nm: str = Form(...), pwd: str = Form(...)):
   return User(username=nm, password=pwd)
```
## Uploading Files
First of all, to send a file to the server you need to use the HTML form’s enctype as multipart/form-data, and use the input type as the file to render a button, which when clicked allows you to select a file from the file system.
```
<html>
   <body>
      <form action="http://localhost:8000/uploader" method="POST"       enctype="multipart/form-data">
         <input type="file" name="file" />
         <input type="submit"/>
      </form>
   </body>
</html>
```
This HTML form is rendered as a template with following code
```
from fastapi import FastAPI, File, UploadFile, Request
import uvicorn
import shutil
from fastapi.responses import HTMLResponse
from fastapi.templating import Jinja2Templates
app = FastAPI()
templates = Jinja2Templates(directory="templates")
@app.get("/upload/", response_class=HTMLResponse)
async def upload(request: Request):
   return templates.TemplateResponse("upload.html", {"request": request})
```
The upload operation is handled by **UploadFile** function in FastAPI
```
@app.post("/uploader/")
async def create_uploadfile(file:UploadFile=File(...)):
    with open("lion-king.jpeg","wb") as buffer:
        shutil.copyfileobj(file.file,buffer)
    return {"filename":file.filename}
```
## Cookie Parameters

A **cookie** is one of the HTTP headers. The web server sends a response to the client,in addition to the data requested, it also inserts one or more cookie. A cookie is a very small amount of data,that is stored in the client's machine. On Subsequent connection requests from the same client,this cookie data is also attached along with the HTTP requests.

The cookies are useful for recording information about client's browsing. Cookies are a reliable method of retrieving stateful information in otherwise stateless communication by HTTP protocol.

In FastAPI, the cookie parameter is set on the rresponse object with the help of **set_cookie()** method.

```
response.set_cookie(key,value)

```
 **Example of set_cookie() method**
 We have a JSON response object called content. Call the set_cookie() method on it to set a cookie as key="username" and value="admin".
 ```
 from fastapi import FastAPI
from fastapi.responses import JSONResponse
app = FastAPI()
@app.post("/cookie/")
def create_cookie():
   content = {"message": "cookie set"}
   response = JSONResponse(content=content)
   response.set_cookie(key="username", value="admin")
   return response
 ```
 To read back the cookie on a subsequent visit, use the Cookie object in the FastAPI library.
```
from fastapi import FastAPI, Cookie
app = FastAPI()
@app.get("/readcookie/")
async def read_cookie(username: str = Cookie(None)):
   return {"username": username}
```
## Header Parameter

In order to read the values of an HTTP header that is a part of the client request,import the Header object from the FastAPI library, and declare a parameter of Header type in the operation function defination.The name of the Parameter should match with the HTTP header converted in camel_case.

**Example of Header Parameter**
```
from typing import Optional
from fastapi import FastAPI,Header
app=FastAPI()
@app.get("/headers/")
async def read_header(accept_language:Optional[str]=Header(None)):
      return {"Accept_Language":accept_language}
```
You can push custom as well as predefined headers in the response object. The operation function should have a parameter of **Response** type. In order to set a custom header, its name should be prefixed with **"X"**. In the following case, a custom header called "X-Web-Framework" and a predefined header “Content-Language" is added along with the response of the operation function.
```
@app.get("/rspheader/")
def set_rsp_headers():
   content = {"message": "Hello World"}
   headers = {"X-Web-Framework": "FastAPI", "Content-Language": "en-US"}
   return JSONResponse(content=content, headers=headers)
```

## Response Model
An operation function returns A JSON response to the client. The response can be in the form of Python primary types, i.e., numbers, string, list or dict, etc. It can also be in the form of a Pydantic model. For a function to return a model object, the operation decorator should declare a respone_model parameter.

With the help of reaponse_model,FastAPI converts the output data to a structure of a model class. It validates the data, adds a JSON SChema for the response, in the OPENAPI path operation.

One of the important advatages of response_model parameter is that we can format the output by selecting the fields from the model to cast the response to an output model.

```
from typing import List 
from fastapi import FastAPI
from pydantic import BaseModel,Field
app=FastAPI()
class student(BaseModel):
    id:int
    name:str=Field(None,title="name of student",max_length=10)
    marks:list[int]=[]
    percent_marks:float
class percent(BaseModel):
    id:int
    name:str= Field(None,title="name of student",max_length=10)
    percent_marks:float
@app.post("/marks",response_model=percent)
async def get_percent(s1:student):
    s1.percent_marks=sum(s1.marks)/2
    return s1                                                                                                        
```
## Nested Models

Each attribute of a **Pydantic** model has a type. The type can be a built-in Python type or a model itself. Hence it is possible to declare nested JSON "objects" with specific attribute names, types, and validations.
```
from typing import Tuple

class supplier(BaseModel):
   supplierID:int
   supplierName:str
class product(BaseModel):
   productID:int
   prodname:str
   price:int
   supp:supplier
class customer(BaseModel):
   custID:int
   custname:str
   prod:Tuple[product]
```
The following **POST** operation decorator renders the object of the customer model as the server response.
```
@app.post('/invoice')
async def getInvoice(c1:customer):
   return c1
```

## FastAPI-Dependencies

The built-in dependency injection system of FastAPI makes it possible to integrate components easier when building your API. In programming,Dependency injection refers to the mechanism where an  object receives other objects that it depends on . The other objects are called dependencies. Dependency injection has the following advantages
- reuse the same shared logic
- share database connections
- enforce authentication and security features

FastAPI provides Depends class and its object is used as a common parameter in such cases. First import Depends from FastAPI and define a function to receive these parameters 
```
async def dependency(id: str, name: str, age: int):
   return {"id": id, "name": name, "age": age}
```

Now we can use the return value of this function as a parameter in operation functions
```
@app.get("/user/")
async def user(dep: dict = Depends(dependency)):
   return dep
```
You can use a class for managing dependencies instead of a function. Declare a class with id, name and age as attributes.
```
class dependency:
   def __init__(self, id: str, name: str, age: int):
      self.id = id
      self.name = name
      self.age = age 

```
Use this class as the type of parameters.
```
@app.get("/user/")
async def user(dep: dependency = Depends(dependency)):
   return dep
@app.get("/admin/")
async def admin(dep: dependency = Depends(dependency)):
   return dep 

```
## FastAPI-CORS

**Cross-Origin Resource Sharing (CORS)** is a situation when a frontend application that is running on one client browser tries to communicate with a backend through JavaScript code, and the backend is in a different "origin" than the frontend. The origin here is a combination of protocol, domain name, and port numbers. As a result, http://localhost and https://localhost have different origins.

If the browser with a URL of one origin sends a request for the execution of JavaScript code from another origin, the browser sends an **OPTIONS** HTTP request.

If the backend authorizes the communication from this different origin by sending the appropriate headers it will let the JavaScript in the frontend send its request to the backend. For that, the backend must have a list of "allowed origins".

To specify explicitly the allowed origins, import **CORSMiddleware** and add the list of origins to the app's middleware.
```
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
app = FastAPI()
origins = [
   "http://192.168.211.:8000",
   "http://localhost",
   "http://localhost:8080",
]
app.add_middleware(
   CORSMiddleware,
   allow_origins=origins,
   allow_credentials=True,
   allow_methods=["*"],
   allow_headers=["*"],
)
@app.get("/")
async def main():
   return {"message": "Hello World"}
```

## CRUD Operations

The REST architecture uses HTTP  verbs or methods for the operation on the resources. The POST,GET,PUT and DELETE methods perform respectively CREATE,READ,UPDATE and DELETE operations respectively.
First, let us set up a FastAPI app object and declare a Pydantic model called **Book.**







references
- https://realpython.com/fastapi-python-web-apis/#create-a-first-api
