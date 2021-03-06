---
title: Tipos de conteúdo-QnA Maker
description: Os tipos de conteúdo incluem muitos documentos estruturados padrão, como PDF, DOC e TXT.
services: cognitive-services
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 7c78f9ea261fa636cce50b69524802d0900e9d7b
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650190"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Tipos de conteúdo de documentos que você pode adicionar a uma base de dados de conhecimento
Os tipos de conteúdo incluem muitos documentos estruturados padrão, como PDF, DOC e TXT.

## <a name="file-and-url-data-types"></a>Tipos de dados de arquivo e URL

A tabela a seguir resume os tipos de conteúdo e formatos de arquivo com suporte no QnA Maker.

|Tipo de Fonte|Tipo de conteúdo| Exemplos|
|--|--|--|
|URL|Perguntas frequentes<br> (simples, com seções ou com uma página inicial de tópicos)<br>Páginas de suporte <br> (artigos de instrução de uma página, artigos de solução de problemas etc.)|[Perguntas frequentes simples](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[Perguntas frequentes com links](https://www.microsoft.com/en-us/software-download/faq),<br> [Perguntas frequentes com home page de tópicos](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Artigo de suporte](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|Perguntas frequentes,<br> Manual do Produto,<br> Folhetos,<br> Papel,<br> Política de folheto,<br> Guia de suporte,<br> QnA estruturado,<br> etc.|**Sem troca múltipla**<br>[QnA.doc estruturado](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Exemplo produto Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Exemplo semi-estruturado.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Exemplo White Paper. pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br><br>**Troca múltipla**:<br>[Surface pro (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Benefícios da Contoso (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Benefícios da Contoso (PDF)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|\* Excel|Arquivo QnA estruturado<br> (incluindo suporte RTF, HTML)|[Exemplo de QnA FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|\* TXT/TSV|Arquivo QnA estruturado|[Exemplo de chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Se você precisar de autenticação para sua fonte de dados, considere os seguintes métodos para obter esse conteúdo em QnA Maker:

* Baixar o arquivo manualmente e importar para o QnA Maker
* Adicionar arquivo do local do [SharePoint](../how-to/add-sharepoint-datasources.md) autenticado

## <a name="url-content"></a>Conteúdo da URL

Dois tipos de documentos podem ser importados por meio de **URL** no QnA Maker:

* URLs de perguntas frequentes
* URLs de suporte

Cada tipo indica um formato esperado.

## <a name="file-based-content"></a>Conteúdo baseado em arquivo

Você pode adicionar arquivos a uma base de dados de conhecimento de uma fonte pública ou seu sistema de arquivos local, no [portal de QnA Maker](https://www.qnamaker.ai).

## <a name="content-format-guidelines"></a>Diretrizes de formato de conteúdo

Saiba mais sobre as [diretrizes de formato](../reference-document-format-guidelines.md) para os diferentes arquivos.

## <a name="next-steps"></a>Próximas etapas

Entenda quais informações são armazenadas em um [conjunto de perguntas e respostas (QnA)](question-answer-set.md).