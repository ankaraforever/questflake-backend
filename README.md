# Discord Quest Automation API 🚀

API backend em **Node.js** e **Express** para automação e monitoramento de missões (quests) do Discord, desenvolvida para ser utilizada como base em projetos pessoais e integrações.

---

## ⚡ Recursos

- **Validação de Token e Sessão JWT:** Autenticação segura que valida o token junto ao Discord e gera uma sessão JWT com expiração e token ofuscado.
- **Detecção e Parsing de Quests:** Suporte para missões de assistir vídeos (`WATCH_VIDEO`, `WATCH_VIDEO_ON_MOBILE`) e jogar (`PLAY_ON_DESKTOP`, etc.).
- **Execução Confiável:** Mantém o envio de progresso (vídeos ou heartbeats) até o Discord confirmar formalmente a conclusão (`completed_at`).
- **Segurança Pronta para Produção:**
  - `helmet` para cabeçalhos HTTP seguros.
  - `express-rate-limit` contra abusos e ataques de força bruta.
  - `cors` com origens configuráveis.
  - Validação estrita de formato de token e IDs de missão.

---

## 📁 Estrutura de Pastas

```text
├── auth.js            # Lógica de JWT e ofuscação do token de autenticação
├── discordClient.js   # Chamadas HTTPS diretas para os endpoints do Discord
├── questLogic.js      # Algoritmo de filtragem, prioridade de tarefas e cálculo de recompensas
├── questRunner.js     # Executor em segundo plano com tratamento de ticks e confirmação
├── index.js           # Servidor Express e definição das rotas da API
├── package.json       # Dependências e scripts do projeto
├── .env.example       # Modelo de variáveis de ambiente
└── .gitignore         # Arquivos ignorados pelo Git
```

---

## 🛠️ Instalação e Configuração

### 1. Clonar e Instalar Dependências

```bash
git clone https://github.com/ankaraforever/questflake-backend
cd questflake-backend
npm install
```

### 2. Configurar Variáveis de Ambiente

Copie o arquivo de exemplo `.env.example` para `.env`:

```bash
cp .env.example .env
```

Edite o arquivo `.env`:

```env
# Chave secreta para assinar os tokens JWT (mude para uma chave segura)
JWT_SECRET=sua-chave-secreta-jwt-super-forte

# Chave customizada para ofuscação de tokens
ENCRYPTION_KEY=sua-chave-de-ofuscacao-customizada

# URL do seu frontend permitida no CORS
FRONTEND_URL=http://localhost:3000

# Porta do servidor
PORT=8080
```

### 3. Executar o Servidor

Modo de desenvolvimento ou produção:

```bash
npm start
```

O servidor iniciará em `http://localhost:8080`.

---

## 🔌 Rotas da API

Todas as rotas (exceto validação e health check) exigem o cabeçalho:
`Authorization: Bearer <seu_token_jwt>`

| Método | Rota | Descrição |
|---|---|---|
| `POST` | `/api/auth/validate` | Valida o token Discord e retorna o JWT da sessão |
| `GET` | `/api/quests` | Lista todas as missões ativas e automatizáveis |
| `POST` | `/api/quests/:questId/run` | Inicia a execução em background de uma missão |
| `POST` | `/api/quests/run-all` | Executa todas as missões disponíveis sequencialmente |
| `GET` | `/api/quests/status` | Retorna o status de progresso das missões em andamento |
| `POST` | `/api/quests/:questId/cancel` | Cancela uma missão em andamento |
| `GET` | `/api/health` | Verificação de status do servidor (Health check) |

---

## 🛡️ Segurança e Boas Práticas

- **Nunca compartilhe ou comite seu `.env`:** Mantenha sempre no `.gitignore`.
- **Tokens Discord:** Os tokens nunca são persistidos em bancos de dados em texto plano.
- **Deploy na Vercel:** Se for hospedar na Vercel, configure o `entrypoint` apontando para `index.js` e preencha as variáveis de ambiente em **Project Settings > Environment Variables**.
