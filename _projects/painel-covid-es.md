---
layout: post
title: Painel COVID-19 Espírito Santo
description: Dashboard analítico em Streamlit com pipeline otimizado em Parquet, regras epidemiológicas de letalidade e mapa coroplético do ES.
date: 2026-11-09
image: /assets/images/project/PainelCOVID19.png
order: 2
---

<style>
/* Seletor de Modo de Leitura */
.reading-mode-container {
  background: var(--card-bg, rgba(0, 71, 171, 0.04));
  border: 1px solid rgba(0, 71, 171, 0.2);
  border-radius: 12px;
  padding: 16px 20px;
  margin: 20px 0 30px 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.reading-mode-header {
  font-size: 0.95rem;
  font-weight: 600;
  color: #0047AB;
  display: flex;
  align-items: center;
  gap: 8px;
}
.reading-mode-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}
.mode-btn {
  background: #ffffff;
  color: #333333;
  border: 1px solid #d0d7de;
  border-radius: 20px;
  padding: 8px 16px;
  font-size: 0.88rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  display: inline-flex;
  align-items: center;
  gap: 6px;
}
.mode-btn:hover {
  background: #f3f4f6;
  border-color: #0047AB;
}
.mode-btn.active {
  background: #0047AB;
  color: #ffffff;
  border-color: #0047AB;
  box-shadow: 0 2px 6px rgba(0, 71, 171, 0.25);
}

/* Cards de Perspectiva Dupla */
.perspective-card {
  border-radius: 10px;
  padding: 18px 22px;
  margin: 18px 0;
  line-height: 1.6;
  font-size: 0.95rem;
}
.card-rh {
  background: rgba(16, 185, 129, 0.08);
  border-left: 5px solid #10b981;
}
.card-tech {
  background: rgba(0, 71, 171, 0.07);
  border-left: 5px solid #0047AB;
}
.card-title {
  font-weight: 700;
  font-size: 1rem;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.card-rh .card-title { color: #047857; }
.card-tech .card-title { color: #0047AB; }

/* Tabela de Decisões */
.tradeoff-table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
  font-size: 0.92rem;
}
.tradeoff-table th, .tradeoff-table td {
  padding: 12px 14px;
  border: 1px solid #e1e4e8;
  text-align: left;
}
.tradeoff-table th {
  background: #0047AB;
  color: #ffffff;
}
.tradeoff-table tr:nth-child(even) {
  background: rgba(0, 71, 171, 0.02);
}
</style>

<div class="reading-mode-container">
  <div class="reading-mode-header">
    <i class="fas fa-sliders-h"></i> <strong>Escolha como deseja explorar este projeto:</strong>
  </div>
  <div class="reading-mode-buttons">
    <button class="mode-btn active" id="btn-all" onclick="setReadingMode('all')">
      📖 Visão Completa
    </button>
    <button class="mode-btn" id="btn-rh" onclick="setReadingMode('rh')">
      💼 Visão Executiva & RH (Problema, Impacto e Decisões)
    </button>
    <button class="mode-btn" id="btn-tech" onclick="setReadingMode('tech')">
      ⚙️ Visão Técnica & Engenharia (Código, Dados e Trade-offs)
    </button>
  </div>
</div>

<p align="left" style="display: flex; gap: 10px; flex-wrap: wrap; margin: 15px 0 25px 0;">
  <a href="https://an-lise-de-dados-da-covid-19-epdcsgchyceswrkjnqhd3r.streamlit.app/" target="_blank" style="background-color: #0047AB; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
     <i class="fas fa-chart-line"></i> Aplicação Online (Streamlit)
  </a>
  <a href="https://github.com/denadaiLorena/An-lise-de-Dados-da-COVID-19.git" target="_blank" style="background-color: #24292e; color: #ffffff; padding: 10px 18px; border-radius: 6px; text-decoration: none; font-weight: bold; display: inline-flex; align-items: center; gap: 8px;">
    <i class="fab fa-github"></i> Ver Código no GitHub
  </a>
</p>

---

## 🎯 O Que é o Projeto & Pitch de Impacto

> **Dashboard analítico interativo de alta performance** desenvolvido para exploração, saneamento e visualização dos microdados epidemiológicos da pandemia de COVID-19 no Estado do Espírito Santo.

<div class="perspective-card card-rh view-rh">
  <div class="card-title">💼 O Ponto de Vista do Gestor & RH (Em Termos Simples)</div>
  <p><strong>Imagine o cenário:</strong> No pico de uma crise sanitária, diretores de hospitais e secretários de saúde precisam decidir com urgência: <em>onde faltará leito de UTI amanhã? Quais comorbidades têm maior risco de morte para priorizar medicamentos escassos? Quais cidades precisam de reforço imediato?</em></p>
  <p>Os dados governamentais existiam, mas estavam "trancados" em planilhas gigantescas de mais de 10 gigabytes que travavam computadores comuns. Este projeto transformou esses dados brutos em uma <strong>ferramenta de apoio à decisão rápida na palma da mão</strong>, permitindo que qualquer tomador de decisão filtre cidades, faixas etárias e riscos em segundos sem precisar entender de programação.</p>
</div>

<div class="perspective-card card-tech view-tech">
  <div class="card-title">⚙️ O Ponto de Vista da Engenharia (Arquitetura & Rigor)</div>
  <p>O desafio técnico central foi construir uma aplicação analítica interativa no <strong>Streamlit Cloud</strong> (ambiente com limitação de ~1GB de memória RAM) sobre um dataset bruto em CSV com <strong>mais de 10GB de dados nominais</strong>. A aplicação implementa um pipeline de extração e tratamento offline desacoplado, armazenamento colunar com compressão Snappy em <strong>Apache Parquet</strong>, redução de tipos primitivos (<em>downcasting</em>) e cálculo de métricas epidemiológicas imunes ao viés de atraso de notificação (<em>reporting lag</em>).</p>
</div>

---

## ⚡ O Desafio de Dados: De 10GB Inviáveis para Megabytes Instantâneos

A base oficial de **microdados da COVID-19** disponibilizada pelo Governo do ES reunia milhões de linhas com detalhes de sintomas, testes, histórico médico e evolução dos pacientes.

<div class="perspective-card card-rh view-rh">
  <div class="card-title">💼 Como o problema foi resolvido sem gastar orçamento</div>
  <p>Um arquivo de 10GB não pode ser enviado para o GitHub (que limita arquivos a 100MB) nem roda em servidores gratuitos de nuvem. A alternativa comum seria pagar caro por um banco de dados em nuvem. Em vez de aumentar custos de infraestrutura, apliquei uma técnica inteligente de "compactação seletiva": limpei repetições desnecessárias e converti o formato do arquivo. O resultado? <strong>O arquivo de 10GB encolheu para poucos megabytes</strong>, abrindo instantaneamente no navegador com custo de servidor igual a zero.</p>
</div>

<div class="perspective-card card-tech view-tech">
  <div class="card-title">⚙️ Pipeline ETL: Processamento em Lotes e Downcasting</div>
  <p>Para ler um arquivo de 10GB em máquinas com memória finita sem disparar erros de <em>Out-Of-Memory (OOM)</em>, o script <code>prepare_data.py</code> adota uma estratégia de processamento em chunks:</p>
  <ul>
    <li><strong>Leitura em Chunks de 100.000 linhas:</strong> O Pandas processa o CSV particionado, mantendo a pegada de RAM estável durante toda a execução.</li>
    <li><strong>Downcasting Agressivo de Tipos:</strong> Campos binários e clínicos de comorbidades (cardiopatia, diabetes, tabagismo) foram convertidos de inteiros de 64 bits para <code>int8</code> (<code>1</code> = Sim, <code>0</code> = Não, <code>-1</code> = Ignorado/Não informado), poupando até 87.5% de memória por coluna numérica.</li>
    <li><strong>Compressão Colunar Snappy (Apache Parquet):</strong> A gravação final em Parquet preserva metadados de schema e permite leitura seletiva de colunas via <strong>Apache Arrow</strong> diretamente no Streamlit, acelerando a inicialização em mais de 10x.</li>
  </ul>
</div>

```
[MICRODADOS.csv bruto (>10 GB)] 
       │
       ▼ (prepare_data.py - Leitura em Lotes de 100k linhas)
[Downcasting para int8 + Remoção de redundâncias + Saneamento]
       │
       ▼ (Compressão Snappy via PyArrow)
[dados_es_filtrados.parquet (~poucos MBs)] ──► [Streamlit App (Cache em Memória)]
```

---

## ⚖️ Decisões de Arquitetura & Matriz de Trade-offs

Em engenharia de dados, toda escolha carrega vantagens e concessões. Abaixo estão as decisões deliberadas do projeto:

<table class="tradeoff-table">
  <thead>
    <tr>
      <th>Decisão Tomada</th>
      <th>Alternativa Descartada</th>
      <th>Por que foi tomada? (Motivação & Trade-off)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Arquivo Parquet Local</strong></td>
      <td>Banco de Dados SQL em Nuvem (Postgres / MySQL)</td>
      <td><strong>Arquitetura Zero-Ops:</strong> Bancos gratuitos na nuvem possuem limites rígidos de armazenamento (~500MB), sofrem com <em>cold start</em> (hibernação) e adicionam latência de rede a cada filtro do usuário. O Parquet colunar carrega direto na RAM do app com tempo de resposta milissegundo.</td>
    </tr>
    <tr>
      <td><strong>Streamlit (Python)</strong></td>
      <td>Frontend Personalizado (React / Next.js)</td>
      <td><strong>Velocidade de Entrega:</strong> O Streamlit permitiu ir da análise exploratória à aplicação funcional em tempo recorde com 100% de integração ao ecossistema científico (Pandas, Plotly, NumPy).</td>
    </tr>
    <tr>
      <td><strong>ETL Manual Offline</strong></td>
      <td>Orquestrador Automatizado (Airflow / Cron Jobs)</td>
      <td><strong>Pragmatismo contra Overengineering:</strong> Os dados da COVID-19 são um dataset histórico consolidado. Criar rotinas agendadas contínuas geraria custo e manutenção injustificados para um dado que não sofre mutações diárias contínuas.</td>
    </tr>
  </tbody>
</table>

---

## 🔬 Rigor Estatístico: A Fórmula da Taxa de Letalidade

Um dos erros mais comuns em painéis de saúde pública na época da pandemia foi a publicação de índices distorcidos de letalidade.

$$\text{Taxa de Letalidade Adotada (\%)} = \frac{\text{Óbitos Consolidados}}{\text{Óbitos} + \text{Recuperados}} \times 100$$

<div class="perspective-card card-rh view-rh">
  <div class="card-title">💼 Por que a fórmula tradicional enganava as pessoas?</div>
  <p>Muitos sites dividiam o número de mortos pelo total de notificações. O problema: milhares de pessoas que haviam testado positivo ainda estavam no hospital ou em isolamento, sem um desfecho final. Ao colocar esses casos "em aberto" na conta, a taxa de gravidade parecia falsamente baixa. Ao calcular apenas entre quem <strong>já havia se recuperado ou infelizmente falecido</strong>, garantimos um número confiável e real da gravidade da doença.</p>
</div>

<div class="perspective-card card-tech view-tech">
  <div class="card-title">⚙️ Tratamento Estatístico: Viés de Notificação & Outliers (IQR)</div>
  <p>O pipeline aplicou duas regras de consistência metodológica:</p>
  <ul>
    <li><strong>Blindagem contra Censura à Direita (<em>Right-Censored Data</em>):</strong> Exclusão de notificações com desfecho pendente no denominador da taxa de letalidade, isolando a população de casos fechados.</li>
    <li><strong>Higienização de Anomalias com IQR (Amplitude Interquartil):</strong> No estudo do tempo decorrido entre a notificação e o óbito, foram identificados registros com datas inconsistentes (erros de digitação de datas no passado distante) e internações excepcionais de cauda longa. O método do IQR (corte a $1.5 \times \text{IQR}$) eliminou as distorções que arrastavam a média estatística, permitindo identificar com precisão a <strong>janela clínica típica</strong> de evolução da doença.</li>
  </ul>
</div>

---

## 🎨 Otimização de UX: O Conceito de Performance Percebida

Mapas geográficos interativos baseados em malhas GeoJSON contêm milhares de coordenadas poligonais, exigindo alto processamento gráfico do navegador.

<div class="perspective-card card-rh view-rh">
  <div class="card-title">💼 Experiência do Usuário sem Espera</div>
  <p>Se o usuário entra no site e fica olhando uma tela branca carregando o mapa por vários segundos, ele desiste. Para evitar isso, organizei o painel de forma estratégica: os números principais (KPIs) carregam instantaneamente no topo da tela. Enquanto o usuário bate o olho nos totais e começa a ler, o mapa detalhado termina de renderizar suavemente logo abaixo, sem sensação de lentidão.</p>
</div>

<div class="perspective-card card-tech view-tech">
  <div class="card-title">⚙️ Engenharia de Frontend: Carregamento Above-the-Fold</div>
  <p>Aplicação do princípio de <strong>Performance Percebida (<em>Perceived Performance</em>)</strong>:</p>
  <ul>
    <li><strong>Priorização de Renderização:</strong> Métricas agregadas de alta velocidade (KPI cards em HTML/CSS leve) são renderizadas primeiro no topo (<em>Above-the-Fold</em>).</li>
    <li><strong>Assincronismo Visual:</strong> A renderização do mapa coroplético em Plotly com polígonos GeoJSON dos 78 municípios capixabas fica posicionada na rolagem intermediária, dando tempo ao motor WebGL do navegador sem bloquear a interatividade primária da aplicação.</li>
  </ul>
</div>

---

## 📊 Principais Descobertas Analíticas

O painel reúne diferentes dimensões epidemiológicas guiando o usuário por uma narrativa clara:

1. **Associação Crítica de Comorbidades:** Cruzamento que evidenciou o impacto desproporcional de cardiopatias graves e diabetes na elevação da taxa de letalidade em comparação com a população sem histórico clínico prévio.
2. **Disparidades Regionais:** Análise por município com filtro de corte estatístico mínimo de casos, prevenindo distorções em cidades com população muito pequena onde 1 ou 2 casos poderiam inflar artificialmente a taxa municipal.
3. **Padrão Etário:** Comportamento exponencial da letalidade nas faixas etárias acima de 60 anos, demonstrando visualmente o perfil de vulnerabilidade.
4. **Exportação Controlada (.csv.gz):** Ferramenta para que pesquisadores façam download seguro dos dados filtrados na tela, com limite de segurança de 300.000 registros para não travar a memória da máquina cliente.

---

## 🛠️ Arquitetura Modular do Código

O projeto foi refatorado e desacoplado em módulos independentes seguindo o princípio da responsabilidade única (Single Responsibility Principle):

```text
painel-covid-es/
├── app.py                      # Orquestração do app e layout global
├── prepare_data.py             # Script CLI de ETL: processamento e compressão Parquet
├── requirements.txt            # Dependências com controle de versões
├── covid_app/
│   ├── data.py                 # Leitura com cache (st.cache_data) e vetorização com np.select
│   ├── filters.py              # Barra lateral de filtros dinâmicos de período e município
│   ├── geo.py                  # Integração com malha GeoJSON e normalização fonética
│   ├── export.py               # Serialização e compressão para download (.csv.gz)
│   ├── features.py             # Engenharia de variáveis clínicas e temporais
│   ├── nav.py                  # Sumário e navegação interna por âncoras
│   └── sections/               # Módulos visuais isolados
│       ├── kpis.py             # Indicadores de topo (Above-the-Fold)
│       ├── tabela.py           # Tabela com gradiente condicional
│       ├── mapa_ranking.py     # Mapa coroplético e ranking de letalidade
│       ├── comorbidades.py     # Cruzamento de fatores de risco
│       ├── sobrevida.py        # Curvas de internação e limpeza IQR
│       └── temporal.py         # Séries temporais e evolução longitudinal
└── data/
    └── dados_es_filtrados.parquet  # Base otimizada de produção
```

---

## 🚀 Visão de Maturidade: "O Que Eu Faria Diferente Hoje?"

Olhando criticamente para o projeto com o conhecimento acumulado e a evolução contínua da engenharia de dados:

* **Desacoplamento do Frontend:** O Streamlit é excelente para prototipagem rápida e dashboards internos, mas possui restrições de customização e re-executa o script conforme o usuário interage. Para um produto em escala comercial de milhões de usuários, eu desacoplaria a arquitetura: uma **API FastAPI** servindo os dados para um frontend moderno em **React / Next.js**, oferecendo total controle de renderização e cache granular no navegador.
* **Adoção de DuckDB ou Polars no ETL:** Embora o Pandas em chunks tenha cumprido perfeitamente o objetivo, ferramentas analíticas modernas baseadas em Rust como **Polars** ou motores SQL in-memory como **DuckDB** poderiam processar os 10GB de CSV de forma multi-threaded ainda mais rápida, consumindo menos memória durante a fase de pré-processamento.
* **Cobertura de Testes Automatizados:** Implementaria uma suíte de testes com **Pytest** para garantir que regras críticas de negócio (como o cálculo de letalidade e o tratamento de comorbidades nulas) fossem validadas automaticamente a cada alteração de código.

---

## 🧰 Tecnologias & Ferramentas

* **Linguagem:** Python 3.10+
* **Processamento & Engenharia de Dados:** Pandas, NumPy, PyArrow (Apache Parquet com Snappy)
* **Visualização Analítica & Geospatial:** Plotly, GeoJSON
* **Framework Web:** Streamlit
* **Hospedagem & Deploy:** Streamlit Community Cloud

<script>
function setReadingMode(mode) {
  const btnAll = document.getElementById('btn-all');
  const btnRh = document.getElementById('btn-rh');
  const btnTech = document.getElementById('btn-tech');
  const rhCards = document.querySelectorAll('.view-rh');
  const techCards = document.querySelectorAll('.view-tech');

  // Atualiza classe dos botões
  btnAll.classList.remove('active');
  btnRh.classList.remove('active');
  btnTech.classList.remove('active');

  if (mode === 'rh') {
    btnRh.classList.add('active');
    rhCards.forEach(c => c.style.display = 'block');
    techCards.forEach(c => c.style.display = 'none');
  } else if (mode === 'tech') {
    btnTech.classList.add('active');
    rhCards.forEach(c => c.style.display = 'none');
    techCards.forEach(c => c.style.display = 'block');
  } else {
    btnAll.classList.add('active');
    rhCards.forEach(c => c.style.display = 'block');
    techCards.forEach(c => c.style.display = 'block');
  }
}
</script>
