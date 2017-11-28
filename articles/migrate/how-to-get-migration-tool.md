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
ms.openlocfilehash: d633f140635ba184642a2af999efcde845f3ec31
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2017
---
# <a name="migrate-machines-after-assessment"></a>Migrowanie maszyn po ocenie


[Azure migracji](migrate-overview.md) ocenia lokalnymi maszynami, aby sprawdzić, czy są odpowiednie dla migracji na platformie Azure i zapewnia kosztów i zmiany rozmiaru uzyskać szacunkowe wartości do uruchamiania komputera w systemie Azure. Obecnie migracji Azure tylko ocenia maszyny do migracji. Migracja się aktualnie jest wykonywane przy użyciu innych usług Azure.

W tym artykule opisano, jak uzyskać sugestie dotyczące narzędzia do migracji, po uruchomieniu oceny migracji.

## <a name="migration-methods"></a>Metody migracji

Po dokonaniu oceny przy użyciu migracji Azure w tym miejscu jest co zalecamy:

1. Tworzenie projektu platformy Azure migracji, Odkryj maszyny lokalne i uruchomić oceny migracji. [Dowiedz się więcej](tutorial-assessment-vmware.md).
2. Pobierz i zainstaluj agentów programu Azure migracji na każdej maszynie lokalnej, dla którego chcesz wyświetlić metody zalecane migracji. [Wykonaj poniższą procedurę](how-to-create-group-machine-dependencies.md#prepare-machines-for-dependency-mapping) do instalacji agentów.
2. Zidentyfikuj maszynach lokalnych odpowiednie do migracji przyrostu shift. Są to maszyn wirtualnych, które nie wymaga żadnych zmian do aplikacji działających na nich i mogą być migrowane, ponieważ jest.
3. W przypadku migracji przyrostu shift zalecamy przy użyciu usługi Azure Site Recovery. [Dowiedz się więcej](../site-recovery/tutorial-migrate-on-premises-to-azure.md). Alternatywnie można użyć 3 innych narzędzi, które obsługują migrację do usługi Azure.
4. Jeśli masz maszyny lokalne, które nie są odpowiednie dla migracji przyrostu i shift, na przykład jeśli użytkownik chce migrować specyficzne dla aplikacji, a nie całą maszynę Wirtualną, można użyć innych narzędzi do migracji. Na przykład zalecamy [migracja bazy danych usługi Azure service](https://azure.microsoft.com/campaigns/database-migration/) Jeśli chcesz przeprowadzić migrację lokalnych baz danych programu SQL Server, MySQL lub Oracle na platformie Azure.


## <a name="review-suggested-migration-methods"></a>Przegląd metod sugerowane migracji

1. Zanim będzie można pobrać metody sugerowane migracji, należy utworzyć projekt Azure migracji, Odkryj maszyny lokalnej, a następnie uruchom oceny migracji. [Dowiedz się więcej](tutorial-assessment-vmware.md).
2. Po utworzeniu oceny go wyświetlić, w projekcie > **omówienie** > **pulpitu nawigacyjnego**. Kliknij przycisk **oceny gotowości**

    ![Oceny gotowości](./media/tutorial-assessment-vmware/assessment-report.png)  

3. W **sugerowane narzędzia**, przeglądanie sugestii dla narzędzi można użyć do migracji.

    ![Sugerowane narzędzia](./media/tutorial-assessment-vmware/assessment-suitability.png) 




## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
