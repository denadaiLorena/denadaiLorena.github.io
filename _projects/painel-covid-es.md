---
layout: post
title: Painel COVID-19 Espírito Santo
description: Dashboard analítico em Streamlit com pipeline otimizado em Parquet, regras de negócio para letalidade e mapas coropléticos com Plotly.
date: 2026-11-09
---


> **Dashboard analítico interativo de alta performance** desenvolvido em **Streamlit** e **Plotly** para exploração, saneamento e visualização dos microdados públicos da pandemia de COVID-19 no Estado do Espírito Santo.

<p align="left" style="display: flex; gap: 10px; flex-wrap: wrap; margin: 15px 0;">
  <a href="https://an-lise-de-dados-da-covid-19-epdcsgchyceswrkjnqhd3r.streamlit.app/" target="_blank" style="background-color: #0047AB; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
     Aplicação Online (Streamlit)
  </a>
  <a href="https://github.com/denadaiLorena/analise-de-dados-da-covid-19" target="_blank" style="background-color: #24292e; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
    <i class="fab fa-github"></i> Ver Código no GitHub
  </a>
</p>

---

## Visão Geral & Desafio de Engenharia de Dados

A base oficial de **microdados da COVID-19** disponibilizada pelo Governo do Estado do Espírito Santo reúne milhões de registros nominais e detalhados de notificações, testes, comorbidades e evoluções clínicas. Analisar esses dados em aplicações web interativas apresenta desafios significativos de **engenharia de software e ciência de dados**:

1. **Volume e Gargalo de I/O:** O arquivo bruto em CSV (`MICRODADOS.csv`) ultrapassa gigabytes, tornando inviável a leitura direta sob demanda no ambiente em nuvem ou no navegador.
2. **Inconsistências de Notificação:** Registros epidemiológicos reais frequentemente apresentam campos nulos, registros sem data ou incongruências entre evolução e data de desfecho.
3. **Vício Estatístico no Cálculo de Letalidade:** Calcular a taxa de letalidade dividindo óbitos pelo total bruto de notificações gera distorções severas quando há um grande volume de casos em investigação ou sem desfecho consolidado.

O **Painel COVID-19 ES** foi projetado para solucionar esses gargalos unindo **pipeline ETL colunar otimizado (Apache Parquet)**, **regras epidemiológicas explícitas** e uma interface interativa centrada em **Data Storytelling**.

---

## Pipeline ETL & Otimização de Performance

Para garantir carregamento instantâneo no Streamlit sem esgotar os limites de memória RAM, o projeto adota uma arquitetura em duas camadas:

```
[MICRODADOS.csv bruto] 
       │
       ▼ (prepare_data.py - Leitura em Chunks de 100k linhas)
[Downcasting de tipos + Saneamento + Filtragem de colunas]
       │
       ▼ (Compressão Snappy)
[dados_es_filtrados.parquet] ──► [Streamlit App (Cache colunar PyArrow)]
```

### 1. Processamento em Chunks & Downcasting de Tipos
O script de pré-processamento (`prepare_data.py`):
- Processa o CSV bruto em **lotes de 100.000 linhas**, mantendo a pegada de memória estável e prevenindo erros de *Out-Of-Memory* (OOM).
- Remove colunas textuais redundantes e normaliza variáveis clínicas: comorbidades são convertidas para inteiros ultraleves `int8` (sendo `1` = sim, `0` = não e `-1` = ignorado/desconhecido).
- Gera o arquivo `dados_es_filtrados.parquet` com compressão colunar **Snappy**, reduzindo drasticamente o tamanho em disco e acelerando o tempo de leitura em até 10x em comparação ao CSV original.

### 2. Saneamento Vetorizado no Carregamento
Dentro do módulo `covid_app/data.py`, o carregamento e enriquecimento do dataset são executados de forma vetorizada com **NumPy**:
- Padronização da coluna `Evolucao` (tratamento de hifens e valores nulos).
- Tratamento de inconsistências epidemiológicas (ex.: notificações marcadas como óbito, porém com `DataObito` nula).
- Classificação analítica rápida através de `np.select` para criar a variável `Status_Analise`.

---

## Regras de Negócio & Metodologia Epidemiológica

### 1. Classificação de `Status_Analise`
Cada registro do dataset é classificado conforme seu desfecho consolidado:
- **Óbito:** Evolução confirmada com menção a óbito e data registrada.
- **Recuperado (Cura):** Notificação com evolução confirmada de cura clínica.
- **Em Aberto / Ignorado:** Casos sem data de óbito ou desfecho conclusivo no momento da notificação.
- **Outros:** Demais classificações secundárias.

### 2. Regra Estrita da Taxa de Letalidade
Para evitar distorções estatísticas decorrentes de atrasos de notificação, a taxa de letalidade apresentada no dashboard é calculada **estritamente sobre desfechos finais consolidados**:

$$\text{Taxa de Letalidade (\%)} = \frac{\text{Óbitos}}{\text{Óbitos} + \text{Recuperados}} \times 100$$

> **Por que isso é fundamental?** No auge de ondas de contágio, o volume de casos notificados "em aberto" infla o denominador comum, subestimando artificialmente o risco real. Ao restringir o cálculo aos desfechos fechados (cura ou óbito), obtém-se uma medida fiel da gravidade clínica.

---

## Visualizações & Data Storytelling (Plotly)

O painel foi construído guiando o usuário por uma narrativa visual coerente:

- **Cartões de KPIs Consolidados:** Total de notificações, óbitos confirmados, casos curados e taxa de letalidade ajustada para o recorte selecionado.
- **Tabela Dinâmica por Município:** Visão tabular com gradiente condicional proporcional ao volume de casos.
- **Mapa Coroplético Estadual:** Visualização geoespacial interativa com GeoJSON dos municípios capixabas, permitindo analisar por hover a distribuição geográfica de notificações e letalidade.
- **Ranking Top 10 de Risco:** Municípios com maior índice de letalidade, aplicando filtro de corte estatístico por volume mínimo para evitar distorções de pequenas populações.
- **Perfil de Comorbidades:** Cruzamento de prevalência de fatores de risco pré-existentes (diabetes, cardiopatias, doenças renais, obesidade) versus letalidade associada.
- **Análise por Faixa Etária:** Distribuição etária das infecções e o aumento exponencial do risco nas faixas acima de 60 anos.
- **Estudo de Sobrevida & Tempo até Óbito:** Histograma e boxplots dos dias decorridos entre a notificação e o óbito, com higienização de valores extremos pelo método de **Amplitude Interquartil (IQR)**.
- **Curvas Temporais & Taxa de Cura:** Análise longitudinal da evolução da pandemia ao longo das semanas epidemiológicas.

---

## Exportação Controlada de Dados

Para viabilizar pesquisas e análises downstream por outros profissionais:
- **Exportação do Recorte Ativo:** O botão de download gera exatamente os dados correspondentes aos filtros de município e período temporal ativos na sessão.
- **Compactação Gzip:** O arquivo é exportado no formato `CSV compactado (.csv.gz)`, economizando banda e tempo de download.
- **Mecanismo de Salvaguarda:** Limite de segurança de `300.000` linhas na exportação direta via interface, prevenindo travamento do navegador do usuário.

---

## Arquitetura Modular do Projeto

O código foi estruturado em módulos independentes seguindo o princípio da responsabilidade única:

```text
painel-covid-es/
├── app.py                      # Ponto de entrada, injeção de CSS global e orquestração
├── prepare_data.py             # Script CLI de ETL: CSV -> Parquet em chunks
├── requirements.txt            # Dependências com versões especificadas
├── covid_app/
│   ├── data.py                 # Leitura com cache, saneamento e vetorização com np.select
│   ├── filters.py              # Sidebar de filtros dinâmicos (município e período)
│   ├── geo.py                  # Integração GeoJSON e normalização fonética de nomes
│   ├── export.py               # Serialização e compressão para download (.csv.gz)
│   ├── features.py             # Engenharia de atributos (cálculo de idades e datas)
│   ├── nav.py                  # Sumário e navegação por âncoras na sidebar
│   └── sections/               # Seções analíticas desacopladas em módulos visuais
│       ├── kpis.py             # Indicadores de topo
│       ├── tabela.py           # Tabela com formatação condicional
│       ├── mapa_ranking.py     # Mapa coroplético e ranking top 10
│       ├── comorbidades.py     # Análise de fatores de risco
│       ├── sobrevida.py        # Curvas de sobrevida e remoção de outliers IQR
│       └── temporal.py         # Séries temporais e evolução epidemiológica
└── data/
    └── dados_es_filtrados.parquet  # Base colunar de produção
```

---

## Interface & Acessibilidade

- **Identidade Visual Temática:** Accent color em tom **Azul Cobalto (`#0047AB`)** aplicada a cabeçalhos, destaques e botões de ação.
- **Suporte a Dark Mode:** Ajustes customizados de contraste e tipografia via CSS injetado, garantindo legibilidade tanto no tema claro quanto no escuro.
- **Navegação Inteligente:** Barra lateral com sumário automático e âncoras de navegação direta para facilitar a leitura em telas menores.

---

## Tecnologias Utilizadas

- **Framework Web:** [Streamlit](https://streamlit.io/)
- **Processamento & Engenharia de Dados:** [Pandas](https://pandas.pydata.org/), [NumPy](https://numpy.org/), [PyArrow](https://arrow.apache.org/docs/python/) (Apache Parquet)
- **Visualização de Dados & Geospatial:** [Plotly](https://plotly.com/python/), GeoJSON
- **Linguagem:** Python 3.10+
- **Deploy em Nuvem:** [Streamlit Community Cloud](https://streamlit.io/cloud)
