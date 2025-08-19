# Gestão de Clientes - Mini ERP Full-Stack

![Python](https://img.shields.io/badge/Python-3.11%2B-blue?style=for-the-badge&logo=python)
![Django](https://img.shields.io/badge/Django-4.2%2B-darkgreen?style=for-the-badge&logo=django)
![Vue.js](https://img.shields.io/badge/Vue.js-3.x-brightgreen?style=for-the-badge&logo=vue.js)

Aplicação web full-stack para gerenciamento de clientes, desenvolvida para demonstrar competências em desenvolvimento backend com Django REST Framework e frontend com Vue.js 3.

## 📸 Demonstração

[![Screenshot da Aplicação](https://raw.githubusercontent.com/DaviLOliveira/mini-erp-django-vue/main/docs/screenshot.png)](#demonstração)

## ✨ Funcionalidades

* **CRUD Completo:** Crie, visualize, edite e exclua clientes de forma dinâmica.
* **Interface Reativa:** Adição, edição e exclusão de clientes atualizam a interface em tempo real, sem a necessidade de recarregar a página.
* **Edição em Linha:** Edite os dados de um cliente diretamente na tabela para uma experiência de usuário fluida.
* **API RESTful:** Backend robusto que expõe endpoints para manipulação completa dos dados.

## 💻 Tecnologias Utilizadas

#### **Backend**
* **Python 3.11+**
* **Django 4.2+**
* **Django REST Framework** para a construção da API.
* **SQLite** como banco de dados de desenvolvimento.

#### **Frontend**
* **Vue.js 3** com a Composition API.
* **Vite** como ferramenta de build.
* **Axios** para as requisições à API.
* **CSS puro** para estilização, utilizando Flexbox para um layout responsivo.

## 🚀 Como Executar o Projeto Localmente

Siga os passos abaixo para rodar a aplicação na sua máquina.

### Pré-requisitos

* [Python 3.11+](https://www.python.org/downloads/)
* [Node.js e npm](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

### 1. Clonar o Repositório
```bash
git clone [https://github.com/DaviLOliveira/mini-erp-django-vue.git](https://github.com/DaviLOliveira/mini-erp-django-vue.git)
cd mini-erp-django-vue
```

### 2. Configurar e Rodar o Backend (API)

```bash
# Navegue até a pasta do backend
cd gestao-clientes

# Crie e ative um ambiente virtual
python -m venv venv
# No Windows:
.\venv\Scripts\activate

# Instale as dependências
pip install -r requirements.txt

# Aplique as migrações do banco de dados
python manage.py migrate

# Inicie o servidor do Django
python manage.py runserver
```
O servidor da API estará disponível em `http://127.0.0.1:8000`.

### 3. Configurar e Rodar o Frontend (UI)

```bash
# Em um NOVO terminal, navegue até a pasta do frontend
cd gestao-clientes-ui

# Instale as dependências
npm install

# Inicie o servidor de desenvolvimento
npm run dev
```
A aplicação estará disponível em `http://localhost:5173` (ou a porta que o Vite indicar).

## 📝 Endpoints da API

| Método | Endpoint                | Descrição                                         |
|--------|-------------------------|---------------------------------------------------|
| `GET`  | `/api/clientes/`        | Lista todos os clientes.                          |
| `POST` | `/api/clientes/`        | Cria um novo cliente.                             |
| `GET`  | `/api/clientes/<id>/`   | Detalha um cliente específico.                    |
| `PUT`  | `/api/clientes/<id>/`   | Atualiza todos os dados de um cliente específico. |
| `DELETE`| `/api/clientes/<id>/`   | Exclui um cliente específico.                     |

## 📄 Licença

Este projeto está sob a licença MIT.
