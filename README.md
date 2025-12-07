# 📘 Guia de Estudo: Conexão MySQL com Node.js e Interface Moderna

Este projeto é um exemplo prático de como conectar um **Frontend Moderno** (HTML/CSS/JS) a um **Banco de Dados MySQL** utilizando um **Backend Node.js**.

Abaixo, explico detalhadamente cada arquivo e conceito utilizado.

---

## 🏗️ Estrutura do Projeto

O projeto é dividido em duas partes principais:
1.  **Backend (Servidor)**: Gerencia a conexão com o banco de dados e fornece uma API.
2.  **Frontend (Interface)**: Mostra o status da conexão para o usuário com um design premium.

---

## 🖥️ Backend (Node.js)

O navegador (HTML/JS) **não pode** conectar diretamente ao banco de dados por segurança. Por isso, criamos um servidor intermediário.

### 1. `db.js` (A Conexão)
Este arquivo é responsável por "falar" com o MySQL.

-   **`mysql.createPool`**: Em vez de criar uma única conexão (`createConnection`), usamos um **Pool**.
    -   *Por que?* Um pool mantém várias conexões abertas e as reutiliza. Isso é muito mais rápido e eficiente para aplicações reais.
-   **`process.env`**: As senhas e configurações vêm do arquivo `.env`. Nunca escreva senhas diretamente no código!
-   **`testConnection`**: Uma função assíncrona (`async/await`) que tenta pegar uma conexão do pool. Se conseguir, devolve sucesso; se falhar, devolve o erro.

### 2. `server.js` (O Servidor Web)
Este arquivo usa o **Express** para criar um servidor web.

-   **`app.use(cors())`**: Permite que outras origens acessem sua API (essencial se o front e back estiverem em portas diferentes).
-   **`app.use(express.static('.'))`**: Serve os arquivos do frontend (`index.html`, `style.css`) diretamente.
-   **Rota `/api/status`**:
    -   Quando o frontend acessa `http://localhost:3000/api/status`, o servidor executa a função `testConnection()` do `db.js` e retorna o resultado em formato JSON.

---

## 🎨 Frontend (Interface)

### 3. `index.html` (A Estrutura)
-   Usa fontes modernas (**Outfit**) do Google Fonts.
-   Estrutura semântica (`main`, `header`, `footer`).
-   Elementos com IDs (`id="statusCard"`) para serem facilmente manipulados pelo JavaScript.

### 4. `style.css` (O Design "Glassmorphism")
O efeito de "vidro fosco" é a chave do design premium.

-   **Variáveis CSS (`:root`)**: Facilitam a troca de cores e manutenção.
-   **Glassmorphism**:
    ```css
    background: rgba(255, 255, 255, 0.1); /* Fundo transparente */
    backdrop-filter: blur(16px);          /* Desfoque do que está atrás */
    border: 1px solid rgba(255, 255, 255, 0.2); /* Borda sutil */
    ```
-   **Animações (`@keyframes`)**:
    -   `float`: Faz as bolhas coloridas flutuarem no fundo.
    -   `slideUp`: Faz o cartão aparecer suavemente vindo de baixo.
    -   `rotation`: Gira o loader de carregamento.

### 5. `app.js` (A Lógica)
Este arquivo torna a página "viva".

-   **`fetch('/api/status')`**: Faz uma requisição HTTP ao nosso servidor (backend).
-   **`async/await`**: Permite esperar a resposta do servidor sem travar a tela.
-   **Manipulação do DOM**:
    -   `classList.add('hidden')` / `remove('hidden')`: Mostra ou esconde elementos (como o loader ou o ícone de sucesso).
    -   `textContent`: Altera o texto da tela dinamicamente.

---

## 🚀 Fluxo de Execução

1.  O usuário abre o site.
2.  O `app.js` carrega e chama `checkConnection()`.
3.  O frontend mostra "Verificando..." e o loader gira.
4.  O `app.js` pede ao `server.js` (`/api/status`): "Como está o banco?".
5.  O `server.js` pede ao `db.js`: "Tenta conectar aí".
6.  O `db.js` conecta no MySQL e responde "Sucesso" ou "Erro".
7.  O `server.js` devolve esse JSON para o frontend.
8.  O `app.js` recebe o JSON e atualiza a tela (Verde para sucesso, Vermelho para erro).

---

## 📚 Dicas de Estudo

-   Tente mudar as cores no `style.css` (variável `--bg-gradient`).
-   Quebre a conexão propositalmente (mude a senha no `.env`) e veja o tratamento de erro na tela.
-   Tente adicionar um novo campo na resposta da API no `server.js` e mostre-o no `index.html`.
# Projeto: Conexão Node.js com Banco de Dados MySQL

Este é um projeto de exemplo que demonstra como conectar uma aplicação web, construída com **Node.js** e **Express**, a um banco de dados **MySQL**. A interface do usuário (frontend) é criada com HTML, CSS e JavaScript puro.

## 🎯 Objetivo

O objetivo principal é ilustrar o fluxo completo de uma aplicação web moderna:

1.  **Frontend**: Interage com o usuário.
2.  **Backend**: Atua como intermediário, processando requisições e se comunicando com o banco de dados.
3.  **Banco de Dados**: Armazena os dados da aplicação.

## 🛠️ Tecnologias Utilizadas

-   **Backend**:
    -   **Node.js**: Ambiente de execução para JavaScript no servidor.
    -   **Express.js**: Framework para criar o servidor web e as rotas da API.
    -   **mysql2/promise**: Driver para conectar ao MySQL de forma assíncrona (`async/await`).
    -   **dotenv**: Para gerenciar variáveis de ambiente (credenciais do banco) de forma segura.

-   **Frontend**:
    -   HTML5
    -   CSS3
    -   JavaScript (Vanilla) com `fetch` para requisições à API.

## 📂 Estrutura de Arquivos

-   `server.js`: O coração do backend. Cria o servidor Express, define as rotas da API e serve os arquivos do frontend.
-   `db.js`: Responsável exclusivamente pela lógica de conexão com o banco de dados.
-   `index.html`: A página web que o usuário vê.
-   `app.js`: O JavaScript do frontend, que faz a chamada para a API do backend e atualiza a página.
-   `style.css`: A folha de estilos da página.
-   `.env`: Arquivo para armazenar as variáveis de ambiente (não deve ser enviado para o repositório).
-   `package.json`: Define as dependências e scripts do projeto.

## 🚀 Como Rodar o Projeto

Para executar este projeto em sua máquina local, siga os passos abaixo.

### Pré-requisitos

-   [Node.js](https://nodejs.org/) instalado.
-   Um servidor MySQL em execução.

### Passos

1.  **Clone o repositório:**
    ```bash
    git clone <url-do-repositorio>
    cd <pasta-do-projeto>/00
    ```

2.  **Instale as dependências:**
    Execute o comando abaixo no terminal para instalar os pacotes definidos no `package.json`.
    ```bash
    npm install
    ```

3.  **Configure as Variáveis de Ambiente:**
    -   Crie um arquivo chamado `.env` na pasta `00`.
    -   Copie o conteúdo do arquivo `.env.example` (se existir) ou adicione as seguintes variáveis, preenchendo com suas credenciais do MySQL:
        ```
        DB_HOST=localhost
        DB_USER=root
        DB_PASSWORD=sua_senha_aqui
        DB_NAME=seu_banco_de_dados_aqui
        DB_PORT=3306
        ```

4.  **Inicie o Servidor:**
    Execute o comando para iniciar o servidor Node.js.
    ```bash
    npm start
    ```
    O terminal deverá exibir a mensagem: `Servidor rodando em http://localhost:3000`.

5.  **Acesse a Aplicação:**
    Abra seu navegador e acesse a URL:
    [http://localhost:3000](http://localhost:3000)

A página irá carregar e testar automaticamente a conexão com o banco de dados, exibindo o resultado na tela.