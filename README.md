# djangostarter
`djangostarter` is a template and guide of Django Web Framework using `Django version 2.1` and `Python version 3.6` with `Virtual Environment Wrapper` by Tran Duc Loi at `loitranduc@gmail.com` for his own purposes.
## Create new project
```
django-admin startproject mysite
cd mysite
python manage.py startapp polls
python manage.py runserver
python manage.py runserver 8080
```

## Git prepare
```
cd mysite
git remote add origin https://github.com/user/repo.git
git remote -v
```
## Migrate database
By default, `INSTALLED_APPS` contains the following apps, all of which come with Django. These applications are included by default as a convenience for the common case. Some of these applications make use of at least one database table, though, so we need to create the tables in the database before we can use them. To do that, run the following command:\
	python manage.py migrate
## Create new models\
New models in `polls/models.py`. Like this:\
```python
from django.db import models
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
```
## Activate models\
Add application configures:\
```python
INSTALLED_APPS = [
	'polls.apps.PollsConfig',
```
## Make the migration
```python
python manage.py makemigrations polls
```
## Apply the migration
```python
python manage.py migrate
```
## Create admin
```python
python manage.py createsuperuser
```
Now, open a Web browser and go to `“/admin/”` on your local domain
## Make the poll app modifiable in the admin
To do this, open the `polls/admin.py` file, and edit it to look like this:\
```python
from django.contrib import admin
from .models import Question
admin.site.register(Question)
```
## Writting more views
Now let’s add a few more views to `polls/views.py`:
```py
def detail(request, question_id):
	return HttpResponse("You're looking at question %s." % question_id)
```
## Wire these new views into the polls.urls module
by adding the following path() calls:
```py
from django.urls import path
from . import views
urlpatterns = [
	# ex: /polls/
	path('', views.index, name='index'),
	# ex: /polls/5/
	path('<int:question_id>/', views.detail, name='detail'),
]
```
## Using template to get data
Put the following code in that template `polls/templates/polls/index.html`:
```py
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```
Now let’s update our index view in `polls/views.py` to use the template:\
```py
from django.http import HttpResponse
from django.template import loader
from .models import Question
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```
in short:
```py
from django.shortcuts import render
from .models import Question
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```
Raising 404 error if need:
```py
try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```
in short
```py
question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```
## Removing hardcoded URLs in templates
```py
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```
with `detail` defined in:
```py
# the 'name' value as called by the {% url %} template tag
path('<int:question_id>/', views.detail, name='detail'),
```
## Write a simple form
