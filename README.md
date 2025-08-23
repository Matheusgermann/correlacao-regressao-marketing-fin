# 📊 Correlação e Regressão Linear Aplicada ao Marketing digital e ao Mercado Financeiro

## 🔎 Introdução
Este projeto apresenta a aplicação prática de ferramentas estatísticas — **correlação e regressão linear** — em dois contextos distintos:

1. **Marketing Digital** → análise de campanhas no Facebook Ads e Google Ads.  
2. **Mercado Financeiro** → comparação entre ativos da Apple (AAPL) e Microsoft (MSFT).

O estudo combina **estatística aplicada + Python + visualização de dados** para demonstrar como a análise quantitativa pode apoiar **decisões estratégicas em negócios e investimentos**.

---

## 🧾 Fundamentação Teórica
- **Correlação** → mede a força e direção da relação linear entre variáveis.  
- **Regressão Linear** → técnica que modela a relação entre variável dependente e variáveis independentes.  
- **Mercado Financeiro** → importância da diversificação e da análise de risco-retorno na construção de carteiras.

---

## ⚙️ Metodologia
1. **Coleta de Dados**
   - *Marketing*: Dataset [Real e-commerce traffic & advertising](https://www.kaggle.com/datasets/rlukasiewicz/e-commerce-traffic-and-advertising).
   - *Mercado Financeiro*: Dataset [All Stocks 5 Years](https://www.kaggle.com/datasets/camnugent/sandp500).

2. **Tratamento e Análise**
   - Limpeza e preparação de dados em **Python (pandas, numpy)**.  
   - Cálculo de métricas: **ROI, CPA, Retorno Médio, Desvio Padrão, Coeficiente de Variação**.  
   - Construção de **matriz de correlação** e ajuste de **modelos de regressão linear**.

3. **Ferramentas Utilizadas**
   - Python, pandas, numpy, seaborn, matplotlib, Kaggle, Excel e Google Colab.

---

## 📈 Resultados

### 📊 Marketing Digital
- O **Facebook Ads** apresentou **maior ROI e menor CPA** comparado ao Google Ads.  
- A regressão mostrou relação positiva entre investimento e receita, embora com dispersão (R² moderado).  
- Estratégias de segmentação e otimização mostraram-se mais relevantes do que apenas aumentar o orçamento.

### 💹 Mercado Financeiro
- **Apple (AAPL):** Retorno médio = 0,0786% | CV = 18,56.  
- **Microsoft (MSFT):** Retorno médio = 0,1039% | CV = 13,68.  
- **Correlação AAPL x MSFT = 0,3666** (positiva, moderada).  
- Microsoft apresentou **melhor relação risco-retorno** no período analisado.

---

## 💻 Exemplos de Código (Python)

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Exemplo: matriz de correlação
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Dados de anúncios do Facebook
tabela_facebook_anuncio = pd.read_csv("/content/drive/MyDrive/Cursos/ADS - Cesuca/Métodos Quantitativos para Tomada de Decisões /2º Atividade Avaliativa A2 (Mercado Financeiro)/fbads_daily.csv")
tabela_facebook_anuncio['data'] = pd.to_datetime(tabela_facebook_anuncio['data'])

# Dados do Google Analytics
tabela_google_analytics = pd.read_csv("/content/drive/MyDrive/Cursos/ADS - Cesuca/Métodos Quantitativos para Tomada de Decisões /2º Atividade Avaliativa A2 (Mercado Financeiro)/ga_daily.csv")
tabela_google_analytics['date'] = pd.to_datetime(tabela_google_analytics['date'])

# Dados Não Pago
tabela_nao_pago = pd.read_csv("/content/drive/MyDrive/Cursos/ADS - Cesuca/Métodos Quantitativos para Tomada de Decisões /2º Atividade Avaliativa A2 (Mercado Financeiro)/ga_non-paid.csv")
tabela_nao_pago['sessions'] = tabela_nao_pago['sessions'].str.replace(',', '').astype(float)
tabela_nao_pago['bounces'] = tabela_nao_pago['bounces'].str.replace(',', '').astype(float)
tabela_nao_pago['date'] = pd.to_datetime(tabela_nao_pago['date'])

# Dados Pago
tabela_pago = pd.read_csv("/content/drive/MyDrive/Cursos/ADS - Cesuca/Métodos Quantitativos para Tomada de Decisões /2º Atividade Avaliativa A2 (Mercado Financeiro)/ga_paid.csv")
tabela_pago['sessions'] = tabela_pago['sessions'].str.replace(',', '').astype(float)
tabela_pago['bounces'] = tabela_pago['bounces'].str.replace(',', '').astype(float)
tabela_pago['date'] = pd.to_datetime(tabela_pago['date'])

# Dados do Google Ads
tabela_google_ads = pd.read_csv("/content/drive/MyDrive/Cursos/ADS - Cesuca/Métodos Quantitativos para Tomada de Decisões /2º Atividade Avaliativa A2 (Mercado Financeiro)/googleads_daily.csv")
tabela_google_ads['data'] = pd.to_datetime(tabela_google_ads['data'])

# Juntando as tabelas
df_merged = pd.merge(tabela_facebook_anuncio, tabela_google_analytics, left_on='data', right_on='date', how='inner')
df_merged = pd.merge(df_merged, tabela_nao_pago, left_on='data', right_on='date', how='inner')
df_merged = pd.merge(df_merged, tabela_pago, left_on='data', right_on='date', how='inner')
df_merged = pd.merge(df_merged, tabela_google_ads, left_on='data', right_on='data', how='inner')

display(df_merged)
```
<br>
<br>
<br>

```python
# Calculando correlações
correlation_matrix = df_merged[['Custo_Facebook', 'Pedidos_Facebook', 'Receita_Facebook',
                                'Custo_GoogleAds', 'Pedidos_GoogleAnalytics', 'Receita_GoogleAnalytics']].corr()

# Visualizando correlações
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', fmt=".2f")
plt.title("Correlação entre Métricas de Campanhas Publicitárias")
plt.show()

# Análise comparativa de ROI e CPA
roi_cpa_df = pd.DataFrame({
    'Plataforma': ['Facebook Ads', 'Google Ads'],
    'ROI Médio': [df_merged['ROI_Facebook'].mean(), df_merged['ROI_GoogleAds'].mean()],
    'CPA Médio': [df_merged['CPA_Facebook'].mean(), df_merged['CPA_GoogleAds'].mean()]
})

# Visualização de ROI e CPA por plataforma
fig, axes = plt.subplots(1, 2, figsize=(12, 6))

sns.barplot(data=roi_cpa_df, x='Plataforma', y='ROI Médio', ax=axes[0], palette='viridis')
axes[0].set_title('ROI Médio por Plataforma')

sns.barplot(data=roi_cpa_df, x='Plataforma', y='CPA Médio', ax=axes[1], palette='magma')
axes[1].set_title('CPA Médio por Plataforma')

plt.tight_layout()
plt.show()
```

---

## 📘 Artigo Acadêmico
O artigo completo com fundamentação teórica e análise detalhada está disponível em:  
➡️ [Artigo em PDF](https://drive.google.com/file/d/1kEfdCRZLGHLqkNwd5w2OOctiUmxm_q30/view?usp=drive_link)

---

## 📔 Notebooks no Google Colab
Para facilitar o acesso, os dois códigos desenvolvidos estão disponíveis diretamente no Google Colab. Basta clicar nos links abaixo para visualizar e executar as análises:

- 📊 [Notebook - Publicidade (Facebook Ads e Google Ads)](https://colab.research.google.com/drive/1SfTUzwmwN-B0KL_yYA-VocDZBURllzm0?usp=sharing)
- 💹 [Notebook - Mercado Financeiro (Apple e Microsoft)](https://colab.research.google.com/drive/12uNzIcoeDmANOBa19HvgRmtPMi8hBWGT?usp=sharing)

➡️ Caso prefira, você também pode **baixar os notebooks** e executá-los localmente.

---

## 🚀 Como Reproduzir
Você pode acessar os notebooks diretamente pelos links do Google Colab listados acima.  
Caso prefira, também é possível **baixar os arquivos** e rodar localmente no seu ambiente Python.

---

<br>

**Matheus Santos Germann**  
Estudante de Ciência de Dados | Automação Industrial | Python, SQL, Power BI, Machine Learning  
[LinkedIn](https://www.linkedin.com/in/matheus-germann) | [GitHub](https://github.com/Matheusgermann)
