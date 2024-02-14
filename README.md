<H1 align="center">Estrutura Votação</H1>
<p align="center">🚀 Projeto de criação de uma estrutura de enquete utilizando Django para referências futuras</p>

## Recursos Utilizados

* Django 5.0.2
* Python 3.10


## Criação do PollProject

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


<img src="https://cdn.discordapp.com/attachments/1046824853015113789/1207331165669826580/image.png?ex=65df41c5&is=65ccccc5&hm=0294fac52d4376cda51760f25d74e069ce653511c22e5253b6614a27c0387a11&" alt="">


## Criação do landingPage

Criação de um pacote que será responsável pela páginal inicial de carregamento.

 ```
python manage.py startapp landingPage
 ```

## Criação do pollApp

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

 

<img src="https://cdn.discordapp.com/attachments/1046824853015113789/1207323401904201778/image.png?ex=65df3a89&is=65ccc589&hm=4085174401c7f2e3e869aa765ec36fcaa189ba29a6d30933dcda2d560d5142de&" alt="">
 
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


## Criação Template

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


# Resultado

## Index
<img src="https://cdn.discordapp.com/attachments/1046824853015113789/1207328378819059742/image.png?ex=65df3f2c&is=65ccca2c&hm=236341e157657a1eaef317a33f7e729928438890af8a6cef333785119eee53b7&" alt="">

## Questões
<img src="https://cdn.discordapp.com/attachments/1046824853015113789/1207328471437807707/image.png?ex=65df3f42&is=65ccca42&hm=64428f86bb9ddee4e8c7e9383b54b80f08e493d782225ed42224e8e9987c98ad&" alt="">

## Votação
<img src="https://cdn.discordapp.com/attachments/1046824853015113789/1207328605764329513/image.png?ex=65df3f62&is=65ccca62&hm=386839c5344cd943b282b4ce65f1dc47cd448410be701f6bbad9aab52c7a4720&" alt="">

## Votos
<img src="https://cdn.discordapp.com/attachments/1046824853015113789/1207328705513390110/image.png?ex=65df3f7a&is=65ccca7a&hm=717bee5808c78d7d8f9ce8aa78494ea4fbb8bbdaca702451a5b0c9de647aeaf8&" alt="">
