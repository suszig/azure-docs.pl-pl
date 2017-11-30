---
title: Migracja maszyny po ocenie przy migracji Azure | Dokumentacja firmy Microsoft
description: "Opisuje sposób uzyskać zalecenia dotyczące migrowania maszyn po uruchomieniu oceny w usłudze Azure migracji."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 809c6e85-0928-45e2-a7c7-6824d860e134
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 083e614628645d2eb94b7d84f4127b690865d770
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-machines-after-assessment"></a>Migrowanie maszyn po ocenie


[Azure migracji](migrate-overview.md) ocenia lokalnymi maszynami, aby sprawdzić, czy są odpowiednie dla migracji na platformie Azure i zapewnia kosztów i zmiany rozmiaru uzyskać szacunkowe wartości do uruchamiania komputera w systemie Azure. Obecnie migracji Azure tylko ocenia maszyny do migracji. Migracja się aktualnie jest wykonywane przy użyciu innych usług Azure.

W tym artykule opisano, jak uzyskać sugestie dotyczące narzędzia do migracji, po uruchomieniu oceny migracji.

## <a name="migration-tool-suggestion"></a>Sugestia narzędzia migracji

Aby sugestie dotyczące narzędzi migracji, należy wykonać głębokiego ujawniania środowiska lokalnego. Głębokie odnajdywanie odbywa się przez zainstalowanie agentów na komputerach lokalnych.  

1. Utwórz projekt platformy Azure migracji, Odkryj maszyny lokalnej, a oceny migracji. [Dowiedz się więcej](tutorial-assessment-vmware.md).
2. Pobierz i zainstaluj agentów programu Azure migracji na każdej maszynie lokalnej, dla którego chcesz wyświetlić metody zalecane migracji. [Wykonaj poniższą procedurę](how-to-create-group-machine-dependencies.md#prepare-machines-for-dependency-mapping) do instalacji agentów.
2. Zidentyfikuj maszynach lokalnych odpowiednie do migracji przyrostu shift. Są to maszyn wirtualnych, które nie wymaga żadnych zmian do aplikacji działających na nich i mogą być migrowane, ponieważ jest.
3. W przypadku migracji przyrostu shift zalecamy przy użyciu usługi Azure Site Recovery. [Dowiedz się więcej](../site-recovery/tutorial-migrate-on-premises-to-azure.md). Alternatywnie można użyć narzędzia innych firm, które obsługują migrację do usługi Azure.
4. Jeśli masz maszyny lokalne, które nie są odpowiednie dla migracji przyrostu i shift, oznacza to, jeśli chcesz przeprowadzić migrację specyficzne dla aplikacji, a nie całą maszynę Wirtualną, służy inne narzędzia do migracji. Na przykład zalecamy [migracja bazy danych usługi Azure service](https://azure.microsoft.com/campaigns/database-migration/) Jeśli chcesz przeprowadzić migrację lokalnych baz danych programu SQL Server, MySQL lub Oracle na platformie Azure.


## <a name="review-suggested-migration-methods"></a>Przegląd metod sugerowane migracji

1. Zanim będzie można pobrać metody sugerowane migracji, należy utworzyć projekt Azure migracji, Odkryj maszyny lokalnej, a następnie uruchom oceny migracji. [Dowiedz się więcej](tutorial-assessment-vmware.md).
2. Po utworzeniu oceny go wyświetlić, w projekcie > **omówienie** > **pulpitu nawigacyjnego**. Kliknij przycisk **oceny gotowości**

    ![Oceny gotowości](./media/tutorial-assessment-vmware/assessment-report.png)  

3. W **sugerowane narzędzia**, przeglądanie sugestii dla narzędzi można użyć do migracji.

    ![Sugerowane narzędzia](./media/tutorial-assessment-vmware/assessment-suitability.png) 




## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
