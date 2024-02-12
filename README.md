# Azure Machine Learning
Projeto da aula Trabalhando com Machine Learning na Prática no Azure ML

## Acessando o serviço do Azure Machine learning
1. Acessar o portal do Azure [https://portal.azure.com](https://portal.azure.com);
2. Clicar em + Create a resource;
3. Na busca colocar "Machine Learning";
4. Selecionar a opção "Azure Machine Learning";
5. Clicar no botão Create ao lado de Plan - Azure Machine Learning;
6. Preencher o formulário. Dados utilizados:
- Subscription: Azure Subscription 1
- Resource group: Create new >> laboratorio_dio >> OK
- Name: laboratorio_dio_ai900
- Region: East US 2
- Storage account: laboratoriodio**********
- Key vault: laboratoriodio**********
- Application insights: laboratoriodio(**********)
- Container registry: None

## Configurando Modelos e Conjuntos de Dados
1. Após criado, clicar em Go to resource;
2. Clicar em Launch studio;
3. Selecionar na barra esquerda a opção Automated ML;
4. Clicar em New Automated ML job;
5. Preencher o formulário. Dados utilizados:
- Job name: mslearn-bike-automl
- New experiment name: mslearn-bike-automl
- Description: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
- Tags: nenhum;
>> Next
- Select task type: Regression
>> Em "Select data", clicar em + Create
- Name: alugueldebicicletas
- Description: dados históricos de aluguel de bicicletas
- Type: Tabular
>> Next
- Choose a source for your data asset: From web files
>> Next
- Web URL: https://aka.ms/bike-rentals
- Skip data validation: not selescted
>> Next
- File format: Delimited
- Delimiter: Comma
- Encoding: UTF-8
- Column headers: Only first file has headers
- Skip rows: None
- Dataset contains multi-line data: not selected
>> Next
- Todas as colunas seleionadas, com exceção de Path
>> Next
- Clicar em Create
6. Selecionar ao lado da opção que foi criada;
>> Next
7. Preencher o formulário:
- Target Column: rentals (Integer)
- Clicar em View additional configuration settings:
- Primary metric: NormalizedRootMeanSquaredError
- Explain best model: desmarcado;
- Enable ensemble stacking: desmarcado;
- Use all supported models: desmarcado;
- Allowed models: RandomForest e LightGBM selecionados
>> Save
>> Expandir Limits e preencher: 
- Max trials: 3
- Max concurrent trials: 3
- Max nodes: 3
- Metric score threshold: 0,085
- Experiment timeout (minutes): 15
- Iteration timeout (minutes): 15
- Enable early termination: selecionado

- Validation type: Train-validation split
- Percentage validation of data: 10
- Test data: None
>> Next
- Select compute type: serveless
- Virtual machine type: CPU
- Virtual machine tier: Dedicated
- Virtual machine size: Standard_DS3_V2*
- Number of instances: 1
>> Next
8. Clicar em Submit training job

## Análise e teste de modelo
1. No modelo criado, clicar no link abaixo da opção Algorithm name;
2. Selecione a guia Model;
3. Selecione a opção Deploy;
4. Selecione Web service e preencha o formulário:
- Name: predict-rentals
- Description: Predict cycle rentals
- Compute type: Azure Container Instance
- Enable authentication: Selected
5. Clique em Deploy.
6. Após concluir, clique em Endpoints;
7. Selecione ao lado de predict-rentals;
8. Clique na aba Test;
9. No painel Input data to test endpoint, substituir o json pelo descrito a seguir:

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
10. Clique em Test
11. O resultyado obtido no caso foi: 
```
Test result
{1 item
"Results":[1 item
0:float339.85169575809016
]
}
 ```

Ref.: documento criado como projeto em treinamento da Dio e com base na documentação oficial, disponível no endereço: https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html
