<H1 align="center">Voting Structure</H1>
<p align="center">🚀 Project to create a poll structure using Django for future references</p>

## Resources Used

* Django 5.0.2
* Python 3.10

## Index

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/f9dfef95-c1d9-4a02-a7d2-024143239a30" style="width:100%">
</div>

## Questions

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/d8620d7a-1405-460b-a2f9-ff8fe0c6f426" style="width:100%">
</div>

## Voting

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/dd864ebb-130b-4578-b216-6facc577e02e" style="width:100%">
</div>

## Votes

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/68504e9c-6b38-4c29-9dc2-c31d57b31558" style="width:100%">
</div>

## PollProject Create

<details>
 <summary>Click to show content</summary>

Initial project created with main structure, changing urls.py to add polls.urls/landingPage.urls and settings.py to add the packages.
 ```
django-admin startproject PollProject
 ```


### urls.py

```
urlpatterns = [
 path('admin/', admin.site.urls),
 path('', include('landingPage.urls')),
 path('polls/', include('pollApp.urls')),
]
```

### settings.py

```
INSTALLED_APPS = [
 'django.contrib.admin',
 'django.contrib.auth',
 'django.contrib.contenttypes',
 'django.contrib.sessions',
 'django.contrib.messages',
 'django.contrib.staticfiles',
 'pollApp',
 'landingPage'
]
 ```

<div align="center">
<image 53511c22e5253b6614a27c0387a11" style="width:100%">
</div>



</details>



## LandingPage Creation

<details>
 <summary>Click to show content</summary>

Creation of a package that will be responsible for the initial loading page.

 ```
python manage.py startapp landingPage
 ```


</details>


## PollApp Create

<details>
 <summary>Click to show content</summary>

Creation of a package that will be responsible for the voting logic.

 ```
python manage.py startapp pollApp
 ```



### models.py
Model that will be used to create the questions and their respective alternatives.

### admin.py
Mapping each question with options to select correctly, such as a configuration that will be used on the admin page.

Using Choice as a model and informing that it will have 3 alternatives.
 ```
 class ChoiceInLine(admin.TabularInline):
 model = Choice
 extra = 3
 ```

The QuestionAdmin will be the settings that the administrator will have to enter on the admin page.
 ```
 class QuestionAdmin(admin.ModelAdmin):
 fieldsets = [(None, {'fields':['question_text']}), ('Date Information', {'fields': ['pub_date'], 'classes':['collapse']}),]
 inlines = [ChoiceInLine]
 ```



<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/988ff85b-fc8b-4f8f-bd89-51f1401a03d5" style="width:100%">
</div>





 ### urls.py

I have the route mapping and an 'app_name' that will be used as a reference.

The paths and respective views that will render.

```
app_name = 'polls'
urlpatterns = [
 path('', views.index, name = 'index'),
 path('<int:question_id>/', views.detail, name='detail'),
 path('<int:question_id>/results/', views.results, name='results'),
 path('<int:question_id>/vote/', views.vote, name='vote'),
]
```


</details>



## Template Create


<details>
 <summary>Click to show content</summary>

Directory responsible for storing the html pages that will be rendered.

By convention, within "template" project_name/view_name.html is used so that the framework recognizes the path.

 ```
return render(request, 'pollsApp/results.html',{'question':question})
 ```

### HTML pages

* pages: home page that will be loaded through LandingPage.
* partials: A part that will be loaded in all parts of the project, in this case the navigation bar.
* polls: Structure related to voting containing results, details and the index.
* base: Main body that will be used as the base where the views (pages and polls) will be rendered.


### Format of urls within html

```
{% url 'polls:vote' question.id %}"
```

* polls referring to app_name = ‘polls’ in urls.py.

* vote for the path name in urls.py.

* question.id referring to the question id passed as a parameter in urls.py.


```
app_name = 'polls'
path('<int:question_id>/vote/', views.vote, name='vote'),
```


### Views.py
Responsible for the view rendering controller and poll data flow.


</details>
