# 🐍 Guia de Aprendizado: Git + UV + Django

> **Para estagiários** — Este guia ensina do zero como usar **Git** (controle de versão), **UV** (gerenciador de pacotes Python moderno) e **Django** (framework web).  
> Siga o passo a passo com atenção e crie seu próprio projeto na prática.

---

## 📋 Índice

1. [O que você vai aprender](#-o-que-você-vai-aprender)
2. [Pré-requisitos](#️-pré-requisitos)
3. [Parte 1 — Git](#-parte-1--git)
   - [O que é Git?](#o-que-é-git)
   - [Configurando o Git](#configurando-o-git)
   - [Clonando o repositório](#clonando-o-repositório)
   - [Criando o .gitignore](#criando-o-gitignore)
4. [Parte 2 — UV Python](#-parte-2--uv-python)
   - [O que é o UV?](#o-que-é-o-uv)
   - [Instalando o UV](#instalando-o-uv)
   - [Comandos essenciais do UV](#comandos-essenciais-do-uv)
   - [Criando um projeto com UV](#criando-um-projeto-com-uv)
5. [Parte 3 — Django](#-parte-3--django)
   - [O que é Django?](#o-que-é-django)
   - [Criando o projeto Django](#criando-o-projeto-django)
   - [Estrutura do projeto](#estrutura-do-projeto-django)
   - [Rodando o servidor de desenvolvimento](#rodando-o-servidor-de-desenvolvimento)
   - [Criando um App](#criando-um-app-django)
   - [Estrutura de um App](#estrutura-de-um-app)
6. [Parte 4 — Construindo sua primeira aplicação](#-parte-4--construindo-sua-primeira-aplicação)
   - [Banco de Dados e Models](#banco-de-dados-e-models)
   - [Migrations](#migrations)
   - [Django Admin](#django-admin)
   - [Views e URLs](#views-e-urls)
   - [Templates](#templates)
   - [Formulários e votação](#formulários-e-votação)
   - [Views Genéricas](#views-genéricas)
   - [Testes automatizados](#testes-automatizados)
   - [Arquivos Estáticos](#arquivos-estáticos)
   - [Customizando o Admin](#customizando-o-admin)
7. [Referência rápida de comandos](#-referência-rápida-de-comandos)
8. [Solução de problemas comuns](#-solução-de-problemas-comuns)

---

## 🎯 O que você vai aprender

Ao final deste guia, você será capaz de:

- Usar **Git** para versionar e compartilhar código
- Gerenciar projetos Python de forma moderna com **UV**
- Criar um projeto **Django** completo do zero
- Entender a estrutura de pastas e a função de cada arquivo
- Criar **Models**, **Views**, **URLs** e **Templates**
- Utilizar o painel administrativo do Django
- Escrever **testes automatizados** básicos
- Servir arquivos estáticos (CSS, JavaScript e imagens)

---

## 🛠️ Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- **Git** — [Download em git-scm.com](https://git-scm.com/)
- **Visual Studio Code** — [Download em code.visualstudio.com](https://code.visualstudio.com/)
- **WSL** (fortemente recomendado para usuários Windows):

  1. Abra o **PowerShell como administrador**
  2. Execute o comando abaixo, substituindo `"SeuNomePersonalizado"` pelo seu próprio nome (ex: `"FulanoWSL"`):
     ```powershell
     wsl --install Ubuntu --name "SeuNomePersonalizado"
     ```
  3. Reinicie o computador quando solicitado
  4. Após reiniciar, abra o WSL e configure seu usuário e senha no Ubuntu

> 💡 **Por que usar WSL?** Muitas ferramentas Python e o Django funcionam melhor em Linux. O WSL permite rodar um ambiente Linux diretamente no Windows, evitando problemas de compatibilidade.

> ⚠️ **Todos os comandos deste guia devem ser executados no terminal WSL**, não no PowerShell nativo do Windows.

---

## 🗂️ Parte 1 — Git

### O que é Git?

**Git** é o sistema de controle de versão mais usado no mundo. Ele registra o histórico de todas as mudanças no seu código, permitindo:

- Voltar a versões anteriores se algo der errado
- Trabalhar em equipe sem sobrescrever o código dos colegas
- Manter um histórico completo do projeto

> 💡 Pense no Git como um "salvar com histórico infinito" para o seu código.

---

### Configurando o Git

Antes de usar o Git pela primeira vez, configure sua identidade. Esses dados aparecerão em cada commit que você fizer.

```bash
# Configure seu nome
git config --global user.name "Seu Nome Completo"

# Configure seu email
git config --global user.email "seu.email@example.com"

# Verifique se as configurações foram salvas
git config --list | grep user
```

> 💡 O `--global` aplica a configuração a todos os projetos do seu computador. Sem ele, a configuração valeria apenas para o projeto atual.

---

### Clonando o repositório

Agora vamos baixar o repositório do guia e criar sua branch de trabalho:

```bash
# 1. Clone o repositório
git clone https://github.com/PedroJao/Trainee

# 2. Entre na pasta do projeto
cd Trainee

# 3. Crie uma branch pessoal para seu trabalho (substitua SeuNome)
git checkout -b teste_SeuNome

# 4. Confirme que está na branch correta
git status
```

A saída do `git status` deve mostrar algo como:
```
On branch teste_SeuNome
nothing to commit, working tree clean
```

> 💡 **O que é uma branch?** É uma linha de desenvolvimento paralela. Criar sua própria branch garante que suas alterações não afetam o trabalho dos outros estagiários.

---

### Criando o .gitignore

O `.gitignore` lista arquivos e pastas que o Git deve **ignorar** — ou seja, nunca versionar. Isso é importante para não commitar arquivos temporários, senhas ou ambientes virtuais.

Dentro da pasta `Trainee`, crie o arquivo:

```bash
cat > .gitignore << 'EOF'
# Ambiente virtual do UV/Python
.venv/

# Arquivos compilados do Python
__pycache__/
*.py[cod]

# Banco de dados local (não deve ir para o repositório)
db.sqlite3
*.sqlite3

# Variáveis de ambiente e segredos
.env
*.env

# Arquivos do VS Code
.vscode/

# Arquivos do sistema operacional
.DS_Store
Thumbs.db
EOF
```

Confirme que o arquivo foi criado:

```bash
cat .gitignore
```

> ⚠️ **Nunca commite a pasta `.venv/`** — ela contém o ambiente virtual que é específico para cada máquina e pode ter centenas de megabytes.

---

## 🚀 Parte 2 — UV Python

### O que é o UV?

O **UV** é uma ferramenta moderna e extremamente rápida para gerenciar:

- **Versões do Python** — instale e alterne entre diferentes versões
- **Ambientes virtuais** — isolamento de dependências por projeto
- **Pacotes** — instale, atualize e remova bibliotecas Python
- **Projetos** — crie e gerencie projetos com um único arquivo de configuração

Pense no UV como um substituto mais rápido e completo para o `pip` + `venv` + `pyenv` combinados. Ele foi escrito em **Rust**, o que o torna muito mais veloz que as ferramentas tradicionais.

> 💡 **Por que usar UV?** Projetos Python profissionais precisam de ambientes isolados para que as dependências de um projeto não conflitem com as de outro. O UV torna isso simples e rápido.

---

### Instalando o UV

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Após a instalação, **feche e reabra o terminal** para que o comando `uv` seja reconhecido. Depois verifique:

```bash
uv --version
```

Se você ver a versão do UV impressa, a instalação funcionou.

---

### Comandos essenciais do UV

#### Gerenciamento de Python

```bash
# Ver versões de Python disponíveis para instalar
uv python list

# Instalar uma versão específica do Python
uv python install 3.12

# Verificar qual Python está sendo usado no projeto atual
uv python find
```

#### Gerenciamento de projetos

```bash
# Criar um novo projeto Python
uv init nome-do-projeto

# Entrar na pasta do projeto
cd nome-do-projeto
```

> 💡 O UV cria automaticamente o ambiente virtual (`.venv/`) na primeira vez que você roda `uv add`. **Não é necessário** criar ou ativar o ambiente manualmente — use sempre `uv run` para executar comandos.

#### Gerenciamento de pacotes

```bash
# Instalar um pacote
uv add django

# Instalar um pacote apenas para desenvolvimento (ex: ferramentas de teste)
uv add --dev pytest

# Remover um pacote
uv remove nome-do-pacote

# Instalar todas as dependências listadas no pyproject.toml
# (útil ao clonar um repositório já existente)
uv sync

# Listar pacotes instalados
uv pip list

# Executar qualquer comando dentro do ambiente virtual
uv run python manage.py runserver
```

> ⚠️ **Sempre use `uv run`** para executar scripts e comandos Django. Isso garante que o ambiente virtual correto está sendo usado, sem precisar ativá-lo manualmente.

---

### Criando um projeto com UV

Certifique-se de estar dentro da pasta `Trainee` antes de continuar:

```bash
# Confirme em qual pasta você está
pwd
# A saída deve terminar em /Trainee
```

Agora crie o projeto:

```bash
# 1. Crie o projeto UV
uv init meu-projeto-django

# 2. Entre na pasta do projeto
cd meu-projeto-django

# 3. Adicione o Django como dependência
uv add django

# 4. Verifique se o Django foi instalado corretamente
uv run python -m django --version
```

Se você ver a versão do Django (ex: `5.2`), parabéns — o ambiente está pronto!

O UV criou automaticamente os seguintes arquivos:

| Arquivo/Pasta | O que é |
|---|---|
| `.venv/` | Ambiente virtual com o Python e os pacotes instalados |
| `pyproject.toml` | Configurações e dependências do projeto |
| `uv.lock` | Versões exatas de cada pacote (garante que todos usem as mesmas versões) |

> ⚠️ **Commite sempre** o `pyproject.toml` e o `uv.lock`, mas **nunca** commite o `.venv/` (já está no `.gitignore` que criamos).

---

## 🌐 Parte 3 — Django

### O que é Django?

**Django** é um framework web Python de alto nível, gratuito e de código aberto. Ele segue o padrão **MTV (Model - Template - View)**:

- **Model** → Define a estrutura dos dados (tabelas do banco de dados)
- **Template** → Define como os dados são apresentados (HTML)
- **View** → Define a lógica que conecta Models e Templates

Django já vem com muitas funcionalidades prontas, como painel administrativo automático, ORM para banco de dados, sistema de autenticação e proteção contra ataques comuns (CSRF, SQL Injection, XSS).

---

### Criando o projeto Django

Ainda dentro da pasta `meu-projeto-django`, execute:

```bash
uv run django-admin startproject meusite djangotutorial
```

Esse comando cria uma pasta `djangotutorial` com o projeto Django dentro. Sua estrutura de arquivos agora está assim:

```
Trainee/
└── meu-projeto-django/
    ├── .venv/
    ├── pyproject.toml
    ├── uv.lock
    └── djangotutorial/          ← pasta criada pelo django-admin
        ├── manage.py
        └── meusite/
            ├── __init__.py
            ├── settings.py
            ├── urls.py
            ├── asgi.py
            └── wsgi.py
```

> ⚠️ **Atenção:** Não nomeie seu projeto Django como `django` ou `test`, pois esses nomes conflitam com componentes internos do Python.

Entre na pasta onde está o `manage.py` — é daqui que todos os comandos Django serão executados:

```bash
cd djangotutorial
```

---

### Estrutura do Projeto Django

#### `manage.py`

O **coração do gerenciamento do projeto**. Você usará este arquivo para executar praticamente todos os comandos Django.

```bash
# Exemplos de uso (todos rodados a partir da pasta djangotutorial/):
uv run python manage.py runserver        # Inicia o servidor de desenvolvimento
uv run python manage.py migrate          # Aplica migrations ao banco de dados
uv run python manage.py createsuperuser  # Cria um usuário administrador
uv run python manage.py startapp polls   # Cria um novo app
uv run python manage.py shell            # Abre shell Python com o projeto carregado
```

> 🚫 **Nunca modifique o `manage.py`** — ele deve permanecer como foi gerado.

---

#### `meusite/settings.py`

O **arquivo de configuração central** do projeto. Os principais campos que você vai usar:

```python
# Modo de depuração — NUNCA deixe True em produção
DEBUG = True

# Apps instalados no projeto
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# Banco de dados (padrão: SQLite — perfeito para desenvolvimento)
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Fuso horário
TIME_ZONE = 'America/Sao_Paulo'
```

---

#### `meusite/urls.py`

O **roteador principal** do projeto — diz ao Django para onde encaminhar cada requisição. Editaremos este arquivo quando criarmos nosso app.

---

#### `meusite/asgi.py` e `meusite/wsgi.py`

Pontos de entrada para servidores web em produção. Durante o desenvolvimento local, você não precisa se preocupar com esses arquivos.

---

### Rodando o servidor de desenvolvimento

Antes de iniciar o servidor, aplique as migrations iniciais. Elas criam as tabelas padrão do Django no banco de dados (autenticação, sessões, etc.) — aprenderemos migrations em detalhes mais adiante:

```bash
# Execute a partir da pasta djangotutorial/
uv run python manage.py migrate
```

Agora inicie o servidor:

```bash
uv run python manage.py runserver
```

Você verá uma saída similar a:

```
Django version 5.2, using settings 'meusite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Acesse `http://127.0.0.1:8000/` no navegador. Você verá a página de boas-vindas do Django. 🎉

> 💡 Para rodar em uma porta diferente: `uv run python manage.py runserver 8080`

> ⚠️ O servidor **recarrega automaticamente** ao salvar arquivos Python. Mas adicionar novos arquivos pode exigir reinício manual (CTRL+C e rodar novamente).

---

### Criando um App Django

Um **projeto** Django é uma coleção de configurações e **apps**. Um **app** é um módulo independente com uma responsabilidade específica (ex: um blog, um sistema de enquetes, uma loja).

> 💡 **Projeto vs. App:** Um projeto pode conter múltiplos apps. Um app pode ser reutilizado em múltiplos projetos.

Certifique-se de estar na pasta `djangotutorial/` (onde está o `manage.py`) e execute:

```bash
uv run python manage.py startapp polls
```

---

### Estrutura de um App

```
polls/
├── __init__.py           ← Marca como pacote Python
├── admin.py              ← Configuração do painel administrativo
├── apps.py               ← Metadados e configurações do app
├── models.py             ← Modelos (tabelas do banco de dados)
├── tests.py              ← Testes automatizados
├── views.py              ← Lógica das páginas
└── migrations/           ← Histórico de mudanças do banco (gerado automaticamente)
    └── __init__.py
```

Você também criará manualmente:

```
polls/
├── urls.py               ← URLs do app
├── templates/
│   └── polls/
│       ├── index.html
│       ├── detail.html
│       └── results.html
└── static/
    └── polls/
        └── style.css
```

---

## 🏗️ Parte 4 — Construindo sua primeira aplicação

Vamos construir um sistema de **enquetes** (polls). Siga cada seção na ordem!

---

### Banco de Dados e Models

#### Passo 1: Ajustar o fuso horário

Abra `meusite/settings.py` e confirme o fuso horário (já mencionado, mas confirme):

```python
TIME_ZONE = 'America/Sao_Paulo'
```

#### Passo 2: Definir os Models

Edite `polls/models.py`:

```python
import datetime
from django.db import models
from django.utils import timezone


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    def __str__(self):
        return self.question_text

    def was_published_recently(self):
        now = timezone.now()
        # Retorna True apenas para perguntas publicadas no último dia
        # O limite superior (now) evita que datas futuras retornem True
        return now - datetime.timedelta(days=1) <= self.pub_date <= now


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```

> 💡 **Tipos de campos comuns:**
> - `CharField` — texto curto (obrigatório: `max_length`)
> - `TextField` — texto longo
> - `IntegerField` — número inteiro
> - `DateTimeField` — data e hora
> - `BooleanField` — verdadeiro/falso
> - `ForeignKey` — chave estrangeira (relacionamento entre tabelas)

#### Passo 3: Registrar o app no projeto

Edite `meusite/settings.py` e adicione o app à lista `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'polls.apps.PollsConfig',   # ← adicione esta linha
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

---

### Migrations

Migrations são como um **controle de versão para o banco de dados**. Elas registram todas as mudanças nos Models e permitem aplicá-las ao banco de forma controlada.

#### O fluxo de trabalho com migrations

Sempre que você modificar um Model, siga estes 3 passos:

```bash
# 1. Criar a migration (Django detecta o que mudou e gera o arquivo)
uv run python manage.py makemigrations polls

# 2. (Opcional) Ver o SQL que será executado
uv run python manage.py sqlmigrate polls 0001

# 3. Aplicar a migration ao banco de dados
uv run python manage.py migrate
```

> 💡 O comando `uv run python manage.py check` verifica se há problemas no projeto sem tocar no banco de dados.

#### Explorando os dados pelo shell

```bash
uv run python manage.py shell
```

```python
# Dentro do shell:
from polls.models import Question, Choice
from django.utils import timezone

# Criar uma pergunta
q = Question(question_text="O que há de novo?", pub_date=timezone.now())
q.save()

# Listar todas as perguntas
Question.objects.all()

# Filtrar perguntas
Question.objects.filter(question_text__startswith="O que")

# Buscar por chave primária
Question.objects.get(pk=1)

# Criar escolhas relacionadas à pergunta
q = Question.objects.get(pk=1)
q.choice_set.create(choice_text="Nada muito", votes=0)
q.choice_set.create(choice_text="O céu", votes=0)

# Listar escolhas de uma pergunta
q.choice_set.all()
q.choice_set.count()
```

---

### Django Admin

O Django gera automaticamente um painel administrativo completo para gerenciar seus dados.

#### Passo 1: Criar um superusuário

```bash
uv run python manage.py createsuperuser
```

Siga as instruções para definir nome de usuário, email e senha.

#### Passo 2: Registrar o Model no admin

Edite `polls/admin.py`:

```python
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```

#### Passo 3: Acessar o painel

Com o servidor rodando, acesse `http://127.0.0.1:8000/admin/` e faça login com o superusuário criado. Você verá o modelo `Question` disponível para criação, edição e exclusão.

---

### Views e URLs

#### Passo 1: Criar as views

Edite `polls/views.py`:

```python
from django.shortcuts import render, get_object_or_404
from django.http import HttpResponseRedirect
from django.urls import reverse
from django.db.models import F

from .models import Question, Choice


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)


def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})


def results(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/results.html', {'question': question})


def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "Você não selecionou nenhuma opção.",
        })
    else:
        # F() garante que o incremento seja feito pelo banco de dados,
        # evitando perda de votos se dois usuários votarem ao mesmo tempo
        selected_choice.votes = F('votes') + 1
        selected_choice.save()
        # Redireciona após o POST para evitar reenvio ao atualizar a página (padrão PRG)
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

> 💡 **Funções úteis:**
> - `render()` — carrega um template e retorna uma resposta HTTP
> - `get_object_or_404()` — busca um objeto ou retorna erro 404 automaticamente
> - `HttpResponseRedirect()` — redireciona para outra URL
> - `reverse()` — gera a URL a partir do nome da view (evita hardcode de caminhos)
> - `F('campo')` — realiza operações diretamente no banco, evitando condições de corrida

#### Passo 2: Criar o arquivo de URLs do app

Crie o arquivo `polls/urls.py` (esse arquivo não existe ainda, crie-o do zero):

```python
from django.urls import path
from . import views

app_name = 'polls'  # Define o namespace do app

urlpatterns = [
    # /polls/
    path('', views.index, name='index'),
    # /polls/5/
    path('<int:question_id>/', views.detail, name='detail'),
    # /polls/5/results/
    path('<int:question_id>/results/', views.results, name='results'),
    # /polls/5/vote/
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```

> 💡 **Sobre o `app_name`:** Ele cria um "namespace" para as URLs do app. Isso evita conflitos quando múltiplos apps têm views com o mesmo nome (ex: dois apps com uma view chamada `index`). Nos templates, você referencia as URLs como `polls:index` em vez de apenas `index`.

#### Passo 3: Incluir as URLs do app no projeto

Edite `meusite/urls.py`:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

---

### Templates

#### Passo 1: Criar a estrutura de pastas

Execute a partir da pasta `djangotutorial/` (onde está o `manage.py`):

```bash
mkdir -p polls/templates/polls
```

> 💡 A estrutura `templates/polls/` com o nome do app repetido é intencional — evita conflitos de nomes entre diferentes apps.

#### Passo 2: Template da página inicial

Crie o arquivo `polls/templates/polls/index.html`:

```html
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li>
            <a href="{% url 'polls:detail' question.id %}">
                {{ question.question_text }}
            </a>
        </li>
    {% endfor %}
    </ul>
{% else %}
    <p>Nenhuma enquete disponível.</p>
{% endif %}
```

#### Passo 3: Template de detalhes

Crie o arquivo `polls/templates/polls/detail.html`:

```html
<form action="{% url 'polls:vote' question.id %}" method="post">
    {% csrf_token %}
    <fieldset>
        <legend><h1>{{ question.question_text }}</h1></legend>
        {% if error_message %}
            <p><strong>{{ error_message }}</strong></p>
        {% endif %}
        {% for choice in question.choice_set.all %}
            <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
            <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
        {% endfor %}
    </fieldset>
    <input type="submit" value="Votar">
</form>
```

> 💡 **`{% csrf_token %}`** é **obrigatório** em qualquer formulário POST no Django. Ele protege contra ataques CSRF (Cross-Site Request Forgery). Sem ele, o Django rejeita o formulário.

#### Passo 4: Template de resultados

Crie o arquivo `polls/templates/polls/results.html`:

```html
<h1>{{ question.question_text }}</h1>

<ul>
{% for choice in question.choice_set.all %}
    <li>
        {{ choice.choice_text }} — {{ choice.votes }} voto{{ choice.votes|pluralize }}
    </li>
{% endfor %}
</ul>

<a href="{% url 'polls:detail' question.id %}">Votar novamente?</a>
```

#### Tags e filtros de template mais usados

```django
{# Comentário — não aparece no HTML renderizado #}

{# Exibir variável #}
{{ variavel }}
{{ objeto.atributo }}

{# Estruturas de controle #}
{% if condicao %}...{% elif outra %}...{% else %}...{% endif %}
{% for item in lista %}...{% endfor %}

{# Herança de templates #}
{% extends 'base.html' %}
{% block nome %}...{% endblock %}
{% include 'parcial.html' %}

{# URLs (nunca escreva caminhos no código — use sempre a tag url) #}
{% url 'polls:index' %}
{% url 'polls:detail' question.id %}

{# Proteção de formulários #}
{% csrf_token %}

{# Arquivos estáticos #}
{% load static %}
<link rel="stylesheet" href="{% static 'polls/style.css' %}">

{# Filtros comuns #}
{{ texto|lower }}
{{ texto|upper }}
{{ texto|truncatechars:50 }}
{{ numero|pluralize }}
{{ lista|length }}
```

---

### Formulários e votação

A view `vote` já foi criada na seção anterior. Os pontos mais importantes:

1. **`request.POST`** — dicionário com os dados enviados pelo formulário
2. **`{% csrf_token %}`** — proteção obrigatória em formulários POST
3. **`F('votes') + 1`** — incremento feito pelo banco, evita perda de votos simultâneos
4. **`HttpResponseRedirect` + `reverse()`** — após um POST bem-sucedido, sempre redirecione para evitar reenvio ao atualizar a página (padrão PRG: Post/Redirect/Get)

---

### Views Genéricas

O Django oferece **views genéricas** que eliminam código repetitivo para padrões comuns como "mostrar uma lista de objetos" ou "mostrar detalhes de um objeto".

Reescreva `polls/views.py` para usar views baseadas em classes:

```python
from django.http import HttpResponseRedirect
from django.shortcuts import get_object_or_404
from django.urls import reverse
from django.views import generic
from django.db.models import F

from .models import Question, Choice


class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        return Question.objects.order_by('-pub_date')[:5]


class DetailView(generic.DetailView):
    model = Question
    template_name = 'polls/detail.html'


class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'


def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "Você não selecionou nenhuma opção.",
        })
    else:
        selected_choice.votes = F('votes') + 1
        selected_choice.save()
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

E atualize `polls/urls.py` para usar as classes. Note que `question_id` muda para `pk` nas views genéricas — elas esperam a chave primária com esse nome:

```python
from django.urls import path
from . import views

app_name = 'polls'

urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    path('<int:pk>/', views.DetailView.as_view(), name='detail'),
    path('<int:pk>/results/', views.ResultsView.as_view(), name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```

---

### Testes automatizados

Testes garantem que o código funciona como esperado e que mudanças futuras não quebram o que já funcionava.

Edite `polls/tests.py`:

```python
import datetime
from django.test import TestCase
from django.utils import timezone
from django.urls import reverse

from .models import Question


def create_question(question_text, days):
    """
    Cria uma pergunta com o texto dado, publicada `days` dias a partir de agora.
    Negativo = passado. Positivo = futuro.
    """
    time = timezone.now() + datetime.timedelta(days=days)
    return Question.objects.create(question_text=question_text, pub_date=time)


class QuestionModelTests(TestCase):

    def test_was_published_recently_with_future_question(self):
        """was_published_recently() retorna False para perguntas futuras."""
        future_question = create_question("Pergunta futura?", days=30)
        self.assertIs(future_question.was_published_recently(), False)

    def test_was_published_recently_with_old_question(self):
        """was_published_recently() retorna False para perguntas com mais de 1 dia."""
        old_question = create_question("Pergunta antiga?", days=-2)
        self.assertIs(old_question.was_published_recently(), False)

    def test_was_published_recently_with_recent_question(self):
        """was_published_recently() retorna True para perguntas publicadas hoje."""
        recent_question = create_question("Pergunta recente?", days=0)
        self.assertIs(recent_question.was_published_recently(), True)


class QuestionIndexViewTests(TestCase):

    def test_no_questions(self):
        """Se não há perguntas, exibe mensagem adequada."""
        response = self.client.get(reverse('polls:index'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, "Nenhuma enquete disponível.")
        self.assertQuerySetEqual(response.context['latest_question_list'], [])

    def test_past_question(self):
        """Perguntas com pub_date no passado aparecem na página inicial."""
        question = create_question("Pergunta passada?", days=-1)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerySetEqual(
            response.context['latest_question_list'],
            [question],
        )

    def test_future_question(self):
        """Perguntas com pub_date no futuro não aparecem na página inicial."""
        create_question("Pergunta futura?", days=30)
        response = self.client.get(reverse('polls:index'))
        self.assertContains(response, "Nenhuma enquete disponível.")
        self.assertQuerySetEqual(response.context['latest_question_list'], [])
```

Execute os testes:

```bash
uv run python manage.py test polls
```

A saída deve mostrar todos os testes passando:
```
Ran 5 tests in 0.XXXs

OK
```

---

### Arquivos Estáticos

Arquivos estáticos são CSS, JavaScript e imagens que não mudam dinamicamente.

#### Passo 1: Criar a estrutura de pastas

Execute a partir da pasta `djangotutorial/`:

```bash
mkdir -p polls/static/polls
```

#### Passo 2: Criar o arquivo CSS

Crie o arquivo `polls/static/polls/style.css`:

```css
li a {
    color: green;
    text-decoration: none;
}

body {
    background: white;
    font-family: sans-serif;
}
```

#### Passo 3: Referenciar o CSS no template

Atualize `polls/templates/polls/index.html` para carregar o CSS:

```html
{% load static %}
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{% static 'polls/style.css' %}">
    <title>Enquetes</title>
</head>
<body>

{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li>
            <a href="{% url 'polls:detail' question.id %}">
                {{ question.question_text }}
            </a>
        </li>
    {% endfor %}
    </ul>
{% else %}
    <p>Nenhuma enquete disponível.</p>
{% endif %}

</body>
</html>
```

> 💡 **`{% load static %}`** deve estar no topo de qualquer template que usa arquivos estáticos. A tag `{% static 'caminho' %}` gera a URL correta para o arquivo.

---

### Customizando o Admin

Edite `polls/admin.py` para tornar o painel administrativo mais completo:

```python
from django.contrib import admin
from .models import Question, Choice


class ChoiceInline(admin.TabularInline):
    model = Choice
    extra = 3  # Exibe 3 formulários vazios para adicionar escolhas


class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        (None,                   {'fields': ['question_text']}),
        ('Informações de data',  {'fields': ['pub_date'], 'classes': ['collapse']}),
    ]
    inlines = [ChoiceInline]
    list_display = ['question_text', 'pub_date', 'was_published_recently']
    list_filter = ['pub_date']
    search_fields = ['question_text']


admin.site.register(Question, QuestionAdmin)
```

Com isso, o admin exibirá as escolhas diretamente na página de edição da pergunta, colunas personalizadas na lista, filtro por data e barra de busca.

---

## 📚 Referência rápida de comandos

### Git

| Comando | Descrição |
|---|---|
| `git config --global user.name "Nome"` | Configura o nome do autor |
| `git config --global user.email "email"` | Configura o email do autor |
| `git clone <url>` | Clona um repositório remoto |
| `git checkout -b nome-da-branch` | Cria e muda para uma nova branch |
| `git status` | Exibe o estado atual do repositório |
| `git add .` | Adiciona todas as mudanças ao stage |
| `git commit -m "mensagem"` | Registra as mudanças com uma mensagem |
| `git push origin nome-da-branch` | Envia a branch para o repositório remoto |

### UV

| Comando | Descrição |
|---|---|
| `uv init nome-projeto` | Cria um novo projeto |
| `uv add pacote` | Instala um pacote |
| `uv remove pacote` | Remove um pacote |
| `uv sync` | Instala todas as dependências do `pyproject.toml` |
| `uv run comando` | Executa um comando dentro do ambiente virtual |
| `uv python install 3.12` | Instala uma versão do Python |
| `uv python list` | Lista versões de Python disponíveis |

### Django (`manage.py`)

| Comando | Descrição |
|---|---|
| `uv run python manage.py runserver` | Inicia o servidor de desenvolvimento |
| `uv run python manage.py startapp nome` | Cria um novo app |
| `uv run python manage.py makemigrations` | Cria arquivos de migration |
| `uv run python manage.py migrate` | Aplica migrations ao banco de dados |
| `uv run python manage.py createsuperuser` | Cria um usuário administrador |
| `uv run python manage.py shell` | Abre shell Python com o projeto |
| `uv run python manage.py test app` | Executa os testes do app |
| `uv run python manage.py collectstatic` | Coleta arquivos estáticos para produção |
| `uv run python manage.py check` | Verifica problemas no projeto |
| `uv run python manage.py sqlmigrate app 0001` | Exibe o SQL de uma migration |

---

## 🔧 Solução de Problemas Comuns

**`uv: command not found` após instalar o UV**
Feche e reabra o terminal. O UV precisa que o shell recarregue o PATH.

**"You have unapplied migrations"**
Execute `uv run python manage.py migrate` antes de iniciar o servidor.

**"That port is already in use"**
Escolha outra porta: `uv run python manage.py runserver 8001`  
Ou identifique o processo ocupando a porta: `lsof -i :8000` (Linux/WSL)

**Projeto não carrega no navegador**
Confirme que o servidor está rodando e acesse `http://127.0.0.1:8000/`. Certifique-se de ter rodado `migrate` antes.

**Templates não encontrados (TemplateDoesNotExist)**
Confirme que o app está em `INSTALLED_APPS` no `settings.py` e que a estrutura de pastas é `polls/templates/polls/nome.html`.

**Arquivos estáticos não carregam**
Confirme que `{% load static %}` está no topo do template e que a estrutura é `polls/static/polls/style.css`.

**Problemas com dependências ao clonar o repositório**
Execute `uv sync` na pasta do projeto para reinstalar tudo a partir do `pyproject.toml`.

**`DEBUG=True` em produção**
Nunca deixe `DEBUG = True` ao publicar o projeto. Altere para `False` em `settings.py`.