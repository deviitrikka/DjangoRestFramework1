# **Building a CRUD API with Django Rest Framework (DRF)**

Hey everyone! In this video, I will show you **how to build a CRUD (Create, Read, Update, Delete) API** using **Django Rest Framework (DRF)**.

As you can see in the presentation, it explains how **DRF works** and highlights its **main components**.  

Like always, I’ll walk you through this in two parts:

1. **How Django Rest Framework (DRF) works** and how it helps with **CRUD operations**.  
2. **How to set up the project from scratch**, create an API, and perform CRUD operations using APIs.  

Now, let’s jump straight into the first part!

---

## **How Django Rest Framework Works**  

In Django Rest Framework (DRF), we have **four important components**:

- **URLs** (`urls.py`) – Defines API endpoints.
- **Views** (`views.py`) – Handles logic for processing API requests.
- **Serializers** (`serializers.py`) – Converts data between JSON and Django models.
- **Models** (`models.py`) – Defines database tables.

These are just Python files inside your Django project that help **interact with the database using APIs**.

### **How a Request Works in DRF**  

Whenever you make an **HTTP request** (using Postman, a command line tool, or a browser), here’s what happens:

1. The request first reaches **`urls.py`**.
2. This file checks the URL and sends the request to the **corresponding view** (`views.py`).
3. The view processes the request and, if needed, interacts with the **database** (`models.py`).
4. If data needs to be converted (e.g., from JSON to a database format), it goes through the **serializer** (`serializers.py`).
5. The **response** is sent back, either as JSON or another format.

For example, if you **send a request to create a new record**, the API:
- **Receives your request** (JSON data).
- **Processes it** in `views.py`.
- **Uses the serializer** to convert the data.
- **Saves it to the database** using `models.py`.
- **Returns a response** saying the data was saved.

Similarly, if you **request data**, the API retrieves it from the database, **converts it to JSON**, and sends it back.

---

## **Setting Up the Project**  

### **Step 1: Install Required Packages**  

Before we start, make sure your system has the required Python libraries installed.  

Since I am using **Windows**, I will install **Django and Django Rest Framework** by running these commands:

```sh
pip install django
pip install djangorestframework
pip install psycopg2  # Required for PostgreSQL
```

- **Django** is the main framework.
- **Django Rest Framework (DRF)** is an add-on that makes API development easier.
- **Psycopg2** allows Django to connect with **PostgreSQL** (our database).

---

### **Step 2: Create a Django Project**  

Now, we create a Django project. I’ll name mine **CloudQuickLabs**:

```sh
django-admin startproject CloudQuickLabs
```

This creates a folder called **CloudQuickLabs**, which contains the main project files:

- **`manage.py`** – The main script to run the project.
- **A subfolder (`CloudQuickLabs/`)** – Contains project settings and configurations.

Navigate into the project folder:

```sh
cd CloudQuickLabs
```

---

### **Step 3: Create an App Inside the Project**  

A Django project can have multiple **apps** (modules). I’ll create an app called **cars**:

```sh
python manage.py startapp cars
```

Now, inside our project, we have a **new folder `cars/`**, which contains:

- **`models.py`** – Defines the database structure.
- **`views.py`** – Contains logic for handling API requests.
- **`urls.py`** (we will create this later) – Defines the API endpoints.

---

### **Step 4: Configure Django Settings**  

Open **`settings.py`** (inside `CloudQuickLabs/`) and update these settings:

1. **Add Installed Apps:**  

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    # Add these
    'rest_framework',
    'cars',
]
```

2. **Configure the Database (PostgreSQL)**  

If using **PostgreSQL**, modify the **DATABASES** section:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'cars_db',
        'USER': 'postgres',
        'PASSWORD': 'root',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

Save the file.

---

### **Step 5: Define the Database Model**  

Go to `cars/models.py` and define the **Car model**:

```python
from django.db import models

class Car(models.Model):
    name = models.CharField(max_length=100)
    brand = models.CharField(max_length=100)
    model = models.CharField(max_length=100)

    def __str__(self):
        return self.name
```

Run migrations to create the table in the database:

```sh
python manage.py makemigrations
python manage.py migrate
```

---

### **Step 6: Create a Serializer**  

Create a file `cars/serializers.py` and define a serializer for **Car**:

```python
from rest_framework import serializers
from .models import Car

class CarSerializer(serializers.ModelSerializer):
    class Meta:
        model = Car
        fields = '__all__'
```

---

### **Step 7: Create Views for CRUD Operations**  

Modify `cars/views.py`:

```python
from rest_framework import viewsets
from .models import Car
from .serializers import CarSerializer

class CarViewSet(viewsets.ModelViewSet):
    queryset = Car.objects.all()
    serializer_class = CarSerializer
```

---

### **Step 8: Define API URLs**  

Create `cars/urls.py` and add:

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import CarViewSet

router = DefaultRouter()
router.register(r'cars', CarViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```

Also, modify **`CloudQuickLabs/urls.py`**:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('cars.urls')),  # Include the API
]
```

---

### **Step 9: Run the Server**  

Start the Django development server:

```sh
python manage.py runserver
```

Visit **http://127.0.0.1:8000/api/cars/** to test the API.

---

### **Step 10: Test CRUD Operations**  

You can test the API using **Postman** or Django's built-in API browser.

- **Create a Car (POST)**:  
  ```json
  {
    "name": "BMW",
    "brand": "BMW",
    "model": "X5"
  }
  ```
- **Get All Cars (GET)**  
- **Get a Specific Car (GET `/api/cars/1/`)**  
- **Update a Car (PATCH `/api/cars/1/`)**  
- **Delete a Car (DELETE `/api/cars/1/`)**  

---

## **Conclusion**  

We have built a **fully functional CRUD API** using Django Rest Framework. 🎉

✅ Set up Django and DRF  
✅ Defined models, serializers, views, and URLs  
✅ Implemented CRUD operations  
✅ Successfully tested the API  

Hope this helps! Let me know if you have any questions. 🚀
