# djangostarter
`djangostarter` using `Django version 2.1` and `Python version 3.6` with `Virtual Environment Wrapper` by Tran Duc Loi at `loitranduc@gmail.com` for his own purposes.

## Git prepare
	git remote add origin https://github.com/user/repo.git
	git remote -v

## Migrate database
By default, INSTALLED_APPS contains the following apps, all of which come with Django. These applications are included by default as a convenience for the common case. Some of these applications make use of at least one database table, though, so we need to create the tables in the database before we can use them. To do that, run the following command:\
	python manage.py migrate
* Create new models\
New models in `polls/models.py`
* Activate models\
Add application configures:\
	```
	INSTALLED_APPS = [
		'polls.apps.PollsConfig',
	```
* Make the migration
	```
	python manage.py makemigrations polls
	```
* Apply the migration
	```
	python manage.py migrate
	```
* Create admin
	```
	python manage.py createsuperuser
	```
Now, open a Web browser and go to “/admin/” on your local domain
* Make the poll app modifiable in the admin
To do this, open the `polls/admin.py` file, and edit it to look like this:
	```
	from django.contrib import admin
	from .models import Question
	admin.site.register(Question)
	```
* Writting more views
Now let’s add a few more views to `polls/views.py`:
	```
	def detail(request, question_id):
		return HttpResponse("You're looking at question %s." % question_id)
	```
* Wire these new views into the polls.urls module
by adding the following path() calls:
	```
	from django.urls import path
	from . import views
	urlpatterns = [
		# ex: /polls/
		path('', views.index, name='index'),
		# ex: /polls/5/
		path('<int:question_id>/', views.detail, name='detail'),
	]
	```
