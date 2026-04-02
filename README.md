# ep-ciencia-de-dados
Trabalho Desenvolvido ao longo da Disciplina ACH2177 - Introdução à Ciência de Dados (2026)

# Instruções de Uso

## Criação e ativação do ambiente virtual

Para criar ambiente virtual, onde estarão as bibliotecas paradronizadas para os membros do grupo, rode ou comando no terminal:

``bash
python3 -m venv .venv
``
Para Linux ou

``bash
python -m venv .venv
``
Para Windows.

Em seguida, ative o mesmo com o comando para ativar o ambiente virtual:

``bash
.venv\Scripts\activate
``

Faça todas as instalações e execuções com o ambiente virtual ativado.

## Instalação das bibliotecas

Havendo um arquivo chamado requirements.txt no repositório, execute:

``bash
pip install -r requirements.txt
``

Toda vez que instalar uma biblioteca nova, é necessário atualizar o arquivo requirements.txt. É possível fazer isso pelo comando

``bash
pip freeze > requirements.txt
``
