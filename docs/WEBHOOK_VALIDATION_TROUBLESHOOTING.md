# Solução de Problemas - Validação do Webhook Meta/WhatsApp

## ❌ Problema: "Não foi possível validar a URL de callback ou o token de verificação"

Este documento explica o problema de validação do webhook entre Meta Developers e n8n, e a solução.

## O Que Está Acontecendo?

Quando você tenta registrar o webhook no Meta Developers, Meta envia uma requisição **GET** com parâmetros especiais para verificar se a URL é válida:

```
GET /webhook/[seu_id]?hub.mode=subscribe&hub.verify_token=seu_token&hub.challenge=abc123
```

Meta espera receber **exatamente** o valor de `hub.challenge` como resposta (não JSON, apenas texto plano).

## ✅ Solução

### Passo 1: Configurar o Webhook Node

1. Abra seu workflow "WhatsApp AI Growth Engine" em n8n
2. Clique no nó **"Webhook"**
3. Na aba **"Parameters"**, mude o campo **"Respond"** de "Immediately" para **"Using 'Respond to Webhook' Node"**
4. Salve o workflow

### Passo 2: Configurar o Respond to Webhook Node

1. Você deve ter um nó **"Respond to Webhook"** conectado após o Webhook
2. Configure-o para responder com:
   - **Body**: O valor do `hub.challenge` do webhook
   - **Status Code**: 200

### Passo 3: Adicionar Validação do Token (Opcional mas Recomendado)

Antes de responder ao Meta, você pode validar o token:

```javascript
// Em um nó "Execute Code" ou "Function" entre Webhook e Respond
if ($input.first().json.hub_verify_token !== 'webhook_verify_token_marina_2025') {
    throw new Error('Token inválido!');
}

return $input.first().json;
```

## 📝 Verificação Rápida

Desde que você:
- ✅ Mudou o Webhook "Respond" para "Using 'Respond to Webhook' Node"
- ✅ Tem o nó "Respond to Webhook" configurado
- ✅ Salvou o workflow

Tente novamente em Meta Developers > Configuração > Assinar os webhooks > "Verificar e salvar"

## Se Ainda Não Funcionar

### Checklist de Diagnóstico:

1. **URL está correta?**
   ```
   https://n8n-service-atrs.onrender.com/webhook/87b282f4-f1fb-4f45-8858-2c765aba285
   ```
   (Não use localhost!)

2. **Token foi preenchido?**
   ```
   webhook_verify_token_marina_2025
   ```

3. **Workflow foi salvo após mudanças?**
   - Confirme que vê "Saved" no botão no topo do n8n

4. **Workflow não está em modo de "teste"?**
   - Não clique em "Listen for test event"
   - Use a URL de **produção**, não de teste

5. **Há um delay?**
   - Após salvar, aguarde ~30 segundos
   - Depois tente novamente em Meta

### Teste Manual do Webhook

Você pode testar se seu webhook está respondendo corretamente:

```bash
curl "https://n8n-service-atrs.onrender.com/webhook/87b282f4-f1fb-4f45-8858-2c765aba285?hub.mode=subscribe&hub.verify_token=webhook_verify_token_marina_2025&hub.challenge=test_challenge_value"
```

Ele deve retornar:
```
test_challenge_value
```

## Estrutura Esperada do Webhook

Quando Meta enviar mensagens reais, o format será:

```json
{
  "entry": [{
    "changes": [{
      "value": {
        "messages": [{
          "from": "551199999999",
          "text": {
            "body": "Olá!"
          },
          "type": "text",
          "id": "wamid.xxx"
        }]
      }
    }]
  }]
}
```

Este é o formato que seu workflow processor (Groq + WhatsApp) receberá.

## Próximos Passos Após Validação Bem-Sucedida

Uma vez que a validação funcione:

1. Mude para a aba **"Messages"** no Meta Developers > Configuração
2. Assinale a opção **"messages"** (já deve estar marcada)
3. Clique em **"Salvar"**
4. Aguarde confirmação
5. Agora você pode enviar mensagens de teste pelo WhatsApp!

## Mais Informações

- [Documentação completa de configuração](./N8N_WHATSAPP_WORKFLOW_CONFIG.md)
- [Meta Webhook Docs](https://developers.facebook.com/docs/whatsapp/cloud-api/webhooks/setup-webhooks)
- [n8n Webhook Node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/)

## Support

Se o problema persistir:
1. Verifique os Logs do n8n (abra o workflow e clique em "Logs" na parte inferior)
2. Procure por erros na seção "Executions"
3. Confirme que o Render não está dormindo (visite a URL do webhook no navegador)
