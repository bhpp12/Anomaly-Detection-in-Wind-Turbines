# Detecção de Anomalias em Turbinas Eólicas

## Sobre o projeto

Projeto desenvolvido para a disciplina de **Mineração de Dados da Universidade Federal do Ceará (UFC)**, com o objetivo de identificar anomalias em dados de telemetria de turbinas eólicas.

A proposta consiste em analisar informações operacionais das turbinas, identificar comportamentos atípicos e selecionar um método de Machine Learning para gerar, no máximo, 300 alertas de possíveis anomalias.

O desenvolvimento foi realizado no Google Colab, utilizando técnicas de análise exploratória, métodos estatísticos e algoritmos não supervisionados de detecção de anomalias.

## Tecnologias e bibliotecas

- Python
- Google Colab
- Pandas e NumPy
- Matplotlib
- Scikit-learn
- SciPy

## Dados utilizados

O dataset contém informações de telemetria das turbinas eólicas, incluindo:

| Variável | Descrição |
|---|---|
| `id` | Identificador do registro. |
| `timestamp` | Data e horário da leitura. |
| `turbina` | Identificação da turbina. |
| `vento_ms` | Velocidade do vento (m/s). |
| `potencia_kw` | Potência gerada (kW). |
| `rotor_rpm` | Rotação do rotor. |
| `temp_caixa_engrenagem_c` | Temperatura da caixa de engrenagem (°C). |
| `vibracao_mm_s` | Vibração (mm/s). |
| `temp_ambiente_c` | Temperatura ambiente (°C). |

## Desenvolvimento

### 1. Análise exploratória dos dados

Inicialmente, foi realizada uma análise das variáveis de telemetria, utilizando gráficos de dispersão e séries temporais para investigar relações entre velocidade do vento, potência gerada e temperatura dos componentes.

### 2. Detecção estatística de anomalias

Foram utilizados dois métodos estatísticos como baseline:

- **Z-Score:** identifica observações que apresentam valores distantes da média, considerando o desvio padrão.
- **IQR (Intervalo Interquartil):** identifica valores fora dos limites definidos pelos quartis da distribuição.

Resultados:
- Z-Score: 87 alertas.
- IQR: 100 alertas.

Esses métodos apresentam limitações na identificação de anomalias contextuais e multivariadas, pois analisam os valores de maneira independente.

### 3. Engenharia de atributos (Feature Engineering)

Foram criadas duas novas variáveis para melhorar a identificação de comportamentos anormais:

- `vento_std_1h`: desvio padrão da velocidade do vento em uma janela móvel de uma hora, considerando seis leituras.
- `vib_congelada`: identifica possíveis sensores de vibração congelados, com valores constantes durante uma hora.

Também foi aplicada a normalização dos dados utilizando `StandardScaler`, preparando as variáveis para os algoritmos de detecção de anomalias.

### 4. Modelos de Machine Learning

Foram implementados quatro algoritmos não supervisionados:

**Isolation Forest**

Identifica anomalias por meio de processos de isolamento, atribuindo pontuações maiores às observações consideradas mais anormais.

**k-Nearest Neighbors (k-NN)**

Utiliza a distância média entre cada observação e seus 10 vizinhos mais próximos para identificar registros isolados.

**Local Outlier Factor (LOF)**

Analisa a densidade local das observações em comparação com seus vizinhos para identificar possíveis anomalias.

**One-Class SVM**

Utiliza uma função de decisão para identificar observações que se afastam do comportamento considerado normal.

Cada algoritmo foi utilizado para selecionar as 300 observações com maiores pontuações de anomalia.

### 5. Comparação dos resultados

Os quatro modelos produziram 300 alertas cada.

Também foram analisadas as intersecções entre os conjuntos de alertas para identificar registros apontados simultaneamente por diferentes algoritmos.

| Comparação | Alertas em comum |
|---|---:|
| Isolation Forest e LOF | 99 |
| Isolation Forest e k-NN | 132 |
| Isolation Forest e One-Class SVM | 160 |
| LOF e k-NN | 152 |

### 6. Diagnóstico das anomalias

Foi realizada uma análise dos três alertas mais críticos identificados pelo Isolation Forest.

Os registros apresentaram características como:

- Velocidade do vento entre 2,5 e 2,8 m/s.
- Potência variando entre 0 e 53 kW.
- Rotor em movimento, mesmo com baixa velocidade do vento.
- Temperatura da caixa de engrenagem acima de 40 °C.

Esse comportamento pode indicar uma anomalia contextual relacionada ao aquecimento da caixa de engrenagem em condições de baixa geração, levantando hipóteses como falhas na refrigeração ou atrito mecânico.

## Decisão para produção

**Método selecionado: Isolation Forest.**

A escolha foi baseada em três fatores principais:

1. **Limite de alertas:** permite priorizar as 300 observações mais anômalas, respeitando a capacidade de investigação disponível.
2. **Desempenho computacional:** apresenta boa escalabilidade em comparação com métodos baseados em cálculos de distância entre observações.
3. **Detecção multivariada:** permite identificar combinações atípicas entre variáveis, incluindo anomalias contextuais que métodos estatísticos univariados podem não detectar.

## Exportação dos resultados

Ao final, os 300 registros selecionados pelo Isolation Forest são exportados para um arquivo CSV contendo os identificadores das observações:

`alertas_equipe_bhpp.csv`

Também são exibidos os três alertas considerados mais críticos para auxiliar no diagnóstico.

## Autor

**Bruno Henrique Pacheco Prudêncio**

Estudante de Ciência de Dados — Universidade Federal do Ceará (UFC).

[GitHub](https://github.com/bhpp12)
