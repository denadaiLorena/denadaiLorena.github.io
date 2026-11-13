---
layout: about
title: Início
permalink: /
is_home: true
---

<!-- 1. HERO SECTION -->
<section class="hero-section">
  <div class="hero-grid">
    <div class="hero-content">
      <span class="hero-kicker">Engenharia de Software · Dados · Inteligência Artificial</span>
      <h1 class="hero-title">Lorena de Nadai</h1>
      <p class="hero-lead">Desenvolvo soluções tecnológicas integrando engenharia de software, dados e inteligência artificial para resolver problemas reais.</p>
      <p class="hero-subtext">
        Estudante de <strong>Ciência da Computação</strong> (6º período) e estagiária de desenvolvimento no setor público (<a href="https://iema.es.gov.br/" target="_blank" rel="noopener">IEMA</a>).
      </p>

      <div class="hero-actions">
        <a href="{{ '/projects/' | relative_url }}" class="hero-btn hero-btn-primary">
          <i class="fa-solid fa-arrow-right"></i> Ver Projetos
        </a>
        <a href="https://denadailorena.github.io/AutoCV/" target="_blank" rel="noopener" class="hero-btn hero-btn-secondary">
          <i class="fa-regular fa-file-lines"></i> Currículo
        </a>
      </div>

      <div class="hero-social">
        <a href="https://github.com/{{ site.github }}" target="_blank" rel="noopener" aria-label="GitHub">
          <i class="fa-brands fa-github"></i>
        </a>
        <a href="https://www.linkedin.com/in/{{ site.linkedin }}" target="_blank" rel="noopener" aria-label="LinkedIn">
          <i class="fa-brands fa-linkedin-in"></i>
        </a>
        <a href="mailto:{{ site.email }}" aria-label="E-mail">
          <i class="fa-regular fa-envelope"></i>
        </a>
      </div>
    </div>

    <div class="hero-media">
      <div class="hero-avatar-wrapper">
        <img src="{{ '/assets/images/profile.png' | relative_url }}" alt="Foto de Lorena de Nadai" class="hero-avatar">
      </div>
    </div>
  </div>
</section>

<!-- 2. ÁREAS DE ATUAÇÃO -->
<section class="home-section">
  <div class="section-header">
    <span class="section-kicker">Competências</span>
    <h2 class="section-title">Áreas de Atuação Integrada</h2>
    <p class="section-subtitle">Pilares complementares aplicados de ponta a ponta na construção de soluções robustas.</p>
  </div>

  <div class="pillars-grid">
    <div class="pillar-card">
      <div class="pillar-icon">
        <i class="fa-solid fa-code"></i>
      </div>
      <h3 class="pillar-title">Software</h3>
      <p class="pillar-desc">Construção de aplicações completas, estruturação de serviços e interfaces eficientes para o usuário final.</p>
      <ul class="pillar-list">
        <li>Desenvolvimento de sistemas full-stack</li>
        <li>Construção e consumo de APIs REST com Python</li>
        <li>Arquitetura modular e integração de aplicações</li>
        <li>Interfaces dinâmicas com Vue.js, TypeScript e Quasar</li>
      </ul>
    </div>

    <div class="pillar-card">
      <div class="pillar-icon">
        <i class="fa-solid fa-database"></i>
      </div>
      <h3 class="pillar-title">Dados</h3>
      <p class="pillar-desc">Engenharia analítica e saneamento para transformar bases volumosas e brutas em decisões confiáveis.</p>
      <ul class="pillar-list">
        <li>Pipelines ETL automatizados</li>
        <li>Modelagem relacional e consultas estruturadas em PostgreSQL</li>
        <li>Otimização de I/O colunar com Apache Parquet</li>
        <li>Análise estatística e visualizações em Streamlit e Plotly</li>
      </ul>
    </div>

    <div class="pillar-card">
      <div class="pillar-icon">
        <i class="fa-solid fa-brain"></i>
      </div>
      <h3 class="pillar-title">Inteligência Artificial</h3>
      <p class="pillar-desc">Aplicação de modelos preditivos e generativos integrados aos processos de negócio e tomada de decisão.</p>
      <ul class="pillar-list">
        <li>Aplicações e copilotos analíticos com LLMs</li>
        <li>Sistemas RAG para recuperação e consulta contextual de informações</li>
        <li>Integração de modelos de linguagem com dados estruturados</li>
        <li>Modelagem de séries temporais e detecção estatística de anomalias</li>
        <li>Automação de workflows e orquestração de IA via n8n</li>
      </ul>
    </div>
  </div>
</section>

<!-- 3. PROJETOS EM DESTAQUE -->
<section class="home-section">
  <div class="section-header">
    <span class="section-kicker">Portfólio</span>
    <h2 class="section-title">Projetos em Destaque</h2>
    <p class="section-subtitle">Sistemas e análises aplicadas utilizando dados reais e arquitetura moderna.</p>
  </div>

  <div class="home-featured-grid">
    {% assign sorted_projects = site.projects | sort: "order" %}
    {% for project in sorted_projects %}
      <article class="featured-card">
        <div class="featured-card-media">
          {% if project.image %}
            <img src="{{ project.image | relative_url }}" alt="Demonstração do projeto {{ project.title }}" loading="lazy">
          {% else %}
            <div class="featured-placeholder" aria-hidden="true">
              <i class="fa-solid fa-chart-line"></i>
            </div>
          {% endif %}
        </div>

        <div class="featured-card-body">
          <h3 class="featured-card-title">
            <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
          </h3>
          <p class="featured-card-desc">{{ project.description }}</p>
          <div class="featured-card-footer">
            <a href="{{ project.url | relative_url }}" class="featured-link">
              Ver case study <i class="fa-solid fa-arrow-right"></i>
            </a>
          </div>
        </div>
      </article>
    {% endfor %}
  </div>

  <div class="home-section-action">
    <a href="{{ '/projects/' | relative_url }}" class="hero-btn hero-btn-secondary">
      Explorar todos os projetos <i class="fa-solid fa-arrow-right"></i>
    </a>
  </div>
</section>

<!-- 4. EXPERIÊNCIA PROFISSIONAL -->
<section class="home-section">
  <div class="section-header">
    <span class="section-kicker">Carreira</span>
    <h2 class="section-title">Experiência Profissional</h2>
  </div>

  <div class="experience-list">
    <div class="experience-item">
      <div class="experience-header">
        <div class="experience-title-block">
          <span class="experience-role">Estagiária de Desenvolvimento</span>
          <span class="experience-company">
            <a href="https://iema.es.gov.br/" target="_blank" rel="noopener">IEMA</a>
            <span class="experience-company-label">Instituto Estadual de Meio Ambiente e Recursos Hídricos</span>
          </span>
        </div>
        <div class="experience-meta">
          <span class="experience-period">2024 – Presente</span>
          <span class="experience-location">Vitória, ES</span>
        </div>
      </div>

      <p class="experience-intro">
        Atuação no desenvolvimento e evolução de soluções institucionais, abrangendo sistemas de gestão, engenharia de dados, indicadores e integração de aplicações.
      </p>

      <ul class="experience-bullets">
        <li><strong>Sistemas institucionais:</strong> Desenvolvimento de funcionalidades para gestão de solicitações, contemplando formulários, análise documental, cálculos, controle de estoque, gerenciamento de custos e histórico de alterações.</li>
        <li><strong>Engenharia de Dados e Indicadores:</strong> Desenvolvimento de dashboards gráficos para diferentes projetos, construção de Data Warehouse a partir de dados da API do GitLab, implementação de rotinas batch de atualização e otimização da aplicação.</li>
        <li><strong>APIs e Integração de Sistemas:</strong> Desenvolvimento de APIs para consulta de multas e notificações de fiscalização, integrando bases de dados SQL Server e PostgreSQL.</li>
      </ul>

      <p class="experience-tech">
        <strong>Tecnologias principais:</strong> PHP · Python · FastAPI · Flask · PostgreSQL · SQL Server · Vue.js · TypeScript · Quasar
      </p>
    </div>
  </div>
</section>

<!-- 5. TRAJETÓRIA E FORMAÇÃO -->
<section class="home-section">
  <div class="section-header">
    <span class="section-kicker">Background</span>
    <h2 class="section-title">Trajetória e Formação</h2>
  </div>

  <div class="trajectory-content">
    <p>
      O interesse pela área tecnológica começou cedo, impulsionado pela curiosidade em entender como sistemas funcionavam por baixo do capô e como a automação podia simplificar processos repetitivos. O primeiro passo prático se deu no curso técnico de <strong>Montagem e Manutenção de Computadores pelo SENAI</strong>, onde adquiri disciplina de diagnóstico, análise de hardware e infraestrutura física.
    </p>

    <p>
      Essa base prática consolidou a escolha pelo bacharelado em <strong>Ciência da Computação</strong>. Hoje, alinho a teoria de estruturas de dados e algoritmos à vivência diária no <strong>IEMA (Instituto Estadual de Meio Ambiente e Recursos Hídricos)</strong>, onde atuo no desenvolvimento de soluções voltadas à gestão e serviços públicos.
    </p>

    <p>
      Meu foco está na intersecção entre a <strong>estabilidade da engenharia de software</strong> e o <strong>poder analítico da ciência de dados</strong>, aplicando inteligência preditiva para transformar bases volumosas em ferramentas de decisão estratégica.
    </p>

    <blockquote class="trajectory-quote">
      <strong>Interesses multidisciplinares:</strong> Fora das telas, dedico-me ao estudo de <strong>teclado e violino</strong>. Acredito que a sensibilidade, criatividade e disciplina da música complementam de forma harmônica o pensamento lógico da matemática e da computação.
    </blockquote>
  </div>
</section>
