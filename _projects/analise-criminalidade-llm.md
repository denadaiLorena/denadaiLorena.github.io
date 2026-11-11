---
layout: post
title: Análise de Criminalidade & LLM
description: Pipeline ETL no PostgreSQL, dashboard Streamlit e integração com LLM via n8n para análise e recomendações de segurança pública.
date: 2026-11-09
order: 3
---


> **Sistema integrado de inteligência em segurança pública:** pipeline de dados automatizado (ETL) sobre microdados do SINESP, modelagem relacional em PostgreSQL, dashboard analítico no Streamlit e copiloto baseado em **LLM orquestrado via n8n** para geração de recomendações personalizadas de proteção.

<p align="left" style="display: flex; gap: 10px; flex-wrap: wrap; margin: 15px 0;">
  <a href="https://youtu.be/oeBOw0o7HhY" target="_blank" style="background-color: #FF0000; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
    <i class="fab fa-youtube"></i> Vídeo de Demonstração
  </a>
  <a href="https://github.com/JoaoPPH/projeto_integrador_4_llm_analise_de_criminalidade" target="_blank" style="background-color: #24292e; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
    <i class="fab fa-github"></i> Ver Código no GitHub
  </a>
</p>

---

## Contexto & Desafio do Projeto

A criminalidade e a segurança pública são desafios estruturais no Brasil. Embora o **SINESP (Sistema Nacional de Informações de Segurança Pública)** forneça dados abertos sobre ocorrências policiais em escala nacional, esses dados costumam estar dispersos em múltiplos arquivos volumosos, com desafios de qualidade, ausência de indexação unificada e sem canais diretos para que o cidadão ou analista obtenha recomendações contextuais para o seu dia a dia.

Desenvolvido como projeto prático para a disciplina de **Projeto Integrador 4** (Curso de Ciência da Computação), este sistema aborda esse desafio de ponta a ponta: desde a extração automatizada e modelagem relacional de alta performance até a criação de um **agente inteligente de segurança preventiva** orientado por LLMs.

---

## Demonstração em Vídeo

Confira a apresentação do protótipo funcional, demonstrando a navegação pelas abas analíticas, a performance das consultas SQL e a integração do copiloto conversacional com n8n:

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; border-radius: 8px; margin: 20px 0; box-shadow: 0 4px 12px rgba(0,0,0,0.15);">
  <iframe src="https://www.youtube.com/embed/oeBOw0o7HhY" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border:0;" allowfullscreen></iframe>
</div>

---

## Arquitetura de Dados & Pipeline ETL

O fluxo do projeto foi desenhado para assegurar integridade referencial, velocidade de consulta e desacoplamento entre armazenamento e inteligência:

```
[Bases SINESP / Gov.br]
          │
          ▼ (etl/extracao_dados.py)
[Tratamento e Normalização com Pandas] (etl/transformacao_dados.py)
          │
          ▼ (Carga Otimizada via COPY com SQLAlchemy)
[Banco de Dados PostgreSQL Indexado]
          │
          ├───────────────────────────────┐
          ▼                               ▼
[Dashboard Streamlit (Views Analíticas)]   [Webhook n8n -> Agente LLM]
          │                               │
          └────────► [Cidadão / Analista] ◄┘
```

### 1. Extração & Transformação (`etl/`)
- **Extração Automatizada:** Download programático dos conjuntos de dados públicos de ocorrências criminais.
- **Tratamento de Dados:** Higienização de campos de data, normalização de UFs e nomes de municípios, tratamento de nulos e estruturação categórica de tipos de crimes e armamentos.

### 2. Carga Otimizada no PostgreSQL
- Em vez de inserções linha a linha (`INSERT`), o carregamento utiliza o comando nativo **`COPY`** do PostgreSQL, acelerando em ordens de magnitude a ingestão de centenas de milhares de linhas.
- Índices B-Tree compostos criados via SQLAlchemy nas colunas mais consultadas (`UF`, `município`, `data_ocorrencia`, `tipo_crime`), garantindo filtros instantâneos no front-end.

---

## Dashboard Analítico Segmentado (Streamlit & Plotly)

A aplicação no Streamlit conta com uma interface organizada em **6 abas especializadas**, cada uma orientada a uma dimensão analítica do combate à violência:

1. **Geral de Crimes:** Panorama macro de ocorrências por estado, volume anual e distribuição temporal.
2. **Vítimas por Gênero:** Cruzamento demográfico de vitimização com recorte de violência contra a mulher e feminicídios.
3. **Armas Apreendidas:** Mapeamento de circulação e apreensão de calibres e armas de fogo ilegais por região.
4. **Morte de Agentes do Estado:** Telemetria de confronto e vitimização de forças de segurança pública em serviço ou folga.
5. **Drogas Apreendidas:** Volume e distribuição territorial de tipos de substâncias ilícitas interceptadas.
6. **Análise Assistida por IA (LLM):** Módulo de interação em linguagem natural integrado ao fluxo cognitivo.

---

## Integração com LLM & Fluxo Agentic no N8N

A inteligência generativa da solução não atua apenas como um chatbot genérico, mas como um **consultor de segurança situacional**:

- **Arquitetura de Webhooks:** O front-end Streamlit (`services/n8n_client.py`) despacha o contexto estruturado da consulta (localidade, horário, perfil) para um endpoint de webhook no **n8n**.
- **Orquestração N8N:** O workflow processa o payload, consulta diretrizes de segurança e aciona o modelo de linguagem (LLM) com engenharia de prompts refinada.
- **Recomendações Contextuais:** O agente analisa quatro pilares de vulnerabilidade:
  - **Meio de Transporte:** Precauções específicas para transporte público, aplicativo, veículo próprio ou a pé.
  - **Acompanhantes:** Cuidados específicos para deslocamentos individuais, com crianças ou idosos.
  - **Período do Dia:** Risco associado à luminosidade, movimentação de rua e janelas horárias críticas.
  - **Objetivo da Viagem:** Fatores de risco conforme o motivo (lazer, trabalho noturno, áreas comerciais ou residenciais).

---

## Qualidade de Software, Testes & Avaliação da IA

O projeto adota práticas formais de validação tanto para o código quanto para as respostas do modelo generativo:

### 1. Testes Unitários com `pytest`
O arquivo `tests/test_n8n_client.py` valida o comportamento do cliente HTTP sem necessidade do servidor n8n estar online, utilizando mocks de requisição:
- `test_uf_obrigatoria`: Assegura validação e tratamento para inputs vazios de UF.
- `test_municipio_obrigatorio`: Garante que consultas exijam seleção municipal válida.
- `test_resposta_sucesso_n8n`: Testa o parsing e desserialização da resposta JSON do agente.

### 2. Framework de Avaliação Manual da IA (`evals/`)
Para mensurar a confiabilidade das orientações geradas pelo LLM, foi implementado um script de benchmark (`evals/avaliacao_ia.py`) com matriz de critérios:

| Critério Avaliado | Escala (0 a 2) | Foco de Avaliação |
| :--- | :---: | :--- |
| **Transporte** | 0 / 1 / 2 | Considera os riscos e cuidados inerentes ao modal escolhido? |
| **Acompanhantes** | 0 / 1 / 2 | Oferece diretrizes proporcionais à vulnerabilidade do grupo? |
| **Período** | 0 / 1 / 2 | Ajusta o nível de alerta para o horário do deslocamento? |
| **Objetivo** | 0 / 1 / 2 | Respeita a finalidade e a dinâmica do local de destino? |
| **Total Máximo** | **8 pontos** | Índice percentual de conformidade da recomendação |

---

## Estrutura de Pastas e Módulos

```text
projeto_integrador_4_llm_analise_de_criminalidade/
├── database/                  # Módulos de conexão e modelo relacional PostgreSQL
│   ├── main.py                # Validação isolada de conexão e payloads
│   ├── models.py              # Declarações SQLAlchemy, tabelas e índices
│   └── queries.py             # Consultas analíticas SQL pré-compiladas
├── etl/                       # Pipeline de Extração, Transformação e Carga
│   ├── extracao_dados.py      # Download dos microdados fonte (SINESP)
│   ├── transformacao_dados.py # Limpeza, tipagem e padronização com Pandas
│   ├── carregamento.py        # Carga massiva otimizada via COPY
│   └── orquestrador_etl.py    # Orquestrador sequencial do pipeline
├── services/                  # Clientes e integrações externas
│   └── n8n_client.py          # Comunicação com o webhook do N8N
├── views/                     # Abas modulares do Streamlit
│   ├── aba_geral.py           # Análise geral de ocorrências
│   ├── aba_genero.py          # Vitimização por gênero
│   ├── aba_armas.py           # Apreensão de armamentos
│   ├── aba_agentes.py         # Mortes de policiais e agentes de segurança
│   ├── aba_drogas.py          # Apreensões de substâncias ilícitas
│   └── aba_analise_ia.py      # Interface conversacional com o LLM
├── evals/                     # Avaliação de qualidade e métricas da IA
│   ├── avaliacao_ia.py        # Script interativo de pontuação por rubrica
│   └── resultado_avaliacao.md # Relatório qualitativo de alinhamento do modelo
├── tests/                     # Suíte de testes automatizados
│   └── test_n8n_client.py     # Testes unitários com pytest
├── app.py                     # Entrypoint da aplicação Streamlit
├── requirements.txt           # Dependências do projeto
└── README.md                  # Especificação técnica
```

---

## Tecnologias Utilizadas

- **Linguagem & Framework Web:** Python 3.10+, [Streamlit](https://streamlit.io/)
- **Armazenamento & ORM:** PostgreSQL 15, [SQLAlchemy](https://www.sqlalchemy.org/)
- **Engenharia de Dados:** Pandas, NumPy
- **Orquestração de IA & Workflows:** [n8n](https://n8n.io/), Webhooks, LLMs (Large Language Models)
- **Machine Learning:** Scikit-Learn, XGBoost, Random Forest
- **Visualização de Dados:** Plotly Express & Plotly Graph Objects
- **Qualidade & DevOps:** Docker, Pytest

---

## Equipe & Informações Acadêmicas

Projeto desenvolvido para a disciplina de **Projeto Integrador 4** do curso de Ciência da Computação:

- **Orientador:** Prof. Howard Cruz
- **Integrantes da Equipe:**
  - Lorena Moreira de Nadai
  - Estevão Matos de Souza
  - Gabriel Ferreira Paulo
  - João Pereira Paes Henriques
  - Pedro de Oliveira Guimarães
  - Thaís Peroni Custodio Lino
