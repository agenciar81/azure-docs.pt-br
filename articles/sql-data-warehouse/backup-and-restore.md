---
title: Backup e restauração-instantâneos, com redundância geográfica
description: Saiba como o backup e a restauração funcionam no pool do SQL do Azure Synapse Analytics. Use backups para restaurar seu data warehouse para um ponto de restauração na região primária. Use backups com redundância geográfica para restaurar para uma região geográfica diferente.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/04/2020
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019"
ms.openlocfilehash: 2b689588bcbca640dd55b25c52c462ad1a363da5
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78296330"
---
# <a name="backup-and-restore-in-azure-synapse-sql-pool"></a>Backup e restauração no pool do SQL Synapse do Azure

Saiba como usar o backup e a restauração no pool do SQL Synapse do Azure. Use pontos de restauração do pool do SQL para recuperar ou copiar seu data warehouse para um estado anterior na região primária. Use backups de data warehouse com redundância geográfica para restaurar para uma região geográfica diferente.

## <a name="what-is-a-data-warehouse-snapshot"></a>O que é um instantâneo data warehouse

Um *instantâneo de data warehouse* cria um ponto de restauração que pode ser aproveitado para recuperar ou copiar seu data warehouse para um estado anterior.  Como o pool do SQL é um sistema distribuído, um instantâneo de data warehouse consiste em muitos arquivos que estão localizados no armazenamento do Azure. Os instantâneos capturam as alterações incrementais dos dados armazenados no data warehouse.

A *restauração de um data warehouse* é um novo data warehouse criado por meio de um ponto de restauração de um data warehouse existente ou excluído. Restaurar o seu banco de dados é uma parte essencial de qualquer estratégia de recuperação de desastre e de continuidade dos negócios, porque ela recria seus dados após uma exclusão ou corrupção acidental. O data warehouse também é um mecanismo eficiente para criar cópias do seu data warehouse para fins de teste ou desenvolvimento.  As taxas de restauração do pool SQL podem variar dependendo do tamanho do banco de dados e do local da data warehouse de origem e de destino. 

## <a name="automatic-restore-points"></a>Pontos de restauração automática

Instantâneos são um recurso interno do serviço que cria pontos de restauração. Não é necessário habilitar essa funcionalidade. No entanto, o pool do SQL deve estar em um estado ativo para a criação do ponto de restauração. Se o pool do SQL for pausado com frequência, os pontos de restauração automáticas não poderão ser criados, portanto, certifique-se de criar um ponto de restauração definido pelo usuário antes de pausar o pool SQL. Atualmente, os pontos de restauração automáticos não podem ser excluídos pelos usuários, pois o serviço usa esses pontos de restauração para manter os SLAs para recuperação.

Os instantâneos de seus data warehouse são obtidos durante o dia criando pontos de restauração que estão disponíveis por sete dias. Esse período de retenção não pode ser alterado. O pool SQL dá suporte a um RPO (objetivo de ponto de recuperação) de oito horas. É possível restaurar seu data warehouse na região primária com base em qualquer um dos instantâneos tirados nos últimos sete dias.

Para ver quando o último instantâneo foi iniciado, execute esta consulta em seu pool do SQL online.

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs
order by run_id desc
;
```

## <a name="user-defined-restore-points"></a>Pontos de restauração definidos pelo usuário

Esse recurso permite que você acione manualmente os instantâneos para criar pontos de restauração do seu data warehouse antes e depois de grandes modificações. Esse recurso garante que os pontos de restauração sejam logicamente consistentes, o que fornece proteção de dados adicional no caso de quaisquer interrupções de carga de trabalho ou erros de usuário para o tempo de recuperação rápida. Os pontos de restauração definidos pelo usuário ficam disponíveis por sete dias e são excluídos automaticamente em seu nome. Não é possível alterar o período de retenção de pontos de restauração definidos pelo usuário. Garantimos apenas **42 pontos de restauração definidos pelo usuário** a qualquer momento; é necessário [excluir](https://go.microsoft.com/fwlink/?linkid=875299) algum ponto antes da criação de outro ponto de restauração. É possível disparar instantâneos para criar pontos de restauração definidos pelo usuário por meio do [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint#examples) ou do Portal do Azure.

> [!NOTE]
> Caso precise de pontos de restauração de mais de 7 dias, escolha esta funcionalidade [aqui](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/35114410-user-defined-retention-periods-for-restore-points). Você também pode criar um ponto de restauração definido pelo usuário e restaurar com base no ponto de restauração criado recentemente para um novo data warehouse. Depois de ter restaurado, você tem o pool do SQL online e pode pausá-lo indefinidamente para economizar custos de computação. O banco de dados em pausa tem encargos de armazenamento com a taxa de armazenamento Premium do Azure. Se você precisar de uma cópia ativa do data warehouse restaurado, poderá retomar, o que deve levar apenas alguns minutos.

### <a name="restore-point-retention"></a>Retenção do ponto de restauração

A lista a seguir detalha os períodos de retenção do ponto de restauração:

1. O pool do SQL exclui um ponto de restauração quando atinge o período de retenção de 7 dias **e** quando há pelo menos 42 pontos de restauração totais (incluindo os definidos pelo usuário e automaticamente).
2. Os instantâneos não são feitos quando um pool do SQL é pausado.
3. A idade de um ponto de restauração é medida pelos dias de calendário absolutos a partir do momento em que o ponto de restauração é realizado, incluindo quando o pool do SQL é pausado.
4. Em qualquer ponto no tempo, é garantido que um pool SQL seja capaz de armazenar até 42 pontos de restauração definidos pelo usuário e 42 pontos de restauração automáticos, contanto que esses pontos de restauração não tenham atingido o período de retenção de 7 dias
5. Se um instantâneo for tirado, o pool do SQL será pausado por mais de 7 dias e, em seguida, será retomado, o ponto de restauração persistirá até que haja 42 de pontos de restauração totais (incluindo os definidos pelo usuário e automaticamente)

### <a name="snapshot-retention-when-a-sql-pool-is-dropped"></a>Retenção de instantâneo quando um pool de SQL é Descartado

Quando você remove um pool SQL, um instantâneo final é criado e salvo por sete dias. Você pode restaurar o pool SQL para o ponto de restauração final criado na exclusão. Se o pool do SQL for descartado em um estado de pausa, nenhum instantâneo será obtido. Nesse cenário, certifique-se de criar um ponto de restauração definido pelo usuário antes de descartar o pool SQL.

> [!IMPORTANT]
> Se você excluir uma instância lógica do SQL Server, todos os bancos de dados que pertencerem a essa instância também serão excluídos e não poderão ser recuperados. Você não pode restaurar um servidor excluído.

## <a name="geo-backups-and-disaster-recovery"></a>Backups geográficos e recuperação de desastre

Um backup geográfico é criado uma vez por dia para um [Data Center emparelhado](../best-practices-availability-paired-regions.md). O RPO de uma restauração geográfica é de 24 horas. Você pode restaurar o backup geográfico para um servidor em qualquer outra região em que o pool SQL tenha suporte. Um backup geográfico garante que você possa restaurar um data warehouse caso não seja possível acessar os pontos de restauração em sua região primária.

> [!NOTE]
> Se precisar de um RPO mais curto para backups geográficos, escolha esta funcionalidade [aqui](https://feedback.azure.com/forums/307516-sql-data-warehouse). Você também pode criar um ponto de restauração definido pelo usuário e restaurar com base no ponto de restauração criado recentemente para um novo data warehouse em uma região diferente. Após a restauração, seu data warehouse estará online e será possível pausá-lo indefinidamente para em economizar custos de computação. O banco de dados em pausa tem encargos de armazenamento com a taxa de armazenamento Premium do Azure. Caso você precise de uma cópia ativa do data warehouse, poderá retomar, o que deve levar apenas alguns minutos.

## <a name="backup-and-restore-costs"></a>Custos de backup e restauração

Você observará que a fatura do Azure tem um item de linha para Armazenamento e um item de linha para Armazenamento para Recuperação de Desastre. O encargo de armazenamento é o custo total para armazenar seus dados na região primária junto com as alterações incrementais capturadas por instantâneos. Para obter uma explicação mais detalhada de como os instantâneos são cobrados, veja [Understanding how Snapshots Accrue Charges](https://docs.microsoft.com/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges?redirectedfrom=MSDN#snapshot-billing-scenarios) (Noções básicas sobre como os instantâneos acumulam cobranças). O encargo com redundância geográfica abrange o custo para armazenar os backups geográficos.  

O custo total para o data warehouse primário e de sete dias de alterações de instantâneos é arredondado para o TB mais próximo. Por exemplo, se seu data warehouse é de 1,5 TB e os instantâneos capturam 100 GB, você será cobrado pelo equivalente a 2 TB de dados segundo as taxas do Armazenamento Premium do Azure.

Se você estiver usando armazenamento com redundância geográfica, você receberá uma cobrança de armazenamento separada. O armazenamento com redundância geográfica é cobrado segundo a taxa padrão de RA-GRS (Armazenamento com Redundância Geográfica com Acesso de Leitura).

Para obter mais informações sobre os preços do Azure Synapse, consulte [preços do Azure Synapse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/). Você não será cobrado pela saída de dados durante a restauração entre regiões.

## <a name="restoring-from-restore-points"></a>Restauração a partir dos pontos de restauração

Cada instantâneo cria um ponto de restauração que representa a hora em que o instantâneo é iniciado. Para restaurar um data warehouse, escolha um ponto de restauração e emita um comando de restauração.  

É possível manter o data warehouse restaurado e o atual ou excluir um deles. Se desejar substituir o data warehouse atual pelo data warehouse restaurado, você poderá renomeá-lo usando [ALTER DATABASE (pool do SQL)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) com a opção Modify Name.

Para restaurar um data warehouse, consulte [restaurar um pool do SQL usando o portal do Azure](sql-data-warehouse-restore-database-portal.md), [restaurar um pool do SQL usando o PowerShell](sql-data-warehouse-restore-database-powershell.md)ou [restaurar um pool do SQL usando APIs REST](sql-data-warehouse-restore-database-rest-api.md).

Para restaurar um data warehouse excluído ou em pausa, será possível [criar um tíquete de suporte](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="cross-subscription-restore"></a>Restauração de assinatura cruzada

Se você precisar restaurar diretamente na assinatura, vote nesse recurso [aqui](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/36256231-enable-support-for-cross-subscription-restore). Restaure para um servidor lógico diferente e [' mova '](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources) o servidor em assinaturas para executar uma restauração de assinatura cruzada. 

## <a name="geo-redundant-restore"></a>Restauração com redundância geográfica

Você pode [restaurar seu pool SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-from-geo-backup#restore-from-an-azure-geographical-region-through-powershell) para qualquer região que ofereça suporte ao pool do SQL no nível de desempenho escolhido.

> [!NOTE]
> Para executar uma restauração com redundância geográfica, você não deve ter recusado esse recurso.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre planejamento de desastre, consulte [Visão geral sobre a continuidade dos negócios](../sql-database/sql-database-business-continuity.md)
