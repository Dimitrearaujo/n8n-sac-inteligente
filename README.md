# 🎙️ SAC com Gravação Inteligente

> Agente de atendimento ao cliente via **e-mail** com memória de contexto — responde automaticamente às mensagens do Gmail usando GPT e mantém histórico da conversa.

[![n8n](https://img.shields.io/badge/n8n-workflow-orange?logo=n8n)](https://n8n.io)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-412991?logo=openai)](https://openai.com)
[![Gmail](https://img.shields.io/badge/Gmail-API-red?logo=gmail)](https://developers.google.com/gmail)

---

## 📌 O que este workflow faz

1. **Monitora** a caixa de entrada do Gmail a cada minuto
2. **Lê** o e-mail recebido e carrega o histórico da conversa (memória por sessão)
3. **Processa** com um Agente IA (GPT-4o) seguindo as instruções do sistema SAC
4. **Responde** automaticamente ao remetente pelo próprio Gmail

### Caso de uso
Ideal para equipes de suporte que precisam de uma primeira resposta automática e inteligente — reduz tempo de espera e garante atendimento 24/7.

---

## 🏗️ Arquitetura

```
Gmail Trigger (polling 1min)
        │
        ▼
   [Novo e-mail?] ──── Não ──── Ignorar
        │ Sim
        ▼
   AI Agent (GPT-4o)
   ├── Memory Buffer (histórico da conversa)
   └── Instruções do SAC
        │
        ▼
   Reply to Message (Gmail)
```

---

## 🔧 Integrações

| Serviço | Uso |
|---|---|
| **Gmail** | Trigger (leitura) + envio de resposta |
| **OpenAI GPT-4o** | Processamento da mensagem e geração da resposta |
| **Memory Buffer** | Contexto da conversa por thread de e-mail |

---

## ✅ Pré-requisitos

- [ ] n8n v1.0+ instalado
- [ ] Conta Google com Gmail API habilitada
- [ ] Chave de API da OpenAI

---

## ⚙️ Configuração

### 1. Credenciais necessárias

| Nó | Credencial | Como obter |
|---|---|---|
| `Gmail Trigger` | Gmail OAuth2 | [Google Cloud Console](https://console.cloud.google.com) → APIs & Services → Credentials |
| `Reply to a message` | Gmail OAuth2 | Mesma credencial acima |
| `OpenAI Chat Model` | OpenAI API Key | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |

### 2. Importar no n8n

```
Menu lateral → Workflows → + New → ··· → Import from File
```
Selecione o arquivo `workflow.json`.

### 3. Personalizar o agente

Após importar, abra o nó **AI Agent** e edite o campo `System Message` com as instruções do seu SAC:
- Tom de voz da empresa
- Produtos/serviços disponíveis
- Políticas de atendimento
- O que escalar para humanos

### 4. Ativar o workflow

Clique em **Activate** (canto superior direito) para iniciar o monitoramento.

---

## 📊 Métricas do workflow

| Métrica | Valor |
|---|---|
| Total de nós | 6 |
| Intervalo de polling | 1 minuto |
| Modelo LLM | GPT-4o (configurável) |
| Janela de memória | Últimas mensagens da thread |

---

## 💡 Dicas de personalização

- **Filtros de e-mail**: adicione um nó `Filter` antes do agente para processar só e-mails de domínios específicos
- **Escalonamento**: use um nó `If` para detectar palavras-chave (ex: "urgente", "cancelamento") e redirecionar para humanos
- **Múltiplas caixas**: duplique o workflow com credenciais diferentes para monitorar vários e-mails

---

[← Voltar ao índice](../README.md)
