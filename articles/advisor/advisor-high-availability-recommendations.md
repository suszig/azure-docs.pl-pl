---
title: "Zalecenia usługi Advisor wysokiej dostępności Azure | Dokumentacja firmy Microsoft"
description: "Za pomocą usługi Azure doradcy zwiększenia wysokiej dostępności Azure wdrożeń."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 7b63de2453180e562596c211d40cebe85b95bd54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-high-availability-recommendations"></a>Zalecenia doradcy w zakresie wysokiej dostępności

Azure Advisor pomaga zapewnić i zapewnić ciągłość aplikacji biznesowych o znaczeniu krytycznym. Można uzyskać wysoką dostępność zalecenia usługi Advisor z **wysokiej dostępności** pulpitu nawigacyjnego usługi Advisor.

![Wysoka dostępność przycisk na pulpicie nawigacyjnym usługi Advisor](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Zapewnić odporność na uszkodzenia maszyny wirtualnej

Klasyfikator identyfikuje maszyn wirtualnych, które nie są częścią zestawu dostępności i zaleca przenoszenia ich do zestawu dostępności. Aby zapewnić nadmiarowość aplikacji, zalecamy grupowanie co najmniej dwóch maszyn wirtualnych w zestawie dostępności. Ta konfiguracja temu podczas albo planowanego lub nieplanowanego zdarzenia konserwacji, co najmniej jednej maszyny wirtualnej jest dostępna i spełnia umowy SLA dla maszyny wirtualnej platformy Azure. Możesz utworzyć zbiór dostępności dla maszyny wirtualnej lub Dodaj maszynę wirtualną do istniejącego zestawu dostępności.

> [!NOTE]
> Jeśli wybierzesz opcję utworzenia zestawu dostępności, należy dodać do niego co najmniej jednej maszyny wirtualnej więcej. Zaleca się tego grupowanie co najmniej dwie maszyny wirtualne w dostępności ustawioną upewnij się, że co najmniej jedna maszyna jest dostępna podczas wystąpienia awarii.

![Zalecenia usługi Advisor: nadmiarowości maszyny wirtualnej, użyj zestawów dostępności](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Upewnij się, że odporność na uszkodzenia zestawu dostępności 

Klasyfikator identyfikuje zestawów dostępności, które zawierają jednej maszyny wirtualnej i zaleca dodanie do niej co najmniej jednej maszyny wirtualnej. Aby zapewnić nadmiarowość aplikacji, zalecamy grupowanie co najmniej dwóch maszyn wirtualnych w zestawie dostępności. Ta konfiguracja temu podczas albo planowanego lub nieplanowanego zdarzenia konserwacji, co najmniej jednej maszyny wirtualnej jest dostępna i spełnia umowy SLA dla maszyny wirtualnej platformy Azure. Można albo można utworzyć maszyny wirtualnej lub użyj istniejącej maszyny wirtualnej i dodaj go do zestawu dostępności.  

![Zalecenia usługi Advisor: Dodaj co najmniej jednej maszyny wirtualnej do tego zestawu dostępności](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Zapewnić odporność na uszkodzenia bramy aplikacji
Aby zapewnić ciągłość kluczowych aplikacji, które są obsługiwane przez usługę bramy aplikacji, usługi Advisor identyfikuje wystąpień bramy aplikacji, które nie są skonfigurowane na uszkodzenia, i sugeruje akcji korygowania, które należy wykonać. Średnich i dużych pojedyncze wystąpienie aplikacji bramy identyfikuje Advisor i zaleca się dodawania co najmniej jedno wystąpienie więcej. Również instance jednego lub kilku aplikacji małych bramy identyfikuje i zaleca migracji do średnich i dużych jednostki SKU. Klasyfikator zaleca te akcje, aby upewnić się, że z wystąpień bramy aplikacji są skonfigurowane do spełnia bieżące wymagania umowy SLA dla tych zasobów.

![Zalecenia usługi Advisor: Wdrażanie dwóch lub więcej wystąpień bramy średnich i dużych aplikacji o określonym rozmiarze](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Zwiększyć wydajność i niezawodność dysków maszyny wirtualnej

Klasyfikator identyfikuje maszyn wirtualnych przy użyciu standardowych dysków i zaleca się uaktualniania do dysków premium.
 
Usługa Azure Premium Storage oferuje obsługę dysków o wysokiej wydajności i małych opóźnieniach maszyn wirtualnych uruchomionych obciążeń/O wykonujących. Dyski maszyn wirtualnych, które używają konta premium magazynu przechowywania danych na dyskach półprzewodnikowych (SSD). Aby uzyskać najlepszą wydajność aplikacji zaleca się przeprowadzenie migracji wszystkich dysków maszyny wirtualnej wymagające wysokiej wartość IOPS dla magazyn w warstwie premium. 

Jeśli dyski nie wymagają wysokiej IOPS, można ograniczyć koszty, przechowując ich magazynu w warstwie standardowa. Standardowe magazynu przechowuje dane dysku maszyny wirtualnej na stacje dysków twardych (HDD) zamiast dysków SSD. Można przeprowadzić migrację dysków maszyny wirtualnej do dysków premium. Dyski w warstwie Premium są obsługiwane w większości maszyny wirtualnej jednostki SKU. Jednak w niektórych przypadkach, jeśli chcesz używać dysków premium, może być konieczne uaktualnienie maszyny wirtualnej jednostki SKU również.

![Zalecenia usługi Advisor: zwiększyć niezawodność dysków maszyny wirtualnej przez uaktualnienie do dysków w warstwie premium](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Ochrona danych maszyny wirtualnej przed przypadkowym usunięciem
Klasyfikator identyfikuje maszyny wirtualne, których kopia zapasowa nie jest włączona i zaleca się włączenie kopii zapasowej. Konfigurowanie kopii zapasowej maszyny wirtualnej zapewnia dostępność danych biznesowych o znaczeniu krytycznym i zapewnia ochronę przed przypadkowym usunięciem lub uszkodzenia.

![Zalecenia usługi Advisor: Skonfiguruj kopia zapasowa maszyny wirtualnej, aby chronić dane krytycznym](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Zalecenia dotyczące wysokiej dostępności dostępu klasyfikatora

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W okienku po lewej stronie kliknij **więcej usług**.

3. W okienku usługi menu w obszarze **monitorowanie i zarządzanie**, kliknij przycisk **Azure Advisor**.  
 Zostanie wyświetlony pulpit nawigacyjny usługi Advisor.

4. Na pulpicie nawigacyjnym usługi Advisor, kliknij przycisk **wysokiej dostępności** , a następnie wybierz subskrypcję, dla którego chcesz otrzymywać zalecenia.

> [!NOTE]
> Aby uzyskać dostęp do zalecenia doradcy w zakresie, należy najpierw *zarejestrować swoją subskrypcję* usłudze Advisor. Subskrypcja jest zarejestrowana przy *właściciela subskrypcji* uruchamia pulpitu nawigacyjnego usługi Advisor i klika pozycję **Uzyskaj zalecenia** przycisk. Jest to *jednorazowa operacja*. Po zarejestrowaniu subskrypcji są dostępne zalecenia doradcy w zakresie jako *właściciela*, *współautora*, lub *czytnika* dla określonego zasobu, grupy zasobów lub subskrypcji.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących zalecenia doradcy w zakresie zobacz:
* [Wprowadzenie do usługi Advisor Azure](advisor-overview.md)
* [Wprowadzenie do usługi Advisor](advisor-get-started.md)
* [Zalecenia doradcy w zakresie koszt](advisor-performance-recommendations.md)
* [Zalecenia doradcy w zakresie wydajności](advisor-performance-recommendations.md)
* [Zalecenia doradcy w zakresie zabezpieczeń](advisor-security-recommendations.md)

