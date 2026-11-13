---
layout: post
title: "Previsão Preditiva de Demanda (Holt-Winters 7D)"
description: "Modelo de séries temporais com suavização exponencial tripla para prever vendas e faturamento, com intervalo de confiança de 95%."
tags: [python, séries temporais, previsão]
image: /assets/images/blog/holt-winters.png
---

Modelo de séries temporais baseado em **suavização exponencial tripla (Holt-Winters)** projetado para antecipar o volume de vendas e faturamento:
- Modelação de tendência linear e **sazonalidade semanal aditiva/multiplicativa**;
- Cálculo dinâmico de **intervalo de confiança a 95%**;
- Validação através de backtesting histórico de acurácia contra a série real de pedidos.