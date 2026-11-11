---
layout: post
title: Ecommerce AI Dashboard
description: Plataforma executiva com IA preditiva (Holt-Winters), anomalias (Z-Score), simulador What-If e copiloto LLM no IBM Carbon.
date: 2026-11-09
image: /assets/images/EcommerceAIDashboard.png
order: 1
---


> **Plataforma analítica executiva de e-commerce** com inteligência preditiva, detecção contínua de anomalias, simulador estratégico *What-If* e copiloto de IA analítica generativa. Construída com base nas diretrizes de usabilidade e tokens do **IBM Carbon Design System**.


<p align="left" style="display: flex; gap: 10px; flex-wrap: wrap; margin: 15px 0;">
  <a href="https://ecommerce-ai-dashboard-1.onrender.com/#/" target="_blank" style="background-color: #0047AB; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
     Aplicação Online
  </a>
  <a href="https://github.com/denadaiLorena/Ecommerce-AI-Dashboard.git" target="_blank" style="background-color: #24292e; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
    <i class="fab fa-github"></i> Ver Código no GitHub
  </a>
</p>

<p align="center" style="margin: 25px 0;">
  <img src="{{ '/assets/images/EcommerceAIDashboard.png' | relative_url }}" alt="Ecommerce AI Dashboard" style="width: 100%; border-radius: 12px; box-shadow: 0 4px 16px rgba(0,0,0,0.12);">
</p>

---

##  Visão Geral & Desafio de Negócio

Em grandes operações de comércio eletrônico, diretores e tomadores de decisão frequentemente enfrentam o problema da **saturação de dados sem contexto acionável**: dezenas de gráficos estáticos que mostram *o que aconteceu*, mas falham em explicar *por que aconteceu* e *o que acontecerá a seguir*.

O **Ecommerce AI Dashboard (AI Metric Pulse)** foi concebido como uma solução analítica full-stack voltada para diretores e gestores de e-commerce. Integrado à base de dados real do marketplace brasileiro **Olist** (~100.000 pedidos reais), o sistema combina telemetria de negócios de alta precisão com algoritmos estatísticos de *Machine Learning* e inteligência artificial generativa via LLM.

---

##  Principais Capacidades & Inteligência Analítica

### 1. Co-Piloto de IA Executiva (Watsonx Style)
Assistente analítico conversacional integrado diretamente à interface executiva. Alimentado por LLM via **Groq Cloud** (`openai/gpt-oss-120b`), o copiloto é capaz de:
- Responder em tempo real a perguntas analíticas abertas em linguagem natural sobre o desempenho da operação;
- Executar diagnósticos automáticos correlacionando múltiplos indicadores (ex.: queda de conversão em determinada região cruzada com aumento de lead time de frete);
- Fornecer recomendações executivas fundamentadas nos dados reais consolidados da sessão.

### 2. Previsão Preditiva de Demanda (Holt-Winters 7D)
Modelo de séries temporais baseado em **suavização exponencial tripla (Holt-Winters)** projetado para antecipar o volume de vendas e faturamento:
- Modelação de tendência linear e **sazonalidade semanal aditiva/multiplicativa**;
- Cálculo dinâmico de **intervalo de confiança a 95%**;
- Validação através de backtesting histórico de acurácia contra a série real de pedidos.

### 3. Detecção Estatística de Anomalias (Z-Score)
Monitoramento estatístico contínuo em janela móvel de **28 dias**:
- Detecção instantânea de picos anômalos de demanda ou quedas bruscas de faturamento;
- Threshold estatístico baseado no desvio padrão ($\pm 3\sigma$);
- Sinalização visual na linha do tempo para investigação rápida antes que impactos operacionais afetem a reputação da loja.

### 4. Simulador de Decisões Estratégicas (*What-If*)
Módulo interativo de simulação de cenários que permite ao gestor testar o impacto de decisões táticas antes de implementá-las:
- **Alavancas simuladas:** Redução no prazo de entrega, expansão de opções de parcelamento sem juros e subsídio de frete para regiões específicas (Norte e Nordeste);
- **Cálculo de Elasticidade:** Projeta matematicamente o reflexo estimado no faturamento bruto, margem operacional e na nota média de satisfação (**CSAT**).

### 5. Diagnóstico da Cadeia Logística & Clientes Detratores
Decomposição cirúrgica dos pontos críticos da jornada de atendimento:
- **Decomposição de Lead Time:** Separação granular do tempo gasto pelo vendedor (tempo até despacho) versus o tempo sob responsabilidade da transportadora (trânsito logístico);
- **Análise de Causa-Raiz de Detratores:** Agrupamento e categorização dos motivos por trás de avaliações negativas (reviews de 1 e 2 estrelas), permitindo atuar diretamente no gargalo operacional.

### 6. Exportação & Relatórios Executivos Oficiais
- **Relatório Executivo A4/PDF:** Emissão formatada de documento executivo pronto para reuniões de diretoria e impressão;
- **Exportação Consolidada CSV:** Download da base de indicadores com codificação UTF-8 com BOM, assegurando compatibilidade imediata com Microsoft Excel e ferramentas de BI.

---

## Design System: IBM Carbon

A experiência de usuário e interface visual foram desenvolvidas seguindo estritamente as diretrizes, tokens e padrões do **[IBM Carbon Design System](https://carbondesignsystem.com)**:

- **Regra 60-30-10:** 60% neutros de superfície (camadas estruturadas *Layer 01* e *Layer 02*), 30% secundários estruturais (*Carbon Shell Header* escuro `#161616` com tipografia nítida) e 10% cor de destaque.
- **Cor Primária Única:** *IBM Blue 60* (`#0f62fe` no tema claro e `#4589ff` no tema escuro) para navegação, chamadas de ação e elementos interativos.
- **Cores Semânticas Estritas:** Verde, amarelo e vermelho reservados exclusivamente para status, feedback e alertas estatísticos de anomalia — nunca utilizados com função puramente ornamental.
- **Tipografia Oficial:** Famílias **IBM Plex Sans** (alta legibilidade em telas e densidade de dados) e **IBM Plex Mono** (para códigos, identificadores e métricas analíticas).
- **Grade e Espaçamento (Base 8px):** Alinhamento em escala modular de 8px e container central limitado ao breakpoint oficial do Carbon (*1584px*), prevenindo quebras visuais em monitores ultrawide.
- **Acessibilidade:** Níveis de contraste testados e em conformidade estrita com as diretrizes **WCAG AA/AAA** em ambos os modos (Light e Dark).

---

## Arquitetura da Solução

O sistema foi arquitetado de forma desacoplada entre uma API de alta performance orientada a dados e uma Single Page Application reativa:

```text
FoodCommerce-AI-Dashboard/
├── backend/
│   ├── data/                   # Datasets públicos do Olist E-commerce (CSV)
│   ├── services/
│   │   ├── ai_service.py       # Integração com Groq LLM (GPT-OSS 120B)
│   │   └── data_service.py     # Pipeline Pandas, Holt-Winters, Z-Score & Métricas
│   ├── app.py                  # API REST Flask com rotas de métricas e health check
│   ├── requirements.txt        # Dependências Python (Flask, Pandas, Gunicorn, Groq)
│   ├── Dockerfile              # Container de produção com Python 3.11 e Gunicorn
│   ├── Procfile                # Comando de deploy para plataformas PaaS
│   └── .env.example            # Modelo de variáveis de ambiente
├── frontend/
│   ├── src/
│   │   ├── components/         # Componentes Vue (KPIs, Gráficos, IA, Simulador, Modal)
│   │   ├── css/
│   │   │   ├── tokens.css      # Design tokens centralizados do IBM Carbon
│   │   │   └── app.css         # Estilização global e resets Carbon
│   │   ├── layouts/
│   │   │   └── MainLayout.vue  # Header corporativo Carbon Shell (48px)
│   │   ├── pages/
│   │   │   └── IndexPage.vue   # Dashboard panorâmico em linhas completas
│   │   ├── api.ts              # Cliente Axios desacoplado com fallback dinâmico
│   │   └── tokens.ts           # Exportador de tokens Carbon para scripts e ApexCharts
│   ├── package.json
│   └── quasar.config.ts        # Configuração do Quasar Framework & Vite
├── start.bat                   # Inicializador rápido para Windows (CMD)
├── start.ps1                   # Inicializador rápido para Windows (PowerShell)
├── .gitignore                  # Regras globais de exclusão do Git
└── README.md
```

---

## Catálogo de Endpoints da API REST

A API do backend disponibiliza endpoints especializados para consulta assíncrona dos módulos estatísticos e visuais:

| Método | Endpoint | Finalidade / Retorno |
| :--- | :--- | :--- |
| `GET` | `/health` | Checagem de integridade do serviço (status de disponibilidade) |
| `GET` | `/api/kpis` | Métricas consolidadas (faturamento total, volume de pedidos, lead time médio, cancelamentos, CSAT) |
| `GET` | `/api/charts/orders-by-hour` | Distribuição temporal de pedidos por hora do dia |
| `GET` | `/api/charts/sales-by-month` | Série temporal de faturamento histórico mensal |
| `GET` | `/api/charts/payments` | Proporção de vendas por método de pagamento (cartão, boleto, voucher) |
| `GET` | `/api/charts/top-states` | Ranking dos Top 10 estados brasileiros por volume transacionado |
| `GET` | `/api/charts/top-categories` | Top 10 categorias de produtos por receita gerada |
| `GET` | `/api/charts/delivery-vs-score` | Correlação empírica entre dias decorridos de entrega e nota de avaliação |
| `GET` | `/api/charts/anomalies` | Série histórica com identificação de anomalias estatísticas ($\pm 3\sigma$) |
| `GET` | `/api/charts/forecast` | Projeção preditiva dos próximos 30 dias com intervalos de confiança |
| `GET` | `/api/charts/lead-time` | Funil do ciclo do pedido (aprovação, despacho pelo lojista, trânsito final) |
| `GET` | `/api/charts/freight-by-region`| Razão percentual do custo de frete sobre o valor do pedido por região |
| `GET` | `/api/charts/installments-ticket` | Ticket médio por quantidade de parcelas no cartão de crédito |
| `GET` | `/api/charts/reviews-distribution` | Distribuição de notas dos clientes (escala de 1 a 5 estrelas) |
| `GET` | `/api/charts/review-topics` | Categorização semântica das causas-raiz de insatisfação |
| `POST` | `/api/simulate` | Simulação dinâmica de cenários *What-If* com cálculo de elasticidade |
| `POST` | `/api/ai-insight` | Consulta analítica generativa com Groq LLM |
| `GET` | `/api/export/csv` | Download do relatório completo de métricas em formato CSV UTF-8 |

---

## Tecnologias Utilizadas

- **Frontend & Visualização:** [Vue 3](https://vuejs.org/), [Quasar Framework v2](https://quasar.dev/), [Vite](https://vitejs.dev/), [TypeScript](https://www.typescriptlang.org/), [ApexCharts](https://apexcharts.com/), [Axios](https://axios-http.com/).
- **Design System:** [IBM Carbon Design System](https://carbondesignsystem.com/) & tipografia [IBM Plex](https://www.ibm.com/plex/).
- **Backend & Engenharia de Dados:** [Python 3.11](https://www.python.org/), [Flask](https://flask.palletsprojects.com/), [Pandas](https://pandas.pydata.org/), [NumPy](https://numpy.org/), [Gunicorn](https://gunicorn.org/).
- **Inteligência Artificial:** [Groq Cloud](https://groq.com/) (modelo `openai/gpt-oss-120b`).
- **DevOps & Infraestrutura:** [Docker](https://www.docker.com/), [Procfile](https://devcenter.heroku.com/articles/procfile), scripts de automação PowerShell/Batch.

---

## Licença & Dados

Desenvolvido para fins demonstrativos e executivos. O conjunto de dados utilizado é proveniente do [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), disponibilizado publicamente no Kaggle.
