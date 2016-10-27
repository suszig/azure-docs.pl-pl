<properties
    pageTitle="Co to jest usługa Site Recovery? | Microsoft Azure"
    description="Zawiera omówienie usługi Azure Site Recovery oraz podsumowanie scenariuszy wdrażania."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>


#  <a name="what-is-site-recovery?"></a>Co to jest usługa Site Recovery?

Azure Site Recovery — Zapraszamy! Ten artykuł zawiera krótkie omówienie usługi Site Recovery oraz sposobu, w jaki ułatwia ona działanie firmy.

Twoja organizacja wymaga strategii zapewniającej ciągłość biznesową i odzyskiwanie po awarii (BCDR, Business Continuity and Disaster Recovery), która zapewnia, że aplikacje, obciążenia i dane są bezpieczne i pozostają dostępne podczas planowanych lub nieplanowanych przestojów oraz są jak najszybciej przywracane do normalnych warunków roboczych. Usługa Site Recovery na platformie Azure ułatwia realizację tej strategii.

Usługa Site Recovery organizuje replikację obciążeń działających lokalnie serwerów fizycznych i maszyn wirtualnych. Serwery i maszyny wirtualne można replikować z głównego centrum danych do chmury (Azure) lub pomocniczego centrum danych. W przypadku wystąpienia awarii w lokacji głównej następuje przejście do lokacji dodatkowej w trybie failover, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót do lokalizacji głównej następuje wtedy, gdy powróci ona do normalnego działania.

## <a name="site-recovery-in-the-azure-portal"></a>Usługa Site Recovery w portalu Azure

Platforma Azure oferuje dwa różne [modele wdrażania](../resource-manager-deployment-model.md) związane z tworzeniem zasobów i pracą z nimi. Model oparty na usłudze Azure Resource Manager oraz klasyczny model zarządzania usługami. Platforma Azure ma dwa portale — [klasyczny portal Azure](https://manage.windowsazure.com/), który obsługuje klasyczny model wdrażania, oraz witrynę [Azure Portal](https://portal.azure.com), która obsługuje zarówno model klasyczny, jak i model oparty na usłudze Resource Manager.

- Usługa Site Recovery jest dostępna zarówno w portalu klasycznym, jak i w portalu Azure.
- W klasycznym portalu Azure można korzystać z usługi Site Recovery przy użyciu klasycznego modelu zarządzania usługami.
- W witrynie Azure Portal można korzystać z wdrożeń opartych na modelu klasycznym lub na usłudze Resource Manager. 

Informacje przedstawione w tym artykule dotyczą wdrożeń klasycznych i przy użyciu portalu Azure. Różnice są odnotowane wedle potrzeby.


## <a name="why-deploy-site-recovery?"></a>Dlaczego warto wdrożyć usługę Site Recovery?

Oto, co usługa Site Recovery może zrobić dla Twojej firmy:

- **Uproszczenie strategii BCDR**— może obsługiwać replikację, pracę w trybie failover i odzyskiwanie wielu obciążeń w jednej lokalizacji w witrynie Azure Portal. Usługa Site Recovery organizuje replikację i tryb failover, ale nie przechwytuje danych aplikacji ani nie zbiera żadnych informacji ich dotyczących.
- **Zapewnianie elastycznej replikacji** — przy użyciu usługi Site Recovery można replikować obciążenia uruchomione na obsługiwanych maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych VMware oraz serwerach fizycznych systemu Windows/Linux.
- **Łatwe przeprowadzanie testów replikacji** — usługa Site Recovery umożliwia skorzystanie z testowych trybów failover, które umożliwiają przetestowanie odzyskiwania po awarii bez wywierania wpływu na środowiska produkcyjne.
- **Tryb failover i odzyskiwanie** — wykonywać można zaplanowane działania w trybie failover, które odbywają się w przypadku przewidywanych przerw w działaniu i nie powodują utraty danych, lub nieplanowane działania w trybie failover, które mogą wiązać się z minimalną utratą danych (zależnie od częstotliwości replikacji) i odbywać się w razie nieoczekiwanych awarii. Po przejściu do trybu failover można powrócić do lokacji głównych. Usługa Site Recovery zapewnia plany odzyskiwania, które mogą uwzględniać skrypty i skoroszyty automatyzacji platformy Azure. Dzięki nim można dostosować tryb failover i odzyskiwanie aplikacji wielowarstwowych.
- **Wyeliminowanie pomocniczego centrum danych**— obciążenia można replikować na platformie Azure zamiast w lokacji dodatkowej. Pozwala to wyeliminować koszty i ograniczyć złożoność obsługi pomocniczego centrum danych. Replikowane dane są przechowywane w usłudze Azure Storage z maksymalną dostępną odpornością. Po przejściu do trybu failover maszyny wirtualne są tworzone z replikowanych danych.
- **Integracja z istniejącymi technologiami BCDR** — usługa Site Recovery współpracuje z innymi funkcjami strategii BCDR. Usługi Site Recovery można na przykład używać, aby chronić zaplecze obciążeń firmowych programu SQL Server. Usługa oferuje natywne wsparcie funkcji AlwaysOn programu SQL Server, umożliwiając zarządzanie trybem failover dla grup dostępności.

## <a name="what-can-i-replicate?"></a>Co mogę replikować?

Poniżej przedstawiono podsumowanie elementów, które można replikować przy użyciu usługi Site Recovery.

![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLIKUJ** | **REPLIKUJ DO** 
---|---
Obciążenia uruchomione na lokalnych maszynach wirtualnych VMware | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Lokacja dodatkowa](site-recovery-vmware-to-vmware.md)
Obciążenia uruchomione na lokalnych maszynach wirtualnych funkcji Hyper-V zarządzanych w chmurach programu VMM  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Lokacja dodatkowa](site-recovery-vmm-to-vmm.md) 
Obciążenia uruchomione na lokalnych maszynach wirtualnych funkcji Hyper-V zarządzanych w chmurach programu VMM, z magazynem SAN|  [Lokacja dodatkowa](site-recovery-vmm-san.md)
Obciążenia uruchomione na lokalnych maszynach wirtualnych funkcji Hyper-V, bez programu VMM | [Azure](site-recovery-hyper-v-site-to-azure.md)
Obciążenia uruchomione na lokalnych serwerach fizycznych systemu Windows/Linux | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Lokacja dodatkowa](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect?"></a>Jakie obciążenia mogę chronić?

Usługa Site Recovery umożliwia realizację strategii BCDR obejmującej aplikacje, dzięki czemu w razie wystąpienia awarii obciążenia i aplikacje będą nadal działać w spójny sposób. Usługa Site Recovery zapewnia:

- **Migawki spójne z aplikacjami** — maszyny są replikowane przy użyciu migawek spójnych z aplikacjami dla aplikacji jednej warstwy lub n-warstwowych. Oprócz danych dyskowych migawki spójne z aplikacjami przechwytują wszystkie dane w pamięci i wszystkie trwające transakcje.
- **Replikacja bliska synchronicznej** — usługa Site Recovery umożliwia częstotliwość replikacji nawet do 30 sekund w przypadku funkcji Hyper-V i replikację ciągłą w przypadku maszyn VMware.
- **Elastyczne plany odzyskiwania**— plany odzyskiwania można tworzyć i dostosowywać, używając skryptów zewnętrznych i ręcznie wykonywanych czynności. Integracja z elementami runbook usługi Azure Automation umożliwia odzyskanie całego stosu aplikacji jednym kliknięciem.
- **Integracja z funkcją AlwaysOn programu SQL Server** — możesz zarządzać trybem failover grup dostępności w planach odzyskiwania usługi Site Recovery.
- **Biblioteka automatyzacji** — bogata biblioteka automatyzacji zapewnia gotowe do produkcji skrypty dopasowane do danych aplikacji, które można pobrać i zintegrować z usługą Site Recovery.
- **Proste zarządzanie siecią** — zaawansowane zarządzanie siecią w usłudze Site Recovery i na platformie Azure upraszcza wymagania sieciowe aplikacji, w tym rezerwowanie adresów IP, konfigurowanie równoważenia obciążenia i integrowanie usługi Azure Traffic Manager w celu zapewnienia efektywnych przełączeń sieci.


## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej w temacie [Jakie obciążenia może chronić usługa Site Recovery?](site-recovery-workload.md)
- Dowiedz się więcej o architekturze usługi Site Recovery w temacie [W jaki sposób działa usługa Site Recovery?](site-recovery-components.md)
 



<!--HONumber=Oct16_HO3-->


