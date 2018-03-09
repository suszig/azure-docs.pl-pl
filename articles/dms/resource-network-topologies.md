---
title: "Sieci topologii dla migracji wystąpienia zarządzane bazy danych SQL Azure przy użyciu usługi migracji bazy danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się konfiguracje źródłowa i docelowa usługa migracji bazy danych."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/06/2018
ms.openlocfilehash: 892cff02b5b70f09236bb37ae786f180ddca9316
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Topologie sieci migracji wystąpienia zarządzane bazy danych SQL Azure przy użyciu usługi migracji bazy danych Azure
W tym artykule dowiesz się o różnych topologii sieci czy usługa migracji bazy danych Azure może współpracować z zapewnienie obsługi płynną migrację do wystąpienia zarządzane bazy danych SQL Azure z lokalnych serwerów SQL.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure wystąpienia bazy danych SQL zarządzane skonfigurowane dla obciążeń hybrydowego 
Jeśli Twoje Azure wystąpienia bazy danych SQL zarządzanych jest podłączony do sieci lokalnej, należy użyć tej topologii. To rozwiązanie zapewnia najbardziej uproszczony routingu sieciowego i zwraca maksymalną przepustowość podczas migracji.

![Topologia sieci dla obciążeń hybrydowego](media\resource-network-topologies\hybrid-workloads.png)

**Wymagania**
- W tym scenariuszu Azure wystąpienia bazy danych SQL zarządzane i wystąpienie usługi migracji bazy danych Azure są tworzone w tej samej sieci Wirtualnej Azure, ale korzystają z różnych podsieci.  
- Sieci Wirtualnej, w tym scenariuszu jest również połączona z siecią lokalną przy użyciu usługi ExpressRoute lub sieci VPN.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure wystąpienia bazy danych SQL zarządzane odizolowanych od sieci lokalnej
Użyj tej topologii sieci, jeśli dane środowisko wymaga co najmniej jednego z następujących scenariuszy:
- Azure SQL bazy danych zarządzanych wystąpienie jest odizolowana od łączności z lokalnymi, ale wystąpienie usługi migracji bazy danych Azure jest podłączony do sieci lokalnej.
- Zasady kontroli dostępu na podstawie (RBAC) roli są na miejscu i ograniczać dostęp użytkowników do tej samej subskrypcji, obsługujący Azure wystąpienia bazy danych SQL zarządzane.
- Sieci wirtualne używane dla wystąpienia zarządzane bazy danych SQL Azure i usługi migracji bazy danych Azure znajdują się w różnych subskrypcji.

![Topologia sieci dla wystąpienia zarządzane odizolowanych od sieci lokalnej](media\resource-network-topologies\mi-isolated-workload.png)

**Wymagania**
- Sieć Wirtualną, która korzysta z usługi migracji bazy danych Azure w tym scenariuszu należy również połączona z siecią lokalną przy użyciu usługi ExpressRoute lub sieci VPN.
- Tworzenie sieci Wirtualnej komunikacji równorzędnej między sieciami Wirtualnymi, używany do wystąpienia zarządzane bazy danych SQL Azure i usługi migracji bazy danych Azure.


## <a name="cloud-to-cloud-migrations"></a>Chmury do migracji w chmurze
Użyj tej topologii, jeśli źródło programu SQL Server znajduje się w maszynie wirtualnej platformy Azure.

![Topologia sieci dla migracji w chmurze do chmury](media\resource-network-topologies\cloud-to-cloud.png)

**Wymagania**
- Tworzenie sieci Wirtualnej komunikacji równorzędnej między sieciami Wirtualnymi, używany do wystąpienia zarządzane bazy danych SQL Azure i usługi migracji bazy danych Azure.

## <a name="see-also"></a>Zobacz też
- [Migrowanie serwera SQL do zarządzanego wystąpienia bazy danych Azure SQL](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Przegląd wymagań wstępnych dotyczących używania usługi Azure bazy danych migracji](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Tworzenie sieci wirtualnej przy użyciu witryny Azure Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Kolejne kroki
Omówienie usługi migracji bazy danych Azure i regionalnych dostępność w publicznej wersji zapoznawczej, zobacz artykuł [co to jest podgląd z migracji bazy danych Azure](dms-overview.md). 