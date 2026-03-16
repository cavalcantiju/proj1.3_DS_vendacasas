Projeto de Previsão de Preços de Imóveis (House Pricing)
Este repositório documenta a evolução de um modelo de regressão linear para estimar preços de venda de casas, focando na análise de variáveis e na saúde estatística do modelo.

1. Análise Exploratória e Visualização
Para entender quais variáveis realmente impactam o preço, utilizamos o pairplot.

Global: sns.pairplot(dados) foi usado para ver a correlação entre todas as colunas: área, banheiros, garagem e qualidade da cozinha.

Focado: sns.pairplot(dados, y_vars = 'preco_de_venda', x_vars = [...]) serviu para identificar visualmente quais características ajudam a explicar o preço final.

2. O Uso do sm.add_constant
No statsmodels, a regressão não inclui o intercepto por padrão. Adicionamos uma coluna de 1.0 para permitir que o modelo calcule o valor base do imóvel.

Sem a constante: O preço seria 0 se as características fossem 0 (irrealista).

Com a constante: O modelo ganha um "ponto de partida" (valor do terreno), tornando as previsões muito mais precisas.

🛠️ 3. Evolução dos Modelos e Multicolinearidade
Durante o projeto, ajustamos 4 versões do modelo para resolver o aviso de "Condition number is large".

O que é o Condition Number e Multicolinearidade?
Um número de condição alto (como o inicial de 3.240) indica que variáveis estão "contando a mesma história".

Exemplo: existe_segundo_andar e area_segundo_andar. Se uma existe, a outra tem valor. Isso confunde o modelo matemático.

Histórico de Ajustes:
Modelo 1 (R²: 0.741): Incluía todas as variáveis. Tinha o maior poder explicativo, mas apresentava erro de multicolinearidade.

Modelo 2 (R²: 0.708): Removemos a area_segundo_andar. O modelo ficou menos confuso (coeficientes positivos), mas o número de condição ainda era alto devido à garagem.

Modelo 3 (R²: 0.651): Removemos a garagem. O Condition Number caiu para 547, indicando um modelo muito mais saudável e estável, apesar do R² menor.

Comparativo de Performance (R²):
Modelo 0: 0.377 (Apenas área do 1º andar)

Modelo 1: 0.741 (Completo, mas instável)

Modelo 3: 0.651 (Equilibrado e estatisticamente confiável)

4. Métodos de Seleção de Variáveis
Além da nossa seleção manual, aprendemos que existem métodos automáticos:

Forward Selection: Começa vazio e adiciona variáveis uma a uma.

Backward Selection: Começa com tudo e remove o que menos ajuda.

Stepwise: Combina os dois, adicionando ou removendo conforme a necessidade.

Conclusão dos Parâmetros (Modelo 3)
O modelo final nos deu os seguintes "pesos" para o preço:

Base (Constante): -129.979 (ajuste do modelo)

Área 1º Andar: +6.119 por m²

Existe 2º Andar: +221.306 se houver

Banheiros: +149.036 por unidade

Cozinha Excelente: +444.391 de valorização
