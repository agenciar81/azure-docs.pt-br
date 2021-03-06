---
title: 'Treinar Recomendador SVD: referência de módulo'
titleSuffix: Azure Machine Learning
description: Saiba como usar o módulo treinar Recomendador SVD em Azure Machine Learning para treinar um recomendador Bayesiana usando o algoritmo SVD.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 1778ba543e070bbffbbc8579b280373d834492fd
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77920783"
---
# <a name="train-svd-recommender"></a>Treinar o sistema de recomendação SVD

Este artigo descreve como usar o módulo treinar Recomendador SVD no designer de Azure Machine Learning (versão prévia). Use este módulo para treinar um modelo de recomendação com base no algoritmo de decomposição de valor único (SVD).  

O módulo treinar Recomendador SVD lê um conjunto de uma série de processamentos de usuário-item-classificação. Ele retorna um recomendador SVD treinado. Em seguida, você pode usar o modelo treinado para prever classificações ou gerar recomendações usando o módulo de [recomendação SVD de Pontuação](score-svd-recommender.md) .  


  
## <a name="more-about-recommendation-models-and-the-svd-recommender"></a>Mais sobre modelos de recomendação e o recomendador SVD  

O objetivo principal de um sistema de recomendação é recomendar um ou mais *itens* aos *usuários* do sistema. Exemplos de um item podem ser um filme, restaurante, livro ou música. Um usuário pode ser uma pessoa, um grupo de pessoas ou outra entidade com preferências de item.  

Há duas abordagens principais para sistemas de recomendação: 

+ Uma abordagem **baseada em conteúdo** usa recursos para usuários e itens. Os usuários podem ser descritos por propriedades como idade e sexo. Os itens podem ser descritos por propriedades como autor e fabricante. Você pode encontrar exemplos típicos de sistemas de recomendação baseados em conteúdo em sites de relacionamento social. 
+ A **filtragem colaborativa** usa apenas identificadores dos usuários e os itens. Ele obtém informações implícitas sobre essas entidades de uma matriz (esparsa) de classificações dadas pelos usuários para os itens. Podemos aprender sobre um usuário a partir dos itens que eles classificaram e de outros usuários que classificaram os mesmos itens.  

O recomendador SVD usa identificadores dos usuários e os itens e uma matriz de classificações dadas pelos usuários para os itens. É um *recomendador colaborativo*. 

Para obter mais informações sobre o recomendador SVD, consulte o documento de pesquisa relevante: [técnicas de fatoração de matriz para sistemas de recomendação](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf).


## <a name="how-to-configure-train-svd-recommender"></a>Como configurar o Recomendador SVD de treinamento  

### <a name="prepare-data"></a>Preparar dados

Antes de usar o módulo, os dados de entrada devem estar no formato esperado pelo modelo de recomendação. Um conjunto de dados de treinamento de percurso de classificação de usuário-item é necessário.

+ A primeira coluna contém identificadores de usuário.
+ A segunda coluna contém identificadores de item.
+ A terceira coluna contém a classificação para o par usuário-item. Os valores de classificação devem ser do tipo numérico.  

O conjunto de jogos de **classificações de filmes** no designer de Azure Machine Learning (selecione **conjuntos de valores** e, em seguida, **amostras**) demonstra o formato esperado:

![Classificações de filme](media/module/movie-ratings-dataset.png)

Neste exemplo, você pode ver que um único usuário classificou vários filmes. 

### <a name="train-the-model"></a>Treinar o modelo

1.  Adicione o módulo treinar Recomendador SVD ao seu pipeline no designer e conecte-o aos dados de treinamento.  
   
2.  Para **número de fatores**, especifique o número de fatores a serem usados com o recomendador.  
    
    Cada fator mede o quanto o usuário está relacionado ao item. O número de fatores também é a dimensionalidade do espaço de fator Lated. Com o aumento do número de usuários e itens, é melhor definir um número maior de fatores. Mas se o número for muito grande, o desempenho poderá cair.
    
3.  O **número de iterações do algoritmo de recomendação** indica quantas vezes o algoritmo deve processar os dados de entrada. Quanto mais alto for esse número, mais precisas serão as previsões. No entanto, um número mais alto significa treinamento mais lento. O valor padrão é 30.

4.  Para **taxa de aprendizagem**, insira um número entre 0,0 e 2,0 que define o tamanho da etapa para aprendizado.

    A taxa de aprendizagem determina o tamanho da etapa em cada iteração. Se o tamanho da etapa for muito grande, você poderá exceder a solução ideal. Se o tamanho da etapa for muito pequeno, o treinamento levará mais tempo para encontrar a melhor solução. 
  
5.  Execute o pipeline.  


## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Consulte o [conjunto de módulos disponíveis](module-reference.md) para Azure Machine Learning. 
