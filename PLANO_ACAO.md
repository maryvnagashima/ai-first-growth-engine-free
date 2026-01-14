# 🚀 PLANO DE AÇÃO: AI-First Growth Engine

## Phase 1: Setup Inicial (Dia 1 - 4 horas)

### 1.1 Configurar conta Groq (IA)
- [ ] Ir para https://console.groq.com
- [ ] Criar conta (apenas email)
- [ ] Gerar API key
- [ ] Guardar em local seguro (ex: arquivo .env)

### 1.2 Configurar WhatsApp Cloud API
- [ ] Criar app em https://developers.facebook.com
- [ ] Nomear: "ai-growth-engine"
- [ ] Adicionar produto "WhatsApp"
- [ ] Obter numero de teste (ou usar número próprio com Business Account)
- [ ] Gerar webhook token e access token

### 1.3 Configurar n8n (já em Render)
- [ ] Acessar https://n8n-service-atrs.onrender.com/
- [ ] Configurar primeira senha
- [ ] Criar nova workflow

---

## Phase 2: MVP - WhatsApp + IA (Dia 2 - 6 horas)

### 2.1 Criar fluxo no n8n
- [ ] Adicionar webhook HTTP para receber msgs WhatsApp
- [ ] HTTP POST from WhatsApp API
- [ ] Parse JSON do body (extrair telïone + texto)
- [ ] Switch node: verificar palavra "consentimento"
- [ ] Se SIM: gerar link de consentimento LGPD
- [ ] Se NAO: enviar para Groq API com prompt
- [ ] Enviar resposta de volta via WhatsApp API
- [ ] Logar em Google Sheets (ID conversa, timestamp, prompt, resposta)

### 2.2 Testar MVP
- [ ] Enviar "Oi" no seu WhatsApp
- [ ] Receber "Quer consentir com LGPD?"
- [ ] Responder "sim"
- [ ] Fazer pergunta → receber resposta da IA
- [ ] Verificar logs em Google Sheets

---

## Phase 3: Program MGM (Dia 3 - 4 horas)

### 3.1 Adicionar lógica de indicação
- [ ] Detectar keywords: "indicar", "convidar", "programa"
- [ ] Gerar link único: `base_url?ref={user_id}_{random_code}`
- [ ] Guardar em Airtable: referral_link, user_id, status
- [ ] Responder ao usuário com link

### 3.2 Rastrear referências
- [ ] Quando novo lead entra com `?ref=xxx`, marcar em Airtable como "converted"
- [ ] Dar crédito ao referrer (ex: desconto, cashback)

---

## Phase 4: Testes A/B (Dia 4 - 5 horas)

### 4.1 Criar dois prompts
- [ ] `prompts/onboarding_v1.json`: tom empático, acolhedor
- [ ] `prompts/onboarding_v2.json`: tom direto, focado em valor

### 4.2 Dividir tráfego 50/50
- [ ] n8n: Random node para escolher prompt (50%/50%)
- [ ] Salvar qual prompt foi usado em Sheets (coluna "variant")

### 4.3 Analisar após 50 interações
- [ ] Qual variant tem mais respostas? Qual menos rejeitações?
- [ ] Atualizar arquivo `experiment-log.csv` com resultado
- [ ] Congelar o prompt vencedor

---

## Phase 5: Inteligência Competitiva (Dia 5 - 3 horas)

### 5.1 Criar script Python
```bash
python3 scripts/scrape_competitors.py
```
- [ ] Script monitora 3 concorrentes
- [ ] Procura por mudancas em preços / promoções
- [ ] Usa Groq para gerar insights
- [ ] Salva em `docs/competitive-intel.md`

### 5.2 Automatizar com GitHub Actions
- [ ] Criar `.github/workflows/daily-intel.yml`
- [ ] Rodar script todos os dias às 8h da manhã
- [ ] Fazer push automático dos resultados

---

## Phase 6: Dashboard & Comunicação (Dia 6 - 4 horas)

### 6.1 Criar dashboard Google Data Studio
- [ ] Conectar Google Sheets como fonte
- [ ] Gráficos:
  - Consentimento (%)
  - CPA simulado (tendência)
  - Taxa de ativação MGM
  - Tempo de resposta médio

### 6.2 Escrever Executive Summary
- [ ] Já feito em `docs/executive-summary.md`
- [ ] Traduzir para inglés (v2)

---

## Phase 7: Demo & Publicação (Dia 7 - 2 horas)

### 7.1 Gravar vídeo de demo (90s)
- [ ] Problema (30s): "Escalabilidade com conformidade"
- [ ] Solução (30s): "Fluxo WhatsApp + IA + testes"
- [ ] Resultado (30s): "35% menos CPA, pronto para 50k /mês"
- [ ] Publicar no YouTube (público)

### 7.2 LinkedIn Post
- [ ] Título: "Como eu resolveria o maior desafio de Growth da Stellantis hoje — com R$ 0"
- [ ] Carregar vídeo
- [ ] Ligar para link do repo

### 7.3 Atualizar CV/Portfólio
- [ ] Adicionar projeto com skills: n8n, WhatsApp API, Groq, Airtable, GA4, Python

---

## ⚠️ Checklist de Recursos Obrigatórios

- [ ] **Groq API Key** (gratis)
- [ ] **Meta App ID + WhatsApp token** (gratis para 1k msg/mês)
- [ ] **n8n instancia** (já em Render, gratis)
- [ ] **Google Sheets** (gratis)
- [ ] **Airtable Free** (gratis)
- [ ] **GitHub** (gratis)
- [ ] **Google Data Studio** (gratis)
- [ ] **Python 3.9+** (instalar se não tiver)

---

## ⏰ Timeline Sugerida

| Dia | Fase | Horas | Status |
|-----|------|-------|--------|
| 1 | Setup Groq + WhatsApp + n8n | 4h | [ ] |
| 2 | MVP WhatsApp + IA + Logging | 6h | [ ] |
| 3 | MGM (referral program) | 4h | [ ] |
| 4 | A/B Testing | 5h | [ ] |
| 5 | Competitive Intel | 3h | [ ] |
| 6 | Dashboard + Docs | 4h | [ ] |
| 7 | Vídeo + LinkedIn + CV | 2h | [ ] |
| **Total** | | **28h** | **Semana Intensa!** |

---

## 📞 Suporte Rápido

De forem bloqueado:
1. **n8n não conecta**: reinicie o serviço no Render dashboard
2. **WhatsApp timeout**: verificar webhook token (39 caracteres, sem espaços)
3. **Groq error 429**: rate limit; aguarde 1 min ou use modele menor
4. **Google Sheets auth**: reiniciar OAuth no n8n

---

## 🌟 Próximos passos após MVP

- Adicionar suporte a **imagens** no WhatsApp
- Integrar **CRM profissional** (Pipedrive API gratis)
- Implementar **memory** do conversa (contexto entre mensagens)
- Expandir para **SMS** (via Twilio free tier)
- **Monetizar**: oferecer como SaaS para agencias

Good luck! 🚀
