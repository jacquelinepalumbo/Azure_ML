# Azure_ML
Train using Azure Machine Learning Compute

# Na prática usando Azure Machine Learning

[![LinkedIn](https://img.shields.io/badge/LinkedIn-000?style=for-the-badge&logo=LinkedIn&logoColor=30A3DC)](https://www.linkedin.com/in/jacqueline-ribeiro-743876247/)


### Criando um modelo de previsão com os devidos pontos de extremidade configurados

####  Descrição: Esta documentação tem como objetivo demonstrar um passo a passo de como criar um modelo de machine learning (aprendizado de máquina) automatizada na plataforma de estúdio da Azure. O modelo é uma simulação de um serviço de aluguel de bicicletas, baseando-se em um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo prevê o número de aluguéis de bicicletas esperados em um determinado dia, com base em características sazonais e meteorológicas.

#### ⚠️⚠ É necessário logar-se em uma conta da microsoft e criar uma workspace (espaço de trabalho) na aba "estúdio de aprendizado de máquina".

### Criando uma machine learning automatizada
1. No ambiente workspace, clique em "ML automatizado", localizado à esquerda da tela.
2. Na próxima janela, clique em "+Novo trabalho de ML automatizado"
3. Um formulário será aberto. Preencha com os dados de configuração: 
 Nome do trabalho: mslearn-bike-automl
* Nome do experimento
* Descrição
* Tags: Sem preenchimento 

4. Na próxima etapa, preencha os campos com os seguintes dados:

#### Tipo de tarefa e dados:

Seleção | Valor
--------------------------|-----------------
Selecionar tipo de tarefa | Regressão
Selecionar conjunto de dados | Crie um novo conjunto de dados com as seguintes configurações:

#### Tipo de dados:

Seleção | Valor
--------------------------|-----------------
Nome | bike-rentals
Descrição | Dados históricos de aluguer de bicicletas
Tipo | Tabular

#### Fonte de dados:
* Selecionar de arquivos da Web
* URL da Web: https://aka.ms/bike-rentals
* Ignorar validação de dados: não selecione

#### Configurações:

* Formato de arquivo: Delimitado
* Delimitador: Vírgula
* Codificação: UTF-8
* Cabeçalhos de coluna: Somente o primeiro arquivo tem cabeçalhos
* Pular linhas: Nenhum
* O conjunto de dados contém dados de várias linhas: não selecione
* Esquema:
* Incluir todas as colunas diferentes de Caminho
* Revisar os tipos detectados automaticamente
* Selecione Criar. Depois que o conjunto de dados for criado, selecione o conjunto de dados de aluguel de bicicletas para continuar a enviar o trabalho de ML automatizado.

#### Configurações da tarefa:
* Tipo de tarefa: Regressão
* Conjunto de dados: aluguel de bicicletas
* Coluna de destino: Aluguéis (inteiro)

#### Definições de configuração adicionais
* Métrica primária: Erro quadrático médio da raiz normalizada
* Explicar melhor modelo: Não selecionado
* Use todos os modelos suportados: Nãoselecionado. Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.
* Modelos permitidos: selecione apenas RandomForest e LightGBM — normalmente você gostaria de tentar o maior número possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.

#### Limites:
* Máximo de tentativas: 3
* Máximo de tentativas simultâneas: 3
*  Nós máximos: 3
* Limiar de pontuação métrica: 0,085 (de modo que, se um modelo atingir uma pontuação métrica quadrática média normalizada de 0,085 ou menos, o trabalho termina.)
* Tempo limite: 15
* Tempo limite de iteração: 15
* Habilitar rescisão antecipada: Selecionado

#### Validação e teste:
* Tipo de validação: Divisão de validação de trem
* Porcentagem de dados de validação: 10
* Conjunto de dados de teste: Nenhum

#### Computação:
* Selecione o tipo de computação: Serverless
* Tipo de máquina virtual: CPU
* Camada de máquina virtual: Dedicado
* Tamanho da máquina virtual: Standard_DS3_V2*
* Número de instâncias: 1
* Se sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.

5. Envie o trabalho de treinamento. Ele começa automaticamente.


* Aguarde a conclusão do trabalho. Pode demorar um pouco, em média 15 minutos dependendo da conexão de internet. 
 
#### Implantando e testando o modelo 


6. Na guia Modelo para obter o melhor modelo treinado pelo seu trabalho de aprendizado de máquina automatizado, selecione Implantar e usar a opção Serviço Web para implantar o modelo com as seguintes configurações:
* Nome: predict-rentals
* Descrição: Prever aluguéis de ciclos
* Tipo de computação: Instância de Contêiner do Azure
* Habilitar autenticação: Selecionado
* Aguarde o início da implantação - isso pode levar alguns segundos. O status de implantação para o ponto de extremidade de aluguel de previsão será indicado na parte principal da página como Em execução.
* Aguarde até que o status Implantar seja alterado para Bem-sucedido. Isso pode levar de 5 a 10 minutos.
* Após isso, vamos aos testes!

7. No estúdio do Aprendizado de Máquina do Azure, no menu à esquerda, selecione Pontos de extremidade e abra o ponto de extremidade em tempo real de aluguéis de previsão.

##### Na página de ponto de extremidade em tempo real de aluguéis de previsão, exiba a guia Teste.

##### No painel Dados de entrada para testar o ponto de extremidade, substitua o modelo JSON pelos seguintes dados de entrada:

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
8. Clique no botão Testar.

9. Analise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada.


 ```
{
   "Results": [
     444.27799000000000
   ]
 }

```

##### O painel de teste pegou os dados de entrada e usou o modelo que você treinou para retornar o número previsto de aluguéis.

&nbsp;

# Referências:


[!Explore Automated Machine Learning in Azure Machine Learning](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html)

&nbsp;
