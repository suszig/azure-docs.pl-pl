---
title: "Tworzenie planów odzyskiwania dla trybu failover i odzyskiwania w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Opisuje, jak tworzyć i dostosowywać plany odzyskiwania w programie Azure Site Recovery w tryb failover i odzyskiwania maszyn wirtualnych i serwerów fizycznych"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/25/2017
ms.author: raynew
ms.openlocfilehash: 202e0ac8be36e9156ec16fadc1b722f4eb3d1432
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2017
---
# <a name="create-recovery-plans"></a>Tworzenie planów odzyskiwania


Ten artykuł zawiera informacje na temat tworzenia i dostosowywania planów odzyskiwania w [usługi Azure Site Recovery](site-recovery-overview.md).

Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Utworzyć plany odzyskiwania, aby wykonać następujące czynności:

* Definiowanie grup komputerów, które w tryb failover ze sobą, a następnie uruchom ze sobą.
* Model zależności między komputerami, grupując je do odzyskiwania Zaplanuj grupy. Na przykład do pracy awaryjnej i wyświetlić określonej aplikacji, możesz grupy wszystkich maszyn wirtualnych dla tej aplikacji w tej samej grupie planu odzyskiwania.
* Uruchom tryb failover. Można uruchomić testu, planowane, ani nieplanowane przełączenie awaryjne, w ramach planu odzyskiwania.


## <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

1. Kliknij przycisk **planów odzyskiwania** > **utworzenie planu odzyskiwania**.
   Określ nazwę planu odzyskiwania i źródłowym i docelowym. Lokalizacja źródłowa musi mieć maszyn wirtualnych, które są włączone dla trybu failover i odzyskiwania.

    - Dla programu VMM do replikacji programu VMM, wybierz **typ źródła** > **VMM**i serwerach VMM źródłowych i docelowych. Kliknij przycisk **funkcji Hyper-V** wyświetlić chmury, które są chronione.
    - Dla programu VMM do platformy Azure, wybierz **typ źródła** > **VMM**.  Wybierz źródłowy serwer VMM i **Azure** jako element docelowy.
    - W przypadku replikacji funkcji Hyper-V do platformy Azure (bez VMM) wybierz **typ źródła** > **lokacji funkcji Hyper-V**. Wybierz lokację jako źródła, a **Azure** jako element docelowy.
    - Dla maszyny Wirtualnej VMware lub serwera lokalnego fizycznych do platformy Azure jako źródła, wybierz serwer konfiguracji i **Azure** jako element docelowy.
    - Plan odzyskiwania Azure do platformy Azure wybierz region platformy Azure jako źródło i dodatkowej region platformy Azure jako element docelowy. Dodatkowej regiony platformy Azure są tylko te, dla których są chronione maszyny wirtualne.
2. W **wybierz maszyny wirtualne**, wybierz maszyny wirtualne (lub grupa replikacji), który ma zostać dodany do domyślnej grupy (Grupa 1) w planie odzyskiwania.

## <a name="customize-and-extend-recovery-plans"></a>Dostosowywanie i rozszerzanie planów odzyskiwania

Można dostosować i rozszerzyć planów odzyskiwania:

- **Dodaj nowe grupy**— Dodawanie grup planu odzyskiwania dodatkowe (maksymalnie 7) do domyślnej grupy, a następnie dodaj więcej komputerów lub grup replikacji dla grup planu odzyskiwania. Grupy są numerowane w kolejności, w którym dodać. Maszyny wirtualnej lub grupy replikacji tylko mogą zostać włączone w grupie planu odzyskiwania jednego.
- **Dodaj akcję ręczną**— można dodać ręczne akcje, które są uruchamiane przed lub po grupie planu odzyskiwania. Po uruchomieniu planu odzyskiwania przestaje w momencie, jaką dodaje akcji ręcznej. Okno dialogowe zostanie wyświetlony monit o określenie, że akcja ręczna została zakończona.
- **Dodawanie skryptu**— można dodać skryptów uruchamianych przed lub po grupie planu odzyskiwania. Po dodaniu skrypt dodaje nowy zestaw akcje dla grupy. Na przykład, zostanie utworzony zestaw kroków wstępne 1 grupy o nazwie: Grupa 1: wstępne czynności. Wszystkie kroki przed będzie wyświetlane w tym zestawie. Skrypt można dodać w lokacji głównej, jeśli serwer VMM wdrożone.
- **Dodaj elementy runbook Azure**— można rozszerzyć plany odzyskiwania z elementów runbook platformy Azure. Na przykład w celu zautomatyzowania zadań lub utworzyć pojedynczy krok odzyskiwania. [Dowiedz się więcej](site-recovery-runbook-automation.md).

## <a name="add-scripts"></a>Dodawanie skryptów

Można użyć skryptów środowiska PowerShell w planów odzyskiwania.

 - Upewnij się, że skrypty używać bloków try-catch, tak aby bezpiecznie obsługi wyjątków.
    - W przypadku wyjątku w skrypcie, przestaje działać, i pokazuje zadania nie powiodło się.
    - Jeśli wystąpi błąd, pozostała część skrypt nie działa.
    - Jeśli błąd wystąpi po uruchomieniu nieplanowanego trybu failover, nadal planu odzyskiwania.
    - Jeśli wystąpi błąd podczas uruchamiania planowanego trybu failover, zatrzymuje planu odzyskiwania. Należy naprawić skryptu, sprawdź, czy działa zgodnie z oczekiwaniami, a następnie uruchom odzyskiwanie Zaplanuj ponownie.
- Polecenie Write-Host nie działa w skryptu planu odzyskiwania, a skrypt zakończy się niepowodzeniem. Aby utworzyć dane wyjściowe, należy utworzyć skrypt serwera proxy, który z kolei uruchamia skrypt głównego. Upewnij się, że wszystkie dane wyjściowe jest przekazywany w potoku się przy użyciu >> polecenia.
  * Limit czasu skryptu, jeśli nie zwraca w 600 sekund.
  * Jeśli wszystko jest zapisywane do strumienia wyjściowego STDERR, skrypt jest klasyfikowany jako zakończony niepowodzeniem. Te informacje są wyświetlane w szczegółach wykonanie skryptu.

Jeśli używasz programu VMM w ramach wdrożenia:

* Skrypty w planie odzyskiwania są uruchamiane w kontekście konta usługi VMM. Upewnij się, że to konto ma uprawnienia do odczytu do udziału zdalnego, na którym zlokalizowany jest skrypt. Testowanie skryptu do uruchomienia na poziomie uprawnień konta usługi programu VMM.
* Poleceń cmdlet programu VMM są dostarczane w moduł programu Windows PowerShell. Moduł jest zainstalowany, po zainstalowaniu konsoli programu VMM. Może być załadowany do skryptu, za pomocą następującego polecenia w skrypcie:
   - Import-Module-Name virtualmachinemanager. [Dowiedz się więcej](https://technet.microsoft.com/library/hh875013.aspx).
* Upewnij się, że co najmniej jednego serwera biblioteki w danym wdrożeniu programu VMM. Domyślnie ścieżkę udziału biblioteki na serwerze VMM znajduje się lokalnie na serwerze VMM MSCVMMLibrary nazwą folderu.
    * Jeśli Twoje ścieżki udziału bibliotecznego jest zdalne (lub lokalnym, ale nie są udostępniane MSCVMMLibrary), skonfigurować udział w następujący sposób (przy użyciu \\libserver2.contoso.com\share\ jako przykład):
      * Otwórz Edytor rejestru i przejdź do **Recovery\Registration lokacji HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure**.
      * Edytuj wartość **ScriptLibraryPath** i umieszczenie go w formie \\libserver2.contoso.com\share\. Określ pełną nazwę FQDN. Podaj uprawnieniami do lokalizacji udziału. Należy pamiętać, że ten węzeł główny udziału. **Aby to sprawdzić, możesz wyszukać biblioteki w węźle głównym w programie VMM. Ścieżka, którego kliknięcie spowoduje otwarcie będzie główny ścieżki — jeden będą musieli używać w zmiennej**.
      * Upewnij się, aby przetestować skrypt przy użyciu konta użytkownika, który ma te same uprawnienia jak konto usługi VMM. Sprawdza, że autonomiczny przetestować skrypty uruchamiane w taki sam sposób jak zostaną planów odzyskiwania. Na serwerze programu VMM ustawienie zasad wykonywania, aby pominąć w następujący sposób:
        * Otwórz **64-bitowego systemu Windows PowerShell** konsoli, korzystając z podwyższonym poziomem uprawnień.
        * Typ: **obejścia Set-executionpolicy**. [Dowiedz się więcej](https://technet.microsoft.com/library/ee176961.aspx).

> [!IMPORTANT]
> Należy ustawić zasady wykonywania na obejście na programu powershell 64-bitowych. Jeśli ustawiono go dla programu powershell 32-bitowy, skrypty będą nie exeute.

## <a name="add-a-script-or-manual-action-to-a-plan"></a>Dodaj akcję skryptu lub ręcznie do planu

Skrypt można dodać do domyślnej grupy planu odzyskiwania po utworzeniu dodane do niej maszyny wirtualne lub grup replikacji i utworzony plan.

1. Otwórz planu odzyskiwania.
2. Kliknij element **krok** , a następnie kliknij przycisk **skryptu** lub **Akcja ręczna**.
3. Określ, czy chcesz dodać skrypt lub akcji przed lub po wybranego elementu. Użyj **Przenieś w górę** i **Przenieś w dół** przyciski przeniesienia pozycji skryptu w górę lub w dół.
4. Jeśli dodasz skryptu programu VMM, wybierz **trybu Failover do skryptu VMM**. W **ścieżka skryptu**, wpisz względną ścieżkę do udziału. W poniższym przykładzie programu VMM, określ ścieżkę: **\RPScripts\RPScript.PS1**.
5. Po dodaniu usługi Automatyzacja Azure Uruchom książki Określ konto usługi Automatyzacja Azure, w której znajduje się element runbook, a następnie wybierz skrypt odpowiednich elementów runbook platformy Azure.
6. Czy tryb failover planu odzyskiwania, aby upewnić się, że skrypt działa zgodnie z oczekiwaniami.


### <a name="add-a-vmm-script"></a>Skrypt programu VMM

Lokację źródłową programu VMM można utworzyć skrypt na serwerze programu VMM i dołącz ją do planu odzyskiwania.

1. Utwórz nowy folder w udziale biblioteki. Na przykład \<nazwa > \MSSCVMMLibrary\RPScripts. Umieść go w źródle i VMM serwery docelowe.
2. Utwórz skrypt (na przykład RPScript), a następnie sprawdź, czy działa on zgodnie z oczekiwaniami.
3. Umieść skrypt w lokalizacji \<nazwa > \MSSCVMMLibrary na serwerach VMM źródłowych i docelowych.


## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej](site-recovery-failover.md) dotyczące uruchamiania trybu failover.
