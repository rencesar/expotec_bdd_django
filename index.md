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

[Renato César](https://github.com/rencesar), 20 anos, graduando de Sistemas para Internet, Desenvolvedor Django.


</script></section>

<section data-markdown data-background-image="{{ "/images/trouxas.jpg" | prepend: site.baseurl }}"><script type="text/template">


##### BDD entenda mesmo sendo Trouxa


<div style="background-color: white;
border-radius: 15px;
-webkit-box-shadow: 3px 5px 12px 3px rgba(0,0,0,0.65);
-moz-box-shadow: 3px 5px 12px 3px rgba(0,0,0,0.65);
box-shadow: 3px 5px 12px 3px rgba(0,0,0,0.65);">
Originado do TDD (Test-Drive-Development), o BDD foca na escrita de testes por comportamento, permitindo desta forma pessoas sem conhecimento técnico entender o que está acontecendo.
</div>


</script></section>

<section data-markdown><script type="text/template">


## Benefícios

Certo, mas quais os benefícios de utilizar?

<img class="plain" src={{ "/images/beneficios.jpg" | prepend: site.baseurl }}>

</script></section>

<section data-markdown><script type="text/template">


### Desenvolvimento Agil

Escrever testes demanda tempo? Não com BDD, uma vez escrito um teste parecido por que não o reusar apenas escrevendo seu comportamento?
* Efeito **Snowball**
* Variação
* Reuso de código
* Inclusão das outras *roles*


</script></section>

<section data-markdown><script type="text/template">

### 10000% COVERAGE

Muitas vezes testes unitários *não dão conta do recado*, na cobertura de testes da sua aplicação. Mas se juntarmos todos os testes unitários, e os chamar na ordem correta, tudo muda.



</script></section>

<section data-markdown><script type="text/template">


### Legibilidade

Fator principal do BDD, nada como ler um código em linguagem *humana*.

<img class="plain" src={{ "/images/primeira_func_bdd.png" | prepend: site.baseurl }}>


</script></section>

<section data-markdown><script type="text/template">


## Como utilizar

Seguindo os mesmos princípios do TDD, o ponto chave é escrever os testes antes da aplicação. Os testes consistem em três etapas:

1. Escrever o comportamento *falho*
2. Escrever os testes unitários *falhos*
3. Fazer os testes unitários passarem
4. Refatora código
5. Faz o teste de aceitação passar

</script></section>

<section data-markdown><script type="text/template">

<img class="plain" src={{ "/images/bdd-cicle.png" | prepend: site.baseurl }}>


</script></section>

<section data-markdown><script type="text/template">



## Exemplo de Implementação

<img class="plain" src={{ "/images/exemple_simple.png" | prepend: site.baseurl }}>

</script></section>

<section data-markdown><script type="text/template">


### Refatorando

<img class="plain" src={{ "/images/exemple_refactor.png" | prepend: site.baseurl }}>


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
* Factory-Boy
* Faker


</script></section>

<section data-markdown><script type="text/template">


# Vamos a prática


</script></section>

<section data-markdown><script type="text/template">


## Preparando ambiente

### Ferramentas
* Python 3.6
* Django 1.11
* aloe_django 0.1.3 *(BDD)*
* django_nose *(Testes unitários)*
* splinter *(Testes automaticos web)*


</script></section>

<section data-markdown><script type="text/template">


## Iniciando ambiente Virtual

Utilize virtualenvwrapper como ambiente virtual, verifique a instalação:
https://virtualenvwrapper.readthedocs.io/en/latest/

{% highlight python %}
$ mkvirtualenv -p python3.6 expotec_bdd
(expotec_bdd)$ cd Projeto
(expotec_bdd)$ setvirtualenvproject
(expotec_bdd)$ pip install django==1.11 aloe-django==0.1.3 django-nose==1.4.4 splinter==0.7.6

{% endhighlight %}

*Utilizaremos chromedrive para os testes então baixe pelo site: https://sites.google.com/a/chromium.org/chromedriver/downloads*


</script></section>

<section data-markdown><script type="text/template">


## Inicie o Django

Crie um projeto com nome "Expotec_Bdd" e uma aplicação de nome "crud"
{% highlight python %}
$ django-admin startproject expotec_bdd .
$ python manage.py startapp crud
{% endhighlight	%}

</script></section>

<section data-markdown><script type="text/template">


## Resultado

<img class="plain" src={{ "/images/tree.png" | prepend: site.baseurl }}>


</script></section>

<section data-markdown><script type="text/template">


## Configurando Django

{% highlight python %}
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
{% endhighlight %}


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

{% highlight python %}
from aloe import world, before, after
from splinter import Browser


@before.all
def before_all():
	world.browser = Browser('chrome')


@after.all
def teardown_browser():
    world.browser.quit()

{% endhighlight %}


</script></section>

<section data-markdown><script type="text/template">


## Primeira funcionalidade

Minha primeira funcionalidade será fazer uma página de cadastro de usuário. Então meu comportamento será algo assim:

<img class="plain" src={{ "/images/primeira_func_bdd.png" | prepend: site.baseurl }}>

</script></section>

<section data-markdown><script type="text/template">


## Escrever os testes unitários

Em steps.py você deve criar os testes unitários e atribuir rótulos a eles para cada linha do comportamento, exemplo:

{% highlight python %}
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


@step(u'estarei na página "([^"]*)"')
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
{% endhighlight %}


</script></section>

<section data-markdown><script type="text/template">


## Escrever a aplicação

Após escrever o comportamento e os testes unitários é hora de escrever a aplicação. Precisamos criar uma view, para página inicial (até então em branca), e uma página de cadastro.



</script></section>

<section data-markdown><script type="text/template">


## Views

{% highlight python %}
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
{% endhighlight %}


</script></section>

<section data-markdown><script type="text/template">



## Urls

**expotec_bdd/urls.py**
{% highlight python %}
from django.conf.urls import url, include
from django.contrib import admin


urlpatterns = [
	url(r'^', include('crud.urls', namespace='crud')),
	url(r'^admin/', admin.site.urls),
]
{% endhighlight %}

**crud/urls.py**
{% highlight python %}
from django.conf.urls import url

from . import views


urlpatterns = [
	url(r'^$', views.index, name='home'),
	url(r'^cadastro/', views.CreateUserView.as_view(), name='cadastro'),
]
{% endhighlight %}


</script></section>

<section data-markdown><script type="text/template">


## Templates

Dentro da pasta **crud** crie outra pasta chamada **templates**, e nela crie dois outros arquivos (*index.html*, *cadastro.html*)


</script></section>

<section data-markdown><script type="text/template">
	### index.html
	{% highlight html %}
	{% raw %}
	<!DOCTYPE html>
	<html>
	  <head>
	    <meta charset="utf-8">
	    <title>Index</title>
	  </head>
	  <body>
	    <a href="{% url 'crud:cadastro' %}">Criar Conta</a>
	    {% for message in messages %}{{ message }}{% endfor %}
	  </body>
	</html>
	{% endraw %}
	{% endhighlight %}
</script></section>

<section data-markdown><script type="text/template">
### cadastro.html
{% highlight html linenos %}
	{% raw %}
	<!DOCTYPE html>
	<html>
	  <head>
	    <meta charset="utf-8">
	    <title>Cadastro</title>
	  </head>
	  <body>
	    <form method="post">
	      {% csrf_token %}
	      <dl>
	        {% for field in form %}
	          <dt>{{ field.label }}:</dt>
	          <dd>{{ field }}</dd>
	        {% endfor %}
	      </dl>
	      <button type="submit" name="button">Cadastrar</button>
	    </form>
	  </body>
	</html>
	{% endraw %}
{% endhighlight %}

</script></section>

<section data-markdown><script type="text/template">


## Pronto!!!

Com esses passos concluídos você já implementou seu código e os testes dele, seguindo a filosofia do BDD e do Django, sempre respeitando a PEP-8 conforme explicado.
Feito isso execute o seguinte comando para testar seu código:
```
(expotec_bdd)$ python manage.py harvest --attr wip
```
Veja a mágica acontecer! Para um teste mais verboso, e com menos informações *inúteis* utilize:
```
(expotec_bdd)$ python manage.py harvest -v 3 --attr wip --nologcapture --nocapture
```

Quer rodar um pdb? É bem simples, só passar como parametro "*--pdb*"!

</script></section>

<section data-markdown><script type="text/template">

# Perguntas?

</script></section>

<section data-markdown><script type="text/template">

## Renato César Lira Borges
### https://github.com/rencesar
### [ren_cesar@outlook.com](mailto:ren_cesar@outlook.com)
### +55 (83) 99860-5008

</script></section>
