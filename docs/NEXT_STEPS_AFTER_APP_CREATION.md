# 🚀 PRÓXIMOS PASSOS APÓS CRIAR A APP NO META

## 🌟 Status atual:

Você já criou a app **"ai-growth-engine"** no Facebook Developers.

✅ App criada
✅ Dashboard acessível
❌ Ainda não tem WhatsApp integrado
❌ Ainda não tem Access Token
❌ Ainda não tem Webhook configurado

---

## 📑 Próximos Passos (5 passos)

### **PASSO 1: Copiar seu App ID (2 minutos)**

**Onde você está:**
```
Meta for Developers > ai-growth-engine > Painel
```

**O que fazer:**

1. No painel, procure por **"App ID"** ou **"ID do app""
2. Você verá um número assim:
   ```
   1853302932214689
   ```
3. **Copie este número!**
4. Salve em um local seguro (notepad, arquivo .env, etc)

⚠️ Você vai precisar disto depois.

---

### **PASSO 2: Acessar "Configurações do app" (1 minuto)**

No menu esquerdo do dashboard, clique em:
```
☑ Configurações do app
```

Você verá:
- App ID (já copiou)
- **App Secret** (um código longo, GUARDE COM SEGURANÇA)
- URLs de redirect
- Outros dados

**Copie o App Secret:**
```
Abstrait123456789abc...
```

Salve junto com o App ID.

---

### **PASSO 3: Adicionar Produto WhatsApp (5 minutos) ⭐⭐⭐**

Este é o passo CRUCIAL.

**No menu esquerdo, procure:**
```
☑ Produtos
```

Ou vá direto para: **"Ações necessárias"** no topo do painel.

**Você verá uma seção:**
```
Personalização do app e requisitos
├─ Personalizar o caso de uso "Conectar-se com os clientes pelo WhatsApp"
├─ Testar os casos de uso
├─ Verificar se todos os requisitos foram atendidos
```

**Clique no primeiro item:**
```
"Personalizar o caso de uso 'Conectar-se com os clientes pelo WhatsApp'"
```

---

### **PASSO 4: Configurar WhatsApp Business Account (10 minutos)**

Depois do passo anterior, você será redirecionado para:
```
WhatsApp Setup (Configuração do WhatsApp)
```

**Você terá 3 opções:**

**Opção A: Usar conta de teste (RECOMENDADO PRIMEIRO)**
```
☑ "Get started with test credentials"
Ou
☑ "Use test account"
```
Meta vai gerar um número de telefone de teste.

**Opção B: Conectar com Business Account existente**
```
☑ "I have an existing WhatsApp Business Account"
```

**Opção C: Criar novo Business Account**
```
☑ "Create a new WhatsApp Business Account"
```

📄 **Para este projeto, escolha A (teste).**

---

### **PASSO 5: Copiar tokens importantes (5 minutos) 🚨**

Após configurar WhatsApp, você verá na tela:

**Copie estes dados:**

1. **Phone Number ID**
   ```
   ☑ Número de ID do telefone (começa com números)
   ```

2. **Business Account ID**
   ```
   ☑ ID da Conta de Negócio
   ```

3. **Access Token** (O MAIS IMPORTANTE!)
   ```
   ☑ Token de acesso (começa com "EAA..." muito longo)
   ```
   ⚠️ **Copie AGORA! Só aparece uma vez!**

4. **Test Phone Number**
   ```
   ☑ Número de teste (ex: +55999999999)
   ```

---

## 💾 Salvar todos os tokens

Crie um arquivo `.env` na raiz do projeto:

```bash
# .env (NÃO FAZER COMMIT NO GIT!)

META_APP_ID=1853302932214689
META_APP_SECRET=abc123xyz...
META_ACCESS_TOKEN=EAABZAj2qVfgBAPLc4zrZAZCGlqV....(muito longo)
WHATSAPP_BUSINESS_ACCOUNT_ID=1234567890
WHATSAPP_PHONE_NUMBER_ID=1234567890
WHATSAPP_TEST_PHONE_NUMBER=+55999999999
WHATSAPP_WEBHOOK_TOKEN=webhook_token_12345_xyz_abc
```

⚠️ **NÃO envie este arquivo para o GitHub!**
Adicione `.env` ao `.gitignore`.

---

## 📋 Resumo Visual

```
Meta for Developers
    │
    ├── ai-growth-engine
    │       │
    │       ├── Painel
    │       │     (🔍 copiar App ID)
    │       │
    │       ├── Configurações do app
    │       │     (🔍 copiar App Secret)
    │       │
    │       ├── Ações necessárias
    │       │     (📄 clique em "Personalizar... WhatsApp")
    │       │
    │       ├── WhatsApp Setup
    │             (🔍 copiar Phone ID, Account ID, Access Token)
    │
    퉑4── [Salvar tudo em .env]
```

---

## ⭐ Ordem exata de cliques

1. Painel > copiar App ID
2. Configurações do app > copiar App Secret
3. Ações necessárias > clique em "Personalizar... WhatsApp"
4. WhatsApp Setup > escolher teste > copiar tokens
5. Salvar tudo em `.env`

---

## ✅ Checklist

- [ ] App ID copiado
- [ ] App Secret copiado
- [ ] WhatsApp adicionado (com teste ou Business Account)
- [ ] Phone Number ID copiado
- [ ] Business Account ID copiado
- [ ] Access Token copiado e salvo
- [ ] Test Phone Number anotado
- [ ] Arquivo `.env` criado com todos os dados
- [ ] `.env` adicionado ao `.gitignore`
- [ ] Pronto para próximo passo!

---

## 🚀 Próximo passo APÓS isto:

1. Volte para `PLANO_ACAO.md`
2. Siga a **Fase 2: MVP - WhatsApp + IA**
3. Use os tokens do `.env` no n8n

---

## 🆘 Problemas comuns?

### "Não estou vendo 'Personalizar... WhatsApp'"
**Solução:**
1. Recarregue a página
2. Verifique se a app foi criada corretamente
3. Tente clicar em "Produtos" no menu esquerdo

### "Não consigo copiar o Access Token"
**Solução:**
1. O token só aparece UMA VEZ
2. Se perdeu, gere um novo em "Settings > System Users"
3. Crie um novo System User e gere novo token

### "Não consigo ver Business Account ID"
**Solução:**
1. Va em "Ações necessárias"
2. Clique em "Testar os casos de uso"
3. Deve mostrar o ID lá

---

**Sucesso! Agora você tem tudo que precisa para integrar com n8n!** 🚀
