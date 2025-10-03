

# Previsão de Séries Temporais: Temperaturas Mínimas Diárias em Melbourne (1981–1990)

Este projeto apresenta um estudo comparativo de modelos de previsão aplicados a uma série temporal real: as temperaturas mínimas diárias registradas em Melbourne, Austrália, entre os anos de 1981 e 1990. O objetivo foi analisar o desempenho de modelos estatísticos e de redes neurais na tarefa de previsão, utilizando métricas consolidadas na literatura.

## Base de Dados

- Fonte: Kaggle  
- Link: https://www.kaggle.com/datasets/samfaraday/daily-minimum-temperatures-in-me  
- Estrutura: 3.650 observações com colunas `Date` (data) e `Temp` (temperatura mínima diária em °C)

## Objetivo

Avaliar, comparar e justificar o desempenho de diferentes abordagens de previsão para séries temporais, com foco em:

- Modelos estatísticos clássicos (`Prophet`, `NaiveForecaster`)
- Redes neurais recorrentes (`LSTM`)
- Validação cruzada do tipo walk-forward
- Ajuste leve de hiperparâmetros (tuning do LSTM)

## Modelos Aplicados

1. **NaiveForecaster (Sktime)**: modelo base que utiliza o último valor como previsão futura.
2. **Prophet (Facebook)**: modelo aditivo com componentes automáticos de tendência e sazonalidade.
3. **LSTM padrão**: rede neural recorrente treinada com janelas de 30 dias.
4. **LSTM com validação walk-forward**: simulação de previsões progressivas em tempo real com reavaliação periódica.
5. **LSTM ajustado**: versão com regularização (Dropout) e mais épocas de treinamento para melhorar a generalização.

## Avaliação e Métricas

Foram utilizadas duas métricas amplamente adotadas em tarefas de previsão:

### MAE — Erro Absoluto Médio (Mean Absolute Error)

Indica o erro absoluto médio entre os valores reais e os previstos. É uma métrica robusta a outliers e de fácil interpretação.

$$
MAE = \frac{1}{n} \sum_{i=1}^n |y_i - \hat{y}_i|
$$

### RMSE — Raiz do Erro Quadrático Médio (Root Mean Squared Error)

Penaliza erros grandes com mais intensidade, sendo útil em aplicações sensíveis a desvios extremos.

$$
RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^n (y_i - \hat{y}_i)^2}
$$

Segundo Hyndman e Athanasopoulos (2018), ambas as métricas são dependentes da escala dos dados ("scale-dependent") e devem ser usadas quando os modelos estão sendo comparados **sobre o mesmo conjunto de dados**. O trecho citado no original diz:

> “Both MAE and RMSE are scale-dependent measures of forecast accuracy and should be used when comparing models on the same dataset.”

Ou seja, essas métricas não permitem comparações diretas entre diferentes séries temporais (com escalas distintas), mas são apropriadas para comparar diferentes modelos aplicados à **mesma série** — como é o caso deste projeto.

## Resultados Obtidos

| Modelo                   | MAE   | RMSE  |
|--------------------------|-------|--------|
| Sktime (Naive)           | 2.55  | 3.40   |
| Prophet                  | 1.88  | 2.55   |
| LSTM (padrão)            | 1.82  | 2.32   |
| LSTM (walk-forward)      | 2.06  | 2.63   |
| LSTM (ajustado)          | 1.74  | 2.21   |

O modelo LSTM ajustado apresentou o melhor desempenho em ambas as métricas, seguido de perto pelo Prophet. O modelo estatístico NaiveForecaster, por sua simplicidade, obteve os piores resultados, servindo como baseline.

## Conclusão

O experimento confirmou que modelos baseados em redes neurais, mesmo com configurações simples, podem superar abordagens estatísticas tradicionais. No entanto, modelos como o Prophet ainda oferecem vantagens importantes, como menor complexidade de implementação e interpretabilidade dos componentes sazonais.

A escolha do modelo ideal depende dos objetivos do projeto, tempo disponível para ajuste, e sensibilidade à interpretabilidade versus desempenho.

## Referências (ABNT)

HYNDMAN, Rob J.; ATHANASOPOULOS, George. Forecasting: principles and practice. 2. ed. Melbourne: OTexts, 2018. Disponível em: https://otexts.com/fpp2/. Acesso em: 25 mar. 2025.

TAYLOR, Sean J.; LETHAM, Benjamin. Forecasting at scale. *The American Statistician*, v. 72, n. 1, p. 37–45, 2018.
