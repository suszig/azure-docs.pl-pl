---
title: "Co to jest usługa Site Recovery? | Microsoft Docs"
description: "Zawiera omówienie usługi Azure Site Recovery oraz podsumowanie scenariuszy wdrażania."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/06/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 7455d6f99ed8ceb401224f98105f7b651f55c724
ms.openlocfilehash: 98bf94960c39565243995a1c4dd45787478b8f40


---
# <a name="what-is-site-recovery"></a>Co to jest usługa Site Recovery?
Usługa Azure Site Recovery — Zapraszamy! Ten artykuł zawiera krótkie omówienie usługi Site Recovery.

Twoja organizacja wymaga strategii zapewniającej ciągłość biznesową i odzyskiwanie po awarii (BCDR, Business Continuity and Disaster Recovery), dzięki której aplikacje i dane będą bezpieczne i dostępne podczas planowanych lub nieplanowanych przestojów oraz jak najszybciej przywracane do normalnych warunków roboczych.

Usługa Site Recovery wspiera strategię zapewniającą ciągłość biznesową i odzyskiwanie po awarii dzięki zarządzaniu replikacją lokalnych maszyn wirtualnych i serwerów fizycznych. Serwery i maszyny wirtualne można replikować z głównego lokalnego centrum danych do chmury (Azure) lub do pomocniczego centrum danych.

W przypadku wystąpienia awarii w lokacji głównej następuje przejście do lokacji dodatkowej w trybie failover, dzięki czemu obciążenia są nadal dostępne. Powrót po awarii może mieć miejsce do lokalizacji głównej, gdy powróci ona do normalnego działania.

## <a name="site-recovery-in-the-azure-portal"></a>Usługa Site Recovery w portalu Azure
Platforma Azure oferuje dwa różne [modele wdrażania](../azure-resource-manager/resource-manager-deployment-model.md) związane z tworzeniem zasobów i pracą z nimi. Model oparty na usłudze Azure Resource Manager oraz klasyczny model zarządzania usługami. Platforma Azure ma dwie witryny Portal. [Klasyczną witrynę Azure Portal](https://manage.windowsazure.com/) i [witrynę Azure Portal](https://portal.azure.com).

* Usługa Site Recovery może zostać wdrożona zarówno w klasycznej witrynie Portal, jak i w witrynie Azure Portal.
* W klasycznej witrynie Azure Portal możliwe jest korzystanie z usługi Site Recovery przy użyciu klasycznego modelu zarządzania usługami.
* W witrynie Azure Portal można korzystać z wdrożeń opartych na modelu klasycznym lub na usłudze Resource Manager.

Informacje przedstawione w tym artykule dotyczą wdrożeń klasycznych i przy użyciu portalu Azure. Występujące różnice zostały odnotowane.

## <a name="why-deploy-site-recovery"></a>Dlaczego warto wdrożyć usługę Site Recovery?
Oto, co usługa Site Recovery może zrobić dla Twojej firmy:

* **Uproszczenie strategii BCDR** — może obsługiwać replikację, pracę w trybie failover i odzyskiwanie wielu obciążeń z poziomu jednej lokalizacji w witrynie Azure Portal. Usługa Site Recovery organizuje replikację i pracę w trybie failover, ale nie przechwytuje danych aplikacji.
* **Zapewnianie elastycznej replikacji** — istnieje możliwość replikowania obciążeń uruchomionych na obsługiwanych maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych VMware oraz serwerach fizycznych z systemem Windows/Linux.
* **Wyeliminowanie pomocniczego centrum danych**— obciążenia można replikować na platformie Azure zamiast w lokacji dodatkowej. Pozwala to wyeliminować koszty i ograniczyć złożoność obsługi pomocniczego centrum danych. Replikowane dane są przechowywane w usłudze Azure Storage z odpowiednią odpornością. Kiedy nastąpi przejście do trybu failover, maszyny wirtualne platformy Azure są tworzone przy użyciu replikowanych danych.
* **Łatwe przeprowadzanie testów replikacji** — korzystając z testowych trybów failover, można łatwo przetestować odzyskiwanie po awarii bez wpływu na środowiska produkcyjne.
* **Tryb failover i odzyskiwanie** — w przypadku przewidywanych przerw w działaniu można uruchomić planowane tryby failover bez utraty danych, a w przypadku nieoczekiwanych awarii — nieplanowane tryby failover, które mogą wiązać się z minimalną utratą danych (zależnie od częstotliwości replikacji). Kiedy lokacja główna będzie znowu dostępna, można do niej wrócić.
* **Wiele maszyn wirtualnych w trybie failover** — można skonfigurować plany odzyskiwania obejmujące skrypty i automatyzacje elementów Runbook platformy Azure. Plany odzyskiwania umożliwiają modelowanie i dostosowywanie trybu failover oraz odzyskiwanie aplikacji wielowarstwowych, które obejmują wiele maszyn wirtualnych.
* **Integracja z istniejącymi technologiami BCDR** — usługa Site Recovery współpracuje z innymi technologiami w ramach strategii BCDR. Usługi Site Recovery można na przykład używać, aby chronić zaplecze obciążeń firmowych programu SQL Server. Usługa oferuje natywne wsparcie funkcji AlwaysOn programu SQL Server, umożliwiając zarządzanie trybem failover dla grup dostępności.

## <a name="what-can-i-replicate"></a>Co mogę replikować?
Poniżej przedstawiono podsumowanie elementów, które można replikować przy użyciu usługi Site Recovery.

![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-overview/asr-overview-graphic.png)

| **REPLIKUJ** | **REPLIKUJ DO** |
| --- | --- |
| Lokalne maszyny wirtualne VMware |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Lokacja dodatkowa](site-recovery-vmware-to-vmware.md) |
| Lokalne maszyny wirtualne funkcji Hyper-V zarządzane w chmurach programu VMM |[Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Lokacja dodatkowa](site-recovery-vmm-to-vmm.md) |
| Lokalne maszyny wirtualne funkcji Hyper-V zarządzane w chmurach programu VMM, z magazynem SAN |[Lokacja dodatkowa](site-recovery-vmm-san.md) |
| Lokalne maszyny wirtualne funkcji Hyper-V, bez programu VMM |[Azure](site-recovery-hyper-v-site-to-azure.md) |
| Lokalne serwery fizyczne z systemem Windows/Linux |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Lokacja dodatkowa](site-recovery-vmware-to-vmware.md) |

## <a name="how-does-site-recovery-protect-workloads"></a>Jak usługa Site Recovery chroni obciążenia?
Usługa Site Recovery umożliwia replikację z uwzględnieniem aplikacji, dzięki czemu w razie wystąpienia awarii obciążenia i aplikacje będą nadal działać w spójny sposób.

* **Migawki spójne z aplikacjami** — maszyny są replikowane przy użyciu migawek spójnych z aplikacjami dla aplikacji jednej warstwy lub wielowarstwowych. Oprócz danych dyskowych migawki spójne z aplikacjami przechwytują wszystkie dane w pamięci i wszystkie trwające transakcje.
* **Replikacja bliska synchronicznej** — usługa Site Recovery umożliwia częstotliwość replikacji wynoszącą nawet 30 sekund w przypadku funkcji Hyper-V i replikację ciągłą w przypadku maszyn VMware.
* **Elastyczne plany odzyskiwania**— plany odzyskiwania można tworzyć i dostosowywać, używając skryptów zewnętrznych i ręcznie wykonywanych czynności. Integracja z elementami runbook usługi Azure Automation umożliwia odzyskanie całego stosu aplikacji jednym kliknięciem.
* **Integracja z funkcją Zawsze włączone programu SQL Server** — możesz zarządzać trybem failover grup dostępności przy użyciu planów odzyskiwania.
* **Biblioteka automatyzacji** — bogata biblioteka automatyzacji zapewnia gotowe do produkcji skrypty dopasowane do danych aplikacji, które można pobrać i zintegrować z usługą Site Recovery.
* **Proste zarządzanie siecią** — zaawansowane zarządzanie siecią w usłudze Site Recovery i na platformie Azure upraszcza wymagania sieciowe aplikacji, w tym rezerwowanie adresów IP, konfigurowanie równoważenia obciążenia i integrowanie usługi Azure Traffic Manager w celu zapewnienia efektywnych przełączeń sieci.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej w temacie [Jakie obciążenia może chronić usługa Site Recovery?](site-recovery-workload.md)
* Dowiedz się więcej o architekturze usługi Site Recovery w temacie [W jaki sposób działa usługa Site Recovery?](site-recovery-components.md)




<!--HONumber=Nov16_HO4-->


