# 🚀 GUIA COMPLETO: Criar App Meta para WhatsApp

## 📍 Overview
Este documento mostra **PASSO A PASSO** como criar uma app Meta e conectar com WhatsApp Cloud API.

**Tempo estimado**: 30-45 minutos
**Custo**: GRÁTIS até 1.000 mensagens/mês
**Dificuldade**: ⭐⭐☆ (Fácil)

---

## 📋 O que você vai conseguir ao final

✅ **App ID** (ex: `1234567890`)
✅ **Access Token** (começa com `EAA...`)
✅ **Webhook Token** (você cria)
✅ **WhatsApp Business Account ID**
✅ **Número de telefone para testes**

---

## ✅ PASSO 1: Acessar Meta for Developers (5 minutos)

### 1.1 Ir para o site
```
https://developers.facebook.com
```

### 1.2 Fazer login
- Use sua conta Facebook pessoal (ou crie uma se não tiver)
- Se tiver 2FA ativado, terá que confirmar no celular

### 1.3 Aceitar Termos
- Meta pode pedir para aceitar alguns termos
- Clique em **"Agree and Create Account"** ou similar

---

## ✅ PASSO 2: Criar uma Nova App (10 minutos)

### 2.1 No dashboard, procure "Create an App"
- Na página principal, deve ter um botão **"My Apps"** ou **"Create App"**
- Clique em **"Create App"**

### 2.2 Preencher formulário

**App Name**: `ai-growth-engine` (ou o nome que quiser)

**App Email**: seu_email@gmail.com

**App Purpose** (App Type): Escolha **"Business"** ou **"None - I'll set this up myself"**

**Category**: Deixe como está ou escolha **"Business"**

### 2.3 Clicar em "Create App"
- Você será redirecionado para o dashboard da app

---

## ✅ PASSO 3: Adicionar Produto WhatsApp (5 minutos)

### 3.1 No dashboard da app, procure "Add Product" ou "Configure"
- Procure por uma seção que diz **"Add Products"**
- Ou vá em: **Settings > Products** (no menu esquerdo)

### 3.2 Procurar por "WhatsApp"
- Você verá várias opções de produtos
- Procure por **"WhatsApp Business"** ou apenas **"WhatsApp"**
- Clique em **"Set Up"** ou **"Add"**

### 3.3 Selecionar tipo de conta
- **"Create a Test Webhook"** (para testes) — ✅ **ESCOLHA ISTO PRIMEIRO**
- Ou conectar com WhatsApp Business Account existente

---

## ✅ PASSO 4: Obter Tokens (SUPER IMPORTANTE!)

### 4.1 Achar a página de WhatsApp

No menu esquerdo, procure:
```
WhatsApp > Getting Started
```

Ou vá direto para:
```
https://developers.facebook.com/docs/whatsapp/cloud-api/get-started
```

### 4.2 **APP ID**

Você verá na página um ID assim:
```
App ID: 1234567890
```

**Copie e salve em um lugar seguro!** (ex: arquivo .env ou anotador)

---

## ✅ PASSO 5: Gerar Access Token (CRÍTICO!)

Este é o token que n8n vai usar para enviar mensagens.

### 5.1 Localizar onde pegar o token

Vá em:
```
WhatsApp > API Setup
```

Ou no menu esquerdo:
```
Settings > System Users
```

### 5.2 Criar System User

**Opção A: Se já existir System User**
- Clique no nome do usuário
- Procure por **"Generate Token"**

**Opção B: Se não existir nenhum**
- Clique em **"Create System User"**
- Nome: `ai-growth-webhook`
- Role: **Admin**
- Clique em **Create**

### 5.3 Gerar o Access Token

- Com o System User selecionado, procure **"Generate New Token"**
- **Token Expiration**: Selecione **"Never"** (recomendado para prod, ou 60 dias para teste)
- **Permissions**: Marque **"whatsapp_business_messaging"** e **"whatsapp_business_account_management"**
- Clique em **"Generate Token"**

### 5.4 COPIAR O TOKEN IMEDIATAMENTE!

Um token assim vai aparecer:
```
EAABZAj2qVfgBAPLc4zrZAZCGlqV...(muito comprido)....
```

⚠️ **IMPORTANTE**: Você SÓ VÊ ESTE TOKEN UMA VEZ!

**Copie agora e salve em lugar seguro!**

---

## ✅ PASSO 6: Obter Webhook Token

Este é um token simples que você **cria você mesmo**.

### 6.1 Escolher uma senha segura

**Exemplo**:
```
webhook_token_12345_xyz_abc
```

Ou gere uma aqui:
```
https://www.uuidgenerator.net/
```

### 6.2 Salvar em lugar seguro

Você vai usar isso em n8n para validar que mensagens vêm de WhatsApp (segurança).

---

## ✅ PASSO 7: Obter Número de Telefone para Testes (10 minutos)

### 7.1 Usar número de teste (GRATUITO)

No dashboard de WhatsApp, você verá:
```
"Start testing your app"
Test Phone Number: +55999999999
```

**Copie este número!** Este é o que você vai usar para simular mensagens.

### 7.2 Opção alternativa: Usar seu próprio número

Se quiser usar SEU WhatsApp pessoal:

1. Crie uma **Business Account** (https://business.facebook.com)
2. Adicione seu número de telefone
3. Conecte com a app
4. Aguarde aprovação da Meta (pode levar 24-48h)

⚠️ Para começar, recomendo usar o número de teste!

---

## ✅ PASSO 8: Configurar Webhook (5 minutos)

Agora você precisa conectar n8n para RECEBER mensagens.

### 8.1 No dashboard Meta, procure por:
```
WhatsApp > Configuration
```

Ou:
```
Settings > Webhooks
```

### 8.2 Preencher campos de Webhook

**Callback URL** (este é o endereço do n8n):
```
https://n8n-service-atrs.onrender.com/webhook/whatsapp
```

(Substitua se seu n8n tiver outra URL)

**Verify Token** (este é o que você criou no Passo 6):
```
webhook_token_12345_xyz_abc
```

**Subscribe to Events**:
- ✅ `messages` (para receber mensagens)
- ✅ `message_status` (para saber se foi entregue)

### 8.3 Clicar em "Save" ou "Subscribe"

Meta vai fazer uma verificação (webhook handshake).

⚠️ **Se der erro**: Verifique se a URL do n8n está correta e se o n8n está rodando.

---

## 🎯 Resumo: Tokens que você coletou

Coloque em um arquivo `.env` na raiz do projeto:

```bash
# .env (NÃO fazer commit disso no GitHub!)

META_APP_ID=1234567890
META_ACCESS_TOKEN=EAABZAj2qVfgBAPLc4zrZAZCGlqV....
WHATSAPP_WEBHOOK_TOKEN=webhook_token_12345_xyz_abc
WHATSAPP_BUSINESS_ACCOUNT_ID=1234567890
WHATSAPP_PHONE_NUMBER_ID=1234567890
TEST_PHONE_NUMBER=+55999999999
```

Ou, se usar no n8n:
- Vá em **Settings > Variables**
- Adicione as variáveis de ambiente
- Use delas nas nodes do n8n

---

## 🚨 Troubleshooting

### Problema: "Webhook não conecta"
**Solução**: 
- Verifique se a URL do n8n está correta
- Reinicie o serviço no Render
- Verifique se há firewalls bloqueando

### Problema: "Token inválido"
**Solução**:
- Access Tokens expiram! Gere um novo
- Copie o token SEM espaços
- Verifique se tem as permissões corretas

### Problema: "Número não recebe mensagens de teste"
**Solução**:
- Aguarde 5-10 minutos para Meta processar
- Verifique se usou o número de TESTE fornecido
- Tente enviar uma mensagem de teste via dashboard Meta

---

## ✅ Próximo Passo

Depois de coletar todos os tokens:

1. Ir para `PLANO_ACAO.md`
2. Seguir a **Fase 2: MVP - WhatsApp + IA**
3. Usar estes tokens no n8n

---

## 📚 Links Úteis

- **Meta Developers**: https://developers.facebook.com
- **WhatsApp Cloud API Docs**: https://developers.facebook.com/docs/whatsapp/cloud-api/
- **Postman Collection** (para testar): https://www.postman.com/downloads/
- **n8n WhatsApp Integration**: https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.whatsapp/

---

## 🎯 Checklist Final

- [ ] Conta criada em https://developers.facebook.com
- [ ] App "ai-growth-engine" criada
- [ ] Produto WhatsApp adicionado
- [ ] App ID copiado e salvo
- [ ] Access Token gerado e salvo
- [ ] Webhook Token criado e salvo
- [ ] Número de teste ou pessoal obtido
- [ ] Webhook configurado no Meta
- [ ] `.env` file criado com todos os tokens (NÃO commitado no Git)
- [ ] Pronto para Fase 2!

**Sucesso! Agora você pode começar a integrar com n8n!** 🚀
