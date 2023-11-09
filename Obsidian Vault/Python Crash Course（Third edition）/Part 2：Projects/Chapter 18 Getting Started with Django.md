# Starting an App
A Django *project* is organized as a group of individual *apps* that work together to make the project work as a whole. For now, we'll create one app to do most of our project's work. We'll add another app in Chapter 19 to manage user accounts.
You should leave the development server running in the terminal window you opened earlier. Open a new terminal window(or tab) and navigate to the directory that contains *manage.py*. Activate the virtual environment, and then run the **startapp** command:
```powershell
.\ll_env\Scripts\activate
python manage.py startapp learning_logs
ls
ls learning_logs/
```
![[Pasted image 20231105154819.png]]
The command *startapp appname* tells Django to create the infrastructure needed to build an app. When you look in the project directory now, you'll see a new folder called *learning_logs*. Use the **ls** command to see what Django has created. The most important files are models.py,*admin.py*,and *views.py*. We'll use *models.py* to define the data we want to manage in our app. We'll look at *admin.py* and *views.py* a little later.
## Defining Models 
Let's think about our data for a moment. Each user will need to create a number of topics in their learning log. Each entry they make will be tied to a topic, and these entries will be displayed as text. We'll also need to store the timestamp of each entry so we can show users when they made each one.
Open the file *models.py* and look at its existing content:
```python
from django.db import models

# Create your models here.

```
A module called *models* is being imported, and we're being invited to create models of our own. A *model* tells Django how to work with the data that will be stored in the app. A model is a class; it has attributes and methods, just like every class we've discussed. Here's the model for the topics users will store:
```python
from django.db import models

# Create your models here.
class Topic(models.Model):
	"""A topic the user is learning about."""
	text = models.CharField(max_length=200)
	date_added = models.DateTimeField(auto_now_add=True)
	def __str__(self):
	"""Return a string representation of the model."""
		return self.text
	

```
We've created a class called Topic, which inherits from Model---a parent class included in Django that defines a model's basic functionality. We add two attributes to the Topic class: text and date_added.
The text attribute is a CharField, a piece of data that's made up of characters or text. You use CharField when you want to store a small amount of text, such as a name, a title, or a city. When we define a CharField attribute, we have to tell Django how much space it should reserve in the database. Here we give a max_length of 200 characters, which should be enough to hold most topic names.
![[Pasted image 20231107213304.png|各个属性之间的关系]]
The *date_added* attribute is a **DateTimeField**, a piece of data that will record a date and time.We pass the argument *auto_now_add=True*,which tells Django to automatically set this attribute to the current date and time whenever the user creates a new topic.
It's a good idea to tell Django how you want it to represent an instance of a model.If a model has a $\_\_str\_\_()$ method,Django calls that method whenever it needs to generate output referring to an instance of that model.Here we've written a $\_\_str\_\_()$ method that returns the value assigned to the text attribute.
To see the different kinds of fields you can use in a model,see the "Model Field Reference" page at [API Reference | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/4.1/ref/).You won't need all the information right now,but it will be extremely useful when you're developing your own Django projects.
## Activating Models 
To use our models,we have to tell Django to include our app in the overall project. Open *settings.py*(in the *ll_project* directory); you'll see a section that tells Django which apps are installed in the project:
```python
INSTALLED_APPS = [

    "django.contrib.admin",

    "django.contrib.auth",

    "django.contrib.contenttypes",

    "django.contrib.sessions",

    "django.contrib.messages",

    "django.contrib.staticfiles",

]

```
Add our app to this list by modifying INSTALLED_APPS so it looks like this:
```python
INSTALLED_APPS = [

    # My apps.

    "learning_logs",

    # Default django apps.

    "django.contrib.admin",

    "django.contrib.auth",

    "django.contrib.contenttypes",

    "django.contrib.sessions",

    "django.contrib.messages",

    "django.contrib.staticfiles",

]
```
