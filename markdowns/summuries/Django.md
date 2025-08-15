
# 🌐 Django Project Setup & Development Guide
# Django Project Setup 

## 🧱 Environment Setup

```bash
python.exe -m pip install --upgrade pip # upgrade pip
pip install virtualenv # install virtualenv 
virtualenv env  # Create virtual environment
env\Scripts\activate  # Activate environment
pip install django  # Install Django in venv
```

## 📁 Project Initialization

```bash
django-admin startproject projectname # making django project 
python manage.py migrate # migrate
code .  # Open with VSCode
python manage.py createsuperuser  # Create admin
python manage.py runserver  # Start development server
```

## 📦 Requirements

```bash
pip freeze > requirements.txt  # Save dependencies
pip install -r requirements.txt  # Install dependencies
```
# ⚙️ App Creation

```bash
python manage.py startapp appname
```

### 🔌 Connect App to Project

-   In `project/settings.py`, add:
```python
'appname.apps.AppnameConfig'
# Example: 'test.apps.TestConfig'
```
-   Create `urls.py` inside your app folder:
    

```python
from django.urls import path, include
urlpatterns = [
    path('appname/', include('appname.urls')),
]
```
# 📄 Views

```python
from django.http import HttpResponse

def example(request):
    return HttpResponse('hello world')
```

### In app's `urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.example, name='appname'),
]
```

## Using Templates:

```python
from django.shortcuts import render

def product(request):
    return render(request, 'products/product.html')
```

### Passing Data to Templates:

```python
def product(request):
    return render(request, 'products/product.html', {'name': 'Ali'})
```

In HTML:

```python
{{ name }}
```
# 🗂️ Template Configuration

1.  Create a `templates` folder.
    
2.  In `settings.py`:
    

```python
import os
'DIRS': [os.path.join(BASE_DIR, 'templates')],
```

## Sample Template Usage

```python
{% extends 'base.html' %}
{% block content %}HTML content{% endblock content %}
{% include 'example.html' %}
{% if condition %}YES{% else %}NO{% endif %}
{% for item in list %}{{ item }}{% endfor %}
```

### Static Files ( import css or js or picture )

```python
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<a href="{% url 'about' %}">About</a>
```
## 🎨 Static Files Setup
1.  Create `static` folder and subfolders like `css`, `js`.
2.  In `settings.py`:
```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'projectname/static')
]
```
3.  Run the command:
```bash
python manage.py collectstatic
```
# 🧩 Models
## Create model

1. Create your class
```python
class Product(models.Model):
    name = models.CharField(max_length=50)
    content = models.TextField()
    price = models.DecimalField(max_digits=6, decimal_places=3)
    image = models.ImageField(upload_to='photos/%y/%m/%d')
    active = models.BooleanField(default=True)
    guest = models.ForeignKey(Guest, related_name='reservation', on_delete=models.CASCADE) # foreign key
```
2. Run the command
```bash
python manage.py makemigrations
python manage.py migrate
```

## 👤 Integration into Admin panel
on app/admin
```python
from .models import Product
admin.site.register(Product)
```
## 👁️ Views: Display & Querying
you cab use it in the next way :
```python
from .models import Product

def products(request):
    return render(request, 'products/products.html', {'pro': Product.objects.all()})
```
and you can use the next filters
```python
# Filters
Product.objects.get(price=50)
Product.objects.filter(price=50)
Product.objects.exclude(price=50)
Product.objects.order_by("price")
str(Product.objects.count())
```


## 🧾 Templates & Rendering Data

```python
{% for i in pro %}
    <h2>{{ i.name }}</h2>
{% endfor %}
```
## 👁️ Display & Querying in Views

```python
from .models import Product

def products(request):
    return render(request, 'products/products.html', {'pro': Product.objects.all()})

# Filters
Product.objects.get(price=50)
Product.objects.filter(price=50)
Product.objects.exclude(price=50)
Product.objects.order_by("price")
str(Product.objects.count())
```


## 🧾 Templates & Rendering Data

```python
{% for i in pro %}
    <h2>{{ i.name }}</h2>
{% endfor %}
```


## 📌 Model Customization

this function to select what can u display in admin page
```python
def __str__(self):
    return self.name
```

```python
class Meta:
    verbose_name = 'ModelName' # to change model (or class) name 	
    ordering = ['name'] # to order objects with name 
```

### Field customization 

```python
category = models.CharField(blank=True, null=True)
choices = [
    ('phone', 'phone'),
    ('computer', 'computer'),
]
category = models.CharField(choices=choices)
```

# 🖼️ Media (Files & Images)

### 📌 Settings Configuration

Add the following in your `settings.py`:

```python
MEDIA_URL = 'media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

### 📌 URL Configuration

Update `urls.py` of your project:

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

# 🧾 Forms & Requests
you can directly use 
```python
def example(request):
	x = request.POST.get('username')
	y = request.POST.get('password')
	data = user(username=x,username=y)
	data.save()
	return render(request,'pages/signin.html') 
```
or using Form
## using Form
### Creating a Form
 create file `forms.py` :
```python
from django import forms
from .models import Product

class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = '__all__'
```

### Views with Form Handling

```python
from .forms import ProductForm

def add_product(request):
    form = ProductForm()
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
    return render(request, 'products/add_product.html', {'form': form})
```

### Updating and Deleting

```python
def update_product(request, id):
    product = Product.objects.get(id=id)
    form = ProductForm(instance=product)
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES, instance=product)
        if form.is_valid():
            form.save()
    return render(request, 'products/update_product.html', {'form': form})

def delete_product(request, id):
    product = Product.objects.get(id=id)
    product.delete()
    return redirect('products')
``` 

# ⚙️ APIs ( Application Programming Interface )

## 🟠 Method 1: Basic JSON Response 
```python
from django.http import JsonResponse

def no_rest_no_model(request):
    guests = [
        {'id': 1, 'name': 'omar', 'mobile': 666666},
        {'id': 2, 'name': 'samy', 'mobile': 555555},
    ]
    return JsonResponse(guests, safe=False)
```
## 🟡 Method 2: JSON Response Using Django Models
```python
from django.http import JsonResponse
from .models import Guest

def no_rest_from_model(request):
    data = Guest.objects.all()
    response = {
        'guest': list(data.values('name', 'mobile'))
    }
    return JsonResponse(response)
```
## 🟢 Method 3: Using Django REST Framework

# ⚙️ REST Framework (DRF)

## 📦 Installation

To use the Django REST Framework (DRF), install it via pip:

```bash
pip install djangorestframework
```

Then, add it to your `INSTALLED_APPS` in `settings.py`:

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```
## 🧰 Serializers in DRF

Serializers allow complex data types such as querysets and model instances to be converted to native Python datatypes that can then be easily rendered into JSON.

### 🔹 Basic Serializer Example

Create a `serializers.py` file in your app folder (e.g., `tickets/serializers.py`):

```python
from rest_framework import serializers
from tickets.models import Guest, Movie, Reservation

class MovieSerializer(serializers.ModelSerializer):
    class Meta:
        model = Movie
        fields = '__all__'
```


### 🔸 Advanced Serializer with Extra Fields

DRF allows adding computed or related fields and marking them as read/write only.

```python
class MovieSerializer(serializers.ModelSerializer):
    # Example extras:
    # test = serializers.IntegerField(default=1)
    # courses = CoursesSerializer(many=True, read_only=True, source='fk_related_name')
    # instructor = serializers.SerializerMethodField()

    class Meta:
        model = Movie
        fields = '__all__'

    # def get_instructor(self, instance):
    #     return instance.instructor.username
```

### 🔐 Read-Only / Write-Only Fields

You can restrict fields in your serializers using `extra_kwargs`:

```python
class AdminSerializer(serializers.ModelSerializer):
    class Meta:
        model = Admin
        fields = ['id', 'username', 'email', 'password', 'role', 'is_staff']
        extra_kwargs = {
            'password': {'write_only': True},
            'role': {'read_only': True},
            'is_staff': {'write_only': True},
        }
```

## 🧭 Based Views (BV) in DRF

### 🧪 Function-Based Views (FBV)
```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from .models import Guest
from .serializers import GuestSerializer
```

#### 📌 GET and POST — List & Create

```python
# URL: path('FBV_List/', views.FBV_List),
@api_view(['GET', 'POST'])
def FBV_List(request):
    if request.method == 'GET':
        guests = Guest.objects.all()
        serializer = GuestSerializer(guests, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = GuestSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

#### 📌 GET, PUT, DELETE — Single Item

```python
# URL: path('FBV_pk/<int:pk>/', views.FBV_pk),
@api_view(['GET', 'PUT', 'DELETE'])
def FBV_pk(request, pk):
    try:
        guest = Guest.objects.get(pk=pk)
    except Guest.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'GET':
        serializer = GuestSerializer(guest)
        return Response(serializer.data)

    elif request.method == 'PUT':
        serializer = GuestSerializer(guest, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_202_ACCEPTED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        guest.delete()
        return Response(status=status.HTTP_200_OK)
```

### 🧱 Class-Based Views (CBV)


```python

from rest_framework.views import APIView
from django.http import Http404
```
#### 📌 GET and POST — List & Create
```python
# URL: path('CBV_List/', views.CBV_List.as_view()),

class CBV_List(APIView):
    def get(self, request):
        guests = Guest.objects.all()
        serializer = GuestSerializer(guests, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = GuestSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

#### 📌 GET, PUT, DELETE — Single Item

```python
# URL: path('CBV_pk/<int:pk>/', views.CBV_pk.as_view()),
class CBV_pk(APIView):
    def get_object(self, pk):
        try:
            return Guest.objects.get(pk=pk)
        except Guest.DoesNotExist:
            raise Http404

    def get(self, request, pk):
        guest = self.get_object(pk)
        serializer = GuestSerializer(guest)
        return Response(serializer.data)

    def put(self, request, pk):
        guest = self.get_object(pk)
        serializer = GuestSerializer(guest, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_202_ACCEPTED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def delete(self, request, pk):
        guest = self.get_object(pk)
        guest.delete()
        return Response(status=status.HTTP_200_OK)
```
## ⚒️ Mixins
we have this:
```python
mixins.ListModelMixin # get all objects 
mixins.CreateModelMixin # create new object 
mixins.RetrieveModelMixin  # get 1 object 
mixins.UpdateModelMixin # update 1 object 
mixins.DestroyModelMixin # delete 1 object 
```

```python
from rest_framework import mixins, generics
from .models import Guest
from .serializers import GuestSerializer
```

### 📌 GET and POST — List & Create

```python
# URL path('mixins_list/', views.mixins_list.as_view()),
class mixins_list(mixins.ListModelMixin,
                  mixins.CreateModelMixin,
                  generics.GenericAPIView):
    queryset = Guest.objects.all()
    serializer_class = GuestSerializer

    def get(self, request):
        return self.list(request)

    def post(self, request):
        return self.create(request)
```

### 📌 GET, PUT, DELETE — Single Item

```python
# URL: path('mixins_pk/<int:pk>/', views.mixins_pk.as_view()),
class mixins_pk(mixins.RetrieveModelMixin,
                mixins.UpdateModelMixin,
                mixins.DestroyModelMixin,
                generics.GenericAPIView):
    queryset = Guest.objects.all()
    serializer_class = GuestSerializer

    def get(self, request, pk):
        return self.retrieve(request)

    def put(self, request, pk):
        return self.update(request)

    def delete(self, request, pk):
        return self.destroy(request)
```


## 🧬 Generics

we have this:
```python
generics.ListAPIView # List 
generics.ListCreateAPIView # List + create 
generics.RetrieveUpdateDestroyAPIView #Retrieve + Update + Destroy
```
```python
from rest_framework import generics
from .models import Guest
from .serializers import GuestSerializer
```

### 📌 GET and POST — List & Create

```python
# URL: path('generics_list/', views.generics_list.as_view()),
class generics_list(generics.ListCreateAPIView):
    queryset = Guest.objects.all()
    serializer_class = GuestSerializer
```

### 📌 GET, PUT, DELETE — Single Item

```python
# URL: path('generics_pk/<int:pk>/', views.generics_pk.as_view())
class generics_pk(generics.RetrieveUpdateDestroyAPIView):
    queryset = Guest.objects.all()
    serializer_class = GuestSerializer
```
## 🔄 ViewSets & Routers
### 📌 CRUD in One Class

```python
from rest_framework import viewsets
class viewsets_guest(viewsets.ModelViewSet):
    queryset = Guest.objects.all()
    serializer_class = GuestSerializer
```
and on `urls.py` 
```python
# URL
from rest_framework.routers import DefaultRouter
router = DefaultRouter()
router.register('guests', views.viewsets_guest)

urlpatterns = [
    ...
    path('viewsets/', include(router.urls)),
]
```
## 🎯 Examples
### 🔍 Find Movie by Query Param

```python
@api_view(['GET'])
def find_movie(request):
    mov = Movie.objects.all()
    movies = mov.filter(
        movie=request.query_params.get('movie'),
    )
    serializer = MovieSerializer(movies, many=True)
    return Response(serializer.data)
```

### 📝 Create Reservation

```python
@api_view(['POST'])
def reserve(request):
    movie = Movie.objects.get(
        movie=request.query_params.get('movie'),
        hall=request.query_params.get('hall')
    )
    guest = Guest.objects.get(
        pk=request.query_params.get('pk')
    )
    reservation = Reservation()
    reservation.guest = guest
    reservation.movie = movie
    reservation.save()
    return Response(status=status.HTTP_201_CREATED)
```


# 🔐 Token Authentication and Permissions
### 📌 Setup Token Auth

Add token authentication in `settings.py`:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
    # Optional global permissions
    # 'DEFAULT_PERMISSION_CLASSES': [
    #     'rest_framework.permissions.IsAuthenticated',
    # ]
}
```

### 📌 Token Generation via URL

In your `urls.py`, include:

```python
from rest_framework.authtoken.views import obtain_auth_token

urlpatterns = [
    ...
    path('api-token/', obtain_auth_token),
]
```

You can now POST a username and password to `/api-token/` and receive a token in response.

### 📌 Token Auth in a View

```python
from rest_framework.authentication import TokenAuthentication
from rest_framework.permissions import IsAuthenticated

class ExampleView(APIView):
	...
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]
```

### 📌 Use Token in Requests

Send the token in headers:

```
Authorization: Token 72a9a9276f0d3a907586a832a2bbc0d3216eba89
```

### 📌 Automatically Create Token on User Creation

In `models.py` or a `signals.py` file:

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from rest_framework.authtoken.models import Token
from django.conf import settings

@receiver(post_save, sender=settings.AUTH_USER_MODEL)
def TokenCreate(sender, instance, created, **kwargs):
    if created:
        Token.objects.create(user=instance)
```
## 🔑 Permissions

### 📌 Built-in Permissions
> Note
if u search on google "django rest framework github" and open the first result , then on 
"/rest_framework/permissions.py" u can get the code of default permission (AllowAny // IsAuthenticated // IsAdminuser // IsAuthenticatedReadOnly...)

DRF provides several built-in permission classes:

-   `AllowAny`: No restriction
    
-   `IsAuthenticated`: Only authenticated users
    
-   `IsAdminUser`: Only admin users
    
-   `IsAuthenticatedOrReadOnly`: Read for everyone, write for authenticated users
    

You can apply one or more permission classes to individual views or globally via `settings.py`.


### 📌 Apply to a View

```python
from rest_framework.permissions import IsAuthenticated

class ProtectedView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"message": "Authenticated access only!"})
```

### 📌 Custom Permissions Example

Create a `permissions.py` file in your app:

```python
from rest_framework import permissions

class IsAuthorOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:
            return True
        return obj.author == request.user
```

Then apply it in your view:

```python
from .permissions import IsAuthorOrReadOnly

class ExampleDetailView(APIView):
	...
    permission_classes = [IsAuthorOrReadOnly]
```

# 👤 Custom Users in Django
## 📌 Step 1: Create a New App and Update Settings

In `settings.py`, set the custom user model:

```python
AUTH_USER_MODEL = 'appname.User'
```

## 📌 Step 2: Define User Model in Your App
In `models.py`
```python
from typing import Iterable, Optional
from django.db import models
from django.contrib.auth.models import AbstractUser, BaseUserManager

class User(AbstractUser):
    class Role(models.TextChoices):
        ADMIN = 'ADMIN', 'Admin'
        STUDENT = 'STUDENT', 'Student'
        TEACHER = 'TEACHER', 'Teacher'

    base_role = Role.ADMIN
    role = models.CharField(max_length=50, choices=Role.choices, blank=True)

    def save(self, *args, **kwargs):
        if not self.role:
            self.role = self.base_role
        super().save(*args, **kwargs)
...
```

## 📌 Step 3: Create Proxy Model for Roles

```python
class StudentManager(BaseUserManager):
    def get_queryset(self, *args, **kwargs):
        result = super().get_queryset(*args, **kwargs)
        return result.filter(role=User.Role.STUDENT)

class Student(User):
    base_role = User.Role.STUDENT
    student = StudentManager()

    class Meta:
        proxy = True
```
### 📄 License
This cheat sheet is licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You may use and modify this freely, but please give credit. 🙏
![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4Nzk4MDU4NDJdfQ==
-->