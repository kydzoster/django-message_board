## Development steps

- **cd ~/Desktop**
- **mkdir Message**
- **cd Message**
- **pipenv install django==3.0.3**
- **pipenv shell**
- **django-admin startproject mb_project .**
- **python manage.py startapp posts**

Inside *mb_project/settings.py* inside *INSTALLED_APPS* add:

        'posts.apps.PostsConfig',

- **python manage.py migrate**

Inside *posts/models.py* add model *class Post*:

        class Post(models.Model):
            text = models.TextField()

- **python manage.py makemigrations posts**
- **python manage.py migrate**
- **python manage.py createsuperuser**

Inside *posts/admin.py* add:

        from django.contrib import admin
        from .models import Post

        admin.site.register(Post)

Inside *posts/models.py* inside *class Post* add:

        def __str__(self):
            return self.text[:50] # This will display the first 50 characters of the text field.

Inside posts/view.py add:

        from django.views.generic import ListView
        from .models import Post

        class HomePageView(ListView):
            model = Post
            template_name = 'home.html'

- **mkdir templates**
- **touch templates/home.html**

Inside *mb_project/settings.py* under *TEMPLATES* replace *DIRS* field with:

        'DIRS': [os.path.join(BASE_DIR, 'templates')],

Inside *templates/home.html* insert:

        <h1>Message board homepage</h1>
        <ul>
            {% for post in object_list %}
                <li>{{ post.text }}</li>
            {% endfor %}
        </ul>

Inside *posts/views.py* inside *class HomePageView* add:

        context_object_name = 'all_posts_list'

Inside *templates/home.html* replace *object_list* with:

        all_posts_list

Inside *mb_project/urls.py* add:

        from django.contrib import admin
        from django.urls import path, include

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('', include('posts.urls')),
        ]

**touch posts/urls.py**
Inside posts/urls.py insert:

        from django.urls import path
        from .views import HomePageView

        urlpatterns = [
            path('', HomePageView.as_view(), name='home'),
        ]

## Testing

Inside posts/tests.py add:

        from django.test import TestCase
        from .models import Post

        class PostModelTest(TestCase):
            def setup(self):
                Post.objects.create(text='just a test')

            def test_text_content(self):
                post=Post.objects.get(id=1)
                excpected_object_name = f'{post.text}'
                self.assertEqual(expected_object_name, 'just a test')

- 

## Push to GitHub