PROJETO MINI-TWITTER

DESCRIÇÃO
Implementar uma API REST em que um usuário possa realizar um cadastro, publicar Posts e ver as publicações de outros usuários.

REQUISITOS TÉCNICOS
Criar a aplicação em formato REST;
Python 3/PIP;
Django REST Framework (preferencialmente). Mas pode utilizar qualquer framework em Python. Queremos avaliar se você conhece os conceitos da web;
Banco de dados: relacional. Preferencialmente PostgreSQL;



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




