<properties
    pageTitle="Co to jest usługa Site Recovery? | Microsoft Azure" 
    description="Zawiera omówienie usługi Azure Site Recovery i wyjaśnia sposób wdrażania usługi." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="02/22/2016" 
    ms.author="raynew"/>

#  Co to jest usługa Site Recovery?

Azure Site Recovery — Zapraszamy! Rozpocznij od tego artykułu, aby uzyskać szybkie omówienie usługi Site Recovery oraz sposobu, w jaki usługa może wnieść wkład w Twoją strategię dotyczącą ciągłości biznesowej i odzyskiwania po awarii (BCDR).

Platforma Azure ma dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [Resource Manager i model klasyczny](../resource-manager-deployment-model.md). Ten artykuł dotyczy obu modeli. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager.

## Omówienie

Ważną częścią strategii BCDR organizacji stanowi poznanie sposobu na utrzymanie działających obciążeń i aplikacji firmowych podczas zaplanowanych przerw i nieplanowanych awarii.

Usługa Site Recovery pomaga w tym procesie, organizując replikację, tryb failover i odzyskiwanie obciążeń i aplikacji, aby były dostępne z lokalizacji dodatkowej, jeśli lokalizacja główna przestanie działać. 

## Dlaczego warto używać usługi Site Recovery? 

Oto, co usługa Site Recovery może zrobić dla Twojej firmy:

- **Uproszczenie strategii BCDR** — usługa Site Recovery ułatwia obsługę replikacji, trybu failover i odzyskiwania wielu firmowych obciążeń i aplikacji z jednej lokalizacji. Usługa Site Recovery organizuje replikację i tryb failover, ale nie przechwytuje danych aplikacji ani nie zbiera żadnych informacji ich dotyczących.
- **Zapewnianie elastycznej replikacji** — przy użyciu usługi Site Recovery możesz replikować obciążenia uruchomione na maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych VMware oraz serwerach fizycznych systemu Windows/Linux. 
- **Łatwe przechodzenie do trybu failover i odzyskiwanie** — usługa Site Recovery umożliwia skorzystanie z testowych trybów failover, które wspierają testowanie odzyskiwania po awarii bez wywierania wpływu na środowiska produkcyjne. Możesz również uruchomić planowane tryby failover (brak utraty danych) w przypadku przewidywanych przerw w działaniu lub nieplanowane tryby failover (minimalna utrata danych, zależna od częstotliwości replikacji) w przypadku nieoczekiwanych awarii. Po przejściu w tryb failover możesz powrócić do lokacji głównych po awarii. Usługa Site Recovery zapewnia plany odzyskiwania, które mogą uwzględniać skrypty i skoroszyty automatyzacji platformy Azure. Dzięki nim możesz dostosować tryb failover i odzyskiwanie dla aplikacji wielowarstwowych. 
- **Wyeliminowanie dodatkowego centrum danych** — możesz przeprowadzać replikację do dodatkowej lokacji lokalnej lub do platformy Azure. Użycie platformy Azure jako miejsca docelowego dla odzyskiwania po awarii eliminuje koszty i złożone operacje związane z utrzymywaniem lokacji dodatkowej oraz umożliwia przechowywanie replikowanych danych w usłudze Azure Storage, ze wszystkimi opcjami zapewniającymi odporność.
- **Integracja z istniejącymi technologiami BCDR** — usługa Site Recovery współpracuje z innymi funkcjami BCDR aplikacji. Przykładowo możesz użyć usługi Site Recovery, aby ochronić zaplecze programu SQL Server obciążeń firmowych. Usługa obejmuje natywne wsparcie dla funkcji AlwaysOn programu SQL Server, umożliwiając zarządzanie trybem failover dla grup dostępności. 

## Co mogę replikować?

Poniżej przedstawiono podsumowanie elementów, które może replikować usługa Site Recovery.

![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLIKACJA** | **REPLIKACJA Z** | **REPLIKACJA DO** | **ARTYKUŁ**
---|---|---|---
Obciążenia uruchomione na maszynach wirtualnych VMware | Lokalny serwer VMware | Azure Storage | [Wdrożenie](site-recovery-vmware-to-azure-classic.md)
Obciążenia uruchomione na maszynach wirtualnych VMware | Lokalny serwer VMware | Lokacja dodatkowa VMware | [Wdrożenie](site-recovery-vmware-to-vmware.md) 
Obciążenia uruchomione na maszynach wirtualnych funkcji Hyper-V | Lokalny serwer hosta funkcji Hyper-V w chmurze VMM | Azure Storage | [Wdrożenie](site-recovery-vmm-to-azure.md)
Obciążenia uruchomione na maszynach wirtualnych funkcji Hyper-V | Lokalny serwer hosta funkcji Hyper-V w chmurze VMM | Lokacja dodatkowa programu VMM | [Wdrożenie](site-recovery-vmm-to-vmm.md)
Obciążenia uruchomione na maszynach wirtualnych funkcji Hyper-V | Lokalny serwer hosta funkcji Hyper-V w chmurze VMM z magazynem sieci SAN| Lokacja dodatkowa programu VMM z magazynem sieci SAN | [Wdrożenie](site-recovery-vmm-san.md)
Obciążenia uruchomione na maszynach wirtualnych funkcji Hyper-V | Lokalna lokacja funkcji Hyper-V (bez VMM) | Azure Storage | [Wdrożenie](site-recovery-hyper-v-site-to-azure.md)
Obciążenia uruchomione na serwerach fizycznych systemu Windows/Linux | Lokalny serwer fizyczny | Azure Storage | [Wdrożenie](site-recovery-vmware-to-azure-classic.md)
Obciążenia uruchomione na serwerach fizycznych systemu Windows/Linux | Lokalny serwer fizyczny | Dodatkowe centrum danych | [Wdrożenie](site-recovery-vmware-to-vmware.md) 


## Jakie obciążenia mogę chronić?

Usługa Site Recovery może pomóc w stosowaniu strategii BCDR obejmującej aplikacje, dzięki czemu obciążenia i aplikacje będą nadal działać w spójny sposób w przypadku wystąpienia awarii. Usługa Site Recovery zapewnia: 

- **Migawki spójne z aplikacjami** — replikacja przy użyciu migawek spójnych z aplikacjami dla aplikacji jednej warstwy lub n-warstwowych.
**Niemal synchroniczna replikacja** — częstotliwość replikacji nawet do 30 sekund w przypadku funkcji Hyper-V i replikacja ciągła w przypadku maszyn VMware.
**Integracja z funkcją AlwaysOn programu SQL Server** — możesz zarządzać trybem failover grup dostępności w planach odzyskiwania usługi Site Recovery. 
- **Elastyczne plany odzyskiwania** — możesz tworzyć i dostosowywać plany odzyskiwania z wykorzystaniem zewnętrznych skryptów, działań ręcznych i elementów runbook usługi Azure Automation, które umożliwiają odzyskanie całego stosu aplikacji za pomocą jednego kliknięcia.
- **Biblioteka automatyzacji** — bogata biblioteka automatyzacji zapewnia gotowe do produkcji skrypty dopasowane do danych aplikacji, które można pobrać i zintegrować z usługą Site Recovery. 
-**Proste zarządzanie siecią** — zaawansowane zarządzanie siecią w usłudze Site Recovery i na platformie Azure upraszcza wymagania sieciowe aplikacji, w tym rezerwowanie adresów IP, konfigurowanie równoważenia obciążenia i integrowanie usługi Azure Traffic Manager w celu zapewniania efektywnych przełączeń sieci.


## Następne kroki

- Dowiedz się więcej w temacie [Jakie obciążenia może chronić usługa Site Recovery?](site-recovery-workload.md)
- Dowiedz się więcej o architekturze usługi Site Recovery w temacie [W jaki sposób działa usługa Site Recovery?](site-recovery-components.md)
 



<!--HONumber=Jun16_HO2-->


