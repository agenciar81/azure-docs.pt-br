---
title: Solucionar erros de módulo
titleSuffix: Azure Machine Learning
description: Solucionar problemas de exceções de módulo no designer de Azure Machine Learning usando códigos de erro
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 12/03/2019
ms.openlocfilehash: ea132578a08b9f0002084374838c615a01fa820f
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425792"
---
# <a name="exceptions-and-error-codes-for-the-designer-preview"></a>Exceções e códigos de erro para o designer (versão prévia)

Este artigo descreve as mensagens de erro e os códigos de exceção no designer de Azure Machine Learning (versão prévia) para ajudá-lo a solucionar problemas de pipelines do Machine Learning.

Há duas maneiras de obter o texto completo de uma mensagem de erro no designer:  

- Clique no link, **exiba o log de saída**, no painel direito e role até a parte inferior. A mensagem de erro detalhada é exibida nas duas últimas linhas da janela.  
  
- Selecione o módulo que tem o erro e clique no X vermelho. Somente o texto de erro pertinente é exibido.

## <a name="error-0001"></a>Erro 0001  
 Ocorrerá uma exceção se uma ou mais das colunas especificadas do conjunto de dados não puder ser encontrada.  

 Você receberá esse erro se uma seleção de coluna for feita para um módulo, mas as colunas selecionadas não existirem no conjunto de dados de entrada. Esse erro pode ocorrer se você tiver digitado manualmente um nome de coluna ou se o seletor de coluna tiver fornecido uma coluna sugerida que não existia no conjunto de seus quando você executou o pipeline.  

**Resolução:** Revisite o módulo lançando essa exceção e valide se o nome ou os nomes da coluna estão corretos e se todas as colunas referenciadas existem.  

|Mensagens de Exceção|
|------------------------|
|Uma ou mais colunas especificadas não foram encontradas.|
|A coluna com o nome ou o índice "{column_id}" não foi encontrada.|
|A coluna com o nome ou o índice "{column_id}" não existe em "{arg_name_missing_column}".|
|A coluna com o nome ou o índice "{column_id}" não existe em "{arg_name_missing_column}", mas existe em "{arg_name_has_column}".|


## <a name="error-0002"></a>Erro 0002  
 Ocorrerá uma exceção se um ou mais parâmetros não puder ser analisado ou convertido do tipo especificado no exigido pelo tipo de método de destino.  

 Esse erro ocorre em Azure Machine Learning quando você especifica um parâmetro como entrada e o tipo de valor é diferente do tipo esperado e a conversão implícita não pode ser executada.  

**Resolução:** Verifique os requisitos do módulo e determine qual tipo de valor é necessário (cadeia de caracteres, inteiro, duplo, etc.)  

|Mensagens de Exceção|
|------------------------|
|Falha ao analisar o parâmetro.|
|Falha ao analisar o parâmetro "{arg_name_or_column}".|
|Falha ao converter o parâmetro "{arg_name_or_column}" em "{to_type}".|
|Falha ao converter o parâmetro "{arg_name_or_column}" de "{from_type}" para "{to_type}".|
|Falha ao converter o valor do parâmetro "{arg_name_or_column}" "{arg_value}" de "{from_type}" em "{to_type}".|
|Falha ao converter o valor "{arg_value}" na coluna "{arg_name_or_column}" de "{from_type}" para "{to_type}" com o uso do formato "{fmt}" fornecido.|


## <a name="error-0003"></a>Erro 0003  
 Ocorrerá uma exceção se uma ou mais das entradas for nula ou estiver vazia.  

 Você receberá esse erro em Azure Machine Learning se quaisquer entradas ou parâmetros para um módulo forem nulos ou vazios.  Esse erro pode ocorrer, por exemplo, quando você não digitou nenhum valor para um parâmetro. Isso também pode acontecer se você escolher um conjunto de um DataSet que tem valores ausentes ou um conjunto de um DataSet vazio.  

**Resolução:**

+ Abra o módulo que produziu a exceção e verifique se todas as entradas foram especificadas. Certifique-se de que todas as entradas necessárias sejam especificadas. 
+ Verifique se os dados carregados do armazenamento do Azure estão acessíveis e se o nome ou a chave da conta não foi alterado.  
+ Verifique os dados de entrada quanto a valores ausentes ou nulos.
+ Se estiver usando uma consulta em uma fonte de dados, verifique se os dados estão sendo retornados no formato esperado. 
+ Verifique se há erros de digitação ou outras alterações na especificação dos dados.
  
|Mensagens de Exceção|
|------------------------|
|Uma ou mais entradas são nulas ou estão vazias.|
|A entrada "{Name}" é nula ou está vazia.|


## <a name="error-0004"></a>Erro 0004  
 Ocorrerá uma exceção se o parâmetro for menor ou igual ao valor específico.  

 Você receberá esse erro em Azure Machine Learning se o parâmetro na mensagem estiver abaixo de um valor de limite necessário para o módulo processar os dados.  

**Resolução:** Revisite o módulo lançando a exceção e modifique o parâmetro para que seja maior que o valor especificado.  

|Mensagens de Exceção|
|------------------------|
|O parâmetro deve ser maior que o valor do limite.|
|O valor do parâmetro "{arg_name}" deve ser maior que {lower_boundary}.|
|O parâmetro "{arg_name}" tem o valor "{actual_value}", que deve ser maior que {lower_boundary}.|


## <a name="error-0005"></a>Erro 0005  
 Ocorrerá uma exceção se o parâmetro for menor que um valor específico.  

 Você receberá esse erro em Azure Machine Learning se o parâmetro na mensagem estiver abaixo ou for igual a um valor de limite necessário para o módulo processar os dados.  

**Resolução:** Revisite o módulo lançando a exceção e modifique o parâmetro para que seja maior ou igual ao valor especificado.  

|Mensagens de Exceção|
|------------------------|
|O parâmetro deve ser maior ou igual ao valor do limite.|
|O valor do parâmetro "{arg_name}" deve ser maior ou igual a {lower_boundary}.|
|O parâmetro "{arg_name}" tem o valor "{value}", que deve ser maior ou igual a {lower_boundary}.|


## <a name="error-0006"></a>Erro 0006  
 Ocorrerá uma exceção se o parâmetro for maior ou igual ao valor especificado.  

 Você receberá esse erro em Azure Machine Learning se o parâmetro na mensagem for maior ou igual ao valor de limite necessário para o módulo processar os dados.  

**Resolução:** Revisite o módulo lançando a exceção e modifique o parâmetro para ser menor que o valor especificado.  

|Mensagens de Exceção|
|------------------------|
|Incompatibilidade de parâmetros. Um dos parâmetros deve ser menor que o outro.|
|O valor do parâmetro "{arg_name}" deve ser menor que o valor do parâmetro "{upper_boundary_parameter_name}".|
|O parâmetro "{arg_name}" tem o valor "{value}", que deve ser menor que {upper_boundary_parameter_name}.|


## <a name="error-0007"></a>Erro 0007  
 Ocorrerá uma exceção se o parâmetro for maior que um valor específico.  

 Você receberá esse erro em Azure Machine Learning se, nas propriedades do módulo, você tiver especificado um valor maior do que o permitido. Por exemplo, você pode especificar um dado que está fora do intervalo de datas com suporte ou pode indicar que cinco colunas serão usadas quando apenas três colunas estiverem disponíveis. 

 Você também poderá ver esse erro se estiver especificando dois conjuntos de dados que precisam corresponder de alguma maneira. Por exemplo, se você estiver renomeando colunas e especificar as colunas por índice, o número de nomes que você fornecer deve corresponder ao número de índices de coluna. Outro exemplo pode ser uma operação matemática que usa duas colunas, em que as colunas devem ter o mesmo número de linhas. 

**Resolução:**

 + Abra o módulo em questão e revise as configurações de propriedade numérica.
 + Verifique se os valores de parâmetro estão dentro do intervalo de valores com suporte para essa propriedade.
 + Se o módulo usar várias entradas, certifique-se de que as entradas tenham o mesmo tamanho.
<!-- + If the module has multiple properties that can be set, ensure that related properties have appropriate values. For example, when using [Group Data into Bins](group-data-into-bins.md), if you use the option to specify custom bin edges, the number of bins must match the number of values you provide as bin boundaries.-->
 + Verifique se o DataSet ou a fonte de dados foi alterado. Às vezes, um valor que funcionava com uma versão anterior dos dados falhará depois que o número de colunas, os tipos de dados da coluna ou o tamanho dos dados forem alterados.  

|Mensagens de exceção|
|------------------------|
|Incompatibilidade de parâmetros. Um dos parâmetros deve ser menor ou igual a outro.|
|O valor do parâmetro "{arg_name}" deve ser menor ou igual ao valor do parâmetro "{upper_boundary_parameter_name}".|
|O parâmetro "{arg_name}" tem o valor "{actual_value}", que deve ser menor ou igual a {upper_boundary}.|
|O parâmetro "{arg_name}" valor {actual_value} deve ser menor ou igual ao parâmetro "{upper_boundary_parameter_name}" valor {upper_boundary}.|


## <a name="error-0008"></a>Erro 0008  
 Ocorrerá uma exceção se o parâmetro não estiver no intervalo.  

 Você receberá esse erro em Azure Machine Learning se o parâmetro na mensagem estiver fora dos limites necessários para o módulo processar os dados.  

 Por exemplo, esse erro será exibido se você tentar usar [adicionar linhas](add-rows.md) para combinar dois conjuntos de valores que têm um número diferente de colunas.  

**Resolução:** Revisite o módulo lançando a exceção e modifique o parâmetro para estar dentro do intervalo especificado.  

|Mensagens de Exceção|
|------------------------|
|O valor do parâmetro não está no intervalo especificado.|
|O valor do parâmetro "{arg_name}" não está no intervalo.|
|O valor do parâmetro "{arg_name}" deve estar no intervalo de [{lower_boundary}, {upper_boundary}].|
|O valor do parâmetro "{arg_name}" não está no intervalo. falha|


## <a name="error-0009"></a>Erro 0009  
 Ocorrerá uma exceção quando o nome da conta de armazenamento do Azure ou o nome do contêiner for especificado incorretamente.  

Esse erro ocorre no designer de Azure Machine Learning quando você especifica parâmetros para uma conta de armazenamento do Azure, mas o nome ou a senha não podem ser resolvidos. Erros em senhas ou nomes de conta podem ocorrer por vários motivos:

 + A conta é do tipo incorreto. Não há suporte para alguns novos tipos de conta para uso com Machine Learning designer. Consulte [importar dados](import-data.md) para obter detalhes.
 + Você inseriu o nome da conta incorreta
 + A conta não existe mais
 + A senha da conta de armazenamento está incorreta ou foi alterada
 + Você não especificou o nome do contêiner ou o contêiner não existe
 + Você não especificou totalmente o caminho do arquivo (caminho para o blob)
   

**Resolução:**

Esses problemas geralmente ocorrem quando você tenta inserir manualmente o nome da conta, a senha ou o caminho do contêiner. Recomendamos que você use o novo assistente para o módulo [importar dados](import-data.md) , que ajuda você a Pesquisar e verificar nomes.

Verifique também se a conta, o contêiner ou o blob foram excluídos. Use outro utilitário de armazenamento do Azure para verificar se o nome da conta e a senha foram inseridos corretamente e se o contêiner existe. 

Não há suporte para alguns tipos de conta mais novos pelo Azure Machine Learning. Por exemplo, os novos tipos de armazenamento "quente" ou "frio" não podem ser usados para o aprendizado de máquina. As contas de armazenamento clássicas e as contas de armazenamento criadas como "finalidade geral" funcionam bem.

Se o caminho completo para um blob tiver sido especificado, verifique se o caminho está especificado como **container/blobname**e se o contêiner e o blob existem na conta.  

 O caminho não deve conter uma barra à esquerda. Por exemplo, **/container/blob** está incorreto e deve ser inserido como **contêiner/blob**.  


|Mensagens de Exceção|
|------------------------|
|O nome da conta de armazenamento do Azure ou o nome do contêiner está incorreto.|
|O nome da conta de armazenamento do Azure "{account_name}" ou o nome do contêiner "{container_name}" está incorreto; um nome de contêiner do formato contêiner/blob era esperado.|


## <a name="error-0010"></a>Erro 0010  
 Ocorrerá uma exceção se os conjuntos de dados da entrada tiverem nomes de colunas que deveriam corresponder, mas não correspondem.  

 Você receberá esse erro em Azure Machine Learning se o índice da coluna na mensagem tiver nomes de coluna diferentes nos dois conjuntos de dados de entrada.  

**Resolução:** Use [Editar metadados](edit-metadata.md) ou modificar o conjunto de um original para ter o mesmo nome de coluna para o índice de coluna especificado.  

|Mensagens de Exceção|
|------------------------|
|As colunas de índice correspondente em conjuntos de dados de entrada têm nomes diferentes.|
|Os nomes de coluna não são os mesmos para a coluna {col_index} (baseada em zero) dos conjuntos de dados de entrada ({dataSet1} e {dataset2}, respectivamente).|


## <a name="error-0011"></a>Erro 0011  
 Ocorrerá uma exceção se o argumento de conjunto de colunas passado não se aplicar a qualquer uma das colunas do conjunto de dados.  

 Você receberá esse erro em Azure Machine Learning se a seleção de coluna especificada não corresponder a nenhuma das colunas no conjunto de dados fornecido.  

 Você também poderá obter esse erro se não tiver selecionado uma coluna e pelo menos uma coluna for necessária para que o módulo funcione.  

**Resolução:** Modifique a seleção de coluna no módulo para que ela seja aplicada às colunas no conjunto de linhas.  

 Se o módulo exigir que você selecione uma coluna específica, como uma coluna de rótulo, verifique se a coluna à direita está selecionada.  

 Se forem selecionadas colunas inadequadas, remova-as e execute novamente o pipeline.  

|Mensagens de Exceção|
|------------------------|
|O conjunto de colunas especificado não se aplica a qualquer uma das colunas do conjunto de dados.|
|O conjunto de colunas especificado "{column_set}" não se aplica a nenhuma coluna de conjunto de linhas.|


## <a name="error-0012"></a>Erro 0012  
 Ocorrerá uma exceção se não foi possível criar instância da classe com o conjunto de argumentos passado.  

**Resolução:** Esse erro não é acionável pelo usuário e será preterido em uma versão futura.  

|Mensagens de Exceção|
|------------------------|
|Modelo não treinado; treine primeiro o modelo.|
|Modelo não treinado ({arg_name}), use o modelo treinado.|


## <a name="error-0013"></a>Erro 0013  
 Ocorrerá uma exceção se o aprendiz passado para o módulo for um tipo inválido.  

 Esse erro ocorre sempre que um modelo treinado é incompatível com o módulo de Pontuação conectado. <!--For example, connecting the output of [Train Matchbox Recommender](train-matchbox-recommender.md) to [Score Model](score-model.md) (instead of [Score Matchbox Recommender](score-matchbox-recommender.md)) will generate this error when the pipeline is run.  -->

**Resolução:**

Determine o tipo de aprendiz que é produzido pelo módulo de treinamento e determine o módulo de Pontuação apropriado para o aprendiz. 

Se o modelo foi treinado usando qualquer um dos módulos de treinamento especializados, conecte o modelo treinado somente ao módulo de Pontuação especializado correspondente. 


|Tipo de modelo|Módulo de treinamento| Módulo de Pontuação|
|----|----|----|
|qualquer classificador|[Treinar Modelo](train-model.md) |[Modelo de Pontuação](score-model.md)|
|qualquer modelo de regressão|[Treinar Modelo](train-model.md) |[Modelo de Pontuação](score-model.md)|

<!--| clustering models| [Train Clustering Model](train-clustering-model.md) or [Sweep Clustering](sweep-clustering.md)| [Assign Data to Clusters](assign-data-to-clusters.md)|
| anomaly detection - One-Class SVM | [Train Anomaly Detection Model](train-anomaly-detection-model.md) |[Score Model](score-model.md)|
| anomaly detection - PCA |[Train Model](train-model.md) |[Score Model](score-model.md) </br> Some additional steps are required to evaluate the model. |
| anomaly detection - time series|  [Time Series Anomaly Detection](time-series-anomaly-detection.md) |Model trains from data and generates scores. The module does not create a trained learner and no additional scoring is required. |
| recommendation model| [Train Matchbox Recommender](train-matchbox-recommender.md) | [Score Matchbox Recommender](score-matchbox-recommender.md) |
| image classification | [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md) | [Score Model](score-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) | [Score Vowpal Wabbit Version 7-4 Model](score-vowpal-wabbit-version-7-4-model.md) |   
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-10 Model](train-vowpal-wabbit-version-7-10-model.md) | [Score Vowpal Wabbit Version 7-10 Model](score-vowpal-wabbit-version-7-10-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) | [Score Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) |-->

|Mensagens de Exceção|
|------------------------|
|Aprendiz de tipo inválido foi passado.|
|O aprendiz "{arg_name}" tem um tipo inválido.|
|O aprendiz "{arg_name}" tem um tipo inválido "{learner_type}".|


## <a name="error-0014"></a>Erro 0014  
 Ocorrerá uma exceção se a contagem de valores exclusivos de coluna for maior que o permitido.  

 Esse erro ocorre quando uma coluna contém um número excessivo de valores exclusivos.  Por exemplo, você poderá ver esse erro se especificar que uma coluna seja tratada como dados categóricos, mas há muitos valores exclusivos na coluna para permitir que o processamento seja concluído. Você também poderá ver esse erro se houver uma incompatibilidade entre o número de valores exclusivos em duas entradas.   

**Resolução:**

Abra o módulo que gerou o erro e identifique as colunas usadas como entradas. Para alguns módulos, você pode clicar com o botão direito do mouse na entrada do conjunto de dados e selecionar **Visualizar** para obter estatísticas em colunas individuais, incluindo o número de valores exclusivos e sua distribuição.

Para as colunas que você pretende usar para Agrupamento ou categorização, execute as etapas para reduzir o número de valores exclusivos em colunas. Você pode reduzir de maneiras diferentes, dependendo do tipo de dados da coluna. 
<!--
+ For text data, you might be able to use [Preprocess Text](preprocess-text.md) to collapse similar entries. 
+ For numeric data, you can create a smaller number of bins using [Group Data into Bins](group-data-into-bins.md), remove or truncate values using [Clip Values](clip-values.md), or use machine learning methods such as [Principal Component Analysis](principal-component-analysis.md) or [Learning with Counts](data-transformation-learning-with-counts.md) to reduce the dimensionality of the data.  
-->
> [!TIP]
> Não foi possível encontrar uma resolução que corresponda ao seu cenário? Você pode fornecer comentários sobre este tópico que inclui o nome do módulo que gerou o erro e o tipo de dados e a cardinalidade da coluna. Usaremos as informações para fornecer etapas de solução de problemas mais direcionadas para cenários comuns.   

|Mensagens de Exceção|
|------------------------|
|A quantidade de valores exclusivos da coluna é maior que o permitido.|
|O número de valores exclusivos na coluna: "{column_name}" é maior que o permitido.|
|O número de valores exclusivos na coluna: "{column_name}" excede a contagem de tupla de {limitação}.|


## <a name="error-0015"></a>Erro 0015  
 Ocorrerá uma exceção se a conexão do banco de dados tiver falhado.  

 Você receberá esse erro se inserir um nome de conta SQL incorreto, uma senha, um servidor de banco de dados ou um nome de banco de dados ou se uma conexão com o banco de dados não puder ser estabelecida devido a problemas com o banco de dados ou servidor.  

**Resolução:** Verifique se o nome da conta, a senha, o servidor de banco de dados e o banco de dados foram inseridos corretamente e se a conta especificada tem o nível correto de permissões. Verifique se o banco de dados está acessível no momento.  

|Mensagens de Exceção|
|------------------------|
|Erro ao estabelecer conexão com o banco de dados.|
|Erro ao fazer conexão de banco de dados: {connection_str}.|


## <a name="error-0016"></a>Erro 0016  
 Ocorrerá uma exceção se os conjuntos de dados de entrada passados para o módulo devessem ter tipos de coluna compatíveis, mas não tiverem.  

 Você receberá esse erro em Azure Machine Learning se os tipos das colunas passados em dois ou mais conjuntos de valores não forem compatíveis entre si.  

**Resolução:** Usar [Editar metadados](edit-metadata.md) ou modificar o conjunto de dados de entrada original<!--, or use [Convert to Dataset](convert-to-dataset.md)--> para garantir que os tipos de colunas sejam compatíveis.  

|Mensagens de Exceção|
|------------------------|
|As colunas com índice correspondente em conjuntos de dados de entrada têm tipos incompatíveis.|
|As colunas ' {first_col_names} ' são incompatíveis entre os dados de treinamento e teste.|
|As colunas ' {first_col_names} ' e ' {second_col_names} ' são incompatíveis.|
|Os tipos de elemento de coluna não são compatíveis com a coluna ' {first_col_names} ' (baseada em zero) dos conjuntos de dados de entrada ({first_dataset_names} e {second_dataset_names}, respectivamente).|


## <a name="error-0017"></a>Erro 0017  
 Ocorrerá uma exceção se uma coluna selecionada usar um tipo de dados que não é suportado pelo módulo atual.  

 Por exemplo, você pode receber esse erro em Azure Machine Learning se a seleção de coluna incluir uma coluna com um tipo de dados que não pode ser processado pelo módulo, como uma coluna de cadeia de caracteres para uma operação matemática ou uma coluna de pontuação em que uma coluna de recurso categórico é Necessário.  

**Resolução:**
 1. Identifique a coluna que é o problema.
 2. Examine os requisitos do módulo.
 3. Modifique a coluna para que ela fique em conformidade com os requisitos. Talvez seja necessário usar vários dos seguintes módulos para fazer alterações, dependendo da coluna e da conversão que você está tentando:
    + Use [Editar metadados](edit-metadata.md) para alterar o tipo de dados de colunas ou para alterar o uso da coluna de recurso para numérico, categórica para não categórica e assim por diante.
<!--    + Use [Convert to Dataset](convert-to-dataset.md) to ensure that all included columns use data types that are supported by Azure Machine Learning.  If you cannot convert the columns, consider removing them from the input dataset.
    + Use the [Apply SQL Transformation](apply-sql-transformation.md) or [Execute R Script](execute-r-script.md) modules to cast or convert any columns that cannot be modified using [Edit Metadata](edit-metadata.md). These modules provide more flexibility for working with datetime data types.
    + For numeric data types, you can use the [Apply Math Operation](apply-math-operation.md) module to round or truncate values, or use the [Clip Values](clip-values.md) module to remove out of range values.  -->
 4. Como último recurso, talvez seja necessário modificar o conjunto de dados de entrada original.

> [!TIP]
> Não foi possível encontrar uma resolução que corresponda ao seu cenário? Você pode fornecer comentários sobre este tópico que inclui o nome do módulo que gerou o erro e o tipo de dados e a cardinalidade da coluna. Usaremos as informações para fornecer etapas de solução de problemas mais direcionadas para cenários comuns. 

|Mensagens de Exceção|
|------------------------|
|Não é possível processar a coluna do tipo atual. O tipo não é suportado pelo módulo.|
|Não é possível processar a coluna do tipo {col_type}. O tipo não é suportado pelo módulo.|
|Não é possível processar a coluna "{col_name}" do tipo {col_type}. O tipo não é suportado pelo módulo.|
|Não é possível processar a coluna "{col_name}" do tipo {col_type}. O tipo não é suportado pelo módulo. Nome do parâmetro: {arg_name}.|


## <a name="error-0018"></a>Erro 0018  
 Ocorrerá uma exceção se o conjunto de dados de entrada não for válido.  

**Resolução:** Esse erro no Azure Machine Learning pode aparecer em muitos contextos, portanto, não há uma única resolução. Em geral, o erro indica que os dados fornecidos como entrada para um módulo têm o número incorreto de colunas ou que o tipo de dados não corresponde aos requisitos do módulo. Por exemplo:  

-   O módulo requer uma coluna de rótulo, mas nenhuma coluna é marcada como um rótulo ou você ainda não selecionou uma coluna de rótulo.  
  
-   O módulo requer que os dados sejam categóricos, mas seus dados são numéricos.  

<!---   The module requires a specific data type. For example, ratings provided to [Train Matchbox Recommender](train-matchbox-recommender.md) can be either numeric or categorical, but cannot be floating point numbers.  -->

-   Os dados estão no formato incorreto.  
  
-   Os dados importados contêm caracteres inválidos, valores inválidos ou valores fora do intervalo.  
-   A coluna está vazia ou contém muitos valores ausentes.  

 Para determinar os requisitos e como seus dados podem, examine o tópico da ajuda para o módulo que consumirá o DataSet como entrada.  

 <!--We also recommend that you use [Summarize Data](summarize-data.md) or [Compute Elementary Statistics](compute-elementary-statistics.md) to profile your data, and use these modules to fix metadata and clean values: [Edit Metadata](edit-metadata.md) and [Clean Missing Data](clean-missing-data.md), [Clip Values](clip-values.md)-->.  

|Mensagens de Exceção|
|------------------------|
|O conjunto de dados não é válido.|
|{dataSet1} contém dados inválidos.|
|{dataSet1} e {dataset2} devem ser columnwise consistentes.|
|{dataSet1} contém dados inválidos, {Reason}.|
|{dataSet1} contém {invalid_data_category}. {troubleshoot_hint}|


## <a name="error-0019"></a>Erro 0019  
 Ocorrerá uma exceção se a coluna tiver de conter valores classificados, mas não contiver.  

 Você receberá esse erro em Azure Machine Learning se os valores de coluna especificados estiverem fora de ordem.  

**Resolução:** Classifique os valores de coluna modificando manualmente o conjunto de dados de entrada e execute o módulo novamente.  

|Mensagens de Exceção|
|------------------------|
|Os valores da coluna não estão classificados.|
|Os valores na coluna "{col_index}" não estão classificados.|
|Os valores na coluna "{col_index}" do conjunto de "{DataSet}" não estão classificados.|


## <a name="error-0020"></a>Erro 0020  
 Ocorrerá uma exceção se o número de colunas em alguns dos conjuntos de dados transmitidos para o módulo for muito pequeno.  

 Você receberá esse erro em Azure Machine Learning se não houver colunas suficientes selecionadas para um módulo.  

**Resolução:** Revisite o módulo e verifique se o seletor de coluna tem o número correto de colunas selecionado.  

|Mensagens de Exceção|
|------------------------|
|O número de colunas no conjunto de dados de entrada é menor que o mínimo permitido.|
|O número de colunas no conjunto de dados de entrada "{arg_name}" é menor que o mínimo permitido.|
|O número de colunas no conjunto de dados de entrada é menor que o mínimo permitido de {required_columns_count} coluna (s).|
|O número de colunas no conjunto de dados de entrada "{arg_name}" é menor que o mínimo permitido de {required_columns_count} coluna (s).|


## <a name="error-0021"></a>Erro 0021  
 Ocorrerá uma exceção se o número de linhas em alguns dos conjuntos de dados passados para o módulo for muito pequeno.  

 Esse erro em visto no Azure Machine Learning quando não há linhas suficientes no conjunto de registros para executar a operação especificada. Por exemplo, você poderá ver esse erro se o conjunto de dados de entrada estiver vazio ou se você estiver tentando executar uma operação que exige que um número mínimo de linhas seja válido. Essas operações podem incluir (mas não se limitam a) agrupamento ou classificação com base em métodos estatísticos, determinados tipos de compartimentalização e aprendizado com contagens.  

**Resolução:**

 + Abra o módulo que retornou o erro e verifique as propriedades do conjunto de dados de entrada e do módulo. 
 + Verifique se o conjunto de dados de entrada não está vazio e se há linhas suficientes para atender aos requisitos descritos na ajuda do módulo.  
 + Se os dados forem carregados de uma fonte externa, verifique se a fonte de dados está disponível e se não há erro ou alteração na definição de dados que faria com que o processo de importação obtivesse menos linhas.
 + Se você estiver executando uma operação no upstream de dados do módulo que pode afetar o tipo de dados ou o número de valores, como operações de limpeza, divisão ou junção, verifique as saídas dessas operações para determinar o número de linhas retornadas.  

|Mensagens de Exceção|
|------------------------|
|O número de linhas no conjunto de dados de entrada é menor que o mínimo permitido.|
|O número de linhas no conjunto de dados de entrada é menor que o mínimo permitido de {required_rows_count} linha (s).|
|O número de linhas no conjunto de dados de entrada é menor que o mínimo permitido de {required_rows_count} linha (s). falha|
|O número de linhas no conjunto de dados de entrada "{arg_name}" é menor que o mínimo permitido de {required_rows_count} linha (s).|
|O número de linhas no conjunto de dados de entrada "{arg_name}" é {actual_rows_count}, menor que o mínimo permitido de {required_rows_count} linha (s).|
|O número de linhas "{row_type}" no conjunto de dados de entrada "{arg_name}" é {actual_rows_count}, menor que o mínimo permitido de {required_rows_count} linha (s).|


## <a name="error-0022"></a>Erro 0022  
 Ocorrerá uma exceção se o número de colunas selecionadas no conjunto de dados de entrada não for igual ao número esperado.  

 Esse erro no Azure Machine Learning pode ocorrer quando o módulo downstream ou a operação requer um número específico de colunas ou entradas e você forneceu poucas ou muitas colunas ou entradas. Por exemplo:  

-   Você especifica uma única coluna de rótulo ou coluna de chave e selecionou várias colunas acidentalmente.  
  
-   Você está renomeando colunas, mas forneceu mais ou menos nomes do que as colunas.  
  
-   O número de colunas na origem ou no destino foi alterado ou não corresponde ao número de colunas usadas pelo módulo.  
  
-   Você forneceu uma lista de valores separados por vírgulas para entradas, mas o número de valores não corresponde ou não há suporte para várias entradas.  

**Resolução:** Revisite o módulo e marque a seleção de coluna para garantir que o número correto de colunas esteja selecionado. Verifique as saídas dos módulos upstream e os requisitos das operações downstream.  

 Se você usou uma das opções de seleção de coluna que pode selecionar várias colunas (índices de coluna, todos os recursos, todos os números, etc.), valide o número exato de colunas retornadas pela seleção.  

 <!--If you are trying to specify a comma-separated list of datasets as inputs to [Unpack Zipped Datasets](unpack-zipped-datasets.md), unpack only one dataset at a time. Multiple inputs are not supported.  -->

 Verifique se o número ou o tipo de colunas de upstream não foi alterado.  

 Se você estiver usando um conjunto de uma recomendação para treinar um modelo, lembre-se de que o recomendador espera um número limitado de colunas, que corresponde a pares de item de usuário ou classificações de item de usuário. Remova colunas adicionais antes de treinar o modelo ou dividir conjuntos de valores de recomendação. Para obter mais informações, consulte [dividir dados](split-data.md).  

|Mensagens de Exceção|
|------------------------|
|O número de colunas selecionadas no conjunto de dados de entrada não é igual ao número esperado.|
|O número de colunas selecionadas no conjunto de dados de entrada não é igual a {expected_col_count}.|
|O padrão de seleção de coluna "{selection_pattern_friendly_name}" fornece o número de colunas selecionadas no conjunto de dados de entrada diferente de {expected_col_count}.|
|Espera-se que o padrão de seleção de coluna "{selection_pattern_friendly_name}" forneça {expected_col_count} coluna (s) selecionada no conjunto de dados de entrada, mas {selected_col_count} coluna (s) são, na verdade, fornecidas.|


## <a name="error-0023"></a>Erro 0023  

Ocorrerá uma exceção se a coluna de destino do conjunto de dados de entrada não for válida para o módulo atual do instrutor.  

Esse erro em Azure Machine Learning ocorrerá se a coluna de destino (conforme selecionada nos parâmetros do módulo) não for do tipo de dados válido, contiver todos os valores ausentes ou não for categórica como o esperado.  

**Resolução:** Reveja a entrada do módulo para inspecionar o conteúdo da coluna rótulo/destino. Verifique se ele não tem todos os valores ausentes. Se o módulo estiver esperando que a coluna de destino seja categórica, verifique se há mais de um valor distinto na coluna de destino.  

|Mensagens de Exceção|
|------------------------|
|O conjunto de dados de entrada possui uma coluna de destino sem suporte.|
|O conjunto de dados de entrada tem uma coluna de destino não suportada "{column_index}".|
|O conjunto de dados de entrada tem uma coluna de destino não suportada "{column_index}" para o aprendiz do tipo {learner_type}.|


## <a name="error-0024"></a>Erro 0024  
Ocorrerá uma exceção se o conjunto de dados não contiver uma coluna de rótulo.  

 Esse erro em Azure Machine Learning ocorre quando o módulo requer uma coluna de rótulo e o conjunto de um não tem uma coluna de rótulo. Por exemplo, a avaliação de um conjunto de uma pontuação geralmente requer que uma coluna de rótulo esteja presente para computar métricas de precisão.  

Também pode acontecer que uma coluna de rótulo esteja presente no conjunto de, mas não seja detectada corretamente pelo Azure Machine Learning.

**Resolução:**

+ Abra o módulo que gerou o erro e determine se uma coluna de rótulo está presente. O nome ou tipo de dados da coluna não importa, desde que a coluna contenha um único resultado (ou variável dependente) que você esteja tentando prever. Se você não tiver certeza de qual coluna tem o rótulo, procure um nome genérico, como *classe* ou *destino*. 
+  Se o conjunto de um não incluir uma coluna de rótulo, é possível que a coluna de rótulo tenha sido removida explícita ou acidentalmente upstream. Também pode ser que o conjunto de resultados não seja a saída de um módulo de Pontuação upstream.
+ Para marcar explicitamente a coluna como a coluna de rótulo, adicione o módulo [Editar metadados](edit-metadata.md) e conecte o conjunto de um. Selecione apenas a coluna rótulo e selecione **rótulo** na lista suspensa **campos** . 
+ Se a coluna errada for escolhida como o rótulo, você poderá selecionar **limpar rótulo** nos **campos** para corrigir os metadados na coluna. 
  
|Mensagens de Exceção|
|------------------------|
|Não há nenhuma coluna de rótulo no conjunto de dados.|
|Não há nenhuma coluna de rótulo em "{dataset_name}".|


## <a name="error-0025"></a>Erro 0025  
 Ocorrerá uma exceção se o conjunto de dados não contiver uma coluna de classificação.  

 Esse erro em Azure Machine Learning ocorrerá se a entrada para o modelo de avaliação não contiver colunas de Pontuação válidas. Por exemplo, o usuário tenta avaliar um conjunto de uma antes de ele ter sido pontuado com um modelo treinado correto ou a coluna de pontuação foi explicitamente descartada com upstream. Essa exceção também ocorrerá se as colunas de pontuação nos dois conjuntos de valores forem incompatíveis. Por exemplo, você pode estar tentando comparar a precisão de um regressor linear com um classificador binário.  

**Resolução:** Reveja a entrada para o modelo de avaliação e examine se ela contém uma ou mais colunas de pontuação. Caso contrário, o conjunto de linhas não foi pontuado ou as colunas de Pontuação foram descartadas em um módulo upstream.  

|Mensagens de Exceção|
|------------------------|
|Não há nenhuma coluna de classificação no conjunto de dados.|
|Não há coluna de pontuação em "{dataset_name}".|
|Não há nenhuma coluna de pontuação em "{dataset_name}" produzida por um "{learner_type}". Classificação do conjunto de dados usando o tipo correto de aprendiz.|


## <a name="error-0026"></a>Erro 0026  
 Ocorrerá uma exceção se as colunas com o mesmo nome não forem permitidas.  

 Esse erro em Azure Machine Learning ocorrerá se várias colunas tiverem o mesmo nome. Uma maneira de receber esse erro é se o conjunto de linhas não tiver uma linha de cabeçalho e os nomes de coluna forem atribuídos automaticamente: Col0, Col1, etc.  

**Resolução:** Se as colunas tiverem o mesmo nome, insira um módulo [Editar metadados](edit-metadata.md) entre o conjunto de dados de entrada e o módulo. Use o seletor de coluna em [Editar metadados](edit-metadata.md) para selecionar as colunas a serem renomeadas, digitando os novos nomes na caixa de texto **nomes de nova coluna** .  

|Mensagens de Exceção|
|------------------------|
|Os nomes de colunas iguais são especificados nos argumentos. Não são permitidos nomes de colunas iguais por módulo.|
|Não são permitidos nomes de coluna iguais nos argumentos "{arg_name_1}" e "{arg_name_2}". Especifique nomes diferentes.|


## <a name="error-0027"></a>Erro 0027  
 Ocorrerá uma exceção no caso de dois objetos precisarem ser do mesmo tamanho, mas não forem.  

 Esse é um erro comum no Azure Machine Learning e pode ser causado por muitas condições.  

**Resolução:** Não há nenhuma resolução específica. No entanto, você pode verificar se há condições como as seguintes:  

-   Se você estiver renomeando colunas, certifique-se de que cada lista (as colunas de entrada e a lista de novos nomes) tenha o mesmo número de itens.  
  
-   Se você estiver unindo ou concatenando dois conjuntos de os, verifique se eles têm o mesmo esquema.  
  
-   Se você estiver unindo dois conjuntos de dados que têm várias colunas, verifique se as colunas de chave têm o mesmo tipo de dado e selecione a opção **permitir duplicatas e preservar a ordem das colunas na seleção**.  

|Mensagens de Exceção|
|------------------------|
|O tamanho dos objetos passados está inconsistente.|
|O tamanho de "{friendly_name1}" é inconsistente com o tamanho de "{friendly_name2}".|


## <a name="error-0028"></a>Erro 0028  
 Ocorrerá uma exceção no caso em que o conjunto de colunas contiver nomes de colunas duplicados e isso não for permitido.  

 Esse erro em Azure Machine Learning ocorre quando os nomes de coluna são duplicados; ou seja, não exclusivo.  

**Resolução:** Se alguma coluna tiver o mesmo nome, adicione uma instância de [Editar metadados](edit-metadata.md) entre o conjunto de dados de entrada e o módulo gerando o erro. Use o seletor de coluna em [Editar metadados](edit-metadata.md) para selecionar as colunas a serem renomeadas e digite os nomes das novas colunas na caixa de texto **nomes de nova coluna** . Se você estiver renomeando várias colunas, certifique-se de que os valores digitados nos **novos nomes de coluna** sejam exclusivos.  

|Mensagens de Exceção|
|------------------------|
|O conjunto de colunas contém nomes de colunas duplicados.|
|O nome "{duplicated_name}" está duplicado.|
|O nome "{duplicated_name}" está duplicado em "{arg_name}".|
|O nome "{duplicated_name}" está duplicado. Detalhes: {detalhes}|


## <a name="error-0029"></a>Erro 0029  
 Ocorrerá uma exceção quando um URI inválido for passado.  

 Esse erro em Azure Machine Learning ocorre no caso quando um URI inválido é passado.  Você receberá esse erro se qualquer uma das seguintes condições for verdadeira:  

-   O URI público ou SAS fornecido para o armazenamento de BLOBs do Azure para leitura ou gravação contém um erro.  
  
-   A janela de tempo para a SAS expirou.  
  
-   A URL da Web via origem HTTP representa um arquivo ou um URI de loopback.  
  
-   A URL da Web via HTTP contém uma URL formatada incorretamente.  
  
-   A URL não pode ser resolvida pela origem remota.  

**Resolução:** Revisite o módulo e verifique o formato do URI. Se a fonte de dados for uma URL da Web via HTTP, verifique se a origem pretendida não é um arquivo ou um URI de loopback (localhost).  

|Mensagens de Exceção|
|------------------------|
|Um Uri inválido é passado.|
|O URI "{invalid_url}" é inválido.|


## <a name="error-0030"></a>Erro 0030  
 Ocorrerá uma exceção quando não for possível baixar um arquivo.  

 Essa exceção no Azure Machine Learning ocorre quando não é possível baixar um arquivo. Você receberá essa exceção quando uma tentativa de ler de uma origem HTTP falhar após três (3) tentativas de repetição.  

**Resolução:** Verifique se o URI para a origem HTTP está correto e se o site está acessível no momento pela Internet.  

|Mensagens de Exceção|
|------------------------|
|Não é possível baixar um arquivo.|
|Erro ao baixar o arquivo: {file_url}.|


## <a name="error-0031"></a>Erro 0031  
 Ocorrerá uma exceção se o número de colunas no conjunto de colunas for menor que o necessário.  

 Esse erro em Azure Machine Learning ocorrerá se o número de colunas selecionado for menor que o necessário.  Você receberá esse erro se o número mínimo necessário de colunas não estiver selecionado.  

**Resolução:** Adicione colunas adicionais à seleção de coluna usando o **seletor de coluna**.  

|Mensagens de Exceção|
|------------------------|
|O número de colunas no conjunto de colunas é menor que o necessário.|
|Pelo menos {required_columns_count} coluna (s) deve (m) ser especificada para o argumento de entrada "{arg_name}".|
|Pelo menos {required_columns_count} coluna (s) deve (m) ser especificada para o argumento de entrada "{arg_name}". O número real de colunas especificadas é {input_columns_count}.|


## <a name="error-0032"></a>Erro 0032  
 Ocorrerá uma exceção se o argumento não for um número.  

 Você receberá esse erro em Azure Machine Learning se o argumento for double ou NaN.  

**Resolução:** Modifique o argumento especificado para usar um valor válido.  

|Mensagens de Exceção|
|------------------------|
|O argumento não é um número.|
|"{arg_name}" não é um número.|


## <a name="error-0033"></a>Erro 0033  
 Ocorrerá uma exceção se o argumento for Infinito.  

 Esse erro em Azure Machine Learning ocorrerá se o argumento for infinito. Você receberá esse erro se o argumento for `double.NegativeInfinity` ou `double.PositiveInfinity`.  

**Resolução:** Modifique o argumento especificado para que seja um valor válido.  

|Mensagens de Exceção|
|------------------------|
|O argumento deve ser finito.|
|"{arg_name}" não é finito.|


## <a name="error-0034"></a>Erro 0034  
 Ocorrerá uma exceção se houver mais de uma classificação para um determinado par de usuário-item.  

 Esse erro em Azure Machine Learning ocorrerá na recomendação se um par usuário-item tiver mais de um valor de classificação.  

**Resolução:** Verifique se o par usuário-item possui apenas um valor de classificação.  

|Mensagens de Exceção|
|------------------------|
|Há mais de uma classificação para o (s) valor (es) no conjunto de informações.|
|Mais de uma classificação para o usuário {User} e item {item} na tabela de dados de previsão de classificação.|
|Mais de uma classificação para o usuário {User} e item {item} no {DataSet}.|


## <a name="error-0035"></a>Erro 0035  
 Ocorrerá uma exceção se nenhum recurso for fornecido por um determinado usuário ou item.  

 Esse erro em Azure Machine Learning ocorre que você está tentando usar um modelo de recomendação para pontuação, mas um vetor de recurso não pode ser encontrado.  

**Resolução:**

O recomendador Matchbox tem determinados requisitos que devem ser atendidos ao usar recursos de item ou recursos de usuário.  Esse erro indica que um vetor de recurso está ausente para um usuário ou item que você forneceu como entrada. Verifique se um vetor de recursos está disponível nos dados para cada usuário ou item.  

 Por exemplo, se você treinasse um modelo de recomendação usando recursos como a idade, o local ou a renda do usuário, mas agora deseja criar pontuações para novos usuários que não foram vistos durante o treinamento, você deve fornecer algum conjunto equivalente de recursos (ou seja, idade, local e valores de renda) para os novos usuários a fim de fazer previsões apropriadas para eles. 

 Se você não tiver nenhum recurso para esses usuários, considere a engenharia de recursos para gerar os recursos apropriados.  Por exemplo, se você não tiver valores de idade de usuário ou de renda individuais, poderá gerar valores aproximados a serem usados para um grupo de usuários. 

<!--When you are scoring from a recommendation mode, you can use item or user features only if you previously used item or user features during training. For more information, see [Score Matchbox Recommender](score-matchbox-recommender.md).

For general information about how the Matchbox recommendation algorithm works, and how to prepare a dataset of item features or user features, see [Train Matchbox Recommender](train-matchbox-recommender.md).  -->

 > [!TIP]
 > A resolução não se aplica ao seu caso? Você é bem-vindo a enviar comentários sobre este artigo e fornecer informações sobre o cenário, incluindo o módulo e o número de linhas na coluna. Usaremos essas informações para fornecer etapas de solução de problemas mais detalhadas no futuro.

|Mensagens de Exceção|
|------------------------|
|Nenhum recurso foi fornecido para um item ou usuário necessário.|
|Recursos para {required_feature_name} necessários, mas não fornecidos.|


## <a name="error-0036"></a>Erro 0036  
 Ocorrerá uma exceção se vários vetores do recurso forem fornecidos para um determinado usuário ou item.  

 Esse erro em Azure Machine Learning ocorrerá se um vetor de recurso for definido mais de uma vez.  

**Resolução:** Verifique se o vetor de recurso não está definido mais de uma vez.  

|Mensagens de Exceção|
|------------------------|
|Definição de recurso duplicada para um usuário ou item.|


## <a name="error-0037"></a>Erro 0037  
 Ocorrerá uma exceção se várias colunas de rótulo forem especificadas e apenas uma for permitida.  

 Esse erro em Azure Machine Learning ocorrerá se mais de uma coluna estiver selecionada para ser a nova coluna de rótulo. A maioria dos algoritmos de aprendizado supervisionados exige que uma única coluna seja marcada como o destino ou o rótulo.  

**Resolução:** Certifique-se de selecionar uma única coluna como a nova coluna de rótulo.  

|Mensagens de Exceção|
|------------------------|
|Várias colunas de rótulo estão especificadas.|
|Várias colunas de rótulo são especificadas em "{dataset_name}".|


## <a name="error-0039"></a>Erro 0039  
 Ocorrerá uma exceção se uma operação tiver falhado.  

 Esse erro em Azure Machine Learning ocorre quando uma operação interna não pode ser concluída.  

**Resolução:** Esse erro é causado por muitas condições e não há uma solução específica.  
 A tabela a seguir contém mensagens genéricas para esse erro, que são seguidas por uma descrição específica da condição. 

 Se não houver detalhes disponíveis, [envie comentários](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) e forneça informações sobre os módulos que geraram o erro e as condições relacionadas.

|Mensagens de Exceção|
|------------------------|
|Falha na operação.|
|Erro ao concluir a operação: "{failed_operation}".|
|Erro ao concluir a operação: "{failed_operation}". Motivo: "{Reason}".|


## <a name="error-0042"></a>Erro 0042  
 Ocorrerá uma exceção quando não for possível converter a coluna em outro tipo.  

 Esse erro no Azure Machine Learning ocorre quando não é possível converter a coluna para o tipo especificado.  Você receberá esse erro se um módulo exigir um tipo de dados específico, como DateTime, Text, um número de ponto flutuante ou inteiro, mas não for possível converter uma coluna existente para o tipo necessário.  

Por exemplo, você pode selecionar uma coluna e tentar convertê-la em um tipo de dados numérico para uso em uma operação matemática e obter esse erro se a coluna contiver dados inválidos. 

Outro motivo para você receber esse erro se tentar usar uma coluna que contém números de ponto flutuante ou muitos valores exclusivos como uma coluna categórica. 

**Resolução:**

+ Abra a página de ajuda do módulo que gerou o erro e verifique os requisitos de tipo de dados.
+ Examine os tipos de dados das colunas no conjunto de dado de entrada.
+ Inspecione os dados originados em chamadas de fontes de dados sem esquema.
+ Verifique se há valores ausentes ou caracteres especiais no conjunto de dados que possam bloquear a conversão para o tipo de dado desejado. 
    + Os tipos de dados numéricos devem ser consistentes: por exemplo, verifique se há números de ponto flutuante em uma coluna de inteiros.
    + Procure cadeias de caracteres de texto ou valores na em uma coluna de número. 
    + Valores Boolianos podem ser convertidos em uma representação apropriada dependendo do tipo de dados necessário.
    + Examinar colunas de texto para caracteres não Unicode, caracteres de tabulação ou caracteres de controle
    + Os dados de data e hora devem ser consistentes para evitar erros de modelagem, mas a limpeza pode ser devido complexa para os vários formatos. Considere usar <!--the [Execute R Script](execute-r-script.md) or -->Execute os módulos de [script Python](execute-python-script.md) para executar a limpeza.  
+ Se necessário, modifique os valores no conjunto de dados de entrada para que a coluna possa ser convertida com êxito. A modificação pode incluir compartimentalização, truncamento ou operações de arredondamento, eliminação de exceções ou imputação de valores ausentes. Consulte os seguintes artigos para ver alguns cenários comuns de transformação de dados no aprendizado de máquina:
    + [Limpar dados ausentes](clean-missing-data.md)
    + [Normalizar dados](normalize-data.md)
<!--+ [Clip Values](clip-values.md) 
    + [Group Data Into Bins](group-data-into-bins.md)
  -->

> [!TIP]
> Resolução desclarada ou não aplicável ao seu caso? Você é bem-vindo a enviar comentários sobre este artigo e fornecer informações sobre o cenário, incluindo o módulo e o tipo de dados da coluna. Usaremos essas informações para fornecer etapas de solução de problemas mais detalhadas no futuro.  

|Mensagens de Exceção|
|------------------------|
|Conversão não permitida.|
|Não foi possível converter a coluna do tipo {type1} para a coluna do tipo {type2}.|
|Não foi possível converter a coluna "{col_name1}" do tipo {type1} para a coluna do tipo {type2}.|
|Não foi possível converter a coluna "{col_name1}" do tipo {type1} para a coluna "{col_name2}" do tipo {type2}.|


## <a name="error-0044"></a>Erro 0044  
 Ocorrerá uma exceção quando não for possível derivar o tipo de elemento da coluna a partir dos valores existentes.  

 Esse erro em Azure Machine Learning ocorre quando não é possível inferir o tipo de uma coluna ou colunas em um DataSet. Isso normalmente acontece ao concatenar dois ou mais conjuntos de valores com diferentes tipos de elementos. Se Azure Machine Learning não puder determinar um tipo comum que possa representar todos os valores em uma coluna ou colunas sem perda de informações, ele gerará esse erro.  

**Resolução:** Verifique se todos os valores em uma determinada coluna em ambos os conjuntos de dados que estão sendo combinados são do mesmo tipo (Numeric, Boolean, categórico, String, Date, etc.) ou podem ser forçados para o mesmo tipo.  

|Mensagens de Exceção|
|------------------------|
|Não é possível derivar o tipo de elemento da coluna.|
|Não é possível derivar o tipo de elemento para a coluna "{column_name}"--todos os elementos são referências nulas.|
|Não é possível derivar o tipo de elemento para a coluna "{column_name}" do conjunto de valores "{dataset_name}"--todos os elementos são referências nulas.|


## <a name="error-0045"></a>Erro 0045  
 Ocorrerá uma exceção quando não for possível criar uma coluna devido aos tipos de elemento mistos na origem.  

 Esse erro no Azure Machine Learning é produzido quando os tipos de elementos de dois conjuntos de valores que estão sendo combinados são diferentes.  

**Resolução:** Verifique se todos os valores em uma determinada coluna em ambos os conjuntos de dados que estão sendo combinados são do mesmo tipo (Numeric, Boolean, categórico, String, Date, etc.).  

|Mensagens de Exceção|
|------------------------|
|Não for possível criar colunas com tipos de elemento mistos.|
|Não é possível criar a coluna com a ID "{column_id}" de tipos de elementos mistos:<br />O tipo de dados [{row_1}, {column_id}] é "{type_1}". <br />O tipo de dados [{row_2}, {column_id}] é "{type_2}".|
|Não é possível criar a coluna com a ID "{column_id}" de tipos de elementos mistos:<br />O tipo na parte {chunk_id_1} é "{type_1}". <br />O tipo na parte {chunk_id_2} é "{type_2}" com o tamanho da parte: {chunk_size}.|


## <a name="error-0046"></a>Erro 0046  
 Ocorrerá uma exceção quando não for possível criar um diretório no caminho especificado.  

 Esse erro no Azure Machine Learning ocorre quando não é possível criar um diretório no caminho especificado. Você receberá esse erro se qualquer parte do caminho para o diretório de saída de uma consulta de Hive estiver incorreta ou inacessível.  

**Resolução:** Revisite o módulo e verifique se o caminho do diretório está formatado corretamente e se está acessível com as credenciais atuais.  

|Mensagens de Exceção|
|------------------------|
|Especifique um diretório de saída válido.|
|Diretório: {path} não pode ser criado. Especifique um caminho válido.|


## <a name="error-0047"></a>Erro 0047  
 Ocorrerá uma exceção se o número de colunas de recursos em alguns dos conjuntos de dados transmitidos para o módulo for muito pequeno.  

 Esse erro em Azure Machine Learning ocorrerá se o conjunto de dados de entrada para treinamento não contiver o número mínimo de colunas exigidas pelo algoritmo. Normalmente, o conjunto de linhas está vazio ou contém apenas colunas de treinamento.  

**Resolução:** Reveja o conjunto de dados de entrada para ter certeza de que há uma ou mais colunas adicionais separadas da coluna de rótulo.  

|Mensagens de Exceção|
|------------------------|
|O número de colunas de recursos no conjunto de dados de entrada é menor que o mínimo permitido.|
|O número de colunas de recurso no conjunto de dados de entrada é menor que o mínimo permitido de {required_columns_count} coluna (s).|
|O número de colunas de recursos no conjunto de dados de entrada "{arg_name}" é menor que o mínimo permitido de {required_columns_count} coluna (s).|


## <a name="error-0048"></a>Erro 0048  
 Ocorrerá uma exceção quando não for possível abrir um arquivo.  

 Esse erro no Azure Machine Learning ocorre quando não é possível abrir um arquivo para leitura ou gravação. Você pode receber esse erro por estes motivos:  

-   O contêiner ou o arquivo (BLOB) não existe  
  
-   O nível de acesso do arquivo ou contêiner não permite que você acesse o arquivo  
  
-   O arquivo é muito grande para ler ou para o formato incorreto  

**Resolução:** Revisite o módulo e o arquivo que você está tentando ler.  

 Verifique se os nomes do contêiner e do arquivo estão corretos.  

 Use o portal clássico do Azure ou uma ferramenta de armazenamento do Azure para verificar se você tem permissão para acessar o arquivo.  

  <!--If you are trying to read an image file, make sure that it meets the requirements for image files in terms of size, number of pixels, and so forth. For more information, see [Import Images](import-images.md).  -->

|Mensagens de Exceção|
|------------------------|
|Não é possível abrir um arquivo.|
|Erro ao abrir o arquivo: {file_name}.|
|Erro ao abrir o arquivo: {file_name}. Mensagem de exceção de armazenamento: {Exception}.|


## <a name="error-0049"></a>Erro 0049  
 Ocorrerá uma exceção quando não for possível analisar um arquivo.  

 Esse erro no Azure Machine Learning ocorre quando não é possível analisar um arquivo. Você receberá esse erro se o formato de arquivo selecionado no módulo [importar dados](import-data.md) não corresponder ao formato real do arquivo ou se o arquivo contiver um caractere irreconhecível.  

**Resolução:** Revisite o módulo e corrija a seleção de formato de arquivo se ele não corresponder ao formato do arquivo. Se possível, inspecione o arquivo para confirmar se ele não contém nenhum caractere ilegal.  

|Mensagens de Exceção|
|------------------------|
|Não é possível analisar um arquivo.|
|Erro ao analisar o arquivo {file_format}.|
|Erro ao analisar o arquivo {file_format}: {file_name}.|
|Erro ao analisar o arquivo {file_format}. Motivo: {failure_reason}.|
|Erro ao analisar o arquivo {file_format}: {file_name}. Motivo: {failure_reason}.|


## <a name="error-0052"></a>Erro 0052  
 Ocorrerá uma exceção se a chave da conta de armazenamento do Azure for especificada incorretamente.  

 Esse erro em Azure Machine Learning ocorrerá se a chave usada para acessar a conta de armazenamento do Azure estiver incorreta. Por exemplo, você poderá ver esse erro se a chave de armazenamento do Azure estava truncada quando copiada e colada, ou se a chave errada foi usada.  

 Para obter mais informações sobre como obter a chave para uma conta de armazenamento do Azure, consulte [Exibir, copiar e regenerar chaves de acesso de armazenamento](https://azure.microsoft.com/documentation/articles/storage-create-storage-account-classic-portal/).  

**Resolução:** Revisite o módulo e verifique se a chave de armazenamento do Azure está correta para a conta; Copie a chave novamente no portal clássico do Azure, se necessário.  

|Mensagens de Exceção|
|------------------------|
|A chave da conta de armazenamento do Azure está incorreta.|


## <a name="error-0053"></a>Erro 0053  
 Ocorrerá uma exceção no caso de não haver recursos de usuário ou itens para recomendações de Matchbox.  

 Esse erro no Azure Machine Learning é produzido quando um vetor de recurso não pode ser encontrado.  

**Resolução:** Verifique se um vetor de recurso está presente no conjunto de dados de entrada.  

|Mensagens de Exceção|
|------------------------|
|Os recursos e/ou itens do usuário são necessários, mas não foram fornecidos.|


## <a name="error-0056"></a>Erro 0056  
 Ocorrerá uma exceção se as colunas selecionadas para uma operação violarem os requisitos.  

 Esse erro em Azure Machine Learning ocorre quando você está escolhendo colunas para uma operação que requer que a coluna seja de um tipo de dados específico. 

 Esse erro também pode ocorrer se a coluna for do tipo de dados correto, mas o módulo que você está usando exigir que a coluna também seja marcada como um recurso, rótulo ou coluna categórica.  

  <!--For example, the [Convert to Indicator Values](convert-to-indicator-values.md) module requires that columns be categorical, and will raise this error if you select a feature column or label column.  -->

**Resolução:**

1.  Examine o tipo de dados das colunas que estão selecionadas no momento. 

2. Verificar se as colunas selecionadas são categóricos, rótulo ou colunas de recurso.  
  
3.  Examine o tópico da ajuda para o módulo no qual você fez a seleção de coluna, para determinar se há requisitos específicos para o tipo de dados ou o uso da coluna.  
  
3.  Use [Editar metadados](edit-metadata.md) para alterar o tipo de coluna durante essa operação. Certifique-se de alterar o tipo de coluna de volta para seu valor original, usando outra instância de [Editar metadados](edit-metadata.md), se necessário, para operações de downstream.  

|Mensagens de Exceção|
|------------------------|
|Uma ou mais colunas selecionadas não estavam em uma categoria permitida.|
|A coluna com o nome "{col_name}" não está em uma categoria permitida.|


## <a name="error-0057"></a>Erro 0057  
 Ocorrerá uma exceção ao tentar criar um arquivo ou um blob que já existe.  

 Essa exceção ocorre quando você está usando o módulo [exportar dados](export-data.md) ou outro módulo para salvar os resultados de um pipeline em Azure Machine Learning no armazenamento de BLOBs do Azure, mas tenta criar um arquivo ou BLOB que já existe.   

**Resolução:**

 Você receberá esse erro somente se você tiver definido anteriormente a propriedade **modo de gravação do armazenamento de BLOBs do Azure** para **erro**. Por design, esse módulo gera um erro se você tentar gravar um conjunto de uma em um blob que já existe.

 - Abra as propriedades do módulo e altere a propriedade **modo de gravação do armazenamento de BLOBs do Azure** para **substituir**.
 - Como alternativa, você pode digitar o nome de um BLOB ou arquivo de destino diferente e especificar um blob que ainda não existe.  

|Mensagens de Exceção|
|------------------------|
|O arquivo ou blob já existe.|
|O arquivo ou BLOB "{file_path}" já existe.|


## <a name="error-0058"></a>Erro 0058  
 Esse erro em Azure Machine Learning ocorrerá se o conjunto de um não contiver a coluna de rótulo esperada.  

 Essa exceção também pode ocorrer quando a coluna de rótulo fornecida não corresponde aos dados ou DataType esperados pelo aprendiz ou tem os valores errados. Por exemplo, essa exceção é produzida ao usar uma coluna de rótulo de valor real ao treinar um classificador binário.  

**Resolução:** A resolução depende do aprendiz ou do treinador que você está usando, e os tipos de dados das colunas no seu DataSet. Primeiro, verifique os requisitos do módulo de treinamento ou de algoritmo do Machine Learning.  

 Revisite o conjunto de dados de entrada. Verifique se a coluna que você espera que seja tratada como o rótulo tem o tipo de dados correto para o modelo que você está criando.  

 Verificar as entradas de valores ausentes e eliminá-los ou substituí-los, se necessário.  

 Se necessário, adicione o módulo [Editar metadados](edit-metadata.md) e verifique se a coluna rótulo está marcada como um rótulo.  

|Mensagens de Exceção|
|------------------------|
|Os valores da coluna de rótulo e os valores de coluna de rótulo pontuados não são comparáveis.|
|A coluna de rótulo não é como esperado em "{dataset_name}".|
|A coluna de rótulo não é como esperado em "{dataset_name}", {Reason}.|
|A coluna de rótulo "{column_name}" não é esperada em "{dataset_name}".|
|A coluna de rótulo "{column_name}" não é esperada em "{dataset_name}", {Reason}.|


## <a name="error-0059"></a>Erro 0059  
 Ocorrerá uma exceção se um índice de coluna especificado em um seletor de colunas não puder ser analisado.  

 Esse erro em Azure Machine Learning ocorrerá se um índice de coluna especificado ao usar o seletor de coluna não puder ser analisado.  Você receberá esse erro quando o índice de coluna estiver em um formato inválido que não pode ser analisado.  

**Resolução:** Modifique o índice de coluna para usar um valor de índice válido.  

|Mensagens de Exceção|
|------------------------|
|Um ou mais índices de coluna especificados ou intervalos de índice não puderam ser analisados.|
|Não foi possível analisar o índice de coluna ou o intervalo "{column_index_or_range}".|


## <a name="error-0060"></a>Erro 0060  
 Ocorrerá uma exceção quando um intervalo de coluna fora do intervalo for especificado em um seletor de colunas.  

 Esse erro em Azure Machine Learning ocorre quando um intervalo de coluna fora do intervalo é especificado no seletor de coluna. Você receberá esse erro se o intervalo de coluna no seletor de coluna não corresponder às colunas no conjunto de linhas.  

**Resolução:** Modifique o intervalo de coluna no seletor de coluna para corresponder às colunas no conjunto de linhas.  

|Mensagens de Exceção|
|------------------------|
|Intervalo de índice de coluna inválido ou fora do intervalo especificado.|
|O intervalo de colunas "{column_range}" é inválido ou está fora do intervalo.|


## <a name="error-0061"></a>Erro 0061  
 Ocorrerá uma exceção ao tentar adicionar uma linha a uma DataTable que tenha um número diferente de colunas da tabela.  

 Esse erro em Azure Machine Learning ocorre quando você tenta adicionar uma linha a um conjunto de um DataSet que tem um número diferente de colunas do que o DataSet.  Você receberá esse erro se a linha que está sendo adicionada ao conjunto de dados tiver um número diferente de colunas do conjunto de dados de entrada.  A linha não poderá ser acrescentada ao conjunto de linhas se o número de colunas for diferente.  

**Resolução:** Modifique o conjunto de dados de entrada para ter o mesmo número de colunas que a linha adicionada, ou modifique a linha adicionada para ter o mesmo número de colunas que o conjunto de dados.  

|Mensagens de Exceção|
|------------------------|
|Todas as tabelas devem ter o mesmo número de colunas.|
|As colunas na parte "{chunk_id_1}" são diferentes com a parte "{chunk_id_2}" com o tamanho da parte: {chunk_size}.|
|A contagem de colunas no arquivo "{filename_1}" (Count = {column_count_1}) é diferente com o arquivo "{filename_2}" (Count = {column_count_2}).|


## <a name="error-0062"></a>Erro 0062  
 Ocorrerá uma exceção durante a tentativa de comparar dois modelos com tipos diferentes de aprendiz.  

 Esse erro no Azure Machine Learning é produzido quando as métricas de avaliação para dois conjuntos de valores de DataSet diferentes não podem ser comparadas. Nesse caso, não é possível comparar a eficácia dos modelos usados para produzir os dois conjuntos de valores classificados.  

**Resolução:** Verifique se os resultados pontuados são produzidos pelo mesmo tipo de modelo de aprendizado de máquina (classificação binária, regressão, classificação de várias classes, recomendação, clustering, detecção de anomalias, etc.) Todos os modelos que você compara devem ter o mesmo tipo de aprendiz.  

|Mensagens de Exceção|
|------------------------|
|Todos os modelos devem ter o mesmo tipo de aprendiz.|
|Tipo de aprendiz incompatível obtido: "{actual_learner_type}". Os tipos de aprendizes esperados são: "{expected_learner_type_list}".|


## <a name="error-0064"></a>Erro 0064  
 Ocorrerá uma exceção se o nome da conta de armazenamento do Azure ou a chave de armazenamento for especificado incorretamente.  

 Esse erro em Azure Machine Learning ocorrerá se o nome da conta de armazenamento do Azure ou a chave de armazenamento for especificada incorretamente. Você receberá esse erro se inserir um nome de conta ou senha incorretos para a conta de armazenamento. Isso pode ocorrer se você inserir manualmente o nome da conta ou a senha. Isso também pode ocorrer se a conta tiver sido excluída.  

**Resolução:** Verifique se o nome da conta e a senha foram inseridos corretamente e se a conta existe.  

|Mensagens de Exceção|
|------------------------|
|O nome da conta de armazenamento ou a chave de armazenamento do Azure está incorreto.|
|O nome da conta de armazenamento do Azure "{account_name}" ou a chave de armazenamento para o nome da conta está incorreto.|


## <a name="error-0065"></a>Erro 0065  
 Ocorrerá uma exceção se o nome do blob do Azure estiver especificado incorretamente.  

 Esse erro em Azure Machine Learning ocorrerá se o nome do blob do Azure for especificado incorretamente.  Você receberá o erro se:  

-   O BLOB não pode ser encontrado no contêiner especificado.  

 <!---   The fully qualified name of the blob specified for output in one of the [Learning with Counts](data-transformation-learning-with-counts.md) modules is greater than 512 characters.  -->

-   Somente o contêiner foi especificado como a origem em uma solicitação de [importação de dados](import-data.md) quando o formato era Excel ou CSV com codificação; a concatenação do conteúdo de todos os BLOBs dentro de um contêiner não é permitida com esses formatos.  
  
-   Um URI SAS não contém o nome de um blob válido.  

**Resolução:** Revisite o módulo lançando a exceção. Verifique se o blob especificado existe no contêiner na conta de armazenamento e se as permissões permitem que você veja o blob. Verifique se a entrada está no formato **ContainerName/filename** se você tiver o Excel ou CSV com formatos de codificação. Verifique se um URI de SAS contém o nome de um blob válido.  

|Mensagens de Exceção|
|------------------------|
|O nome do blob de armazenamento do Azure está incorreto.|
|O nome do blob de armazenamento do Azure "{blob_name}" está incorreto.|
|O nome do blob de armazenamento do Azure com o prefixo "{blob_name_prefix}" não existe.|
|Falha ao localizar blobs de armazenamento do Azure no contêiner "{container_name}".|
|Falha ao localizar BLOBs do armazenamento do Azure com o caminho curinga "{blob_wildcard_path}".|


## <a name="error-0066"></a>Erro 0066  
 Ocorrerá uma exceção se um recurso não puder ser carregado para um blob do Azure.  

 Esse erro em Azure Machine Learning ocorrerá se um recurso não pôde ser carregado em um blob do Azure.  <!--You will receive this message if [Train Vowpal Wabbit 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) encounters an error attempting to save either the model or the hash created when training the model.--> Ambos são salvos na mesma conta de armazenamento do Azure que a conta que contém o arquivo de entrada.  

**Resolução:** Revisite o módulo. Verifique se o nome da conta do Azure, a chave de armazenamento e o contêiner estão corretos e se a conta tem permissão para gravar no contêiner.  

|Mensagens de Exceção|
|------------------------|
|Não foi possível carregar o recurso no armazenamento do Azure.|
|Não foi possível carregar o arquivo "{source_path}" no armazenamento do Azure como "{dest_path}".|


## <a name="error-0067"></a>Erro 0067  
 Ocorrerá uma exceção se um conjunto de dados tiver um número diferente de colunas que o esperado.  

 Esse erro em Azure Machine Learning ocorrerá se um conjunto de um DataSet tiver um número diferente de colunas do que o esperado.  Você receberá esse erro quando o número de colunas no conjunto de linhas for diferente do número de colunas que o módulo espera durante a execução.  

**Resolução:** Modifique o conjunto de dados de entrada ou os parâmetros.  

|Mensagens de Exceção|
|------------------------|
|Número de colunas na tabela de dados inesperado.|
|Número inesperado de colunas no conjunto de um "{dataset_name}".|
|Esperava-se colunas "{expected_column_count}", mas foram encontradas "{actual_column_count}" coluna (s) em vez disso.|
|No conjunto de dados de entrada "{dataset_name}", era esperado "{expected_column_count}" coluna (s), mas foram localizadas "{actual_column_count}" coluna (s) em vez disso.|


## <a name="error-0068"></a>Erro 0068  
 Ocorrerá uma exceção se o script de Hive especificado não estiver correto.  

 Esse erro em Azure Machine Learning ocorrerá se houver erros de sintaxe em um script de QL do hive ou se o intérprete do hive encontrar um erro ao executar a consulta ou o script.  

**Resolução:**

A mensagem de erro do hive normalmente é relatada de volta no log de erros para que você possa tomar medidas com base no erro específico. 

+ Abra o módulo e inspecione a consulta para obter erros.  
+ Verifique se a consulta funciona corretamente fora do Azure Machine Learning fazendo logon no console do hive do seu cluster Hadoop e executando a consulta.  
+ Tente inserir comentários no script do hive em uma linha separada, em vez de misturar instruções e comentários executáveis em uma única linha.  

### <a name="resources"></a>Recursos

Consulte os artigos a seguir para obter ajuda com consultas de Hive para aprendizado de máquina:

+ [Criar tabelas do hive e carregar dados do armazenamento de BLOBs do Azure](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-move-hive-tables)
+ [Explorar dados em tabelas com consultas de Hive](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-explore-data-hive-tables)
+ [Criar recursos de dados em um cluster Hadoop usando as consultas do Hive](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-create-features-hive)
+ [Folha de consulta de usuários do hive para SQL (PDF)](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf)

  
|Mensagens de Exceção|
|------------------------|
|O script de hive está incorreto.|


## <a name="error-0069"></a>Erro 0069  
 Ocorrerá uma exceção se o script SQL especificado não estiver correto.  

 Esse erro em Azure Machine Learning ocorrerá se o script SQL especificado tiver problemas de sintaxe ou se as colunas ou a tabela especificada no script não forem válidas. 

 Você receberá esse erro se o mecanismo do SQL encontrar algum erro ao executar a consulta ou o script. A mensagem de erro do SQL normalmente é relatada de volta no log de erros para que você possa tomar medidas com base no erro específico.  

**Resolução:** Revisite o módulo e inspecione a consulta SQL para obter erros.  

 Verifique se a consulta funciona corretamente fora do Azure ML fazendo logon no servidor de banco de dados diretamente e executando a consulta.  

 Se houver uma mensagem gerada pelo SQL relatada pela exceção do módulo, execute uma ação com base no erro relatado. Por exemplo, as mensagens de erro às vezes incluem diretrizes específicas sobre o erro provável:
+ *Nenhuma coluna ou banco de dados ausente*, indicando que você pode ter digitado um nome de coluna errado. Se você tiver certeza de que o nome da coluna está correto, tente usar colchetes ou aspas para colocar o identificador de coluna.
+ *Erro lógico do SQL próximo \<palavra-chave sql\>* , indicando que você pode ter um erro de sintaxe antes da palavra-chave especificada

  
|Mensagens de Exceção|
|------------------------|
|O script SQL está incorreto.|
|A consulta SQL "{sql_query}" não está correta.|
|A consulta SQL "{sql_query}" não está correta. Mensagem de exceção: {Exception}.|


## <a name="error-0070"></a>Erro 0070  
 Ocorrerá uma exceção ao tentar acessar a tabela do Azure inexistente.  

 Esse erro em Azure Machine Learning ocorre quando você tenta acessar uma tabela não existente do Azure. Você receberá esse erro se especificar uma tabela no armazenamento do Azure, que não existe durante a leitura ou gravação no armazenamento de tabelas do Azure. Isso pode acontecer se você digitar incorretamente o nome da tabela desejada ou se houver uma incompatibilidade entre o nome de destino e o tipo de armazenamento. Por exemplo, você pretende ler em uma tabela, mas inseriu o nome de um blob.  

**Resolução:** Revisite o módulo para verificar se o nome da tabela está correto.  

|Mensagens de Exceção|
|------------------------|
|Não existe tabela do Azure.|
|A tabela do Azure "{table_name}" não existe.|


## <a name="error-0072"></a>Erro 0072  
 Ocorrerá uma exceção no caso de tempo limite da conexão.  

 Esse erro no Azure Machine Learning ocorre quando uma conexão atinge o tempo limite. Você receberá esse erro se houver problemas de conectividade com a fonte de dados ou destino, como conectividade lenta com a Internet, ou se o DataSet for grande e/ou se a consulta SQL a ser lida nos dados executar um processamento complicado.  

**Resolução:** Determine se há problemas no momento com conexões lentas com o armazenamento do Azure ou com a Internet.  

|Mensagens de Exceção|
|------------------------|
|O tempo limite da conexão ocorreu.|


## <a name="error-0073"></a>Erro 0073  
 Ocorrerá uma exceção se ocorrer um erro durante a conversão de uma coluna em outro tipo.  

 Esse erro no Azure Machine Learning ocorre quando não é possível converter a coluna em outro tipo.  Você receberá esse erro se um módulo exigir um tipo específico e não for possível converter a coluna para o novo tipo.  

**Resolução:** Modifique o conjunto de dados de entrada para que a coluna possa ser convertida com base na exceção interna.  

|Mensagens de Exceção|
|------------------------|
|Falha ao converter a coluna.|
|Falha ao converter a coluna em {target_type}.|


## <a name="error-0075"></a>Erro 0075  
Ocorrerá uma exceção quando uma função de agrupamento inválida for usada ao quantizar um conjunto de dados.  

Esse erro em Azure Machine Learning ocorre quando você está tentando desagrupar dados usando um método sem suporte ou quando as combinações de parâmetros são inválidas.  

**Resolução:**

O tratamento de erros para esse evento foi introduzido em uma versão anterior do Azure Machine Learning que permitia mais personalização dos métodos compartimentalização. Atualmente, todos os métodos compartimentalização são baseados em uma seleção de uma lista suspensa, portanto, tecnicamente, não deve mais ser possível obter esse erro.

 <!--If you get this error when using the [Group Data into Bins](group-data-into-bins.md) module, consider reporting the issue in the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MachineLearning), providing the data types, parameter settings, and the exact error message.  -->

|Mensagens de Exceção|
|------------------------|
|Função de agrupamento inválido usada.|


## <a name="error-0077"></a>Erro 0077  
 Ocorrerá uma exceção quando o modo de gravação de arquivo de blob desconhecido for passado.  

 Esse erro em Azure Machine Learning ocorrerá se um argumento inválido for passado nas especificações de um destino de arquivo de BLOB ou origem.  

**Resolução:** Em quase todos os módulos que importam ou exportam dados de e para o armazenamento de BLOBs do Azure, os valores de parâmetros que controlam o modo de gravação são atribuídos usando uma lista suspensa; Portanto, não é possível passar um valor inválido, e esse erro não deve aparecer. Esse erro será preterido em uma versão posterior.  

|Mensagens de Exceção|
|------------------------|
|Modo de gravação de blob sem suporte.|
|Modo de gravação de blob sem suporte: {blob_write_mode}.|


## <a name="error-0078"></a>Erro 0078  
 Ocorrerá uma exceção quando a opção HTTP para [importar dados](import-data.md) receber um código de status 3xx indicando redirecionamento.  

 Esse erro em Azure Machine Learning ocorre quando a opção HTTP para [importar dados](import-data.md) recebe um código de status 3xx (301, 302, 304, etc.) indicando o redirecionamento. Você receberá esse erro se tentar se conectar a uma origem HTTP que redireciona o navegador para outra página. Por motivos de segurança, o redirecionamento de sites não é permitido como fontes de dados para Azure Machine Learning.  

**Resolução:** Se o site for um site confiável, insira a URL redirecionada diretamente.  

|Mensagens de Exceção|
|------------------------|
|Redirecionamento de http não permitido.|


## <a name="error-0079"></a>Erro 0079  
 Ocorrerá uma exceção se o nome do contêiner de armazenamento do Azure estiver especificado incorretamente.  

 Esse erro em Azure Machine Learning ocorrerá se o nome do contêiner de armazenamento do Azure for especificado incorretamente. Você receberá esse erro se não tiver especificado o contêiner e o nome do blob (arquivo) usando **a opção caminho para o blob começando com o contêiner** ao gravar no armazenamento de BLOBs do Azure.  

**Resolução:** Revisite o módulo [exportar dados](export-data.md) e verifique se o caminho especificado para o BLOB contém o contêiner e o nome do arquivo, no formato **contêiner/nome**de caminho.  

|Mensagens de Exceção|
|------------------------|
|O nome do contêiner de armazenamento do Azure está incorreto.|
|O nome do contêiner de armazenamento do Azure "{container_name}" está incorreto; um nome de contêiner do formato contêiner/blob era esperado.|


## <a name="error-0080"></a>Erro 0080  
 Ocorrerá uma exceção quando a coluna com todos os valores ausentes não for permitida pelo módulo.  

 Esse erro no Azure Machine Learning é produzido quando uma ou mais colunas consumidas pelo módulo contêm todos os valores ausentes. Por exemplo, se um módulo estiver Computando estatísticas de agregação para cada coluna, ele não poderá operar em uma coluna que não contenha dados. Nesses casos, a execução do módulo é interrompida com essa exceção.  

**Resolução:** Reveja o conjunto de dados de entrada e remova todas as colunas que contêm todos os valores ausentes.  

|Mensagens de Exceção|
|------------------------|
|Colunas com todos os valores ausentes não são permitidas.|
|A coluna {col_index_or_name} tem todos os valores ausentes.|


## <a name="error-0081"></a>Erro 0081  
 Ocorrerá uma exceção no módulo PCA se o número de dimensões a serem reduzidas for igual ao número de colunas de recurso no conjunto de dados de entrada, que contém pelo menos uma coluna de recurso esparsa.  

 Esse erro no Azure Machine Learning será produzido se as seguintes condições forem atendidas: (a) o conjunto de dados de entrada tem pelo menos uma coluna esparsa e (b) o número final de dimensões solicitadas é o mesmo que o número de dimensões de entrada.  

**Resolução:** Considere reduzir o número de dimensões na saída para ser menor que o número de dimensões na entrada. Isso é típico em aplicativos do PCA.   <!--For more information, see [Principal Component Analysis](principal-component-analysis.md).  -->

|Mensagens de Exceção|
|------------------------|
|Para um conjunto de dados que contenha colunas de recursos esparsas, o número de dimensões para o qual reduzir deve ser menor que o número de colunas de recurso.|


## <a name="error-0082"></a>Erro 0082  
 Ocorrerá uma exceção quando um modelo não puder ser desserializado com êxito.  

 Esse erro no Azure Machine Learning ocorre quando um modelo ou uma transformação salva do Machine Learning não pode ser carregada por uma versão mais recente do tempo de execução Azure Machine Learning como resultado de uma alteração significativa.  

**Resolução:** O pipeline de treinamento que produziu o modelo ou a transformação deve ser executado novamente e o modelo ou a transformação deve ser salva novamente.  

|Mensagens de Exceção|
|------------------------|
|O modelo não pôde ser desserializado, pois provavelmente está serializado com um formato de serialização anterior. Retreine e salve novamente o modelo.|


## <a name="error-0083"></a>Erro 0083  
 Ocorrerá uma exceção se o conjunto de dados usado para treinamento não puder ser usado para o tipo concreto de aprendiz.  

 Esse erro no Azure Machine Learning é produzido quando o conjunto de informações é incompatível com o aprendiz sendo treinado. Por exemplo, o conjunto de resultados pode conter pelo menos um valor ausente em cada linha e, como resultado, todo o conjunto de resultados seria ignorado durante o treinamento. Em outros casos, alguns algoritmos de aprendizado de máquina, como a detecção de anomalias, não esperam que os rótulos estejam presentes e possam gerar essa exceção se os rótulos estiverem presentes no conjunto de informações.  

**Resolução:** Consulte a documentação do aprendiz que está sendo usado para verificar os requisitos para o conjunto de dados de entrada. Examine as colunas para ver se todas as colunas necessárias estão presentes.  

|Mensagens de Exceção|
|------------------------|
|O conjunto de dados usado para treinamento é inválido.|
|{data_name} contém dados inválidos para treinamento.|
|{data_name} contém dados inválidos para treinamento. Tipo de aprendiz: {learner_type}.|
|{data_name} contém dados inválidos para treinamento. Tipo de aprendiz: {learner_type}. Motivo: {reason}.|


## <a name="error-0084"></a>Erro 0084  
 Exceção ocorre quando pontuações produzidas a partir de um Script de R são avaliadas. Há suporte atualmente para isso.  

 Esse erro em Azure Machine Learning ocorrerá se você tentar usar um dos módulos para avaliar um modelo com saída de um script R que contém pontuações.  

**Resolução:**

|Mensagens de Exceção|
|------------------------|
|No momento, não há suporte para a avaliação de pontuações produzidas por modelo personalizado.|


## <a name="error-0085"></a>Erro 0085  
 Ocorre uma exceção quando a avaliação de script falha com um erro.  

 Esse erro em Azure Machine Learning ocorre quando você está executando um script personalizado que contém erros de sintaxe.  

**Resolução:** Examine seu código em um editor externo e verifique se há erros.  

|Mensagens de Exceção|
|------------------------|
|Erro durante a avaliação do script.|
|O seguinte erro ocorreu durante a avaliação do script. Consulte o log de saída para obter mais informações:<br />----------Início da mensagem de erro do interpretador {script_language}----------<br />exibida<br />----------Fim da mensagem de erro do interpretador {script_language}----------|


## <a name="error-0090"></a>Erro 0090  
 Ocorrerá uma exceção quando a criação da tabela do hive falhar.  

 Esse erro no Azure Machine Learning ocorre quando você está usando [dados de exportação](export-data.md) ou outra opção para salvar dados em um cluster HDInsight e a tabela Hive especificada não pode ser criada.  

**Resolução:** Verifique o nome da conta de armazenamento do Azure associada ao cluster e verifique se você está usando a mesma conta nas propriedades do módulo.  

|Mensagens de Exceção|
|------------------------|
|Não foi possível criar a tabela Hive. Para um cluster do HDInsight, verifique se o nome da conta de armazenamento do Azure associado ao cluster é o mesmo que o passado por meio do parâmetro do módulo.|
|Não foi possível criar a tabela do hive "{table_name}". Para um cluster do HDInsight, verifique se o nome da conta de armazenamento do Azure associado ao cluster é o mesmo que o passado por meio do parâmetro do módulo.|
|Não foi possível criar a tabela do hive "{table_name}". Para um cluster do HDInsight, verifique se o nome da conta de armazenamento do Azure associado ao cluster é "{cluster_name}".|


## <a name="error-0102"></a>Erro 0102  
 Gerado quando um arquivo ZIP não pode ser extraído.  

 Esse erro no Azure Machine Learning ocorre quando você está importando um pacote compactado com a extensão. zip, mas o pacote não é um arquivo zip ou o arquivo não usa um formato zip com suporte.  

**Resolução:** Verifique se o arquivo selecionado é um arquivo. zip válido e se ele foi compactado usando um dos algoritmos de compactação com suporte.  

 Se você receber esse erro ao importar conjuntos de valores em formato compactado, verifique se todos os arquivos contidos usam um dos formatos de arquivo com suporte e se estão no formato Unicode.  <!--For more information, see [Unpack Zipped Datasets](unpack-zipped-datasets.md).  -->

 Tente ler os arquivos desejados para uma nova pasta compactada compactada e tente adicionar o módulo personalizado novamente.  

|Mensagens de Exceção|
|------------------------|
|O arquivo ZIP fornecido não está no formato correto.|


## <a name="error-0105"></a>Erro 0105  
 Esse erro é exibido quando um arquivo de definição de módulo contém um tipo de parâmetro sem suporte  
  
 Esse erro no Azure Machine Learning é produzido quando você cria uma definição XML de módulo personalizado e o tipo de parâmetro ou argumento na definição não corresponde a um tipo com suporte.  
  
**Resolução:** Verifique se a propriedade Type de qualquer elemento **ARG** no arquivo de definição XML do módulo personalizado é um tipo com suporte.  
  
|Mensagens de Exceção|  
|------------------------|  
|Tipo de parâmetro sem suporte.|  
|Tipo de parâmetro '{0}' não suportado especificado.|  


## <a name="error-0107"></a>Erro 0107  
 Gerado quando um arquivo de definição de módulo define um tipo de saída sem suporte  
  
 Esse erro no Azure Machine Learning é produzido quando o tipo de uma porta de saída em uma definição XML de módulo personalizado não corresponde a um tipo com suporte.  
  
**Resolução:** Verifique se a propriedade Type de um elemento Output no arquivo de definição XML do módulo personalizado é um tipo com suporte.  
  
|Mensagens de Exceção|  
|------------------------|  
|Tipo de saída sem suporte.|  
|Tipo de saída ' {output_type} ' especificado sem suporte.|  


## <a name="error-0125"></a>Erro 0125  
 Gerado quando o esquema para vários conjuntos de valores não corresponde.  

**Resolução:**

|Mensagens de Exceção|
|------------------------|
|O esquema do conjunto de dados não corresponde.|


## <a name="error-0127"></a>Erro 0127  
 O tamanho de pixel da imagem excede o limite permitido  

 Esse erro ocorrerá se você estiver lendo imagens de um conjunto de uma imagem para classificação e as imagens forem maiores do que o modelo pode manipular.  

 <!--**Resolution:**
 For more information about the image size and other requirements, see these topics:  

-   [Import Images](import-images.md)  
  
-   [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)  -->

|Mensagens de Exceção|
|------------------------|
|O tamanho de pixel da imagem excede o limite permitido.|
|O tamanho do pixel da imagem no arquivo ' {file_path} ' excede o limite permitido: ' {size_limit} '.|


## <a name="error-0128"></a>Erro 0128  
 O número de probabilidades condicionais para colunas categóricas excede o limite.  

**Resolução:**

|Mensagens de Exceção|
|------------------------|
|O número de probabilidades condicionais para colunas categóricas excede o limite.|
|O número de probabilidades condicionais para colunas categóricas excede o limite. As colunas ' {column_name_or_index_1} ' e ' {column_name_or_index_2} ' são o par problemático.|


## <a name="error-0129"></a>Erro 0129  
 O número de colunas no conjunto de tempo excede o limite permitido.  

**Resolução:**

|Mensagens de Exceção|
|------------------------|
|O número de colunas no conjunto de tempo excede o limite permitido.|
|O número de colunas no conjunto de um ' {dataset_name} ' excede o permitido.|
|O número de colunas no conjunto de um ' {dataset_name} ' excede o limite permitido de ' {component_name} '.|
|O número de colunas no conjunto de um ' {dataset_name} ' excede o limite permitido ' {limit_columns_count} ' de ' {component_name} '.|


## <a name="error-0134"></a>Erro 0134
Ocorre uma exceção quando a coluna de rótulo está ausente ou tem número insuficiente de linhas rotuladas.  

Esse erro ocorre quando o módulo requer uma coluna de rótulo, mas você não incluiu uma na seleção de coluna ou a coluna de rótulo está sem muitos valores.

Esse erro também pode ocorrer quando uma operação anterior altera o conjunto de registros, de modo que as linhas insuficientes estejam disponíveis para uma operação downstream. Por exemplo, suponha que você use uma expressão no módulo **partição e exemplo** para dividir um conjunto de um DataSet por valores. Se nenhuma correspondência for encontrada para sua expressão, um dos conjuntos de resultados resultante da partição estaria vazio.

Resolução: 

 Se você incluir uma coluna de rótulo na seleção de coluna, mas ela não for reconhecida, use o módulo [Editar metadados](edit-metadata.md) para marcá-la como uma coluna de rótulo.

  <!--Use the [Summarize Data](summarize-data.md) module to generate a report that shows how many values are missing in each column. -->Em seguida, você pode usar o módulo [limpar dados ausentes](clean-missing-data.md) para remover linhas com valores ausentes na coluna rótulo. 

 Verifique os conjuntos de dados de entrada para certificar-se de que eles contêm um dado válido e linhas suficientes para atender aos requisitos da operação. Muitos algoritmos gerarão uma mensagem de erro se exigirem um número mínimo de linhas de dados, mas os dados contiverem apenas algumas linhas ou apenas um cabeçalho.

|Mensagens de Exceção|
|------------------------|
|Ocorre uma exceção quando a coluna de rótulo está ausente ou tem número insuficiente de linhas rotuladas.|
|Ocorre uma exceção quando a coluna de rótulo está ausente ou tem menos de {required_rows_count} linhas rotuladas.|
|Ocorre uma exceção quando a coluna de rótulo no conjunto de um {dataset_name} está ausente ou tem menos de {required_rows_count} linhas rotuladas.|


## <a name="error-0138"></a>Erro 0138  
 A memória foi esgotada, não é possível concluir a execução do módulo. A redução do conjunto de os pode ajudar a aliviar o problema.  

 Esse erro ocorre quando o módulo que está sendo executado requer mais memória do que está disponível no contêiner do Azure. Isso pode acontecer se você estiver trabalhando com um grande conjunto de grandes e a operação atual não couber na memória.  

**Resolução:** Se você estiver tentando ler um grande conjunto de grandes e a operação não puder ser concluída, a redução da resolução do conjunto de um pode ajudar.  

  <!--If you use the visualizations on datasets to check the cardinality of columns, only some rows are sampled. To get a full report, use [Summarize Data](summarize-data.md). You can also use the [Apply SQL Transformation](apply-sql-transformation.md) to check for the number of unique values in each column.  

 Sometimes transient loads can lead to this error. Machine support also changes over time. 

 Try using [Principal Component Analysis](principal-component-analysis.md) or one of the provided feature selection methods to reduce your dataset to a smaller set of more feature-rich columns: [Feature Selection](feature-selection-modules.md)  -->

|Mensagens de Exceção|
|------------------------|
|A memória foi esgotada, não é possível concluir a execução do módulo.|
|A memória foi esgotada, não é possível concluir a execução do módulo. Detalhes: {detalhes}|


## <a name="error-0141"></a>Erro 0141  
 Ocorrerá uma exceção se o número das colunas numéricas selecionadas e os valores exclusivos nas colunas categóricas e de cadeia de caracteres forem muito pequenos.  

 Esse erro em Azure Machine Learning ocorre quando não há valores exclusivos suficientes na coluna selecionada para executar a operação.  

**Resolução:** Algumas operações executam operações estatísticas em colunas de recursos e categóricas e, se não houver valores suficientes, a operação poderá falhar ou retornar um resultado inválido. Verifique o conjunto de recursos para ver quantos valores existem nas colunas de recurso e rótulo e determine se a operação que você está tentando executar é estatisticamente válida.  

 Se o conjunto de dados de origem for válido, você também poderá verificar se alguma operação de manipulação ou metadados upstream alterou os dados e removeu alguns valores.  

 Se as operações de upstream incluírem divisão, amostragem ou reamostragem, verifique se as saídas contêm o número esperado de linhas e valores.  

|Mensagens de Exceção|
|------------------------|
|O número de colunas numéricas selecionadas e valores exclusivos nas colunas categóricas e de cadeia de caracteres é muito pequeno.|
|O número total de colunas numéricas selecionadas e valores exclusivos nas colunas categóricas e de cadeia de caracteres (atualmente, {actual_num}) devem ser pelo menos {lower_boundary}.|


## <a name="error-0154"></a>Erro 0154  
 Ocorre uma exceção quando o usuário tenta unir dados em colunas de chave com tipo de coluna incompatível.

|Mensagens de Exceção|
|------------------------|
|Os tipos de elementos de coluna de chave não são compatíveis.|
|Os tipos de elementos de coluna de chave não são compatíveis. (esquerda: {keys_left}; direita: {keys_right})|


## <a name="error-0155"></a>Erro 0155  
 Ocorre uma exceção quando os nomes de coluna de DataSet não são cadeias de caracteres.

|Mensagens de Exceção|
|------------------------|
|Os nomes de coluna não são cadeias de caracteres.|
|Nomes de coluna: {column_names} não são cadeias de caracteres.|


## <a name="error-0156"></a>Erro 0156  
 Ocorrerá uma exceção quando houve falha na leitura de dados do Azure SQL Database.

|Mensagens de Exceção|
|------------------------|
|Falha ao ler dados do banco de dado SQL do Azure.|
|Falha ao ler dados do banco de dados SQL do Azure: {detailed_message} DB: {database_server_name}: {database_name} consulta: {sql_statement}|


## <a name="error-0157"></a>Erro 0157  
 Repositório de armazenamento não encontrado.

|Mensagens de Exceção|
|------------------------|
|As informações do repositório de dados são inválidas.|
|As informações do repositório de dados são inválidas. Falha ao obter o repositório de armazenamento ' {datastore_name} ' do AzureML no espaço de trabalho ' {workspace_name} '.|


## <a name="error-1000"></a>Erro 1000  
Exceção de biblioteca interna.  

Esse erro é fornecido para capturar erros de mecanismo interno de outra forma sem tratamento. Portanto, a causa desse erro pode ser diferente dependendo do módulo que gerou o erro.  

Para obter mais ajuda, recomendamos que você poste a mensagem detalhada que acompanha o erro no fórum de Azure Machine Learning, junto com uma descrição do cenário, incluindo os dados usados como entradas. Esses comentários nos ajudarão a priorizar erros e identificar os problemas mais importantes para um trabalho adicional.  

|Mensagens de Exceção|
|------------------------|
|Exceção de biblioteca.|
|Exceção da biblioteca: {Exception}.|
|Exceção de biblioteca desconhecida: {Exception}. {customer_support_guidance}.|

