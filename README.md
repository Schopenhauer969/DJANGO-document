# Django — Complete Guide (Beginner → Advanced)

A single reference README covering Django from first install to production-grade patterns. Every section includes full, runnable code.

---

## Table of Contents

1. [What is Django](#1-what-is-django)
2. [Installation & Project Setup](#2-installation--project-setup)
3. [Project Structure](#3-project-structure)
4. [Your First App](#4-your-first-app)
5. [Models & the ORM](#5-models--the-orm)
6. [Migrations](#6-migrations)
7. [Django Admin](#7-django-admin)
8. [Views (Function-Based & Class-Based)](#8-views-function-based--class-based)
9. [URLs & Routing](#9-urls--routing)
10. [Templates](#10-templates)
11. [Forms](#11-forms)
12. [Static Files & Media](#12-static-files--media)
13. [Authentication & Authorization](#13-authentication--authorization)
14. [Django REST Framework (API)](#14-django-rest-framework-api)
15. [Testing](#15-testing)
16. [Signals](#16-signals)
17. [Middleware](#17-middleware)
18. [Caching](#18-caching)
19. [Celery — Background Tasks](#19-celery--background-tasks)
20. [Environment Variables & Settings for Production](#20-environment-variables--settings-for-production)
21. [Deployment Checklist](#21-deployment-checklist)
22. [Useful Commands Cheatsheet](#22-useful-commands-cheatsheet)

---

## 1. What is Django

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. It follows the **MTV** pattern (Model–Template–View), which is Django's take on MVC.

- **Model** → data layer (database schema, ORM)
- **Template** → presentation layer (HTML rendering)
- **View** → business logic (glues models and templates together)

---

## 2. Installation & Project Setup

```bash
# 1. Create a project folder
mkdir myproject && cd myproject

# 2. Create a virtual environment
python -m venv venv

# 3. Activate it
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate

# 4. Install Django
pip install django

# 5. Confirm installation
python -m django --version

# 6. Create the Django project
django-admin startproject config .

# 7. Run the development server
python manage.py runserver
```

Visit `http://127.0.0.1:8000/` — you should see the Django welcome page.

**requirements.txt**

```txt
Django==5.0.6
djangorestframework==3.15.2
python-decouple==3.8
Pillow==10.3.0
psycopg2-binary==2.9.9
celery==5.4.0
redis==5.0.4
gunicorn==22.0.0
```

Install everything at once:

```bash
pip install -r requirements.txt
```

---

## 3. Project Structure

```
myproject/
├── config/                # Project settings package
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── blog/                  # An example app
│   ├── migrations/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── templates/
│   └── base.html
├── static/
├── media/
├── manage.py
└── requirements.txt
```

---

## 4. Your First App

```bash
python manage.py startapp blog
```

Register it in **config/settings.py**:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # Local apps
    'blog',
]
```

---

## 5. Models & the ORM

**blog/models.py**

```python
from django.db import models
from django.conf import settings
from django.urls import reverse


class Category(models.Model):
    name = models.CharField(max_length=100, unique=True)
    slug = models.SlugField(unique=True)

    class Meta:
        verbose_name_plural = "categories"
        ordering = ['name']

    def __str__(self):
        return self.name


class Post(models.Model):
    STATUS_CHOICES = [
        ('draft', 'Draft'),
        ('published', 'Published'),
    ]

    title = models.CharField(max_length=200)
    slug = models.SlugField(unique=True)
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
        related_name='posts'
    )
    category = models.ForeignKey(
        Category,
        on_delete=models.SET_NULL,
        null=True,
        blank=True,
        related_name='posts'
    )
    body = models.TextField()
    status = models.CharField(max_length=10, choices=STATUS_CHOICES, default='draft')
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ['-created_at']
        indexes = [
            models.Index(fields=['-created_at']),
        ]

    def __str__(self):
        return self.title

    def get_absolute_url(self):
        return reverse('blog:post_detail', kwargs={'slug': self.slug})


class Comment(models.Model):
    post = models.ForeignKey(Post, on_delete=models.CASCADE, related_name='comments')
    name = models.CharField(max_length=80)
    email = models.EmailField()
    body = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    active = models.BooleanField(default=True)

    class Meta:
        ordering = ['created_at']

    def __str__(self):
        return f'Comment by {self.name} on {self.post}'
```

### Common ORM queries

```python
from blog.models import Post, Category

# All published posts
Post.objects.filter(status='published')

# Get or 404
from django.shortcuts import get_object_or_404
post = get_object_or_404(Post, slug='my-post', status='published')

# Related lookups
Post.objects.filter(category__name='Tech')

# Aggregation
from django.db.models import Count
Category.objects.annotate(post_count=Count('posts'))

# select_related / prefetch_related for performance
Post.objects.select_related('author', 'category').prefetch_related('comments')

# Creating
Post.objects.create(title='Hello', slug='hello', author=user, body='...')

# Bulk create
Post.objects.bulk_create([
    Post(title='A', slug='a', author=user, body='...'),
    Post(title='B', slug='b', author=user, body='...'),
])
```

---

## 6. Migrations

```bash
# Create migration files based on model changes
python manage.py makemigrations

# Apply migrations to the database
python manage.py migrate

# See SQL that a migration will run
python manage.py sqlmigrate blog 0001

# Roll back to a specific migration
python manage.py migrate blog 0001
```

---

## 7. Django Admin

**blog/admin.py**

```python
from django.contrib import admin
from .models import Category, Post, Comment


@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    list_display = ('name', 'slug')
    prepopulated_fields = {'slug': ('name',)}


class CommentInline(admin.TabularInline):
    model = Comment
    extra = 0


@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'category', 'status', 'created_at')
    list_filter = ('status', 'category', 'created_at')
    search_fields = ('title', 'body')
    prepopulated_fields = {'slug': ('title',)}
    inlines = [CommentInline]
    date_hierarchy = 'created_at'
```

Create a superuser to log in:

```bash
python manage.py createsuperuser
```

---

## 8. Views (Function-Based & Class-Based)

**blog/views.py**

```python
from django.shortcuts import render, get_object_or_404, redirect
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from django.contrib.auth.mixins import LoginRequiredMixin
from .models import Post
from .forms import CommentForm, PostForm


# --- Function-based views ---

def post_list(request):
    posts = Post.objects.filter(status='published')
    return render(request, 'blog/post_list.html', {'posts': posts})


def post_detail(request, slug):
    post = get_object_or_404(Post, slug=slug, status='published')
    comments = post.comments.filter(active=True)
    new_comment = None

    if request.method == 'POST':
        comment_form = CommentForm(data=request.POST)
        if comment_form.is_valid():
            new_comment = comment_form.save(commit=False)
            new_comment.post = post
            new_comment.save()
            return redirect(post.get_absolute_url())
    else:
        comment_form = CommentForm()

    return render(request, 'blog/post_detail.html', {
        'post': post,
        'comments': comments,
        'comment_form': comment_form,
    })


# --- Class-based views ---

class PostListView(ListView):
    model = Post
    template_name = 'blog/post_list.html'
    context_object_name = 'posts'
    paginate_by = 10

    def get_queryset(self):
        return Post.objects.filter(status='published')


class PostDetailView(DetailView):
    model = Post
    template_name = 'blog/post_detail.html'
    context_object_name = 'post'
    slug_field = 'slug'
    slug_url_kwarg = 'slug'


class PostCreateView(LoginRequiredMixin, CreateView):
    model = Post
    form_class = PostForm
    template_name = 'blog/post_form.html'

    def form_valid(self, form):
        form.instance.author = self.request.user
        return super().form_valid(form)


class PostUpdateView(LoginRequiredMixin, UpdateView):
    model = Post
    form_class = PostForm
    template_name = 'blog/post_form.html'


class PostDeleteView(LoginRequiredMixin, DeleteView):
    model = Post
    template_name = 'blog/post_confirm_delete.html'
    success_url = reverse_lazy('blog:post_list')
```

---

## 9. URLs & Routing

**blog/urls.py**

```python
from django.urls import path
from . import views

app_name = 'blog'

urlpatterns = [
    path('', views.PostListView.as_view(), name='post_list'),
    path('post/<slug:slug>/', views.PostDetailView.as_view(), name='post_detail'),
    path('post/new/', views.PostCreateView.as_view(), name='post_create'),
    path('post/<slug:slug>/edit/', views.PostUpdateView.as_view(), name='post_update'),
    path('post/<slug:slug>/delete/', views.PostDeleteView.as_view(), name='post_delete'),
]
```

**config/urls.py**

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls', namespace='blog')),
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

## 10. Templates

**templates/base.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}My Blog{% endblock %}</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
</head>
<body>
    <header>
        <h1><a href="{% url 'blog:post_list' %}">My Blog</a></h1>
    </header>

    <main>
        {% block content %}{% endblock %}
    </main>

    <footer>
        <p>&copy; 2026 My Blog</p>
    </footer>
</body>
</html>
```

**blog/templates/blog/post_list.html**

```html
{% extends "base.html" %}

{% block title %}All Posts{% endblock %}

{% block content %}
    {% for post in posts %}
        <article>
            <h2><a href="{{ post.get_absolute_url }}">{{ post.title }}</a></h2>
            <p>By {{ post.author }} on {{ post.created_at|date:"F j, Y" }}</p>
            <p>{{ post.body|truncatewords:30 }}</p>
        </article>
    {% empty %}
        <p>No posts yet.</p>
    {% endfor %}

    {% if is_paginated %}
        <div class="pagination">
            {% if page_obj.has_previous %}
                <a href="?page={{ page_obj.previous_page_number }}">Previous</a>
            {% endif %}
            <span>Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}</span>
            {% if page_obj.has_next %}
                <a href="?page={{ page_obj.next_page_number }}">Next</a>
            {% endif %}
        </div>
    {% endif %}
{% endblock %}
```

**blog/templates/blog/post_detail.html**

```html
{% extends "base.html" %}

{% block title %}{{ post.title }}{% endblock %}

{% block content %}
    <article>
        <h1>{{ post.title }}</h1>
        <p>By {{ post.author }} on {{ post.created_at|date:"F j, Y" }}</p>
        <div>{{ post.body|linebreaks }}</div>
    </article>

    <section>
        <h3>Comments ({{ comments.count }})</h3>
        {% for comment in comments %}
            <div>
                <strong>{{ comment.name }}</strong> — {{ comment.created_at }}
                <p>{{ comment.body }}</p>
            </div>
        {% endfor %}

        <h3>Add a comment</h3>
        <form method="post">
            {% csrf_token %}
            {{ comment_form.as_p }}
            <button type="submit">Submit</button>
        </form>
    </section>
{% endblock %}
```

Register the templates directory in **config/settings.py** if using a project-level folder:

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

---

## 11. Forms

**blog/forms.py**

```python
from django import forms
from .models import Comment, Post


class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ['name', 'email', 'body']
        widgets = {
            'body': forms.Textarea(attrs={'rows': 4, 'placeholder': 'Write a comment...'}),
        }


class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'slug', 'category', 'body', 'status']

    def clean_title(self):
        title = self.cleaned_data['title']
        if len(title) < 5:
            raise forms.ValidationError('Title must be at least 5 characters long.')
        return title
```

A plain (non-model) form example:

```python
class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)

    def clean(self):
        cleaned_data = super().clean()
        message = cleaned_data.get('message')
        if message and len(message) < 10:
            raise forms.ValidationError('Message is too short.')
        return cleaned_data
```

---

## 12. Static Files & Media

**config/settings.py**

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
STATIC_ROOT = BASE_DIR / 'staticfiles'  # used by collectstatic in production

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

Collect static files for production:

```bash
python manage.py collectstatic
```

---

## 13. Authentication & Authorization

**Built-in auth URLs**

```python
# config/urls.py
from django.urls import path, include

urlpatterns = [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

**Custom registration view**

```python
from django.contrib.auth.forms import UserCreationForm
from django.urls import reverse_lazy
from django.views.generic import CreateView


class SignUpView(CreateView):
    form_class = UserCreationForm
    success_url = reverse_lazy('login')
    template_name = 'registration/signup.html'
```

**Restricting access**

```python
from django.contrib.auth.decorators import login_required, permission_required

@login_required
def dashboard(request):
    return render(request, 'dashboard.html')

@permission_required('blog.add_post', raise_exception=True)
def create_post(request):
    ...
```

**Custom user model** (set this up before the first migration)

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models


class User(AbstractUser):
    bio = models.TextField(blank=True)
    avatar = models.ImageField(upload_to='avatars/', blank=True, null=True)
```

```python
# config/settings.py
AUTH_USER_MODEL = 'accounts.User'
```

---

## 14. Django REST Framework (API)

```bash
pip install djangorestframework
```

```python
# config/settings.py
INSTALLED_APPS += ['rest_framework']

REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ],
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
        'rest_framework.authentication.SessionAuthentication',
    ],
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10,
}
```

**blog/serializers.py**

```python
from rest_framework import serializers
from .models import Post, Comment


class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = ['id', 'name', 'email', 'body', 'created_at']


class PostSerializer(serializers.ModelSerializer):
    comments = CommentSerializer(many=True, read_only=True)
    author = serializers.ReadOnlyField(source='author.username')

    class Meta:
        model = Post
        fields = ['id', 'title', 'slug', 'author', 'body', 'status', 'created_at', 'comments']
```

**blog/api_views.py**

```python
from rest_framework import viewsets, permissions
from .models import Post
from .serializers import PostSerializer


class IsAuthorOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:
            return True
        return obj.author == request.user


class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.filter(status='published')
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsAuthorOrReadOnly]

    def perform_create(self, serializer):
        serializer.save(author=self.request.user)
```

**blog/api_urls.py**

```python
from rest_framework.routers import DefaultRouter
from .api_views import PostViewSet

router = DefaultRouter()
router.register(r'posts', PostViewSet, basename='post')

urlpatterns = router.urls
```

```python
# config/urls.py
urlpatterns += [
    path('api/', include('blog.api_urls')),
]
```

---

## 15. Testing

**blog/tests.py**

```python
from django.test import TestCase
from django.contrib.auth import get_user_model
from django.urls import reverse
from .models import Post, Category

User = get_user_model()


class PostModelTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='tester', password='pass1234')
        self.category = Category.objects.create(name='Tech', slug='tech')
        self.post = Post.objects.create(
            title='Test Post',
            slug='test-post',
            author=self.user,
            category=self.category,
            body='Some content here.',
            status='published',
        )

    def test_post_creation(self):
        self.assertEqual(self.post.title, 'Test Post')
        self.assertEqual(str(self.post), 'Test Post')

    def test_get_absolute_url(self):
        self.assertEqual(self.post.get_absolute_url(), '/post/test-post/')


class PostViewTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username='tester', password='pass1234')
        self.post = Post.objects.create(
            title='Visible Post',
            slug='visible-post',
            author=self.user,
            body='Body text.',
            status='published',
        )

    def test_post_list_view(self):
        response = self.client.get(reverse('blog:post_list'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Visible Post')

    def test_post_detail_view(self):
        response = self.client.get(reverse('blog:post_detail', args=[self.post.slug]))
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, 'blog/post_detail.html')
```

Run tests:

```bash
python manage.py test
python manage.py test blog          # only the blog app
python manage.py test --keepdb      # reuse test DB, faster reruns
coverage run manage.py test && coverage report
```

---

## 16. Signals

**blog/signals.py**

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth import get_user_model
from .models import Post

User = get_user_model()


@receiver(post_save, sender=Post)
def notify_on_publish(sender, instance, created, **kwargs):
    if instance.status == 'published':
        print(f'Post "{instance.title}" was published.')
```

**blog/apps.py**

```python
from django.apps import AppConfig


class BlogConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'blog'

    def ready(self):
        import blog.signals  # noqa
```

---

## 17. Middleware

**blog/middleware.py**

```python
import time
import logging

logger = logging.getLogger(__name__)


class RequestTimingMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        start = time.time()
        response = self.get_response(request)
        duration = time.time() - start
        logger.info(f'{request.method} {request.path} took {duration:.3f}s')
        response['X-Response-Time'] = f'{duration:.3f}'
        return response
```

```python
# config/settings.py
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'blog.middleware.RequestTimingMiddleware',
]
```

---

## 18. Caching

```python
# config/settings.py (Redis-backed cache)
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.redis.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
    }
}
```

```python
from django.core.cache import cache
from django.views.decorators.cache import cache_page


# Cache a whole view for 15 minutes
@cache_page(60 * 15)
def post_list(request):
    ...


# Manual low-level caching
def get_popular_posts():
    posts = cache.get('popular_posts')
    if posts is None:
        posts = list(Post.objects.filter(status='published').order_by('-created_at')[:5])
        cache.set('popular_posts', posts, timeout=60 * 30)
    return posts
```

---

## 19. Celery — Background Tasks

**config/celery.py**

```python
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')

app = Celery('config')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```

**config/\_\_init\_\_.py**

```python
from .celery import app as celery_app

__all__ = ('celery_app',)
```

**blog/tasks.py**

```python
from celery import shared_task
from django.core.mail import send_mail


@shared_task
def send_welcome_email(user_email):
    send_mail(
        subject='Welcome!',
        message='Thanks for joining our blog.',
        from_email='no-reply@example.com',
        recipient_list=[user_email],
    )
```

```python
# config/settings.py
CELERY_BROKER_URL = 'redis://127.0.0.1:6379/0'
CELERY_RESULT_BACKEND = 'redis://127.0.0.1:6379/0'
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
```

Run a worker:

```bash
celery -A config worker -l info
```

---

## 20. Environment Variables & Settings for Production

```bash
pip install python-decouple
```

**.env**

```env
SECRET_KEY=your-secret-key-here
DEBUG=False
ALLOWED_HOSTS=example.com,www.example.com
DATABASE_URL=postgres://user:password@localhost:5432/mydb
```

**config/settings.py**

```python
from pathlib import Path
from decouple import config, Csv

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)
ALLOWED_HOSTS = config('ALLOWED_HOSTS', default='', cast=Csv())

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('DB_NAME'),
        'USER': config('DB_USER'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST', default='localhost'),
        'PORT': config('DB_PORT', default='5432'),
    }
}

SECURE_SSL_REDIRECT = not DEBUG
SESSION_COOKIE_SECURE = not DEBUG
CSRF_COOKIE_SECURE = not DEBUG
SECURE_HSTS_SECONDS = 31536000 if not DEBUG else 0
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'
```

Add `.env` to **.gitignore**:

```gitignore
venv/
__pycache__/
*.pyc
db.sqlite3
.env
media/
staticfiles/
```

---

## 21. Deployment Checklist

- [ ] `DEBUG = False`
- [ ] Strong, unique `SECRET_KEY` stored outside version control
- [ ] `ALLOWED_HOSTS` set correctly
- [ ] Database migrated: `python manage.py migrate`
- [ ] Static files collected: `python manage.py collectstatic --noinput`
- [ ] Run `python manage.py check --deploy` and fix warnings
- [ ] Serve via Gunicorn/Uvicorn behind Nginx (not `runserver`)
- [ ] HTTPS enabled with valid certificate
- [ ] Logging configured (file or external service)
- [ ] Database backups scheduled

**gunicorn** run command:

```bash
gunicorn config.wsgi:application --bind 0.0.0.0:8000 --workers 3
```

**Minimal Procfile (Heroku-style platforms)**

```
web: gunicorn config.wsgi:application
release: python manage.py migrate
```

---

## 22. Useful Commands Cheatsheet

```bash
# Project & apps
django-admin startproject config .
python manage.py startapp appname

# Server
python manage.py runserver
python manage.py runserver 0.0.0.0:8080

# Database
python manage.py makemigrations
python manage.py migrate
python manage.py showmigrations
python manage.py dbshell

# Users
python manage.py createsuperuser
python manage.py changepassword <username>

# Shell
python manage.py shell
python manage.py shell_plus   # requires django-extensions

# Static files
python manage.py collectstatic

# Testing
python manage.py test

# Misc
python manage.py check
python manage.py check --deploy
python manage.py dumpdata > data.json
python manage.py loaddata data.json
```

---

## License

This guide is provided as-is for educational and reference purposes. Feel free to adapt it for your own project's README.
