First we create a [[venv]]
On creation of the application, we have the `manage.py` file that acts as a control center. We can use the `runserver` command to start an Http server at the desired port.
The structure of the project is as follows:
`urls.py` this is the file where we specify endpoints that we want to match and specify corresponding functions that handle the response. The functions which handle the response are present in `views.py`. If the application is quite large, one might create several sub applications( a.k.a installed apps ) to deal with different components. They can have their own `urls.py` file which can be in extension to the base file. 
The development server has HMR built in.

`models.py` is the file where we specify the objects that we would like to have in our database as classes in python, these have specified types (not in python, but abstract ones provided by django that conform to the databases types).
<span style="color:#e1db3d">Serialization</span> refers to the steps that involve getting data from the database as objects of some class and then turning that into a Json response, this is a tedious process and the Rest Framework can help us out here. 