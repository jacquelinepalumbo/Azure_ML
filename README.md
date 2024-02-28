# Azure_ML
Train using Azure Machine Learning Compute

# Na prática usando Azure Machine Learning

[![LinkedIn](https://img.shields.io/badge/LinkedIn-000?style=for-the-badge&logo=LinkedIn&logoColor=30A3DC)](https://www.linkedin.com/in/jacqueline-ribeiro-743876247/)


### Criando um modelo de previsão com os devidos pontos de extremidade configurados.

**Descrição**: Esta documentação tem como objetivo demonstrar um passo a passo de como criar um modelo de Machine Learning automatizada na plataforma Microsoft da Azure. 

O modelo é uma simulação de um serviço de aluguel de bicicletas, baseando-se em um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo, que prevê o número de aluguéis de bicicletas esperados em um determinado dia, com base em características sazonais e meteorológicas.

⚠️⚠ *É necessário logar-se em uma conta da microsoft e criar uma workspace (espaço de trabalho) na aba "estúdio de aprendizado de máquina"*.

### Criando ML automatizada 

1. No estúdio de Aprendizado de Máquina do Azure, exiba a página **ML automatizada** (em **Criação**).

2. Crie um novo trabalho de ML automatizado com as seguintes configurações, usando Avançar conforme necessário para progredir pela interface do usuário:


### Configurações básicas:

* **Nome do trabalho**: *mslearn-bike-automl*
* **Novo nome do experimento**: *mslearn-bike-rental*
* **Descrição**: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
* **Tags**: Nenhuma


3. Na próxima etapa, preencha os campos com os seguintes dados:

#### Tipo de tarefa e dados:


* **Selecionar tipo de tarefa**: Regressão
* **Selecionar conjunto de dados**: Crie um novo conjunto de dados com as seguintes configurações:

#### Tipo de dados:

* **Nome**: *bike-rentals*
* **Descrição**: Dados históricos de aluguer de bicicletas
* **Tipo**: Tabular

#### Fonte de dados:
* Selecionar **de arquivos da Web**
* **URL da Web**: `https://aka.ms/bike-rentals`
* **gnorar validação de dados**: Não selecione

#### Configurações:

* **Formato de arquivo**: Delimitado
* **Delimitador**: Vírgula
* **Codificação**: UTF-8
* **Cabeçalhos de coluna**: Somente o primeiro arquivo tem cabeçalhos
* **Pular linhas**: Nenhum
* **O conjunto de dados contém dados de várias linhas**: Não selecione
* **Esquema**:
* Incluir todas as colunas diferentes de Caminho
* Revisar os tipos detectados automaticamente
* Selecione **Criar**. Depois que o conjunto de dados for criado, selecione o conjunto de dados de aluguel de bicicletas para continuar a enviar o trabalho de ML automatizado.

#### Configurações da tarefa:

* **Tipo de tarefa**: Regressão
* **Conjunto de dados**: Aluguel de bicicletas
* **Coluna de destino**: Aluguéis (inteiro)

#### Definições de configuração adicionais

* **Métrica primária**: Erro quadrático médio da raiz normalizada
* **Explicar melhor modelo**: Não selecionado
* **Use todos os modelos suportados**: Não selecionado.
* Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.

* **Modelos permitidos**: Selecione apenas *RandomForest* e *LightGBM* —
* normalmente você gostaria de tentar o maior número possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.

#### Limites:

* **Máximo de tentativas**: 3
* **Máximo de tentativas simultâneas**: 3
* **Nós máximos**: 3
* **Limiar de pontuação métrica**: 0,085 (de modo que, se um modelo atingir uma pontuação métrica quadrática média normalizada de 0,085 ou menos, o trabalho termina.)
* **Tempo limite**: 15
* **Tempo limite de iteração**: 15
* **Habilitar rescisão antecipada**: Selecionado

#### Validação e Teste:

* **Tipo de validação**: Divisão de validação de trem
* **Porcentagem de dados de validação**: 10
* **Conjunto de dados de teste**: Nenhum

#### Computação:

* **Selecione o tipo de computação**: Serverless
* **Tipo de máquina virtual**: CPU
* Camada de máquina virtual**: Dedicado
* **Tamanho da máquina virtual**: Standard_DS3_V2*
* **Número de instâncias**: 1

Se sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.

4. Envie o trabalho de treinamento. Ele começa automaticamente.


* *Aguarde a conclusão do trabalho*.  

# Reveja o melhor modelo

Quando o trabalho de aprendizado de máquina automatizado for concluído, você poderá revisar o melhor modelo treinado.

1. Na guia Visão geral do trabalho de aprendizado de máquina automatizado, observe o melhor resumo do modelo. 
<img width="889" alt="image" src="https://github.com/jacquelinepalumbo/Azure_ML/assets/119548193/8aec8c9d-933d-4a23-bb14-b47ab3465143">


2. Selecione o texto em **Nome do algoritmo** para o melhor modelo para exibir seus detalhes.

3. Selecione a guia **Métricas** e selecione os **gráficos de resíduos** e **predicted_true** se ainda não estiverem selecionados.

Analise os gráficos que mostram o desempenho do modelo. 

O gráfico **de resíduos** mostra os resíduos (as diferenças entre os valores previstos e reais) como um histograma. 

<img width="847" alt="image" src="https://github.com/jacquelinepalumbo/Azure_ML/assets/119548193/b7274521-7192-4f57-9023-c07d7024e71b">


O gráfico *predicted_true* compara os valores previstos com os valores verdadeiros.

<img width="839" alt="image" src="https://github.com/jacquelinepalumbo/Azure_ML/assets/119548193/a05999f7-5b8c-45e6-9c6d-1e41088367a1">


 
# Implantando e testando o modelo 


1. Na guia **Modelo** para obter o melhor modelo treinado pelo seu trabalho de aprendizado de máquina automatizado, selecione Implantar e usar a opção Serviço Web para implantar o modelo com as seguintes configurações:
- **Nome**: predict-rentals
- **Descrição**: Prever aluguéis de ciclos
- **Tipo de computação**: Instância de Contêiner do Azure
- **Habilitar autenticação**: Selecionado

2. Aguarde o início da implantação - isso pode levar alguns segundos. O status de implantação para o ponto de extremidade de aluguel de previsão será indicado na parte principal da página como Em execução.
3. Aguarde até que o status Implantar seja alterado para Bem-sucedido. 

# Testar o serviço implantado

1. No estúdio do Aprendizado de Máquina do Azure, no menu à esquerda, selecione **Pontos de extremidade** e abra o ponto de extremidade em tempo real **de aluguéis de previsão**.

2. Na página de ponto de extremidade em tempo real de **aluguéis de previsão**, exiba a guia **Teste**.

3. No painel **Dados de entrada para testar o ponto de extremidade**, substitua o modelo JSON pelos seguintes dados de entrada:

```
 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }

 ```
4. Clique no botão **Testar**.

5. Analise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada:


 ```
{
  "Results": [
    362.7247459914348
  ]
}

```

O painel de teste pegou os dados de entrada e usou o modelo treinado para retornar o número previsto de aluguéis.

Foi usado um conjunto de dados de dados históricos de aluguel de bicicletas para treinar um modelo. 
O modelo prevê o número de aluguéis de bicicletas esperados em um determinado dia, com base em características sazonais e meteorológicas, neste caso **362 bikes**.


# Limpeza

O serviço Web que você criou está hospedado em uma *Instância de Contêiner do Azure*. Se você não pretende experimentá-lo mais, exclua o ponto de extremidade para evitar o uso desnecessário do Azure.

1. No [estúdio do Aprendizado de Máquina do Azure](https://ml.azure.com/?azure-portal=true), na guia **Pontos de extremidade**, selecione o ponto **de extremidade de aluguel de previsão**. Em seguida, selecione **Excluir** e confirme que deseja excluir o ponto de extremidade.

A exclusão da computação garante que sua assinatura não será cobrada por recursos de computação. No entanto, será cobrado um pequeno valor pelo armazenamento de dados, desde que o espaço de trabalho do Aprendizado de Máquina do Azure exista em sua assinatura. Se você tiver terminado de explorar o Aprendizado de Máquina do Azure, poderá excluir o espaço de trabalho do Aprendizado de Máquina do Azure e os recursos associados.

* Para excluir seu espaço de trabalho:

1. No portal do Azure, na página **Grupos de recurso**s, abra o grupo de recursos especificado ao criar seu espaço de trabalho do Aprendizado de Máquina do Azure.
2. Clique em **Excluir grupo de recursos**, digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione **Excluir**.
      


# Referências:


[Explore Automated Machine Learning in Azure Machine Learning](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html)

&nbsp;
