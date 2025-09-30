# 📚 Biblioteca Municipal - API e Aplicativo de Busca

Sistema desenvolvido para facilitar o acesso ao acervo da biblioteca pública municipal, permitindo que os usuários pesquisem livros, verifiquem disponibilidade e realizem reservas online.

---

## 🚀 Objetivo do Projeto

Oferecer uma solução digital para modernizar a gestão e o acesso ao acervo da biblioteca, promovendo:

- 🔎 Pesquisa rápida de livros por título, autor, categoria ou ISBN  
- 📍 Verificação de disponibilidade em tempo real  
- ✅ Solicitação de reserva ou empréstimo  
- 👤 Área do usuário com histórico e notificações

---

## 🏗️ Tecnologias Utilizadas

| Camada | Tecnologia |
|--------|------------|
| API Backend | Node.js / Express *(ou outra linguagem caso defina)* |
| Banco de Dados | PostgreSQL / MongoDB |
| Autenticação | JWT (JSON Web Token) |
| Aplicativo / Frontend | React / React Native / Flutter *(especificar)* |
| Documentação da API | Swagger / Postman |

---npm install


## 📦 Instalação e Execução

### 1️⃣ Clonar o repositório

```bash
git clone https://github.com/seuusuario/biblioteca-api.git
cd biblioteca-api
| Método | Rota                     | Descrição                 |
| ------ | ------------------------ | ------------------------- |
| GET    | `/livros`                | Lista todos os livros     |
| GET    | `/livros/:id`            | Detalhes de um livro      |
| GET    | `/usuarios/:id/reservas` | Lista reservas do usuário |
| POST   | `/reservas`              | Cria nova reserva         |
| POST   | `/auth/login`            | Login do usuário          |
