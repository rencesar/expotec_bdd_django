---
layout: slide
---

<section data-markdown><script type="text/template">

# BDD Pythonico com Django
## Behave Drive Development

### Desenvolvimento orientado a escrita de testes por comportamento.

</script></section>

<section data-markdown><script type="text/template">

## Quem sou eu?

Renato César, 20 anos, graduando de Sistemas para Internet, Desenvolvedor Django.


</script></section>

<section data-markdown><script type="text/template">


## BDD entenda mesmo sendo Trouxa

(img de hari do potter)

Originado do TDD (Test-Drive-Development), o BDD foca na escrita de testes por comportamento, permitindo desta forma pessoas sem conhecimento tecnico entender o que está acontecendo.


</script></section>

<section data-markdown><script type="text/template">


## Beneficios

Certo mais quais os beneficios de utilizar?

====

### Desenvolvimento Agil

Escrever testes demanda tempo? Não com BDD, uma vez escrito um teste parecido por que não o reusar apenas escrevendo seu comportamento?
* Efeito **Snowball**
* Variação
* Reuso de código
* Inclusão das outras *roles*


### 10000% COVERAGE

Muitas vezes testes unitarios *não dão conta do recado*, na cobertura de testes da sua aplicação. Mas se juntarmos todos os testes unitarios, e os chamar na ordem correta, tudo muda.


### Legibilidade

Fator principal do BDD, nada como ler um código em linguagem *humana*.

```
#language: pt
Funcionalidade: Realizar login

	Como visitante
	Afim de fazer login no sistema
	Devo ser capaz de acessar a respectiva conta


	Cenário: Fazer login com dados corretos
		Dado que acesso a página inicial
		E que acesso como visitante
		E possuo uma conta cadastrada com dados conforme abaixo:
			|  NOME             |  USERNAME |  EMAIL               |  SENHA     |
			|  Anderson Carlos  |  anderson |  anderson@gmail.com  |  senha123  |
		Quando clico no botão "Fazer Login"
		E preencho os dados de login conforme abaixo:
			|  USERNAME  |  SENHA     |
			|  anderson  |  senha123  |
		E clico no botão "Entrar"
		Então estárei na página "Login efetuado com sucesso!"
```


</script></section>

<section data-markdown><script type="text/template">


## Como utilizar

Seguindo os mesmos principios do TDD, o ponto chave é escrever os testes antes da aplicação. Os testes consistem em três etapas:

1. Escrever o comportamento *falho*
2. Escrever os testes unitarios *falhos*
3. Fazer os testes unitarios passarem
4. Refatora código
5. Faz o teste de aceitação passar

<IMG bdd-cicle>


</script></section>

<section data-markdown><script type="text/template">



## Exemplo de Implementação

```
#language: pt
Funcionalidade: Filtro usuarios pelo interesse

    Como usuario padrão
    Afim de filtrar usuarios pela suas listas de interesses
    Devo ser capaz de filtrar usuarios que compartilhem os mesmos interesses

    Contexto: Existem usuarios e interesses cadastrados
        Dado que existem interesses cadastrados
        E existem usuarios com diferentes interesses

    Cenário: Filtrar usuario por um interesse
        Dado que estou logado como usuario
        Quando filtro usuarios por um unico interesse
        Então verei uma lista de usuarios que compartilham daquele interesse

    Cenário: Filtrar usuarios por multiplos interesses
        Dado que estou logado como usuario
        Quando filtro usuarios por multiplos interesses
        Então verei uma lista de usuarios que compartilham daqueles interesses

    Scenario: Sem resultado na busca
        Dado que estou logado como usuario
        Quando filtro usuarios por um interesses que nenhum usuario possui
        Então não verei nenhum usuario
```

====

### Refatorando

```
#language: pt
Funcionalidade: Filtro usuarios pelo interesse

    Como usuario padrão
    Afim de filtrar usuarios pela suas listas de interesses
    Devo ser capaz de filtrar usuarios que compartilhem os mesmos interesses

    Contexto: Existem usuarios e interesses cadastrados
        Dado que existem os seguintes interesses cadastrados:
            | INTERESSE       |
            | Django          |
            | Testing         |
            | Public Speaking |
            | DevOps          |
            | PHP             |
        E existem os seguintes usuarios com diferentes interesses:
            | NOME                | INTERESSE       |
            | Eduardo Carlos      | Django, Testes  |
            | João das Neves      | Django, Python  |
            | Serverino Serafino  | Testes, Devops  |
            | Romero Brito        | Python, DevOps  |

    Esquema do Cenário: Filtrar usuarios
        Dado que estou logado como usuario
        Quando filtro usuarios por <INTERESSE>
        Então verei <NUMERO> de usuarios que compartilham desse interesse

    Exemplo:
        | INTERESSE       | NUMERO  |
        | Django          | 2       |
        | Django, Testes  | 3       |
        | PHP             | 0       |
```


</script></section>

<section data-markdown><script type="text/template">


# Django

Django é um framework MVC que tem como principal característica o DRY(don't repeat yourself)


</script></section>

<section data-markdown><script type="text/template">


## Dicas básicas

* Class-based-view
* Pep-8
* Zen do Python
* Virtualenv-wrapper


</script></section>

<section data-markdown><script type="text/template">


# Vamos a prática


</script></section>

<section data-markdown><script type="text/template">


# Preparando ambiente

### Ferramentas
* Python 3.6
* Django 1.11
* aloe_django 0.1.3 *(BDD)*
* django_nose *(Testes unitarios)*
* splinter *(Testes automaticos web)*


</script></section>

<section data-markdown><script type="text/template">


## Iniciando ambiente Virtual

Utilize virtualenvwrapper como ambiente virtual, verifique a instalação:
https://virtualenvwrapper.readthedocs.io/en/latest/

```
$ mkvirtualenv -p python3.6 expotec_bdd
(expotec_bdd)$ cd Projeto
(expotec_bdd)$ setvirtualenvproject
(expotec_bdd)$ pip install django==1.11 aloe-django==0.1.3 django-nose==1.4.4 splinter==0.7.6
```

*Utilizaremos chromedrive para os testes então baixe pelo site: https://sites.google.com/a/chromium.org/chromedriver/downloads*


</script></section>

<section data-markdown><script type="text/template">


## Inicie o Django

Crie um projeto com nome "Expotec_Bdd" e uma aplicação de nome "crud"
```
$ django-admin startproject expotec_bdd .
$ python manage.py startapp crud
```


</script></section>

<section data-markdown><script type="text/template">


## Resultado

[img] tree.png


</script></section>

<section data-markdown><script type="text/template">


## Configurando Django

```
INSTALLED_APPS = [
		...
		'aloe_django',
		'django_nose',
		'crud.apps.CrudConfig',                                  
]


# Bdd Test Aloe

TEST_RUNNER = 'django_nose.NoseTestSuiteRunner'

GHERKIN_TEST_CLASS = 'aloe_django.TestCase'

GHERKIN_TEST_RUNNER = 'aloe_django.runner.GherkinTestRunner'
```


</script></section>

<section data-markdown><script type="text/template">


## Configurando o Aloe

* Crie uma pasta **features**
* Dentro de **features** crie um arquivo em branco **hooks.py**
* Ainda dentro de features crie outra pasta **crud** e dentro dela um **steps.py**
* Dentro de **crud** crie um arquivo **primeira.feature**


**Lembrete: Dentro de cada pasta criada crie um arquivo __init__.py**


</script></section>

<section data-markdown><script type="text/template">


### Configurando execução dos testes

Quem ordena as execuções é o arquivo **hooks.py**, então defina que irá utilizar o chromedrive, como plataforma de testes.

```
from aloe import world, before, after
from splinter import Browser


@before.all
def before_all():
	world.browser = Browser('chrome')


@after.all
def teardown_browser():
    world.browser.quit()
```


</script></section>

<section data-markdown><script type="text/template">


## Primeira funcionalidade

Minha primeira funcionalidade sera fazer uma página de cadastro de usuario. Então meu comportamento sera algo assim:

```
#language: pt
Funcionalidade: Cadastro no sistema

	Como visitante
	Afim de criar uma conta no sistema
	Devo ser capaz de preencher meus dados e possuir uma conta

	@wip
	Cenário: Criar uma conta de usuário
		Quando acesso a página inicial
		E clico no link "Criar Conta"
		E preencho um formulário de cadastro de usuário conforme abaixo:
			|  NOME             |  USERNAME  |  EMAIL               |  SENHA     |
			|  Chafundifórnio   |  chaf      |  chaf_und@gmail.com  |  senha123  |
		E clico no botão "Cadastrar"
		Então estárei na página "Usuário cadastrado com sucesso!"
		E terei um usuário "chaf" cadastrado
```


</script></section>

<section data-markdown><script type="text/template">


## Escrever os testes unitarios

Em steps.py você deve criar os testes unitarios e atribuir rotulos a eles para cada linha do comportamento, exemplo:

```
from aloe import step, world
from aloe_django import django_url
from nose.tools import assert_true


@step(r'acesso a página inicial')
def initial_page_access(step):
    world.browser.visit(django_url(step))


@step(u'clico no botão "([^"]*)"')
def click_on_button(step, button):
    button = '//button[text()="{0}"]'.format(button)
    world.browser.find_by_xpath(button).first.click()


@step(u'clico no link "([^"]*)"')
def click_on_button(step, link):
    world.browser.click_link_by_text(link)


@step(u'estárei na página "([^"]*)"')
def in_page_with_message(step, message):
    assert_true(world.browser.is_text_present(message))


@step('preencho um formulário de cadastro de usuário conforme abaixo')
def fill_user_registration_form(step):
    for row in step.hashes:
        world.browser.fill('first_name', row['NOME'])
        world.browser.fill('username', row['USERNAME'])
        world.browser.fill('email', row['EMAIL'])
        world.browser.fill('password', row['SENHA'])


@step('terei um usuário "([^"]*)" cadastrado')
def has_registered_user(step, user):
    assert_true(User.objects.filter(username=user).exists())

```


</script></section>

<section data-markdown><script type="text/template">


## Escrever a aplicação

Após escrever o comportamento e os testes unitarios é hora de escrever a aplicação. Precisamos criar uma view, para página inicial (até então em branca), e uma página de cadastro.



</script></section>

<section data-markdown><script type="text/template">


## Views

```
from django.contrib import messages
from django.contrib.auth.models import User
from django.shortcuts import render
from django.urls import reverse
from django.views import generic


def index(request):
    return render(request, 'index.html')


class CreateUserView(generic.CreateView):
    model = User
    fields = ['first_name', 'username', 'email', 'password']
    template_name = 'cadastro.html'

    def get_success_url(self):
        messages.success(self.request, ('Usuário cadastrado com sucesso!'))
        return reverse('crud:home')
```


</script></section>

<section data-markdown><script type="text/template">



## Urls

**expotec_bdd/urls.py**
```
from django.conf.urls import url, include
from django.contrib import admin


urlpatterns = [
    url(r'^', include('crud.urls', namespace='crud')),
    url(r'^admin/', admin.site.urls),
]
```

**crud/urls.py**
```
from django.conf.urls import url

from . import views


urlpatterns = [
    url(r'^$', views.index, name='home'),
    url(r'^cadastro/', views.CreateUserView.as_view(), name='cadastro'),
]
```


</script></section>

<section data-markdown><script type="text/template">


## Templates

Dentro da pasta **crud** crie outra pasta chamada **templates**, e nela crie dois outros arquivos (*index.html*, *cadastro.html*)

**index.html**
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Index</title>
  </head>
  <body>
    <a href="&#123;% url 'crud:cadastro' %}">Criar Conta</a>
    &#123;% for message in messages %}&#123;{ message }}&#123;% endfor %}
  </body>
</html>
```

**cadastro.html**
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Cadastro</title>
  </head>
  <body>
    <form method="post">
      &#123;% csrf_token %}
      <dl>
        &#123;% for field in form %}
          <dt>&#123;{ field.label }}:</dt>
          <dd>&#123;{ field }}</dd>
        &#123;% endfor %}
      </dl>
      <button type="submit" name="button">Cadastrar</button>
    </form>
  </body>
</html>
```


</script></section>

<section data-markdown><script type="text/template">


## Pronto!!!

Com esses passos concluidos você ja implementou seu código e os testes dele, seguindo a filosofia do BDD e do Django, sempre respeitando a PEP-8 conforme explicado.
Feito isso execute o seguinte comando para testar seu código:
```(expotec_bdd)$ python manage.py harvest --attr wip```
Veja a mágica acontecer!

Para um teste mais verboso, e com menos informações *inuteis* utilize:
```(expotec_bdd)$ python manage.py harvest -v 3 --attr wip --nologcapture --nocapture```

Quer rodar um pdb? É bem simples, so passar como parametro --pdb!
