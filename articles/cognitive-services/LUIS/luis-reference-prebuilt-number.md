---
title: Número de entidades predefinidas-LUIS
titleSuffix: Azure Cognitive Services
description: Este artigo contém informações sobre a entidade predefinida de número de LUIS (Serviço Inteligente de Reconhecimento Vocal).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 85e3589d7467691e2b9a11879510ab980bbd875a
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273457"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Entidade numérica predefinida para um aplicativo LUIS
Há muitas maneiras de usar os valores numéricos para quantificar, expressar e descrever informações. Este artigo aborda apenas alguns dos exemplos possíveis. O LUIS interpreta as variações em enunciados do usuário e retorna valores numéricos consistentes. Uma vez que essa entidade já está treinada, não é necessário adicionar enunciados de exemplo contendo número às intenções do aplicativo.

## <a name="types-of-number"></a>Tipos de número
O número é gerenciado por meio do repositório do GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml)

## <a name="examples-of-number-resolution"></a>Exemplos de resolução de número

| Enunciado        | Entidade   | Resolução |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      |
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


O LUIS inclui o valor reconhecido de uma entidade **`builtin.number`** no campo `resolution` da resposta JSON que ele retorna.

## <a name="resolution-for-prebuilt-number"></a>Resolução para número predefinido

Os seguintes objetos de entidade são retornados para a consulta:

`order two dozen eggs`

#### <a name="v3-response"></a>[Resposta v3](#tab/V3)

O JSON a seguir é com o parâmetro `verbose` definido como `false`:

```json
"entities": {
    "number": [
        24
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3, resposta detalhada](#tab/V3-verbose)

O JSON a seguir é com o parâmetro `verbose` definido como `true`:

```json
"entities": {
    "number": [
        24
    ],
    "$instance": {
        "number": [
            {
                "type": "builtin.number",
                "text": "two dozen",
                "startIndex": 6,
                "length": 9,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[Resposta v2](#tab/V2)

O exemplo a seguir mostra uma resposta JSON de LUIS, que inclui a resolução do valor de 24, para o enunciado "duas dúzias".

```json
"entities": [
  {
    "entity": "two dozen",
    "type": "builtin.number",
    "startIndex": 6,
    "endIndex": 14,
    "resolution": {
      "subtype": "integer",
      "value": "24"
    }
  }
]
```
* * *

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Saiba mais sobre o [ponto de extremidade de previsão V3](luis-migration-api-v3.md).

Saiba mais sobre [moeda](luis-reference-prebuilt-currency.md), [ordinal](luis-reference-prebuilt-ordinal.md) e [percentual](luis-reference-prebuilt-percentage.md).
