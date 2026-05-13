# TCC

# Predição de Longevidade na Carreira de Jogadores de Futebol: Uma Abordagem com Mixture Cure Models

## Visão Geral do Projeto
Este projeto aplica técnicas avançadas de **Análise de Sobrevivência (Survival Analysis)** para determinar os fatores que influenciam o tempo de carreira de jogadores de futebol profissional. O objetivo principal é desenvolver um **Mixture Cure Model (MCM)** para estimar a probabilidade de um atleta atingir a longevidade esportiva (definida como a "cura" — jogar além dos 40 anos) e, para aqueles que não atingem, prever o risco e o tempo até a aposentadoria.

## Arquitetura de Dados e ETL
O pipeline de dados consolida informações relacionais complexas de múltiplas fontes (inspirado nas bases do Transfermarkt), transformando eventos temporais em variáveis de exposição e risco consolidadas por jogador.

* **Coleta de Dados (Scraping):** Uso do framework `Scrapy` para extração em cascata de dados históricos (ex: UEFA Champions League desde 1992/93), capturando clubes, perfis de jogadores e históricos médicos.
* **Engenharia de Features:** Agregação de tabelas relacionais (`PLAYER_PROFILES`, `PLAYER_PERFORMANCES`, `PLAYER_INJURIES`, `GAME_EVENTS`, etc.) em uma base analítica única.
* **Tratamento de Viés:** Implementação de lógica para lidar com **Censura à Direita (Right-Censoring)**, diferenciando atletas efetivamente aposentados daqueles que ainda estão em atividade na última temporada registrada.

## Variáveis Analisadas
O modelo cruza dados biológicos, táticos e de carga de trabalho para determinar os *Hazard Ratios*, incluindo:
* **Carga de Trabalho:** Minutos totais jogados por clubes, convocações para seleção nacional e histórico de titularidade.
* **Histórico Médico:** Frequência de lesões, total de dias afastados, gravidade da pior lesão e lesões ocorridas durante partidas.
* **Fatores Táticos:** Posição em campo (para capturar o desgaste assimétrico entre zagueiros, meias, atacantes e goleiros).

## Modelagem Estatística
A modelagem preditiva é dividida em uma abordagem de duas etapas (Two-Part Model) para lidar com a premissa de "cura":
1. **Modelo de Incidência (Regressão Logística):** Estima a probabilidade basal de um jogador ultrapassar a barreira dos 40 anos em atividade.
2. **Modelo de Latência (Cox Proportional Hazards):** Para a subpopulação suscetível à aposentadoria precoce ou comum ($\le$ 40 anos), avalia como as covariáveis aceleram ou retardam o fim da carreira.

## Stack Tecnológico
* **Linguagem:** Python
* **Engenharia de Dados:** Pandas, NumPy
* **Modelagem e Estatística:** Scikit-Learn, Lifelines (CoxPH)
* **Coleta de Dados:** Scrapy
