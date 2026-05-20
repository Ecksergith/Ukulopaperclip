# Guia de Implementação: Paperclip no Railway

Como o Paperclip requer um servidor persistente e uma base de dados PostgreSQL, o **Railway** é a plataforma ideal para o alojar. Siga estes passos para colocar o seu projeto a funcionar:

## 1. Preparação
Certifique-se de que o seu código está num repositório GitHub (pode usar o que clonámos).

## 2. Criar Projeto no Railway
1. Vá para [Railway.app](https://railway.app/) e faça login com o GitHub.
2. Clique em **"New Project"** > **"Deploy from GitHub repo"**.
3. Selecione o repositório `Ukulopaperclip`.

## 3. Adicionar Base de Dados
O Paperclip precisa de um PostgreSQL externo para persistência na cloud:
1. No painel do seu projeto no Railway, clique em **"Add Service"** > **"Database"** > **"Add PostgreSQL"**.
2. O Railway criará a base de dados e gerará automaticamente uma variável `DATABASE_URL`.

## 4. Configurar Variáveis de Ambiente
No serviço da aplicação (não no da base de dados), vá a **Variables** e adicione:

| Variável | Valor Sugerido | Descrição |
| :--- | :--- | :--- |
| `PORT` | `3100` | A porta que o servidor vai escutar |
| `SERVE_UI` | `true` | Para o servidor entregar o frontend |
| `BETTER_AUTH_SECRET` | `um-segredo-longo-e-aleatorio` | Chave para autenticação |
| `PAPERCLIP_DEPLOYMENT_MODE` | `authenticated` | Ativa o sistema de login |
| `PAPERCLIP_DEPLOYMENT_EXPOSURE` | `public` | Permite acesso via internet |
| `PAPERCLIP_PUBLIC_URL` | `https://o-seu-app.up.railway.app` | O URL que o Railway lhe atribuir |

*Nota: O `DATABASE_URL` será injetado automaticamente pelo Railway se ligar os serviços.*

## 5. Persistência de Ficheiros (Opcional, mas Recomendado)
O Paperclip guarda ficheiros em `/paperclip`. No Railway, pode adicionar um **Volume**:
1. No serviço da aplicação, vá a **Settings** > **Volumes**.
2. Clique em **"Add Volume"**.
3. Mount Path: `/paperclip`.

## 6. Deploy
O Railway detetará o `Dockerfile` e o `railway.json` que configurei e iniciará o build automaticamente. Assim que terminar, a sua instância do Paperclip estará online!

---
**Dica:** Se encontrar erros de permissão no Docker, verifique se a variável `USER_UID` e `USER_GID` estão definidas como `1000`.
