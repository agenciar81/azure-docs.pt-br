---
title: Trabalhando com matrizes e objetos no Azure Cosmos DB
description: Aprenda a sintaxe do SQL para criar matrizes e objetos no Azure Cosmos DB. Este artigo também fornece alguns exemplos para executar operações em objetos de matriz
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 5b2801b0a71f04803955e9d8bc18a97133019996
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246546"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Trabalhando com matrizes e objetos no Azure Cosmos DB

Um recurso importante do Azure Cosmos DB API do SQL é a criação de matriz e objeto.

## <a name="arrays"></a>Matrizes

Você pode construir matrizes, conforme mostrado no exemplo a seguir:

```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

Os resultados são:

```json
    [
      {
        "CityState": [
          "Seattle",
          "WA"
        ]
      },
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

Você também pode usar a [expressão de matriz](sql-query-subquery.md#array-expression) para construir uma matriz de resultados da [subconsulta](sql-query-subquery.md) . Essa consulta obtém todos os nomes distintos de filhos em uma matriz.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a id="Iteration"></a>Iteração

A API do SQL fornece suporte para iteração em matrizes JSON, com uma nova construção adicionada por meio da [palavra-chave in](sql-query-keywords.md#in) na fonte from. No exemplo a seguir:

```sql
    SELECT *
    FROM Families.children
```

Os resultados são:

```json
    [
      [
        {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        }, 
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

A próxima consulta executa a iteração sobre `children` no contêiner de `Families`. A matriz de saída é diferente da consulta anterior. Este exemplo divide `children`e mescla os resultados em uma única matriz:  

```sql
    SELECT *
    FROM c IN Families.children
```

Os resultados são:

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

Você pode filtrar mais detalhadamente cada entrada individual da matriz, conforme mostrado no exemplo a seguir:

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

Os resultados são:

```json
    [{
      "givenName": "Lisa"
    }]
```

Você também pode agregar o resultado de uma iteração de matriz. Por exemplo, a consulta a seguir conta o número de filhos entre todas as famílias:

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

Os resultados são:

```json
    [
      {
        "$1": 3
      }
    ]
```

## <a name="next-steps"></a>Próximas etapas

- [Guia de Introdução](sql-query-getting-started.md)
- [Amostras do .NET no Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Junções](sql-query-join.md)