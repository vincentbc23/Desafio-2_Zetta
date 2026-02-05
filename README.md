# DESAFIO 2 - Ciência e Governança de Dados
## Modelagem Preditiva e Recomendações Estratégicas: Segurança e Educação em Minas Gerais

## Descrição do Projeto

Dando continuidade à análise exploratória iniciada no Desafio 1, este projeto avança para o desenvolvimento de modelos preditivos de **Machine Learning**. O objetivo central é responder à pergunta: *"Como poderíamos avaliar e prever os agentes/fenômenos que mais causam impactos socioeconômicos?"*.

Utilizando algoritmos de regressão, o projeto simula cenários futuros para a **Segurança Pública** (Criminalidade) e **Educação** em Minas Gerais, identificando quais variáveis (como inércia criminal, desigualdade social e desenvolvimento) exercem maior influência sobre o futuro do estado. O produto final consiste em um "Relatório de Inteligência" com recomendações de políticas públicas baseadas em dados.

## Fontes e Escolha dos Dados

Além do conjunto de dados consolidado no Desafio 1 (Criminalidade 2019 - 2024, Educação, População e IDH), foram adquiridos **novos dados** estratégicos para permitir a modelagem temporal e social.

1.  **Série Temporal de Criminalidade (2019-2024):**
    * **Fonte:** Secretaria de Estado de Justiça e Segurança Pública (SEJUSP-MG).
    * **Dados:** Bases completas de crimes violentos dos últimos 6 anos.
    * **Escolha (Justificativa):** A aquisição foi necessária para criar a variável de **"Inércia Criminal" (Lag)**. A criminalidade possui forte dependência temporal (o crime do ano anterior influencia o ano seguinte), algo que a base estática que tinha no Desafio 1 não permitia modelar.

2.  **Dados Sociais Complementares (Gini e Desemprego):**
    * **Fonte:** DATASUS e IBGE (Censo/PNAD).
    * **Dados:** Índice de Gini (desigualdade de renda) e Taxa de Desemprego municipal.
    * **Escolha (Justificativa):** O PIB per capita sozinho não explica a complexidade da violência. A literatura indica que a desigualdade e a falta de oportunidades são vetores cruciais, sendo necessário incluí-los para aumentar a precisão dos modelos.

3.  **Dados do Desafio 1 (Mantidos):**
    * **Educação Básica e Superior (INEP):** Matrículas, ingressantes e concluintes.
    * **População, IDH e PIB (IBGE/PNUD):** Para normalização de taxas e indicador de desenvolvimento.
    * 

## Metodologia e Principais Passos do Notebook

O notebook `Desafio_2_Final.ipynb` estrutura o pipeline de Ciência de Dados da seguinte forma:

1.  **Configuração e Bibliotecas:** Importação de ferramentas de manipulação (Pandas), visualização (Seaborn/Folium) e Machine Learning (`scikit-learn`).
2.  **Engenharia de Atributos (Feature Engineering):**
    * Limpeza e padronização dos novos dados adquiridos (tratamento de encoding e nomes de colunas).
    * Criação da Série Temporal (`df_painel`) e cálculo do **Lag** (variável `crime_ano_anterior`).
    * Fusão (`merge`) dos dados temporais com os dados estruturais (IDH, Gini, Educação).
    * Cálculo de **Taxas por 100 mil habitantes** para eliminar o viés populacional.
3.  **Preparação para Modelagem:**
    * Divisão dos dados em Treino e Teste (`train_test_split`).
    * Definição das variáveis preditoras ($X$: Histórico, Social, Econômico) e alvo ($y$: Taxa de Crime, Taxas de Educação).
4.  **Desenvolvimento e Comparação de Modelos:**
    * **Regressão Linear:** Utilizada como *baseline* (linha de base) para testar a linearidade dos fenômenos.
    * **Random Forest Regressor:** Utilizado para capturar a complexidade e não-linearidade dos dados sociais.
    * **Otimização:** Aplicação de `GridSearchCV` para encontrar os melhores hiperparâmetros (número de árvores, profundidade) e maximizar o $R^2$.
5.  **Análise de Importância de Variáveis:**
    * Extração e visualização do `feature_importances_` para identificar quais fatores mais pesam na decisão do modelo.
6.  **Visualização Geoespacial:**
    * Criação de mapa interativo (`Folium`) para visualizar a distribuição da taxa de criminalidade e de educação. **Deve ser aberto no navegador pelo aquivo HTML**.
7.  **Simulações Estratégicas ("What-If Analysis"):**
    * Criação de cenários hipotéticos para 2025:
        * *Cenário A:* Aumento do IDH (Investimento Social).
        * *Cenário B:* Redução da criminalidade base (Choque de Segurança).
8.  **Conclusão e Recomendações:** Tradução dos números em diretrizes para o Governo.

## Principais Insights e Recomendações

* **Superioridade do Random Forest:** O modelo de Random Forest superou drasticamente a Regressão Linear (R² > 0.85 vs R² < 0.30 em alguns casos), provando que fenômenos sociais são complexos e não-lineares. Modelos simples não conseguem capturar a realidade socioeconômica de MG.
* **A Força da Inércia:** A análise de importância revelou que a variável mais determinante para o crime de 2025 é a **Taxa de Crime de 2024**. Isso indica um ciclo vicioso: a violência passada gera a violência futura.
* **O Paradoxo da Educação (Transição Demográfica):** Identificou-se uma correlação negativa entre aumento do IDH e taxa de matrículas na educação básica.
    * *Interpretação:* Não é uma falha educacional, mas demográfica. Cidades mais desenvolvidas têm famílias menores e menos crianças, reduzindo a demanda quantitativa por escolas.
* **Recomendações Estratégicas para o Governo:**
    1.  **Curto Prazo (Segurança):** As simulações mostram que um "Choque de Segurança" (reduzir o crime atual em 10%) tem o maior impacto imediato para o ano seguinte, pois quebra a inércia criminal.
    2.  **Longo Prazo (Estrutural):** O investimento em IDH (Renda/Saúde) é a "vacina" estrutural. Embora não reduza o crime da noite para o dia, cria a base para uma sociedade menos violenta.
    3.  **Oportunidade na Educação:** Com a redução da demanda por *quantidade* de vagas (devido ao IDH/Demografia), recomenda-se focar o orçamento na *qualidade* e infraestrutura escolar.

## Como Executar

1.  Certifique-se de ter Python e Jupyter Notebook instalados.
2.  Instale as bibliotecas adicionais de Machine Learning e Mapas:
    `pip install scikit-learn folium`
3.  Mantenha a estrutura de pastas: o notebook na raiz e os arquivos `.csv` (tanto os antigos quanto os novos da série temporal) dentro da pasta `dados/` (ou ajuste o caminho no notebook conforme necessário).
4.  Execute o notebook `Desafio_2_Final.ipynb`. Ao final, um arquivo `mapa_mg.html` será gerado com a visualização geoespacial.
5.  Base de Dados
Devido ao tamanho dos arquivos (excedendo o limite do GitHub), a pasta `dados/` completa está hospedada externamente.
**[Clique aqui para baixar os dados (Google Drive)](https://drive.google.com/file/d/1gUcgHZjPmnpjmyzv0D2yhL7nziZ_xulE/view?usp=sharing)**.
**Instrução:** Baixe o arquivo, descompacte e coloque a pasta `dados` na raiz deste projeto para que os notebooks funcionem corretamente.

---

Projeto desenvolvido por Vincent Biazotti Collares, para o Desafio 2 - Zetta lab - Segunda edição.
