---
title: "Docelowe aktualizacje przy użyciu kolekcji programu SCCM w zarządzaniu aktualizacjami pakietu OMS | Microsoft Docs"
description: "Ten artykuł ma na celu ułatwienie konfigurowania programu System Center Configuration Manager za pomocą tego rozwiązania do zarządzania aktualizacjami komputerów zarządzanych przez program SCCM."
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.openlocfilehash: ec97432cd14c6289928f0419c242e1ccc2c8d876
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="integrate-system-center-configuration-manager-with-oms-update-management"></a>Integracja programu System Center Configuration Manager z programem OMS Update Management

Klienci, którzy zainwestowali w program System Center Configuration Manager do zarządzania komputerami, serwerami i urządzeniami przenośnymi polegają również na ich sile i dojrzałości w zarządzania aktualizacjami oprogramowania w ramach cyklu zarządzania aktualizacjami oprogramowania (SUM).  

Opierając się na istniejącej dzisiaj integracji między pakietem OMS a programem Configuration Manager, możesz zgłaszać i aktualizować zarządzane serwery z systemem Windows, tworząc i wstępnie przemieszczając wdrożenia aktualizacji oprogramowania w programie Configuration Manager, oraz uzyskać szczegółowy stan ukończonych wdrożeń aktualizacji przy użyciu [rozwiązania do zarządzania aktualizacjami](../operations-management-suite/oms-solution-update-management.md). Jeśli używasz programu Configuration Manager do raportowania zgodności aktualizacji, ale nie do zarządzania wdrożeniami aktualizacji za pomocą serwerów z systemem Windows, możesz kontynuować raportowanie do programu Configuration Manager, gdy aktualizacje zabezpieczeń są zarządzane za pomocą rozwiązania do zarządzania aktualizacjami pakietu OMS.

## <a name="prerequisites"></a>Wymagania wstępne

* [Rozwiązanie do zarządzania aktualizacjami](../operations-management-suite/oms-solution-update-management.md) musi być dodane do obszaru roboczego usługi Log Analytics i połączone razem z Twoim kontem usługi Automation w tej samej grupie zasobów i regionie.   
* Serwery z systemem Windows obecnie zarządzane przez środowisko programu System Center Configuration Manager również muszą się zgłaszać do obszaru roboczego usługi Log Analytics, która także ma włączone rozwiązanie zarządzania aktualizacjami.  
* Ta funkcja obsługuje program System Center Configuration Manager w wersji Current Branch 1606 i wyższej.  Aby zintegrować swoją centralną lokację administracyjną lub autonomiczną lokację główną programu Configuration Manager z usługą Log Analytics i kolekcjami importowania, zobacz [Connect Configuration Manager to Log Analytics](../log-analytics/log-analytics-sccm.md) (Łączenie programu Configuration Manager z usługą Log Analytics).  
* Agenci dla systemu Windows muszą być skonfigurowani do komunikowania się z serwerem Windows Server Update Services (WSUS) albo mieć dostęp do usługi Microsoft Update, jeśli nie otrzymują aktualizacji zabezpieczeń z programu Configuration Manager.   

To, w jaki sposób zarządzasz klientami hostowanymi w usłudze Azure IaaS za pomocą istniejącego środowiska programu Configuration Manager, zależy głównie od połączenia istniejącego między centrami danych platformy Azure a Twoją infrastrukturą. Połączenie to ma wpływ na wszelkie zmiany projektowe, których wprowadzenie do infrastruktury programu Configuration Manager może być konieczne, oraz wpływa na powiązane koszty obsługi tych niezbędnych zmian.  Aby zrozumieć, jakie kwestie planowania należy ocenić przed kontynuowaniem, zobacz [Menedżer konfiguracji Azure — często zadawane pytania](https://docs.microsoft.com/sccm/core/understand/configuration-manager-on-azure#networking).    

## <a name="configuration"></a>Konfiguracja

### <a name="manage-software-updates-from-configuration-manager"></a>Zarządzanie aktualizacjami oprogramowania z programu Configuration Manager 

Jeśli chcesz kontynuować zarządzanie wdrożeniami aktualizacji z programu Configuration Manager, wykonaj następujące kroki.  Pakiet OMS łączy się z programem Configuration Manager w celu zastosowania aktualizacji na komputerach klienckich podłączonych do Twojego obszaru roboczego usługi Log Analytics. Zawartość aktualizacji jest dostępna w pamięci podręcznej komputera klienckiego tak, jakby wdrożenie było zarządzane przez program Configuration Manager.  

1. Tworzenie wdrożenia aktualizacji oprogramowania z lokacji najwyższego poziomu w hierarchii programu Configuration Manager przy użyciu procesu opisanego w [procesie wdrażania aktualizacji oprogramowania](https://docs.microsoft.com/sccm/sum/deploy-use/deploy-software-updates).  Jedynym ustawieniem, które musi być skonfigurowane inaczej niż dla standardowego wdrożenia, jest opcja **Nie instaluj aktualizacji oprogramowania** służąca do sterowania zachowaniem pakietu wdrożeniowego podczas pobierania. To zachowanie jest zarządzane przez rozwiązanie do zarządzania aktualizacjami pakietu OMS poprzez utworzenie planowego wdrożenia aktualizacji w następnym kroku.  

1. W portalu pakietu OMS otwórz pulpit nawigacyjny zarządzania aktualizacjami.  Utwórz nowe wdrożenie, postępując zgodnie z krokami opisanymi w temacie [Creating an Update Deployment](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment) (Tworzenie wdrożenia aktualizacji), i wybierz z listy rozwijanej odpowiednią kolekcję programu Configuration Manager reprezentowaną jako grupa komputerów pakietu OMS.  Należy pamiętać o następujących ważnych kwestiach:
    1. Jeśli okno obsługi jest zdefiniowane w wybranej kolekcji urządzeń programu Configuration Manager, składowe kolekcji uznają je zamiast ustawienia **Czas trwania** zdefiniowanego w zaplanowanym wdrożeniu w pakiecie OMS.
    1. Składowe kolekcji docelowej muszą mieć połączenie z Internetem (bezpośrednio, za pośrednictwem serwera proxy albo za pośrednictwem bramy pakietu OMS).  

Po zakończeniu wdrażania aktualizacji za pomocą rozwiązania pakietu OMS docelowe komputery będące członkami wybranej grupy komputerów zainstalują aktualizacje o zaplanowanym czasie z ich lokalnej pomięci podręcznej klienta.  Możesz [wyświetlić stan wdrożenia aktualizacji](../operations-management-suite/oms-solution-update-management.md#viewing-update-deployments), aby monitorować wyniki swojego wdrożenia.  


### <a name="manage-software-updates-from-oms"></a>Zarządzanie aktualizacjami oprogramowania za pomocą pakietu OMS

Do zarządzania aktualizacjami maszyn wirtualnych z systemem Windows Server, które są klientami programu Configuration Manager, należy skonfigurować zasady klienta, aby wyłączyć funkcję zarządzania aktualizacjami oprogramowania dla wszystkich klientów zarządzanych przez to rozwiązanie.  Domyślnie ustawienia klienta dotyczą wszystkich urządzeń w hierarchii.  Aby uzyskać więcej informacji na temat tego ustawienia, zasad i sposobu ich konfigurowania, przejrzyj artykuł [Jak skonfigurować ustawienia klienta w programie System Center Configuration Manager](https://docs.microsoft.com/sccm/core/clients/deploy/configure-client-settings).  

Po wprowadzeniu tej zmiany konfiguracji utwórz nowe wdrożenie, postępując zgodnie z krokami opisanymi w temacie [Creating an Update Deployment](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment) (Tworzenie wdrożenia aktualizacji), i wybierz z listy rozwijanej odpowiednią kolekcję programu Configuration Manager reprezentowaną jako grupa komputerów pakietu OMS. 

