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


* Aguarde a conclusão do trabalho.  

# Reveja o melhor modelo

Quando o trabalho de aprendizado de máquina automatizado for concluído, você poderá revisar o melhor modelo treinado.

1. Na guia Visão geral do trabalho de aprendizado de máquina automatizado, observe o melhor resumo do modelo. 
<img width="889" alt="image" src="https://github.com/jacquelinepalumbo/Azure_ML/assets/119548193/8aec8c9d-933d-4a23-bb14-b47ab3465143">


2. Selecione o texto em *Nome do algoritmo* para o melhor modelo para exibir seus detalhes.

3. Selecione a guia *Métricas* e selecione os *gráficos de resíduos* e *predicted_true* se ainda não estiverem selecionados.

Analise os gráficos que mostram o desempenho do modelo. O gráfico *de resíduos* mostra os resíduos (as diferenças entre os valores previstos e reais) como um histograma. O gráfico *predicted_true* compara os valores previstos com os valores verdadeiros.



 
# Implantando e testando o modelo 


1. Na guia *Modelo* para obter o melhor modelo treinado pelo seu trabalho de aprendizado de máquina automatizado, selecione Implantar e usar a opção Serviço Web para implantar o modelo com as seguintes configurações:
- *Nome*: predict-rentals
- *Descrição*: Prever aluguéis de ciclos
- *Tipo de computação*: Instância de Contêiner do Azure
- *Habilitar autenticação*: Selecionado

2. Aguarde o início da implantação - isso pode levar alguns segundos. O status de implantação para o ponto de extremidade de aluguel de previsão será indicado na parte principal da página como Em execução.
3. Aguarde até que o status Implantar seja alterado para Bem-sucedido. 

# Testar o serviço implantado

1. No estúdio do Aprendizado de Máquina do Azure, no menu à esquerda, selecione *Pontos de extremidade* e abra o ponto de extremidade em tempo real *de aluguéis de previsão*.

2. Na página de ponto de extremidade em tempo real de *aluguéis de previsão*, exiba a guia *Teste*.

3. No painel *Dados de entrada para testar o ponto de extremidade*, substitua o modelo JSON pelos seguintes dados de entrada:

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
4. Clique no botão *Testar*.

5. Analise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada:


 ```
{
   "Results": [
     444.27799000000000
   ]
 }

```

O painel de teste pegou os dados de entrada e usou o modelo que você treinou para retornar o número previsto de aluguéis.



# Executar o JSON bruto


```

{
    "runId": "mslearn-bike-automl",
    "runUuid": "e1f617ac-e993-4edd-98a6-6c14f9a77681",
    "parentRunUuid": null,
    "rootRunUuid": "e1f617ac-e993-4edd-98a6-6c14f9a77681",
    "target": "Serverless",
    "status": "Completed",
    "parentRunId": null,
    "dataContainerId": "dcid.mslearn-bike-automl",
    "createdTimeUtc": "2024-02-27T20:19:38.9140176+00:00",
    "startTimeUtc": "2024-02-27T20:19:53.791Z",
    "endTimeUtc": "2024-02-27T20:29:41.016Z",
    "error": null,
    "warnings": null,
    "tags": {
        "_aml_system_automl_mltable_data_json": "{\"Type\":\"MLTable\",\"TrainData\":{\"Uri\":\"azureml://locations/eastus/workspaces/61fb8670-19e5-490b-853a-c78532da45d2/data/bike-rentals/versions/1\",\"ResolvedUri\":null,\"AssetId\":null},\"TestData\":null,\"ValidData\":null}",
        "model_explain_run": "best_run",
        "_aml_system_automl_run_workspace_id": "61fb8670-19e5-490b-853a-c78532da45d2",
        "_aml_system_azureml.automlComponent": "AutoML",
        "pipeline_id_000": "b76be6b5846772ee1128c4d415381c1e9fed455e;faf12f74cf9bbd358ca5525682c5030d36f7be7c;__AutoML_Ensemble__",
        "score_000": "0.10736377039417899;0.09357675586470318;0.09062775770874848",
        "predicted_cost_000": "0.5;0;0",
        "fit_time_000": "0.020339;0.063691;1",
        "training_percent_000": "100;100;100",
        "iteration_000": "1;0;2",
        "run_preprocessor_000": "MaxAbsScaler;MaxAbsScaler;",
        "run_algorithm_000": "RandomForest;LightGBM;VotingEnsemble",
        "automl_best_child_run_id": "mslearn-bike-automl_2"
    },
    "properties": {
        "num_iterations": "3",
        "training_type": "TrainFull",
        "acquisition_function": "EI",
        "primary_metric": "normalized_root_mean_squared_error",
        "train_split": "0",
        "acquisition_parameter": "0",
        "num_cross_validation": "",
        "target": "Serverless",
        "AMLSettingsJsonString": "{\"is_subgraph_orchestration\":false,\"is_automode\":true,\"path\":\"./sample_projects/\",\"subscription_id\":\"bc6f9fa0-de29-45b4-bb19-8800a65dd627\",\"resource_group\":\"ML\",\"workspace_name\":\"LabML\",\"iterations\":3,\"primary_metric\":\"normalized_root_mean_squared_error\",\"task_type\":\"regression\",\"IsImageTask\":false,\"IsTextDNNTask\":false,\"validation_size\":0.1,\"n_cross_validations\":null,\"preprocess\":true,\"is_timeseries\":false,\"time_column_name\":null,\"grain_column_names\":null,\"max_cores_per_iteration\":-1,\"max_concurrent_iterations\":3,\"max_nodes\":3,\"iteration_timeout_minutes\":15,\"enforce_time_on_windows\":false,\"experiment_timeout_minutes\":15,\"exit_score\":\"NaN\",\"experiment_exit_score\":0.085,\"whitelist_models\":[\"RandomForest\",\"LightGBM\"],\"blacklist_models\":null,\"blacklist_algos\":[\"TensorFlowDNN\",\"TensorFlowLinearRegressor\"],\"auto_blacklist\":false,\"blacklist_samples_reached\":false,\"exclude_nan_labels\":false,\"verbosity\":20,\"model_explainability\":false,\"enable_onnx_compatible_models\":false,\"enable_feature_sweeping\":false,\"send_telemetry\":true,\"enable_early_stopping\":true,\"early_stopping_n_iters\":20,\"distributed_dnn_max_node_check\":false,\"enable_distributed_featurization\":true,\"enable_distributed_dnn_training\":true,\"enable_distributed_dnn_training_ort_ds\":false,\"ensemble_iterations\":3,\"enable_tf\":false,\"enable_cache\":false,\"enable_subsampling\":false,\"metric_operation\":\"minimize\",\"enable_streaming\":false,\"use_incremental_learning_override\":false,\"force_streaming\":false,\"enable_dnn\":false,\"is_gpu_tmp\":false,\"enable_run_restructure\":false,\"featurization\":\"auto\",\"vm_type\":\"Standard_DS3_v2\",\"vm_priority\":\"dedicated\",\"label_column_name\":\"rentals\",\"weight_column_name\":null,\"miro_flight\":\"default\",\"many_models\":false,\"many_models_process_count_per_node\":0,\"automl_many_models_scenario\":null,\"enable_batch_run\":true,\"save_mlflow\":true,\"track_child_runs\":true,\"test_include_predictions_only\":false,\"enable_mltable_quick_profile\":\"True\",\"has_multiple_series\":false,\"_enable_future_regressors\":false,\"enable_ensembling\":true,\"enable_stack_ensembling\":false,\"ensemble_download_models_timeout_sec\":300.0,\"stack_meta_learner_train_percentage\":0.2}",
        "DataPrepJsonString": null,
        "EnableSubsampling": "False",
        "runTemplate": "AutoML",
        "azureml.runsource": "automl",
        "_aml_internal_automl_best_rai": "False",
        "ClientType": "Mfe",
        "_aml_system_scenario_identification": "Remote.Parent",
        "PlatformVersion": "DPV2",
        "environment_cpu_name": "AzureML-AutoML",
        "environment_cpu_label": "prod",
        "environment_gpu_name": "AzureML-AutoML-GPU",
        "environment_gpu_label": "prod",
        "root_attribution": "automl",
        "attribution": "AutoML",
        "Orchestrator": "AutoML",
        "CancelUri": "https://eastus.api.azureml.ms/jasmine/v1.0/subscriptions/bc6f9fa0-de29-45b4-bb19-8800a65dd627/resourceGroups/ML/providers/Microsoft.MachineLearningServices/workspaces/LabML/experimentids/5f3fdb23-a93b-414c-b0ba-298d475d92ea/cancel/mslearn-bike-automl",
        "mltable_data_json": "{\"Type\":\"MLTable\",\"TrainData\":{\"Uri\":\"azureml://locations/eastus/workspaces/61fb8670-19e5-490b-853a-c78532da45d2/data/bike-rentals/versions/1\",\"ResolvedUri\":\"azureml://locations/eastus/workspaces/61fb8670-19e5-490b-853a-c78532da45d2/data/bike-rentals/versions/1\",\"AssetId\":\"azureml://locations/eastus/workspaces/61fb8670-19e5-490b-853a-c78532da45d2/data/bike-rentals/versions/1\"},\"TestData\":null,\"ValidData\":null}",
        "ClientSdkVersion": null,
        "snapshotId": "00000000-0000-0000-0000-000000000000",
        "SetupRunId": "mslearn-bike-automl_setup",
        "SetupRunContainerId": "dcid.mslearn-bike-automl_setup",
        "FeaturizationRunJsonPath": "featurizer_container.json",
        "FeaturizationRunId": "mslearn-bike-automl_featurize",
        "ProblemInfoJsonString": "{\"dataset_num_categorical\": 0, \"is_sparse\": true, \"subsampling\": false, \"has_extra_col\": true, \"dataset_classes\": 552, \"dataset_features\": 64, \"dataset_samples\": 657, \"single_frequency_class_detected\": false}"
    },
    "parameters": {},
    "services": {},
    "inputDatasets": null,
    "outputDatasets": [],
    "runDefinition": null,
    "logFiles": {},
    "jobCost": {
        "chargedCpuCoreSeconds": null,
        "chargedCpuMemoryMegabyteSeconds": null,
        "chargedGpuSeconds": null,
        "chargedNodeUtilizationSeconds": null
    },
    "revision": 13,
    "runTypeV2": {
        "orchestrator": "AutoML",
        "traits": [
            "automl",
            "Remote.Parent"
        ],
        "attribution": null,
        "computeType": null
    },
    "settings": {},
    "computeRequest": null,
    "compute": {
        "target": "Serverless",
        "targetType": "AmlCompute",
        "vmSize": "Standard_DS3_v2",
        "instanceType": "Standard_DS3_v2",
        "instanceCount": 1,
        "gpuCount": null,
        "priority": "Dedicated",
        "region": null,
        "armId": null,
        "properties": null
    },
    "createdBy": {
        "userObjectId": "58ab2561-c385-49a3-94b0-16aa667e2ee0",
        "userPuId": "10032002F4A9B09A",
        "userIdp": "live.com",
        "userAltSecId": "1:live.com:00067FFE9EB109E4",
        "userIss": "https://sts.windows.net/cc65d2d3-05dc-4b0e-a4f9-0001fa5db537/",
        "userTenantId": "cc65d2d3-05dc-4b0e-a4f9-0001fa5db537",
        "userName": "Jacqueline Palumbo",
        "upn": null
    },
    "computeDuration": "00:09:47.2244108",
    "effectiveStartTimeUtc": null,
    "runNumber": 1709065178,
    "rootRunId": "mslearn-bike-automl",
    "experimentId": "5f3fdb23-a93b-414c-b0ba-298d475d92ea",
    "userId": "58ab2561-c385-49a3-94b0-16aa667e2ee0",
    "statusRevision": 3,
    "currentComputeTime": null,
    "lastStartTimeUtc": null,
    "lastModifiedBy": {
        "userObjectId": "58ab2561-c385-49a3-94b0-16aa667e2ee0",
        "userPuId": "10032002F4A9B09A",
        "userIdp": "live.com",
        "userAltSecId": "1:live.com:00067FFE9EB109E4",
        "userIss": "https://sts.windows.net/cc65d2d3-05dc-4b0e-a4f9-0001fa5db537/",
        "userTenantId": "cc65d2d3-05dc-4b0e-a4f9-0001fa5db537",
        "userName": "Jacqueline Palumbo",
        "upn": null
    },
    "lastModifiedUtc": "2024-02-27T20:29:40.5664685+00:00",
    "duration": "00:09:47.2244108",
    "inputs": {
        "training_data": {
            "assetId": "azureml://locations/eastus/workspaces/61fb8670-19e5-490b-853a-c78532da45d2/data/bike-rentals/versions/1",
            "type": "MLTable"
        }
    },
    "outputs": {
        "best_model": {
            "assetId": "azureml://locations/eastus/workspaces/61fb8670-19e5-490b-853a-c78532da45d2/models/azureml_mslearn-bike-automl_2_output_mlflow_log_model_350159994/versions/1",
            "type": "MLFlowModel"
        }
    },
    "currentAttemptId": 1
}


```

# Referências:


[Explore Automated Machine Learning in Azure Machine Learning](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html)

&nbsp;
