---
title: "Jak dodać skryptów do planu odzyskiwania w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Wymagania wstępne przy dodawaniu nowego skryptu do planu odzyskiwania w programie VMM na platformie Azure"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: shons
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/13/2017
ms.author: ruturaj
ms.openlocfilehash: 60c6eebd9323028c63f42bd8a0996e3c0a2a1cf1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="add-vmm-scripts-to-a-recovery-plan"></a>Dodaj skrypty programu VMM w ramach planu odzyskiwania


Ten artykuł zawiera informacje o tworzeniu i dodawania skryptów programu VMM dla planów odzyskiwania w [usługi Azure Site Recovery](site-recovery-overview.md).

Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites-of-adding-a-script-to-recovery-plan"></a>Wymagania wstępne dotyczące dodawania skryptu do planu odzyskiwania

Można użyć skryptów środowiska PowerShell w planów odzyskiwania. Należy je tworzyć i umieścić je w bibliotece programu VMM z planu odzyskiwania. Poniżej przedstawiono kilka kwestii podczas pisania skryptu.

* Upewnij się, że skrypty używać bloków try-catch, tak aby bezpiecznie obsługi wyjątków.
    - W przypadku wyjątku w skrypcie, przestaje działać, i pokazuje zadania nie powiodło się.
    - Jeśli wystąpi błąd, pozostała część skrypt nie działa.
    - Jeśli błąd wystąpi po uruchomieniu nieplanowanego trybu failover, nadal planu odzyskiwania.
    - Jeśli wystąpi błąd podczas uruchamiania planowanego trybu failover, zatrzymuje planu odzyskiwania. Należy naprawić skryptu, sprawdź, czy działa zgodnie z oczekiwaniami, a następnie uruchom odzyskiwanie Zaplanuj ponownie.
        - Polecenie Write-Host nie działa w skryptu planu odzyskiwania, a skrypt zakończy się niepowodzeniem. Aby utworzyć dane wyjściowe, należy utworzyć skrypt serwera proxy, który z kolei uruchamia skrypt głównego. Upewnij się, że wszystkie dane wyjściowe jest przekazywany w potoku się przy użyciu >> polecenia.
        - Limit czasu skryptu, jeśli nie zwraca w 600 sekund.
        - Jeśli wszystko jest zapisywane do strumienia wyjściowego STDERR, skrypt jest klasyfikowany jako zakończony niepowodzeniem. Te informacje są wyświetlane w szczegółach wykonanie skryptu.

* Skrypty w planie odzyskiwania są uruchamiane w kontekście konta usługi VMM. Upewnij się, że to konto ma uprawnienia do odczytu do udziału zdalnego, na którym zlokalizowany jest skrypt. Testowanie skryptu do uruchomienia na poziomie uprawnień konta usługi programu VMM.
* Poleceń cmdlet programu VMM są dostarczane w moduł programu Windows PowerShell. Moduł jest zainstalowany, po zainstalowaniu konsoli programu VMM. Może być załadowany do skryptu, za pomocą następującego polecenia w skrypcie:
   - Import-Module -Name virtualmachinemanager. [Dowiedz się więcej](https://technet.microsoft.com/library/hh875013.aspx).
* Upewnij się, że co najmniej jednego serwera biblioteki w danym wdrożeniu programu VMM. Domyślnie ścieżkę udziału biblioteki na serwerze VMM znajduje się lokalnie na serwerze VMM MSCVMMLibrary nazwą folderu.
    * Jeśli Twoje ścieżki udziału bibliotecznego jest zdalne (lub lokalnym, ale nie są udostępniane MSCVMMLibrary), skonfigurować udział w następujący sposób (przy użyciu \\libserver2.contoso.com\share\ jako przykład):
      * Otwórz Edytor rejestru i przejdź do **Recovery\Registration lokacji HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure**.
      * Edytuj wartość **ScriptLibraryPath** i umieszczenie go w formie \\libserver2.contoso.com\share\. Określ pełną nazwę FQDN. Podaj uprawnieniami do lokalizacji udziału. Należy pamiętać, że ten węzeł główny udziału. **Aby to sprawdzić, możesz wyszukać biblioteki w węźle głównym w programie VMM. Ścieżka, którego kliknięcie spowoduje otwarcie będzie główny ścieżki — jeden będą musieli używać w zmiennej**.
      * Upewnij się, aby przetestować skrypt przy użyciu konta użytkownika, który ma te same uprawnienia jak konto usługi VMM. Sprawdza, że autonomiczny przetestować skrypty uruchamiane w taki sam sposób jak zostaną planów odzyskiwania. Na serwerze programu VMM ustawienie zasad wykonywania, aby pominąć w następujący sposób:
        * Otwórz **64-bitowego systemu Windows PowerShell** konsoli, korzystając z podwyższonym poziomem uprawnień.
        * Typ: **obejścia Set-executionpolicy**. [Dowiedz się więcej](https://technet.microsoft.com/library/ee176961.aspx).

> [!IMPORTANT]
> Należy ustawić zasady wykonywania na obejście na programu PowerShell 64-bitowych. Jeśli ustawiono go dla programu PowerShell 32-bitowy, skrypty będą nie exeute.

## <a name="add-the-script-to-the-vmm-library"></a>Dodaj skrypt do biblioteki programu VMM

Lokację źródłową programu VMM można utworzyć skrypt na serwerze programu VMM i dołącz ją do planu odzyskiwania.

1. Utwórz nowy folder w udziale biblioteki. Na przykład \<nazwa > \MSSCVMMLibrary\RPScripts. Umieść go w źródle i VMM serwery docelowe.
2. Utwórz skrypt (na przykład RPScript), a następnie sprawdź, czy działa on zgodnie z oczekiwaniami.
3. Umieść skrypt w lokalizacji \<nazwa > \MSSCVMMLibrary na serwerach VMM źródłowych i docelowych.

## <a name="add-the-script-to-a-recovery-plan"></a>Dodaj skrypt do planu odzyskiwania

Skryptu można dodać do grupy po utworzeniu dodane do niej maszyny wirtualne lub grup replikacji i utworzony plan.

1. Otwórz planu odzyskiwania.
2. Kliknij element **krok** , a następnie kliknij przycisk **skryptu** lub **Akcja ręczna**.
3. Określ, czy chcesz dodać skrypt lub akcji przed lub po wybranego elementu. Użyj **Przenieś w górę** i **Przenieś w dół** przyciski przeniesienia pozycji skryptu w górę lub w dół.
4. Jeśli dodasz skryptu programu VMM, wybierz **trybu Failover do skryptu VMM**. W **ścieżka skryptu**, wpisz względną ścieżkę do udziału. W poniższym przykładzie programu VMM, określ ścieżkę: **\RPScripts\RPScript.PS1**.
5. Po dodaniu usługi Automatyzacja Azure Uruchom książki Określ konto usługi Automatyzacja Azure, w której znajduje się element runbook, a następnie wybierz skrypt odpowiednich elementów runbook platformy Azure.
6. Wykonaj test trybu failover planu odzyskiwania, aby upewnić się, że skrypt działa zgodnie z oczekiwaniami.


## <a name="next-steps"></a>Kolejne kroki

[Dowiedz się więcej](site-recovery-failover.md) dotyczące uruchamiania trybu failover.
