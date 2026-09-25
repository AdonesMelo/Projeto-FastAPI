# 🍕 Pizza Delivery API

Uma API RESTful para gerenciamento de pedidos e usuários em uma pizzaria, desenvolvida em Python utilizando **FastAPI**, **SQLAlchemy** e **Alembic**. A autenticação é realizada via **JWT (JSON Web Tokens)** com senhas criptografadas usando **Bcrypt**.

---

## 🛠️ Tecnologias Utilizadas

- **Linguagem:** Python 3.x
- **Framework Web:** [FastAPI](https://fastapi.tiangolo.com/)
- **Servidor ASGI:** [Uvicorn](https://www.uvicorn.org/)
- **ORM:** [SQLAlchemy](https://www.sqlalchemy.org/)
- **Migrações:** [Alembic](https://alembic.sqlalchemy.org/)
- **Banco de Dados:** SQLite
- **Autenticação & Segurança:** 
  - `python-jose` (Tokens JWT)
  - `passlib` & `bcrypt` (Hashing de senhas)
  - `python-multipart` (Suporte a formulários de login OAuth2)

---

## 📁 Estrutura do Projeto

```text
├── alembic/              # Diretório com scripts e arquivos de migração
├── alembic.ini           # Configuração do Alembic
├── auth_routes.py        # Rotas de autenticação (cadastro, login, refresh token)
├── dependencies.py      # Injeção de dependências (Sessão do banco, verificação de JWT)
├── main.py               # Ponto de entrada da aplicação FastAPI
├── models.py             # Modelos de dados SQLAlchemy (Usuario, Pedido, ItemPedido)
├── order_routes.py       # Rotas de gerenciamento de pedidos e itens
├── schemas.py            # Validação e serialização de dados com Pydantic
├── requirements.txt      # Dependências do projeto
├── banco.db              # Banco de dados SQLite
└── .env                  # Variáveis de ambiente (Chave secreta, tempo do token, etc.)
```

---

## ⚙️ Configuração e Instalação

### 1. Pré-requisitos
Certifique-se de ter o Python 3.10+ instalado no seu sistema.

### 2. Clonar o Repositório e criar ambiente virtual
```bash
# Clone este repositório
git clone <URL_DO_SEU_REPOSITORIO>
cd <NOME_DO_DIRETORIO>

# Crie um ambiente virtual
python -m venv .venv

# Ative o ambiente virtual
# No Windows:
.venv\Scripts\activate
# No Linux/macOS:
source .venv/bin/activate
```

### 3. Instalar Dependências
```bash
pip install -r requirements.txt
```

### 4. Configurar Variáveis de Ambiente
Crie um arquivo `.env` na raiz do projeto com as seguintes chaves (substitua a `SECRET_KEY` por uma chave segura):

```env
SECRET_KEY=sua_chave_secreta_super_segura
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

### 5. Executar Migrações do Banco de Dados
Para criar ou atualizar o esquema da base de dados SQLite:

```bash
alembic upgrade head
```

---

## 🚀 Como Executar a Aplicação

Inicie o servidor Uvicorn no modo de desenvolvimento:

```bash
uvicorn main:app --reload
```

A API estará acessível em `http://127.0.0.1:8000`.

---

## 📚 Documentação da API (Swagger UI)

O FastAPI gera documentação interativa automaticamente. Após iniciar o servidor, você pode testar todas as rotas nos endereços:

- **Swagger UI:** [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- **ReDoc:** [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

---

## 🔐 Autenticação e Rotas Principal

### 🗝️ Autenticação (`/auth`)
- `POST /auth/criar_conta`: Registra um novo usuário.
- `POST /auth/login`: Autentica um usuário via JSON e retorna `access_token` e `refresh_token`.
- `POST /auth/login-form`: Autenticação compatível com OAuth2 Password Flow (usado no Swagger UI).
- `GET /auth/refresh`: Gera um novo `access_token` a partir de um token válido.

### 📦 Pedidos (`/pedidos`)
*(Todas as rotas exigem envio do token Bearer no cabeçalho `Authorization: Bearer <TOKEN>`)*

- `POST /pedidos/pedido`: Cria um novo pedido para um usuário.
- `GET /pedidos/listar`: Lista todos os pedidos cadastrados (**Acesso exclusivo para Administradores**).
- `GET /pedidos/listar/pedidos-usuario`: Lista os pedidos do usuário autenticado.
- `GET /pedidos/pedido/{id_pedido}`: Visualiza detalhes de um pedido específico.
- `POST /pedidos/pedido/adcionar-item/{id_pedido}`: Adiciona uma pizza/item ao pedido especificado.
- `POST /pedidos/pedido/remover-item/{id_item_pedido}`: Remove um item do pedido.
- `POST /pedidos/pedido/finalizar/{id_pedido}`: Altera o status do pedido para `FINALIZADO`.
- `POST /pedidos/pedido/cancelar/{id_pedido}`: Altera o status do pedido para `CANCELADO`.

---

## 🧪 Banco de Dados & Estrutura

- **`Usuario`**: Gerencia clientes e admins (`nome`, `email`, `senha`, `ativo`, `admin`).
- **`Pedido`**: Registra o status do pedido e valor total calculado (`status`, `usuario`, `preco`).
- **`ItemPedido`**: Detalhes dos itens associados a cada pedido (`quantidade`, `sabor`, `tamanho`, `preco_unitario`).

---

## 📝 Licença

Este projeto foi desenvolvido durante o curso da [Hashtag Programação](https://www.youtube.com/@HashtagProgramacao) como parte do aprendizado em FastAPI.

---

## 👤 Autor

**Adones Melo** — [GitHub](https://github.com/AdonesMelo)