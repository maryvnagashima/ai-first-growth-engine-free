# Configuração n8n - WhatsApp AI Growth Engine

## Overview
Este documento fornece instruções completas para configurar o workflow de n8n que integra WhatsApp Business Cloud com IA (Groq).

## Arquitetura do Workflow

O workflow "WhatsApp AI Growth Engine" consiste em três componentes principais:

1. **Webhook (Trigger)**: Recebe mensagens do WhatsApp via Meta
2. **Groq Chat Model**: Processa a mensagem com IA
3. **Send Message (WhatsApp)**: Envia a resposta de volta ao usuário

```
WhatsApp Message → Webhook → Groq AI → WhatsApp Response
```

## Pré-requisitos

- ✅ n8n instância rodando em https://n8n-service-atrs.onrender.com
- ✅ Meta Business Account (criada)
- ✅ WhatsApp Business App (criado)
- ✅ Groq API Key

## Step 1: Configurar Groq API

### 1.1 Obter Groq API Key

1. Acesse https://console.groq.com
2. Faça login com sua conta
3. Navegue para "API Keys"
4. Clique em "Create New Key"
5. Copie a chave gerada

### 1.2 Adicionar Groq Credential em n8n

1. Abra seu n8n em https://n8n-service-atrs.onrender.com
2. Vá para **Settings** (ícone de engrenagem no canto superior direito)
3. Clique em **Credentials**
4. Clique em **+ Create** (ou procure por "Groq")
5. Preencha os campos:
   - **Name**: "Groq API"
   - **API Key**: Cole a chave do Groq
6. Clique em **Save**

## Step 2: Configurar WhatsApp Business Cloud

### 2.1 Obter Meta Credentials

Você já criou o app Meta. Agora precisa obter:

1. **Phone Number ID**
   - Acesse https://developers.facebook.com/
   - Vá para seu app "ai-growth-engine"
   - Navegue até **WhatsApp > Phone numbers**
   - Copie o Phone Number ID

2. **Business Account ID**
   - No mesmo dashboard
   - Navegue até **WhatsApp > Business accounts**
   - Copie o Business Account ID

3. **Access Token**
   - Vá para **Settings > Basic**
   - Encontre seu "App ID" e "App Secret"
   - Vá para **Tools > Token Generator**
   - Selecione seu app e gere um token
   - Copie o Access Token

### 2.2 Adicionar WhatsApp Credential em n8n

1. Abra o workflow "WhatsApp AI Growth Engine"
2. Clique no nó **"Send message"** (ícone verde do WhatsApp)
3. Em "Credential to connect with", clique em **"Select Credential"**
4. Clique em **"+ Create new credential"**
5. Preencha os campos:
   - **Access Token**: Cole o token do Meta
   - **Business Account ID**: Cole o ID da conta de negócios
   - **Allowed HTTP Request Domains**: Deixe como "All"
6. Clique em **Save**

## Step 3: Configurar Webhook para Receber Mensagens

### 3.1 Copiar Webhook URL

1. No workflow, clique no nó **"Webhook"**
2. Copie a **Test URL** (por exemplo):
   ```
   https://n8n-service-atrs.onrender.com/webhook/87b282f4-f1fb-4f45...
   ```
3. Esta será a URL que você configurará no Meta

### 3.2 Configurar Webhook no Meta

1. Acesse https://developers.facebook.com/
2. Vá para seu app "ai-growth-engine"
3. Navegue até **Configuration > Webhooks**
4. Clique em **Edit Webhook**
5. Configure:
   - **Callback URL**: Cole a URL do webhook do n8n
   - **Verify Token**: Use um token seguro (ex: "seu_token_secreto_aqui")
   - **Subscribe Fields**: Marque "messages"
6. Clique em **Verify and Save**
7. **De volta no n8n Webhook:**
   - Copie o **Verify Token** que você usou
   - Vá para o nó Webhook em n8n
   - Em opções avançadas, defina o token de verificação (se houver campo)

## Step 4: Mapear Dados do Webhook

### 4.1 Entender a Estrutura de Dados

Uma mensagem do WhatsApp chegará no seguinte formato:

```json
{
  "entry": [{
    "changes": [{
      "value": {
        "messages": [{
          "from": "5511999999999",
          "text": {"body": "Olá!"},
          "id": "message_id"
        }]
      }
    }]
  }]
}
```

### 4.2 Configurar Mapping no Groq Node

1. Clique no nó **"Groq Chat Model1"**
2. Vá para a aba **"Mapping"**
3. Configure para passar a mensagem do webhook para o Groq
4. Na aba "From AI", configure para extrair o campo de entrada

### 4.3 Configurar Mapping no Send Message Node

1. Clique no nó **"Send message"**
2. Configure os campos:
   - **Sender Phone Number**: Use o Phone Number ID que você copiou
   - **Recipient's Phone Number**: Extraia do webhook (`entry[0].changes[0].value.messages[0].from`)
   - **MessageType**: "Text"
   - **Text Body**: Use a resposta do Groq

## Step 5: Testar o Workflow

### 5.1 Testar com Mock Data

1. Em n8n, clique no nó Webhook
2. Clique em **"Listen for test event"** ou **"set mock data"**
3. Adicione dados de teste:
   ```json
   {
     "entry": [{
       "changes": [{
         "value": {
           "messages": [{
             "from": "5511999999999",
             "text": {"body": "Olá, qual é o seu nome?"},
             "id": "test_message_id"
           }]
         }
       }]
     }]
   }
   ```
4. Clique em "Save"
5. Clique em **"Execute workflow"** para testar

### 5.2 Enviar Mensagem de Teste Real

1. Use um número de teste do WhatsApp (obtido do Meta Dashboard)
2. Envie uma mensagem via WhatsApp
3. Verifique os Logs em n8n para rastrear a execução

## Step 6: Publicar o Workflow

1. Quando tudo estiver funcionando, clique em **"Publish"** no topo
2. Isso ativará o workflow para receber mensagens reais

## Troubleshooting

### Webhook não está recebendo mensagens
- Verifique se a URL do webhook está correta em Meta
- Verifique se o token de verificação está correto
- Veja os logs do n8n para erros

### Erro ao enviar mensagem
- Verifique se as credenciais do WhatsApp estão corretas
- Verifique se o Phone Number ID está correto
- Verifique se o número do destinatário está no formato correto

### Groq não está respondendo
- Verifique se a API Key do Groq está correta
- Verifique se o modelo "llama3-8b-8192" está disponível
- Veja os logs de erro no n8n

## Próximos Passos

- [ ] Integrar histórico de conversas
- [ ] Adicionar contexto de usuário
- [ ] Implementar sistema de fallback
- [ ] Adicionar analytics de conversas
- [ ] Implementar A/B testing de respostas

## Referências

- [n8n Documentation](https://docs.n8n.io)
- [Meta WhatsApp API](https://developers.facebook.com/docs/whatsapp/cloud-api)
- [Groq API Documentation](https://console.groq.com/docs)
