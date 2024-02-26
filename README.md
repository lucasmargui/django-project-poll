<H1 align="center">Estrutura Votação</H1>
<p align="center">🚀 Projeto de criação de uma estrutura de enquete utilizando Django para referências futuras</p>

## Recursos Utilizados

* Django 5.0.2
* Python 3.10


## Criação do PollProject

<details>
  <summary>Clique para mostrar conteúdo</summary>
  
Projeto inicial criado com estrutura principal, alterando urls.py para adicionar polls.urls/landingPage.urls e settings.py para adicionar os packages. 
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
<img src="https://cdn.discordapp.com/attachments/1046824853015113789/1207331165669826580/image.png?ex=65df41c5&is=65ccccc5&hm=0294fac52d4376cda51760f25d74e069ce653511c22e5253b6614a27c0387a11" style="width:100%">
</div>



</details>



## Criação do landingPage

<details>
  <summary>Clique para mostrar conteúdo</summary>
  
Criação de um pacote que será responsável pela páginal inicial de carregamento.

 ```
python manage.py startapp landingPage
 ```


</details>


## Criação do pollApp

<details>
  <summary>Clique para mostrar conteúdo</summary>
  
Criação de um pacote que será responsável pela lógica de votação.

 ```
python manage.py startapp pollApp
 ```



### models.py
Modelo que será utilizado para criação das questões e as respectivas alternativas.

### admin.py
Mapeando cada pergunta com opções para selecionar corretamente, como uma configuração que será utilizada na página de admin.
 
Utilizando Choice como model e informando que ela possuirá 3 alternativas.
 ```
  class ChoiceInLine(admin.TabularInline):
    model = Choice
    extra = 3
 ```

O QuestionAdmin será as configurações que o administrador terá que inserir na página de admin.
  ```
  class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [(None, {'fields':['question_text']}), ('Date Information', {'fields': ['pub_date'], 'classes':['collapse']}),]
    inlines = [ChoiceInLine]
  ```

 

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/988ff85b-fc8b-4f8f-bd89-51f1401a03d5" style="width:100%">
</div>




 
 ### urls.py

Possuí o mapeamento das rotas e um 'app_name' que será usado como referência.  

Os caminhos e as respectivas views que irá renderizar.

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



## Criação Template


<details>
  <summary>Clique para mostrar conteúdo</summary>
  
Diretório responsável por armazenar as páginas htmls que serão renderizadas.

Por convenção, dentro de "template" é utilizado nome_do_projeto/nome_da_view.html para que framework reconheça o caminho.
 
 ```
return render(request, 'pollsApp/results.html',{'question':question})
 ```

### Páginas html

* pages : página inicial que será carregada através do LandingPage.
* partials: Uma parte que será carregada em todas as partes do projeto, no caso a barra de navegação.
* polls: Estrutura relacionada à votação contendo resultados, detalhes e o índex.
* base: Corpo principal que será utilizado como base onde as views (pages e polls) serão renderizadas.


### Formato das urls dentro do html

```
{% url 'polls:vote' question.id %}"
```

* polls referente ao app_name = ‘polls’ em urls.py.

* vote referente ao name do path em urls.py.

* question.id referente ao id da questão passando como parâmetro em urls.py. 


```
app_name = 'polls'
path('<int:question_id>/vote/', views.vote, name='vote'),
```


### Views.py
Responsável pelo controller de renderização de views e fluxo de dados da enquete.

```
# Get questions and display those questions
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)

# Show question and choices
def detail(request, question_id):
    try:
        question = Question.objects.get(pk = question_id)
    except Question.DoesNotExist:
        raise Http404('Question does not exist')
    return render(request, 'polls/detail.html', 
                    {'question': question})

#Get question and display results
def results(request, question_id):
    question = get_object_or_404(Question, pk = question_id)
    return render(request, 'polls/results.html',                  
                    {'question':question})

# Vote for a qerstion choice
def vote(request, question_id):
    question = get_object_or_404(Question, pk = question_id)
    try:
        selected_choice = question.choice_set.get(pk = request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        return render(request, 'polls/detail.html', 
                      { 'question': question,
                        'error_message': 'You did not select a choice.'})
    else:
        selected_choice.votes += 1
        selected_choice.save()
        return HttpResponseRedirect(reverse('polls:results', args = (question.id,)))
```



</details>


# Resultado

## Index

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/f9dfef95-c1d9-4a02-a7d2-024143239a30" style="width:100%">
</div>

## Questões

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/d8620d7a-1405-460b-a2f9-ff8fe0c6f426" style="width:100%">
</div>

## Votação

<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/dd864ebb-130b-4578-b216-6facc577e02e" style="width:100%">
</div>

## Votos


<div align="center">
<img src="https://github.com/lucasmargui/Django_Projeto_Enquete/assets/157809964/68504e9c-6b38-4c29-9dc2-c31d57b31558" style="width:100%">
</div>
