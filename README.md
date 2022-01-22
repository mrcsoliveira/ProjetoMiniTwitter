PROJETO MINI-TWITTER

DESCRIÇÃO
Implementar uma API REST em que um usuário possa realizar um cadastro, publicar Posts e ver as publicações de outros usuários.

REQUISITOS TÉCNICOS
Criar a aplicação em formato REST;
Python 3/PIP;
Django REST Framework (preferencialmente).



Preparação do ambiente no linux

python3 -m venv env
source env/bin/activate

Instalação do Django e DjangoRestFramework

pip install django
pip install djangorestframework


Preparação do projeto e criação da api seguindo a documentação do django

django-admin startproject blog
cd blog
django-admin startapp api

Criando a API do usuário para o Django REST Framework

Os usuários do Django são criados a partir do modelo User definido em django.contrib.auth.

O Django REST Framework usa “serializer” para traduzir conjuntos de consultas e instâncias de modelo em dados JSON. A serialização também determina quais dados sua API retorna em resposta ao cliente.
blog/api/serializers.py:
from rest_framework import serializers
from django.contrib.auth.models import User

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'username']
        
            
            
Para criar uma visualização somente leitura para sua lista de usuários e uma visualização somente leitura para um único usuário, adicione o seguinte a blog/api/views.py:

from rest_framework import generics
from api import serializers
from django.contrib.auth.models import User

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = serializers.UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = serializers.UserSerializer



Criando a API de postagem:
Com uma API de usuário básica configurada, podemos criar uma API completa para um blog, com endpoints para postagens. Comece criando a API Post.


Em blog/api/models.py, crie um modelo Post que herda da classe Model do Django e defina seus campos:

from django.db import models

class Post(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    body = models.TextField(blank=True, default='')
    owner = models.ForeignKey('auth.User', related_name='posts', on_delete=models.CASCADE)

    class Meta:
        ordering = ['created']

Post serializer:

Para adicionar o modelo Post à API, seguiremos um processo semelhante ao que seguiu para o modelo User.
blog/api/serializers.py
from api.models import Post

class PostSerializer(serializers.ModelSerializer):
    owner = serializers.ReadOnlyField(source='owner.username')

    class Meta:
        model = Post
        fields = ['id', 'title', 'body', 'owner']

class UserSerializer(serializers.ModelSerializer):
    posts = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = User
        fields = ['id', 'username', 'posts']
