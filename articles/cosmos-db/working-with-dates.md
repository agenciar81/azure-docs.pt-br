---
title: Trabalhando com datas no Azure Cosmos DB
description: Saiba como armazenar, indexar e consultar objetos de data e hora no Azure Cosmos DB
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 92fa35fbe8e5eef4dbdc8b6c47a9055affd449a5
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273198"
---
# <a name="working-with-dates-in-azure-cosmos-db"></a>Trabalhando com datas no Azure Cosmos DB

O Azure Cosmos DB fornece flexibilidade de esquema e indexação avançada por meio de um modelo de dados [JSON](https://www.json.org) nativo. Todos os recursos do Azure Cosmos DB, incluindo bancos de dados, contêineres, documentos e procedimentos armazenados são modelados e armazenados como documentos JSON. Como um requisito para ser portátil, o JSON (e o Azure Cosmos DB) dá suporte apenas a um conjunto pequeno de tipos básicos: Cadeia de caracteres, Número, Booliano, Matriz, Objeto e Nulo. No entanto, o JSON é flexível e permite aos desenvolvedores e estruturas representar tipos mais complexos usando esses primitivos e compondo-os como objetos ou matrizes.

Além dos tipos básicos, muitos aplicativos precisam do tipo DateTime para representar datas e carimbos de data/hora. Este artigo descreve como os desenvolvedores podem armazenar, recuperar e consultar datas no Azure Cosmos DB usando o SDK do .NET.

## <a name="storing-datetimes"></a>Armazenando DateTimes

Azure Cosmos DB dá suporte a tipos JSON como-String, Number, Boolean, NULL, array, Object. Ele não dá suporte diretamente a um tipo DateTime. Atualmente, Azure Cosmos DB não dá suporte à localização de datas. Portanto, você precisa armazenar DateTimes como cadeias de caracteres. O formato recomendado para cadeias de caracteres DateTime no Azure Cosmos DB é `YYYY-MM-DDThh:mm:ss.sssZ` que segue o padrão ISO 8601 UTC. É recomendável armazenar todas as datas em Azure Cosmos DB como UTC. Converter as cadeias de caracteres de data para esse formato permitirá a classificação de datas modo lexicográfico. Se as datas não-UTC forem armazenadas, a lógica deverá ser tratada no lado do cliente. Para converter um DateTime local em UTC, o deslocamento deve ser conhecido/armazenado como uma propriedade no JSON e o cliente pode usar o deslocamento para calcular o valor DateTime UTC.

A maioria dos aplicativos pode usar a representação de cadeia de caracteres padrão para DateTime pelos seguintes motivos:

* As cadeias de caracteres podem ser comparadas e a ordenação relativa dos valores de DateTime é preservada quando eles são transformados em cadeias de caracteres.
* Essa abordagem não exige qualquer código personalizado ou atributos para conversão de JSON.
* As datas, conforme armazenadas no JSON, são legíveis para humanos.
* Essa abordagem pode aproveitar o índice do Azure Cosmos DB para um desempenho rápido de consulta.

Por exemplo, o snippet a seguir armazena um objeto `Order` que contém duas propriedades DateTime — `ShipDate` e `OrderDate` como um documento usando o SDK do .NET:

```csharp
    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await container.CreateItemAsync(
        new Order
        {
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });
```

Esse documento é armazenado no Azure Cosmos DB da seguinte maneira:

```json
    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
```  

Como alternativa, você pode armazenar DateTimes como carimbos de data/hora do Unix, ou seja, como um número que representa o número de segundos decorridos desde 1º de janeiro de 1970. A propriedade Timestamp (`_ts`) interna do Cosmos DB segue essa abordagem. Você pode usar a classe [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) para serializar DateTimes como números.

## <a name="querying-datetimes-in-linq"></a>Consultando DateTimes no LINQ

O SDK do .NET para SQL dá suporte automaticamente à consulta de dados armazenados no Azure Cosmos DB via LINQ. Por exemplo, o trecho a seguir mostra uma consulta LINQ que filtra os pedidos que foram enviados nos últimos três dias:

```csharp
    IQueryable<Order> orders = container.GetItemLinqQueryable<Order>(allowSynchronousQueryExecution: true).Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
```

Traduzido para a seguinte instrução SQL e executado em Azure Cosmos DB:

```sql
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")
```

Você pode aprender mais sobre a linguagem de consulta SQL do Azure Cosmos DB e o provedor do LINQ ao [consultar Cosmos DB no LINQ](sql-query-linq-to-sql.md).

## <a name="indexing-datetimes-for-range-queries"></a>Indexando DateTimes para consultas de intervalo

As consultas são comuns com valores DateTime. Para executar essas consultas com eficiência, você deve ter um índice definido em todas as propriedades no filtro da consulta.

Saiba mais sobre como configurar as políticas de indexação em [Políticas de indexação do Azure Cosmos DB](index-policy.md). 

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

* Baixe e execute os [Exemplos de código no GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* Saiba mais sobre [consultas SQL](sql-query-getting-started.md)
* Saiba mais sobre as [Políticas de indexação do Azure Cosmos DB](index-policy.md)
