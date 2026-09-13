# Django — Beginner to Advanced

> A complete Django learning guide from **Beginner → Intermediate → Advanced**, with full code examples and explanations in **English 🇬🇧 + Khmer 🇰🇭**.

---

## 📚 Table of Contents

* [1. What is Django?](#1-what-is-django)
* [2. Django Architecture](#2-django-architecture)
* [3. Requirements](#3-requirements)
* [4. Installation](#4-installation)
* [5. Create Your First Project](#5-create-your-first-project)
* [6. Create Your First App](#6-create-your-first-app)
* [7. Django Project Structure](#7-django-project-structure)
* [8. Settings](#8-settings)
* [9. URLs](#9-urls)
* [10. Views](#10-views)
* [11. Templates](#11-templates)
* [12. Static Files](#12-static-files)
* [13. Models](#13-models)
* [14. Migrations](#14-migrations)
* [15. Django ORM](#15-django-orm)
* [16. CRUD](#16-crud)
* [17. Django Admin](#17-django-admin)
* [18. Forms](#18-forms)
* [19. ModelForms](#19-modelforms)
* [20. Validation](#20-validation)
* [21. Authentication](#21-authentication)
* [22. Authorization](#22-authorization)
* [23. Sessions](#23-sessions)
* [24. Messages](#24-messages)
* [25. Class-Based Views](#25-class-based-views)
* [26. Generic Views](#26-generic-views)
* [27. Relationships](#27-relationships)
* [28. File Uploads](#28-file-uploads)
* [29. Pagination](#29-pagination)
* [30. Search](#30-search)
* [31. Transactions](#31-transactions)
* [32. Custom Management Commands](#32-custom-management-commands)
* [33. Middleware](#33-middleware)
* [34. Signals](#34-signals)
* [35. Custom User Model](#35-custom-user-model)
* [36. Email](#36-email)
* [37. JSON API](#37-json-api)
* [38. Async Views](#38-async-views)
* [39. Caching](#39-caching)
* [40. Testing](#40-testing)
* [41. Security](#41-security)
* [42. Environment Variables](#42-environment-variables)
* [43. PostgreSQL](#43-postgresql)
* [44. Production Settings](#44-production-settings)
* [45. Static Files in Production](#45-static-files-in-production)
* [46. Deployment](#46-deployment)
* [47. Performance Optimization](#47-performance-optimization)
* [48. Project Structure for Large Applications](#48-project-structure-for-large-applications)
* [49. Useful Django Commands](#49-useful-django-commands)
* [50. Learning Roadmap](#50-learning-roadmap)

---

# 1. What is Django?

## English

Django is a high-level Python web framework used to build secure, scalable, and maintainable web applications.

Django provides many features out of the box:

* URL routing
* Views
* Templates
* ORM
* Database migrations
* Authentication
* Authorization
* Admin panel
* Forms
* Sessions
* Security protection
* Middleware
* Testing
* Caching
* Email
* File uploads
* Async support

## Khmer

Django គឺជា **Web Framework សម្រាប់ Python** ដែលប្រើសម្រាប់បង្កើត Web Application។

Django មានមុខងារសំខាន់ៗជាច្រើនស្រាប់៖

* URL Routing
* Views
* Templates
* Database ORM
* Migration
* Login / Logout
* User Authentication
* Admin Panel
* Forms
* Sessions
* Security
* Middleware
* Testing
* Caching
* Email
* File Upload
* Async

---

# 2. Django Architecture

Django commonly follows the **MTV architecture**:

```text
User
  |
  v
URL
  |
  v
View
  |
  +------> Model ------> Database
  |
  v
Template
  |
  v
HTML Response
  |
  v
User
```

## M — Model

Responsible for database structure and data.

```python
from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)
```

## T — Template

Responsible for presentation.

```html
<h1>{{ product.name }}</h1>
<p>${{ product.price }}</p>
```

## V — View

Responsible for application logic.

```python
from django.shortcuts import render
from .models import Product


def product_detail(request, product_id):
    product = Product.objects.get(id=product_id)

    return render(
        request,
        "products/detail.html",
        {"product": product},
    )
```

## Khmer

* **Model** = គ្រប់គ្រង Database
* **Template** = បង្ហាញ HTML
* **View** = គ្រប់គ្រង Logic
* **URL** = កំណត់ថា URL ណាទៅ View ណា

---

# 3. Requirements

Recommended:

```text
Python 3.12+
Django 6.0
Git
VS Code
PostgreSQL
```

Django 6.0 officially supports Python 3.12, 3.13 and 3.14.

Check Python:

```bash
python --version
```

Check Git:

```bash
git --version
```

---

# 4. Installation

## Windows

Create a project folder:

```powershell
mkdir django-project
cd django-project
```

Create virtual environment:

```powershell
python -m venv venv
```

Activate it:

```powershell
venv\Scripts\activate
```

Install Django:

```powershell
python -m pip install --upgrade pip
pip install django
```

Verify:

```powershell
python -m django --version
```

---

## Linux / macOS

```bash
mkdir django-project
cd django-project

python3 -m venv venv

source venv/bin/activate

python -m pip install --upgrade pip

pip install django

python -m django --version
```

---

# 5. Create Your First Project

Create project:

```bash
django-admin startproject config .
```

Run server:

```bash
python manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/
```

Stop server:

```text
CTRL + C
```

---

# 6. Create Your First App

Create an app:

```bash
python manage.py startapp products
```

Project:

```text
django-project/
│
├── manage.py
│
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
└── products/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    ├── views.py
    └── migrations/
```

Add the app to:

```python
# config/settings.py

INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",

    "products",
]
```

---

# 7. Django Project Structure

Recommended beginner structure:

```text
project/
│
├── manage.py
│
├── config/
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── products/
│   ├── migrations/
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── forms.py
│   ├── urls.py
│   └── tests.py
│
├── templates/
│   ├── base.html
│   └── products/
│       ├── list.html
│       ├── detail.html
│       ├── create.html
│       └── update.html
│
├── static/
│   ├── css/
│   └── js/
│
├── media/
│
├── requirements.txt
└── .env
```

---

# 8. Settings

Important settings:

```python
# config/settings.py

from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = "change-this-in-production"

DEBUG = True

ALLOWED_HOSTS = []

INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",

    "products",
]

MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]

ROOT_URLCONF = "config.urls"

TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [BASE_DIR / "templates"],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]

WSGI_APPLICATION = "config.wsgi.application"

DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}

LANGUAGE_CODE = "en-us"

TIME_ZONE = "Asia/Phnom_Penh"

USE_I18N = True

USE_TZ = True

STATIC_URL = "static/"

STATICFILES_DIRS = [
    BASE_DIR / "static",
]

DEFAULT_AUTO_FIELD = "django.db.models.BigAutoField"
```

## Khmer

`settings.py` គឺជា file សម្រាប់កំណត់ Configuration របស់ Django Project។

ឧទាហរណ៍៖

* Database
* Installed Apps
* Middleware
* Templates
* Static Files
* Time Zone
* Security Settings

---

# 9. URLs

## Main URL

```python
# config/urls.py

from django.contrib import admin
from django.urls import include, path


urlpatterns = [
    path("admin/", admin.site.urls),
    path("products/", include("products.urls")),
]
```

Create:

```python
# products/urls.py

from django.urls import path

from . import views


urlpatterns = [
    path("", views.product_list, name="product-list"),
    path("<int:product_id>/", views.product_detail, name="product-detail"),
]
```

## Khmer

URL គឺជាផ្លូវដែលភ្ជាប់ Browser ទៅ View។

ឧទាហរណ៍៖

```text
/products/
/products/1/
/products/2/
```

---

# 10. Views

Create a model first:

```python
# products/models.py

from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.name
```

Create migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

View:

```python
# products/views.py

from django.shortcuts import get_object_or_404, render

from .models import Product


def product_list(request):
    products = Product.objects.all()

    return render(
        request,
        "products/list.html",
        {"products": products},
    )


def product_detail(request, product_id):
    product = get_object_or_404(
        Product,
        id=product_id,
    )

    return render(
        request,
        "products/detail.html",
        {"product": product},
    )
```

---

# 11. Templates

Create:

```text
templates/
└── products/
    ├── list.html
    └── detail.html
```

## list.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Products</title>
</head>
<body>

<h1>Products</h1>

{% for product in products %}

    <div>
        <h2>
            <a href="{% url 'product-detail' product.id %}">
                {{ product.name }}
            </a>
        </h2>

        <p>
            ${{ product.price }}
        </p>
    </div>

{% empty %}

    <p>No products found.</p>

{% endfor %}

</body>
</html>
```

## detail.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{{ product.name }}</title>
</head>
<body>

<h1>{{ product.name }}</h1>

<p>Price: ${{ product.price }}</p>

<a href="{% url 'product-list' %}">
    Back
</a>

</body>
</html>
```

## Khmer

Template គឺជា HTML ដែល Django ប្រើសម្រាប់បង្ហាញ Data ពី View។

ឧទាហរណ៍៖

```django
{{ product.name }}
```

មានន័យថា បង្ហាញ `name` របស់ Product។

---

# 12. Static Files

Create:

```text
static/
└── css/
    └── style.css
```

CSS:

```css
body {
    font-family: Arial, sans-serif;
    margin: 40px;
}

h1 {
    margin-bottom: 20px;
}

.product {
    padding: 20px;
    border: 1px solid #ddd;
    margin-bottom: 10px;
}
```

Template:

```html
{% load static %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">

    <link
        rel="stylesheet"
        href="{% static 'css/style.css' %}"
    >

    <title>Products</title>
</head>
<body>

<h1>Products</h1>

</body>
</html>
```

---

# 13. Models

A model represents database data.

```python
from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=100)

    description = models.TextField(
        blank=True,
    )

    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
    )

    quantity = models.PositiveIntegerField(
        default=0,
    )

    created_at = models.DateTimeField(
        auto_now_add=True,
    )

    updated_at = models.DateTimeField(
        auto_now=True,
    )

    def __str__(self):
        return self.name
```

Common field types:

```python
models.CharField()
models.TextField()
models.IntegerField()
models.PositiveIntegerField()
models.DecimalField()
models.FloatField()
models.BooleanField()
models.DateField()
models.DateTimeField()
models.EmailField()
models.URLField()
models.FileField()
models.ImageField()
models.ForeignKey()
models.OneToOneField()
models.ManyToManyField()
```

---

# 14. Migrations

After changing models:

```bash
python manage.py makemigrations
```

Apply migrations:

```bash
python manage.py migrate
```

See migrations:

```bash
python manage.py showmigrations
```

Migration workflow:

```text
models.py
   |
   v
makemigrations
   |
   v
migration files
   |
   v
migrate
   |
   v
Database
```

## Khmer

Migration គឺជាវិធីដែល Django ប្រើដើម្បីបម្លែង Model ក្នុង Python ទៅ Database Schema។

---

# 15. Django ORM

Django ORM lets you work with databases using Python.

## Create

```python
product = Product.objects.create(
    name="Laptop",
    price=999.99,
    quantity=10,
)
```

## Get all

```python
products = Product.objects.all()
```

## Get one

```python
product = Product.objects.get(id=1)
```

## Filter

```python
products = Product.objects.filter(
    price__gte=500
)
```

## Exclude

```python
products = Product.objects.exclude(
    quantity=0
)
```

## Order

```python
products = Product.objects.order_by("-price")
```

## First

```python
product = Product.objects.first()
```

## Last

```python
product = Product.objects.last()
```

## Count

```python
total = Product.objects.count()
```

## Update

```python
Product.objects.filter(
    id=1
).update(
    price=1200
)
```

## Delete

```python
Product.objects.filter(
    id=1
).delete()
```

---

# 16. CRUD

CRUD means:

```text
C = Create
R = Read
U = Update
D = Delete
```

## Create

```python
product = Product.objects.create(
    name="Keyboard",
    price=50,
    quantity=20,
)
```

## Read

```python
products = Product.objects.all()
```

## Update

```python
product = Product.objects.get(id=1)

product.price = 60

product.save()
```

## Delete

```python
product = Product.objects.get(id=1)

product.delete()
```

---

# 17. Django Admin

Create admin user:

```bash
python manage.py createsuperuser
```

Run server:

```bash
python manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/admin/
```

Register model:

```python
# products/admin.py

from django.contrib import admin

from .models import Product


@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    list_display = (
        "id",
        "name",
        "price",
        "quantity",
        "created_at",
    )

    search_fields = (
        "name",
        "description",
    )

    list_filter = (
        "created_at",
    )
```

Django's admin can automatically provide an interface for authenticated users to create, edit, and delete model data.

---

# 18. Forms

Create:

```python
# products/forms.py

from django import forms


class ProductForm(forms.Form):
    name = forms.CharField(
        max_length=100,
    )

    price = forms.DecimalField(
        max_digits=10,
        decimal_places=2,
    )

    quantity = forms.IntegerField(
        min_value=0,
    )
```

View:

```python
from django.shortcuts import redirect, render

from .forms import ProductForm
from .models import Product


def product_create(request):

    if request.method == "POST":

        form = ProductForm(request.POST)

        if form.is_valid():

            Product.objects.create(
                name=form.cleaned_data["name"],
                price=form.cleaned_data["price"],
                quantity=form.cleaned_data["quantity"],
            )

            return redirect("product-list")

    else:

        form = ProductForm()

    return render(
        request,
        "products/create.html",
        {"form": form},
    )
```

Template:

```html
<h1>Create Product</h1>

<form method="post">

    {% csrf_token %}

    {{ form.as_p }}

    <button type="submit">
        Create
    </button>

</form>
```

---

# 19. ModelForms

For database models, `ModelForm` is often more convenient.

```python
# products/forms.py

from django import forms

from .models import Product


class ProductForm(forms.ModelForm):

    class Meta:
        model = Product

        fields = [
            "name",
            "description",
            "price",
            "quantity",
        ]
```

Create view:

```python
from django.shortcuts import redirect, render

from .forms import ProductForm


def product_create(request):

    if request.method == "POST":

        form = ProductForm(request.POST)

        if form.is_valid():

            form.save()

            return redirect("product-list")

    else:

        form = ProductForm()

    return render(
        request,
        "products/create.html",
        {"form": form},
    )
```

---

# 20. Validation

Forms can validate user input.

```python
from django import forms


class ProductForm(forms.ModelForm):

    class Meta:
        model = Product
        fields = [
            "name",
            "description",
            "price",
            "quantity",
        ]

    def clean_price(self):

        price = self.cleaned_data["price"]

        if price < 0:
            raise forms.ValidationError(
                "Price cannot be negative."
            )

        return price
```

## Model validation

```python
from django.core.validators import MinValueValidator


class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
        validators=[
            MinValueValidator(0),
        ],
    )
```

---

# 21. Authentication

Django includes authentication functionality.

Create a login URL:

```python
# config/urls.py

from django.contrib import admin
from django.contrib.auth import views as auth_views
from django.urls import path


urlpatterns = [

    path(
        "admin/",
        admin.site.urls,
    ),

    path(
        "login/",
        auth_views.LoginView.as_view(
            template_name="registration/login.html"
        ),
        name="login",
    ),

    path(
        "logout/",
        auth_views.LogoutView.as_view(),
        name="logout",
    ),
]
```

Create:

```text
templates/
└── registration/
    └── login.html
```

```html
<h1>Login</h1>

<form method="post">

    {% csrf_token %}

    {{ form.as_p }}

    <button type="submit">
        Login
    </button>

</form>
```

Configure:

```python
# settings.py

LOGIN_REDIRECT_URL = "/"
LOGOUT_REDIRECT_URL = "/"
```

---

# 22. Authorization

Protect a view:

```python
from django.contrib.auth.decorators import login_required
from django.shortcuts import render


@login_required
def dashboard(request):

    return render(
        request,
        "dashboard.html",
    )
```

Class-based view:

```python
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import TemplateView


class DashboardView(
    LoginRequiredMixin,
    TemplateView,
):

    template_name = "dashboard.html"
```

---

# 23. Sessions

Store data:

```python
def save_cart(request):

    request.session["cart"] = [
        1,
        2,
        3,
    ]
```

Read:

```python
def get_cart(request):

    cart = request.session.get(
        "cart",
        [],
    )

    return cart
```

Delete:

```python
request.session.pop(
    "cart",
    None,
)
```

Clear:

```python
request.session.flush()
```

---

# 24. Messages

View:

```python
from django.contrib import messages
from django.shortcuts import redirect


def create_product(request):

    # create product

    messages.success(
        request,
        "Product created successfully.",
    )

    return redirect("product-list")
```

Template:

```html
{% if messages %}

    {% for message in messages %}

        <div>
            {{ message }}
        </div>

    {% endfor %}

{% endif %}
```

Message types:

```python
messages.debug()
messages.info()
messages.success()
messages.warning()
messages.error()
```

---

# 25. Class-Based Views

Function-based view:

```python
def product_list(request):

    products = Product.objects.all()

    return render(
        request,
        "products/list.html",
        {"products": products},
    )
```

Class-based view:

```python
from django.views import View
from django.shortcuts import render

from .models import Product


class ProductListView(View):

    def get(self, request):

        products = Product.objects.all()

        return render(
            request,
            "products/list.html",
            {"products": products},
        )
```

URL:

```python
from django.urls import path

from .views import ProductListView


urlpatterns = [
    path(
        "",
        ProductListView.as_view(),
        name="product-list",
    ),
]
```

---

# 26. Generic Views

Django provides generic views to reduce repetitive code.

## ListView

```python
from django.views.generic import ListView

from .models import Product


class ProductListView(ListView):

    model = Product

    template_name = "products/list.html"

    context_object_name = "products"
```

URL:

```python
path(
    "",
    ProductListView.as_view(),
    name="product-list",
)
```

## DetailView

```python
from django.views.generic import DetailView

from .models import Product


class ProductDetailView(DetailView):

    model = Product

    template_name = "products/detail.html"

    context_object_name = "product"
```

## CreateView

```python
from django.urls import reverse_lazy
from django.views.generic import CreateView

from .models import Product


class ProductCreateView(CreateView):

    model = Product

    fields = [
        "name",
        "description",
        "price",
        "quantity",
    ]

    template_name = "products/create.html"

    success_url = reverse_lazy(
        "product-list"
    )
```

## UpdateView

```python
from django.urls import reverse_lazy
from django.views.generic import UpdateView

from .models import Product


class ProductUpdateView(UpdateView):

    model = Product

    fields = [
        "name",
        "description",
        "price",
        "quantity",
    ]

    template_name = "products/update.html"

    success_url = reverse_lazy(
        "product-list"
    )
```

## DeleteView

```python
from django.urls import reverse_lazy
from django.views.generic import DeleteView

from .models import Product


class ProductDeleteView(DeleteView):

    model = Product

    template_name = "products/delete.html"

    success_url = reverse_lazy(
        "product-list"
    )
```

---

# 27. Relationships

## One-to-Many

Example:

```python
class Category(models.Model):

    name = models.CharField(
        max_length=100,
    )

    def __str__(self):
        return self.name


class Product(models.Model):

    category = models.ForeignKey(
        Category,
        on_delete=models.CASCADE,
        related_name="products",
    )

    name = models.CharField(
        max_length=100,
    )

    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
    )
```

Query:

```python
category = Category.objects.get(id=1)

products = category.products.all()
```

---

## One-to-One

```python
class UserProfile(models.Model):

    user = models.OneToOneField(
        "auth.User",
        on_delete=models.CASCADE,
    )

    phone = models.CharField(
        max_length=30,
        blank=True,
    )
```

---

## Many-to-Many

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )


class Tag(models.Model):

    name = models.CharField(
        max_length=100,
    )


class ProductTag(models.Model):

    product = models.ForeignKey(
        Product,
        on_delete=models.CASCADE,
    )

    tag = models.ForeignKey(
        Tag,
        on_delete=models.CASCADE,
    )
```

Or use Django's built-in ManyToManyField:

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    tags = models.ManyToManyField(
        Tag,
        blank=True,
    )
```

---

# 28. File Uploads

Model:

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    image = models.ImageField(
        upload_to="products/",
        blank=True,
        null=True,
    )
```

Install Pillow:

```bash
pip install pillow
```

Settings:

```python
MEDIA_URL = "/media/"

MEDIA_ROOT = BASE_DIR / "media"
```

URLs:

```python
from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path


urlpatterns = [
    path(
        "admin/",
        admin.site.urls,
    ),
]

urlpatterns += static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT,
)
```

Form:

```html
<form
    method="post"
    enctype="multipart/form-data"
>

    {% csrf_token %}

    {{ form.as_p }}

    <button type="submit">
        Upload
    </button>

</form>
```

View:

```python
def create_product(request):

    if request.method == "POST":

        form = ProductForm(
            request.POST,
            request.FILES,
        )

        if form.is_valid():

            form.save()

            return redirect(
                "product-list"
            )

    else:

        form = ProductForm()

    return render(
        request,
        "products/create.html",
        {"form": form},
    )
```

---

# 29. Pagination

View:

```python
from django.core.paginator import Paginator
from django.shortcuts import render

from .models import Product


def product_list(request):

    products = Product.objects.all()

    paginator = Paginator(
        products,
        10,
    )

    page_number = request.GET.get(
        "page"
    )

    page_obj = paginator.get_page(
        page_number
    )

    return render(
        request,
        "products/list.html",
        {
            "page_obj": page_obj,
        },
    )
```

Template:

```html
{% for product in page_obj %}

    <h2>{{ product.name }}</h2>

{% endfor %}

<div>

    {% if page_obj.has_previous %}

        <a
            href="?page={{ page_obj.previous_page_number }}"
        >
            Previous
        </a>

    {% endif %}

    <span>
        Page {{ page_obj.number }}
        of {{ page_obj.paginator.num_pages }}
    </span>

    {% if page_obj.has_next %}

        <a
            href="?page={{ page_obj.next_page_number }}"
        >
            Next
        </a>

    {% endif %}

</div>
```

---

# 30. Search

Search by product name:

```python
from django.db.models import Q
from django.shortcuts import render

from .models import Product


def product_list(request):

    query = request.GET.get(
        "q",
        "",
    )

    products = Product.objects.all()

    if query:

        products = products.filter(
            Q(name__icontains=query)
            | Q(description__icontains=query)
        )

    return render(
        request,
        "products/list.html",
        {
            "products": products,
            "query": query,
        },
    )
```

Template:

```html
<form method="get">

    <input
        type="search"
        name="q"
        value="{{ query }}"
        placeholder="Search products..."
    >

    <button type="submit">
        Search
    </button>

</form>
```

---

# 31. Transactions

Use transactions when multiple database operations must succeed together.

```python
from django.db import transaction


@transaction.atomic
def create_order():

    order = Order.objects.create(
        customer="Heng",
    )

    OrderItem.objects.create(
        order=order,
        product_id=1,
        quantity=2,
    )

    return order
```

If an exception occurs, Django rolls back the transaction.

Another approach:

```python
from django.db import transaction


def create_order():

    with transaction.atomic():

        order = Order.objects.create(
            customer="Heng",
        )

        OrderItem.objects.create(
            order=order,
            product_id=1,
            quantity=2,
        )

        return order
```

---

# 32. Custom Management Commands

Create:

```text
products/
└── management/
    ├── __init__.py
    └── commands/
        ├── __init__.py
        └── seed_products.py
```

Code:

```python
from django.core.management.base import BaseCommand

from products.models import Product


class Command(BaseCommand):

    help = "Create sample products"

    def handle(self, *args, **options):

        Product.objects.create(
            name="Laptop",
            price=999,
            quantity=10,
        )

        Product.objects.create(
            name="Keyboard",
            price=50,
            quantity=20,
        )

        self.stdout.write(
            self.style.SUCCESS(
                "Products created successfully."
            )
        )
```

Run:

```bash
python manage.py seed_products
```

---

# 33. Middleware

Middleware processes requests and responses.

Example:

```python
# products/middleware.py

import time


class RequestTimeMiddleware:

    def __init__(self, get_response):

        self.get_response = get_response

    def __call__(self, request):

        start = time.perf_counter()

        response = self.get_response(
            request
        )

        duration = (
            time.perf_counter() - start
        )

        print(
            f"Request took {duration:.4f}s"
        )

        return response
```

Add to settings:

```python
MIDDLEWARE = [

    "django.middleware.security.SecurityMiddleware",

    "django.contrib.sessions.middleware.SessionMiddleware",

    "django.middleware.common.CommonMiddleware",

    "django.middleware.csrf.CsrfViewMiddleware",

    "django.contrib.auth.middleware.AuthenticationMiddleware",

    "django.contrib.messages.middleware.MessageMiddleware",

    "django.middleware.clickjacking.XFrameOptionsMiddleware",

    "products.middleware.RequestTimeMiddleware",
]
```

## Khmer

Middleware គឺជាស្រទាប់ដែលអាចពិនិត្យ ឬកែប្រែ Request និង Response មុន/ក្រោយ View។

---

# 34. Signals

Signals allow one part of Django to notify another part that something happened.

Example:

```python
# products/signals.py

from django.db.models.signals import post_save
from django.dispatch import receiver

from .models import Product


@receiver(
    post_save,
    sender=Product,
)
def product_created(
    sender,
    instance,
    created,
    **kwargs,
):

    if created:

        print(
            f"Created product: {instance.name}"
        )
```

Import signals in:

```python
# products/apps.py

from django.apps import AppConfig


class ProductsConfig(AppConfig):

    default_auto_field = (
        "django.db.models.BigAutoField"
    )

    name = "products"

    def ready(self):

        from . import signals
```

---

# 35. Custom User Model

For new projects, decide early whether you need a custom user model.

Example:

```python
# accounts/models.py

from django.contrib.auth.models import AbstractUser


class User(AbstractUser):

    phone = models.CharField(
        max_length=30,
        blank=True,
    )
```

Import:

```python
from django.db import models
from django.contrib.auth.models import AbstractUser


class User(AbstractUser):

    phone = models.CharField(
        max_length=30,
        blank=True,
    )
```

Settings:

```python
AUTH_USER_MODEL = "accounts.User"
```

Use:

```python
from django.contrib.auth import get_user_model


User = get_user_model()
```

Do not hard-code:

```python
from django.contrib.auth.models import User
```

inside reusable application code when the project supports a custom user model.

---

# 36. Email

Development email backend:

```python
EMAIL_BACKEND = (
    "django.core.mail.backends.console.EmailBackend"
)
```

Send email:

```python
from django.core.mail import send_mail


send_mail(
    subject="Welcome",
    message="Welcome to our website.",
    from_email="noreply@example.com",
    recipient_list=[
        "user@example.com",
    ],
)
```

For production, configure a real SMTP provider.

---

# 37. JSON API

Django can return JSON without an additional API framework.

```python
from django.http import JsonResponse

from .models import Product


def product_api(request):

    products = Product.objects.all()

    data = [
        {
            "id": product.id,
            "name": product.name,
            "price": str(product.price),
        }
        for product in products
    ]

    return JsonResponse(
        {
            "products": data,
        }
    )
```

URL:

```python
path(
    "api/products/",
    views.product_api,
    name="product-api",
)
```

Response:

```json
{
    "products": [
        {
            "id": 1,
            "name": "Laptop",
            "price": "999.00"
        }
    ]
}
```

---

# 38. Async Views

Django supports asynchronous views.

```python
from django.http import JsonResponse


async def async_products(request):

    return JsonResponse(
        {
            "message": "Hello from async Django"
        }
    )
```

URL:

```python
path(
    "async/",
    views.async_products,
    name="async",
)
```

Async is useful when your application spends significant time waiting on asynchronous I/O.

Do not assume that simply changing every view to `async def` automatically makes database-heavy code faster.

---

# 39. Caching

Simple local-memory cache:

```python
CACHES = {
    "default": {
        "BACKEND": (
            "django.core.cache.backends.locmem.LocMemCache"
        ),
        "LOCATION": "unique-django-cache",
    }
}
```

Use cache:

```python
from django.core.cache import cache


cache.set(
    "product_count",
    100,
    timeout=300,
)
```

Get:

```python
count = cache.get(
    "product_count"
)
```

Delete:

```python
cache.delete(
    "product_count"
)
```

Cache a view:

```python
from django.views.decorators.cache import cache_page


@cache_page(60 * 5)
def product_list(request):

    ...
```

For production systems with multiple application instances, use an appropriate shared cache such as Redis.

---

# 40. Testing

Create tests:

```python
# products/tests.py

from django.test import TestCase
from django.urls import reverse

from .models import Product


class ProductModelTest(TestCase):

    def test_product_string(self):

        product = Product.objects.create(
            name="Laptop",
            price=999,
            quantity=10,
        )

        self.assertEqual(
            str(product),
            "Laptop",
        )


class ProductViewTest(TestCase):

    def test_product_list(self):

        Product.objects.create(
            name="Laptop",
            price=999,
            quantity=10,
        )

        response = self.client.get(
            reverse("product-list")
        )

        self.assertEqual(
            response.status_code,
            200,
        )

        self.assertContains(
            response,
            "Laptop",
        )
```

Run:

```bash
python manage.py test
```

Specific app:

```bash
python manage.py test products
```

---

# 41. Security

Django provides built-in protections for several common web security risks, including:

* CSRF
* XSS
* SQL injection
* Clickjacking
* Host header validation
* Session security
* HTTPS-related configuration

Django's official documentation has a dedicated security section covering these areas.

## CSRF

Always include:

```django
{% csrf_token %}
```

inside POST forms.

Example:

```html
<form method="post">

    {% csrf_token %}

    <input
        type="text"
        name="name"
    >

    <button type="submit">
        Save
    </button>

</form>
```

## Never expose SECRET_KEY

Bad:

```python
SECRET_KEY = "my-real-production-secret"
```

Better:

```python
import os


SECRET_KEY = os.environ["DJANGO_SECRET_KEY"]
```

## Production DEBUG

Never use:

```python
DEBUG = True
```

in production.

Use:

```python
DEBUG = False
```

## ALLOWED_HOSTS

Example:

```python
ALLOWED_HOSTS = [
    "example.com",
    "www.example.com",
]
```

---

# 42. Environment Variables

Install:

```bash
pip install python-dotenv
```

Create:

```text
.env
```

```env
DJANGO_SECRET_KEY=change-me
DJANGO_DEBUG=True
DATABASE_URL=sqlite:///db.sqlite3
```

Settings:

```python
import os

from dotenv import load_dotenv


load_dotenv()


SECRET_KEY = os.environ.get(
    "DJANGO_SECRET_KEY"
)

DEBUG = (
    os.environ.get(
        "DJANGO_DEBUG",
        "False",
    ).lower()
    == "true"
)
```

Add to `.gitignore`:

```gitignore
.env
venv/
__pycache__/
*.pyc
db.sqlite3
media/
staticfiles/
```

---

# 43. PostgreSQL

Install PostgreSQL driver:

```bash
pip install psycopg[binary]
```

Settings:

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "mydatabase",
        "USER": "postgres",
        "PASSWORD": "your-password",
        "HOST": "localhost",
        "PORT": "5432",
    }
}
```

Run:

```bash
python manage.py migrate
```

Create superuser:

```bash
python manage.py createsuperuser
```

For production, keep database credentials in environment variables rather than committing them to Git.

---

# 44. Production Settings

Example:

```python
DEBUG = False

ALLOWED_HOSTS = [
    "example.com",
    "www.example.com",
]

CSRF_TRUSTED_ORIGINS = [
    "https://example.com",
    "https://www.example.com",
]

SECURE_SSL_REDIRECT = True

SESSION_COOKIE_SECURE = True

CSRF_COOKIE_SECURE = True

SECURE_HSTS_SECONDS = 31536000

SECURE_HSTS_INCLUDE_SUBDOMAINS = True

SECURE_HSTS_PRELOAD = True

X_FRAME_OPTIONS = "DENY"
```

Before deploying, run:

```bash
python manage.py check --deploy
```

---

# 45. Static Files in Production

Configure:

```python
STATIC_URL = "/static/"

STATIC_ROOT = BASE_DIR / "staticfiles"
```

Collect:

```bash
python manage.py collectstatic
```

Django's deployment documentation covers deployment and static-file handling as separate production concerns.

Do not use Django's development server for production.

---

# 46. Deployment

A common production architecture:

```text
Internet
   |
   v
Nginx
   |
   v
Gunicorn
   |
   v
Django
   |
   +------ PostgreSQL
   |
   +------ Redis
   |
   +------ Object Storage
```

Install Gunicorn:

```bash
pip install gunicorn
```

Run:

```bash
gunicorn config.wsgi:application
```

For ASGI:

```bash
gunicorn \
    -k uvicorn.workers.UvicornWorker \
    config.asgi:application
```

The exact deployment setup depends on the hosting platform.

---

# 47. Performance Optimization

## Use select_related

For ForeignKey / OneToOne relationships:

```python
products = Product.objects.select_related(
    "category"
)
```

## Use prefetch_related

For ManyToMany / reverse relationships:

```python
products = Product.objects.prefetch_related(
    "tags"
)
```

## Bad

```python
products = Product.objects.all()

for product in products:

    print(product.category.name)
```

This can cause unnecessary database queries.

## Better

```python
products = Product.objects.select_related(
    "category"
)

for product in products:

    print(product.category.name)
```

---

## Only select required fields

```python
products = Product.objects.only(
    "id",
    "name",
    "price",
)
```

---

## Use indexes

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
        db_index=True,
    )
```

Or:

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    class Meta:

        indexes = [
            models.Index(
                fields=["name"]
            ),
        ]
```

---

# 48. Project Structure for Large Applications

For larger projects:

```text
project/
│
├── manage.py
│
├── config/
│   ├── settings/
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── development.py
│   │   └── production.py
│   │
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── apps/
│   ├── accounts/
│   │   ├── migrations/
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── models.py
│   │   ├── urls.py
│   │   ├── views.py
│   │   ├── forms.py
│   │   └── tests/
│   │
│   ├── products/
│   │   ├── migrations/
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── models.py
│   │   ├── urls.py
│   │   ├── views.py
│   │   ├── forms.py
│   │   └── tests/
│   │
│   └── orders/
│       ├── migrations/
│       ├── admin.py
│       ├── apps.py
│       ├── models.py
│       ├── urls.py
│       ├── views.py
│       └── tests/
│
├── templates/
├── static/
├── media/
├── requirements.txt
├── .env
└── .gitignore
```

## Khmer

Project តូចអាចប្រើ structure ធម្មតា។

Project ធំគួរបែងចែកជា apps ដូចជា៖

```text
accounts
products
orders
payments
inventory
reports
```

វាធ្វើឱ្យ code ងាយថែទាំ និងងាយពង្រីក។

---

# 49. Useful Django Commands

## Create project

```bash
django-admin startproject config .
```

## Create app

```bash
python manage.py startapp products
```

## Run server

```bash
python manage.py runserver
```

## Make migrations

```bash
python manage.py makemigrations
```

## Apply migrations

```bash
python manage.py migrate
```

## Create superuser

```bash
python manage.py createsuperuser
```

## Open Django shell

```bash
python manage.py shell
```

## Run tests

```bash
python manage.py test
```

## Check project

```bash
python manage.py check
```

## Production check

```bash
python manage.py check --deploy
```

## Collect static files

```bash
python manage.py collectstatic
```

## Show migrations

```bash
python manage.py showmigrations
```

## Show Django version

```bash
python -m django --version
```

---

# 50. Complete Mini Project

Now let's combine the important beginner/intermediate concepts.

## Project

```text
django-shop/
│
├── manage.py
│
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── products/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py
│   ├── urls.py
│   ├── views.py
│   └── migrations/
│
├── templates/
│   ├── base.html
│   └── products/
│       ├── list.html
│       ├── detail.html
│       ├── create.html
│       └── delete.html
│
├── static/
│   └── css/
│       └── style.css
│
├── requirements.txt
└── .gitignore
```

---

## Step 1 — Create project

```bash
mkdir django-shop
cd django-shop

python -m venv venv
```

Windows:

```powershell
venv\Scripts\activate
```

Linux/macOS:

```bash
source venv/bin/activate
```

Install:

```bash
pip install django
```

Create:

```bash
django-admin startproject config .
python manage.py startapp products
```

---

## Step 2 — Model

```python
# products/models.py

from django.db import models


class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    description = models.TextField(
        blank=True,
    )

    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
    )

    quantity = models.PositiveIntegerField(
        default=0,
    )

    created_at = models.DateTimeField(
        auto_now_add=True,
    )

    updated_at = models.DateTimeField(
        auto_now=True,
    )

    def __str__(self):

        return self.name
```

---

## Step 3 — Form

```python
# products/forms.py

from django import forms

from .models import Product


class ProductForm(forms.ModelForm):

    class Meta:

        model = Product

        fields = [
            "name",
            "description",
            "price",
            "quantity",
        ]
```

---

## Step 4 — Views

```python
# products/views.py

from django.contrib import messages
from django.shortcuts import (
    get_object_or_404,
    redirect,
    render,
)

from .forms import ProductForm
from .models import Product


def product_list(request):

    query = request.GET.get(
        "q",
        "",
    )

    products = Product.objects.all()

    if query:

        products = products.filter(
            name__icontains=query
        )

    return render(
        request,
        "products/list.html",
        {
            "products": products,
            "query": query,
        },
    )


def product_detail(
    request,
    product_id,
):

    product = get_object_or_404(
        Product,
        id=product_id,
    )

    return render(
        request,
        "products/detail.html",
        {
            "product": product,
        },
    )


def product_create(request):

    if request.method == "POST":

        form = ProductForm(
            request.POST
        )

        if form.is_valid():

            form.save()

            messages.success(
                request,
                "Product created successfully.",
            )

            return redirect(
                "product-list"
            )

    else:

        form = ProductForm()

    return render(
        request,
        "products/create.html",
        {
            "form": form,
        },
    )


def product_delete(
    request,
    product_id,
):

    product = get_object_or_404(
        Product,
        id=product_id,
    )

    if request.method == "POST":

        product.delete()

        messages.success(
            request,
            "Product deleted successfully.",
        )

        return redirect(
            "product-list"
        )

    return render(
        request,
        "products/delete.html",
        {
            "product": product,
        },
    )
```

---

## Step 5 — URLs

```python
# products/urls.py

from django.urls import path

from . import views


urlpatterns = [

    path(
        "",
        views.product_list,
        name="product-list",
    ),

    path(
        "<int:product_id>/",
        views.product_detail,
        name="product-detail",
    ),

    path(
        "create/",
        views.product_create,
        name="product-create",
    ),

    path(
        "<int:product_id>/delete/",
        views.product_delete,
        name="product-delete",
    ),
]
```

Main URL:

```python
# config/urls.py

from django.contrib import admin
from django.urls import include, path


urlpatterns = [

    path(
        "admin/",
        admin.site.urls,
    ),

    path(
        "products/",
        include("products.urls"),
    ),
]
```

---

## Step 6 — Admin

```python
# products/admin.py

from django.contrib import admin

from .models import Product


@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):

    list_display = (
        "id",
        "name",
        "price",
        "quantity",
        "created_at",
    )

    search_fields = (
        "name",
        "description",
    )

    list_filter = (
        "created_at",
    )
```

---

## Step 7 — Base Template

```html
<!-- templates/base.html -->

{% load static %}

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >

    <title>
        {% block title %}
            Django Shop
        {% endblock %}
    </title>

    <link
        rel="stylesheet"
        href="{% static 'css/style.css' %}"
    >

</head>

<body>

    <nav>

        <a href="{% url 'product-list' %}">
            Products
        </a>

        <a href="{% url 'product-create' %}">
            Create Product
        </a>

        <a href="/admin/">
            Admin
        </a>

    </nav>

    {% if messages %}

        {% for message in messages %}

            <div class="message">
                {{ message }}
            </div>

        {% endfor %}

    {% endif %}

    <main>

        {% block content %}
        {% endblock %}

    </main>

</body>

</html>
```

---

## Step 8 — Product List

```html
<!-- templates/products/list.html -->

{% extends "base.html" %}

{% block title %}
Products
{% endblock %}

{% block content %}

<h1>Products</h1>

<form method="get">

    <input
        type="search"
        name="q"
        value="{{ query }}"
        placeholder="Search..."
    >

    <button type="submit">
        Search
    </button>

</form>

<hr>

{% for product in products %}

    <article>

        <h2>

            <a
                href="{% url 'product-detail' product.id %}"
            >
                {{ product.name }}
            </a>

        </h2>

        <p>
            ${{ product.price }}
        </p>

        <p>
            Quantity: {{ product.quantity }}
        </p>

    </article>

{% empty %}

    <p>
        No products found.
    </p>

{% endfor %}

{% endblock %}
```

---

## Step 9 — Product Detail

```html
<!-- templates/products/detail.html -->

{% extends "base.html" %}

{% block title %}
{{ product.name }}
{% endblock %}

{% block content %}

<h1>
    {{ product.name }}
</h1>

<p>
    {{ product.description }}
</p>

<p>
    Price: ${{ product.price }}
</p>

<p>
    Quantity: {{ product.quantity }}
</p>

<a
    href="{% url 'product-list' %}"
>
    Back
</a>

|

<a
    href="{% url 'product-delete' product.id %}"
>
    Delete
</a>

{% endblock %}
```

---

## Step 10 — Create Product

```html
<!-- templates/products/create.html -->

{% extends "base.html" %}

{% block title %}
Create Product
{% endblock %}

{% block content %}

<h1>
    Create Product
</h1>

<form method="post">

    {% csrf_token %}

    {{ form.as_p }}

    <button type="submit">
        Create
    </button>

</form>

{% endblock %}
```

---

## Step 11 — Delete Product

```html
<!-- templates/products/delete.html -->

{% extends "base.html" %}

{% block title %}
Delete Product
{% endblock %}

{% block content %}

<h1>
    Delete Product
</h1>

<p>
    Are you sure you want to delete
    "{{ product.name }}"?
</p>

<form method="post">

    {% csrf_token %}

    <button type="submit">
        Yes, Delete
    </button>

    <a
        href="{% url 'product-detail' product.id %}"
    >
        Cancel
    </a>

</form>

{% endblock %}
```

---

## Step 12 — CSS

```css
/* static/css/style.css */

body {
    font-family: Arial, sans-serif;
    max-width: 1000px;
    margin: 0 auto;
    padding: 30px;
}

nav {
    display: flex;
    gap: 20px;
    margin-bottom: 30px;
}

article {
    border: 1px solid #ddd;
    padding: 20px;
    margin-bottom: 15px;
}

.message {
    padding: 10px;
    margin-bottom: 20px;
    background: #eee;
}
```

---

## Step 13 — Database

Run:

```bash
python manage.py makemigrations
python manage.py migrate
```

Create admin:

```bash
python manage.py createsuperuser
```

Start:

```bash
python manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/products/
```

Admin:

```text
http://127.0.0.1:8000/admin/
```

---

# 51. requirements.txt

Generate:

```bash
pip freeze > requirements.txt
```

Example:

```text
Django==6.0
```

Install dependencies later:

```bash
pip install -r requirements.txt
```

---

# 52. .gitignore

Recommended:

```gitignore
# Virtual environment
venv/
.venv/

# Python
__pycache__/
*.py[cod]

# Django
db.sqlite3
media/
staticfiles/

# Environment
.env

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db
```

---

# 53. GitHub Workflow

Initialize Git:

```bash
git init
```

Add files:

```bash
git add .
```

Commit:

```bash
git commit -m "Initial Django project"
```

Create branch:

```bash
git branch -M main
```

Add remote:

```bash
git remote add origin YOUR_GITHUB_REPOSITORY_URL
```

Push:

```bash
git push -u origin main
```

---

# 54. Recommended Development Workflow

Every time you change a model:

```bash
python manage.py makemigrations
python manage.py migrate
```

Before committing:

```bash
python manage.py check
python manage.py test
```

Before production:

```bash
python manage.py check --deploy
python manage.py collectstatic
```

---

# 55. Django Development Flow

A typical request:

```text
Browser
   |
   v
URL
   |
   v
URLconf
   |
   v
View
   |
   +--------> Model
   |             |
   |             v
   |          Database
   |
   v
Template
   |
   v
HTTP Response
   |
   v
Browser
```

Example:

```text
GET /products/
       |
       v
products.urls
       |
       v
product_list()
       |
       v
Product.objects.all()
       |
       v
Database
       |
       v
products/list.html
       |
       v
HTML
```

---

# 56. Beginner → Advanced Roadmap

## 🟢 Beginner

Learn:

```text
1. Python
2. Virtual environments
3. Django installation
4. Project
5. App
6. URLs
7. Views
8. Templates
9. Static files
10. Models
11. Migrations
12. Admin
```

---

## 🟡 Intermediate

Learn:

```text
13. Forms
14. ModelForms
15. Validation
16. CRUD
17. Authentication
18. Authorization
19. Sessions
20. Messages
21. Class-Based Views
22. Generic Views
23. Relationships
24. File Uploads
25. Pagination
26. Search
27. Transactions
28. Email
```

---

## 🔴 Advanced

Learn:

```text
29. Custom User Model
30. Middleware
31. Signals
32. Async Views
33. Caching
34. Query Optimization
35. select_related
36. prefetch_related
37. Database indexes
38. Testing
39. Security
40. PostgreSQL
41. Environment Variables
42. Production Settings
43. Static Deployment
44. Gunicorn
45. Nginx
46. Redis
47. Background Tasks
48. Docker
49. CI/CD
50. Cloud Deployment
```

---

# 57. Recommended Django Learning Order

The best order is:

```text
Python
  ↓
HTTP Basics
  ↓
Django Project
  ↓
URLs
  ↓
Views
  ↓
Templates
  ↓
Models
  ↓
ORM
  ↓
Migrations
  ↓
Admin
  ↓
Forms
  ↓
CRUD
  ↓
Authentication
  ↓
Authorization
  ↓
Relationships
  ↓
Class-Based Views
  ↓
Testing
  ↓
Security
  ↓
PostgreSQL
  ↓
Caching
  ↓
Optimization
  ↓
Deployment
  ↓
Docker
  ↓
CI/CD
```

---

# 58. Django Best Practices

## 1. Use a virtual environment

```bash
python -m venv venv
```

## 2. Never commit `.env`

```gitignore
.env
```

## 3. Never expose production SECRET_KEY

```python
SECRET_KEY = os.environ["DJANGO_SECRET_KEY"]
```

## 4. Keep `DEBUG=False` in production

```python
DEBUG = False
```

## 5. Use PostgreSQL for serious production applications

```text
SQLite
   ↓
Learning / small projects

PostgreSQL
   ↓
Production / larger applications
```

## 6. Write tests

```bash
python manage.py test
```

## 7. Optimize database queries

Use:

```python
select_related()
```

and:

```python
prefetch_related()
```

when appropriate.

## 8. Keep apps focused

Good:

```text
accounts
products
orders
payments
inventory
```

Avoid putting everything into one giant app.

---

# 59. Common Mistakes

## Mistake 1 — Forgetting migrations

Wrong:

```bash
python manage.py makemigrations
```

but never:

```bash
python manage.py migrate
```

Correct:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## Mistake 2 — Forgetting CSRF

Wrong:

```html
<form method="post">
```

Correct:

```html
<form method="post">

    {% csrf_token %}

</form>
```

---

## Mistake 3 — Using development server in production

Do not deploy production traffic using:

```bash
python manage.py runserver
```

Use an appropriate production server architecture instead.

---

## Mistake 4 — Hard-coding secrets

Bad:

```python
PASSWORD = "123456"
```

Good:

```python
PASSWORD = os.environ["DATABASE_PASSWORD"]
```

---

## Mistake 5 — N+1 database queries

Bad:

```python
products = Product.objects.all()

for product in products:

    print(product.category.name)
```

Better:

```python
products = Product.objects.select_related(
    "category"
)
```

---

# 60. Useful Django Concepts

| Concept        | Purpose                        |
| -------------- | ------------------------------ |
| Project        | Whole Django application       |
| App            | Feature/module                 |
| URL            | Route                          |
| View           | Request logic                  |
| Model          | Database structure             |
| ORM            | Database interaction           |
| Template       | HTML presentation              |
| Form           | User input                     |
| ModelForm      | Model-based form               |
| Migration      | Database schema changes        |
| Admin          | Data management                |
| Middleware     | Request/response processing    |
| Signal         | Event notification             |
| Session        | User-specific server-side data |
| Cache          | Temporary fast data            |
| QuerySet       | Database query representation  |
| Manager        | Model query interface          |
| Authentication | Who are you?                   |
| Authorization  | What can you do?               |

---

# 61. Final Checklist

Before calling your Django project complete:

```text
[ ] Virtual environment created
[ ] Django installed
[ ] Project created
[ ] Apps organized
[ ] URLs configured
[ ] Views implemented
[ ] Templates implemented
[ ] Static files configured
[ ] Models created
[ ] Migrations applied
[ ] Admin configured
[ ] Forms validated
[ ] CRUD completed
[ ] Authentication implemented
[ ] Authorization implemented
[ ] Tests written
[ ] Security reviewed
[ ] SECRET_KEY protected
[ ] DEBUG=False in production
[ ] ALLOWED_HOSTS configured
[ ] PostgreSQL configured
[ ] Static files collected
[ ] Production server configured
[ ] Logs configured
[ ] Git repository created
[ ] .gitignore configured
[ ] README written
```

---

# 62. Quick Reference

## Start project

```bash
django-admin startproject config .
```

## Create app

```bash
python manage.py startapp products
```

## Run

```bash
python manage.py runserver
```

## Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

## Admin

```bash
python manage.py createsuperuser
```

## Shell

```bash
python manage.py shell
```

## Test

```bash
python manage.py test
```

## Production check

```bash
python manage.py check --deploy
```

## Static files

```bash
python manage.py collectstatic
```

---

# 63. Official Documentation

The official Django documentation is the best reference for version-specific behavior.

* Django 6.0 Documentation
* Django Tutorial
* Django Models
* Django ORM
* Django Forms
* Django Authentication
* Django Security
* Django Testing
* Django Deployment

Always use documentation matching the Django version installed in your project.

---

# 🎯 Final Goal

After completing this guide, you should be able to build applications such as:

```text
Authentication System
        +
Product Management
        +
Inventory Management
        +
Order Management
        +
Payment Integration
        +
REST API
        +
PostgreSQL
        +
Redis
        +
Background Jobs
        +
Testing
        +
Docker
        +
CI/CD
        +
Production Deployment
```

A professional Django application can be organized around:

```text
Frontend
   |
   v
Django
   |
   +---- Authentication
   |
   +---- Business Logic
   |
   +---- REST API
   |
   +---- PostgreSQL
   |
   +---- Redis
   |
   +---- Background Workers
   |
   +---- Object Storage
   |
   v
Production
```

---

## 🇰🇭 សង្ខេបជាភាសាខ្មែរ

Django គឺជា Python Web Framework ដែលមានមុខងារជាច្រើនស្រាប់ ដូចជា:

```text
URL
 ↓
View
 ↓
Model
 ↓
Database
```

ហើយបន្ទាប់មក៖

```text
Forms
Authentication
Authorization
Admin
API
Testing
Security
Caching
PostgreSQL
Deployment
```

បើចង់រៀន Django ឱ្យខ្លាំង មិនគួររៀនតែ syntax ទេ។ គួររៀនពី **Request → URL → View → ORM → Database → Template → Response** ហើយបន្តទៅ **Security → Testing → Performance → Deployment**។

Django official documentation ក៏រៀបចំមេរៀនចាប់ពី project, models, views, templates, forms, testing, static files រហូតដល់ reusable apps និង third-party packages ផងដែរ។

---

# 🚀 Next Step

After finishing this README, build these projects in order:

```text
Project 1
Simple Blog
    ↓
Project 2
Authentication System
    ↓
Project 3
Product CRUD
    ↓
Project 4
Inventory Management
    ↓
Project 5
E-Commerce
    ↓
Project 6
REST API
    ↓
Project 7
Django + PostgreSQL + Redis
    ↓
Project 8
Dockerized Django
    ↓
Project 9
Production Deployment
    ↓
Project 10
Full Production E-Commerce System
```

**The goal is not just to memorize Django syntax. Build projects until the complete architecture becomes natural.**
