---
title: "Wdrażanie synchronizacji plików Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć synchronizacji plików Azure od początku do końca."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: b31b6ae413f72c626e2601ba860aad44ddaa29cd
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-deploy-azure-file-sync-preview"></a>Wdrażanie synchronizacji plików Azure (wersja zapoznawcza)
Usługa Azure File Sync (wersja zapoznawcza) umożliwia scentralizowanie udziałów plików Twojej organizacji w usłudze Azure Files bez rezygnacji z elastyczności, wydajności i zgodności lokalnego serwera plików. Jest to realizowane poprzez przekształcanie systemów Windows Server w szybką pamięć podręczną udziału plików platformy Azure. Możesz użyć dowolnego dostępnego protokołu w systemie Windows Server w celu uzyskania lokalnego dostępu do danych (w tym protokołu SMB, systemu plików NFS i protokołu FTPS) i możesz mieć dowolną potrzebną Ci liczbę pamięci podręcznych na całym świecie.

Zdecydowanie zaleca się odczytu [planowania wdrożenia usługi pliki Azure](storage-files-planning.md) i [planowania wdrożenia synchronizacji plików Azure](storage-sync-files-planning.md) przed wykonaniem kroków w tym przewodniku.

## <a name="prerequisites"></a>Wymagania wstępne
* Konto magazynu i Azure udziału plików w tym samym regionie, który chcesz wdrożyć synchronizacji plików Azure. Aby uzyskać więcej informacji, zobacz:
    - [Dostępność w danym regionie](storage-sync-files-planning.md#region-availability) synchronizacji plików Azure,
    - [Utwórz konto magazynu](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) dla wskazówki krok po kroku dotyczące tworzenia konta magazynu, i
    - [Tworzenie udziału plików](storage-how-to-create-file-share.md) dla wskazówki krok po kroku dotyczące sposobu tworzenia udziału plików.
* Co najmniej jednym obsługiwanych w systemie Windows Server lub klastra systemu Windows Server do synchronizacji z usługą Azure synchronizacji plików. Zobacz [współdziałanie z systemem Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability) uzyskać więcej informacji o obsługiwanych wersjach systemu Windows Server.

## <a name="deploy-the-storage-sync-service"></a>Wdrażanie usługi synchronizacji magazynu 
Usługa synchronizacji magazynu jest najwyższego poziomu zasobem platformy Azure, reprezentujący synchronizacji plików Azure. Aby wdrożyć usługę synchronizacji magazynu, przejdź do [portalu Azure](https://portal.azure.com/)i poszukaj synchronizacji plików Azure. Po wybraniu "Pliku synchronizacja programu Azure (wersja zapoznawcza)" w wynikach wyszukiwania, wybierz przycisk Utwórz, aby pop kartę Otwórz "Wdrażanie magazynu synchronizacja".

Wynikowa bloku pyta o następujące informacje:

- **Nazwa**: unikatową nazwę usługi synchronizacji magazynu (na subskrypcję).
- **Subskrypcja**: subskrypcji, w którym można utworzyć magazynu usługi synchronizacji. W zależności od organizacji strategii konfiguracji może mieć dostęp do co najmniej jedna subskrypcja. Subskrypcję platformy Azure jest najbardziej podstawową kontenera rozliczeń dla każdej usługi w chmurze (takich jak pliki Azure).
- **Grupa zasobów**: Grupa zasobów to logiczne grupy zasobów platformy Azure, takich jak konta magazynu lub usługa synchronizacji magazynu. Może utworzyć nową grupę zasobów lub użyj istniejącej grupy zasobów dla synchronizacji plików Azure (zaleca się używanie grup zasobów, jak kontenery używane do izolowania zasobów logicznie dla organizacji, takich jak grupowanie HR zasobów lub zasobów dla określonego projektu) .
- **Lokalizacja**: regionu, w której chcesz wdrożyć synchronizacji plików Azure. Tylko obsługiwane regiony są dostępne na tej liście.

Po zakończeniu formularza "Wdrażanie synchronizacji magazynu", kliknij przycisk "Utwórz", aby wdrożyć usługę synchronizacji magazynu.

## <a name="prepare-windows-servers-for-use-with-azure-file-sync"></a>Przygotowywanie serwerów z systemem Windows do użycia z synchronizacji plików Azure
Kiedykolwiek na serwerze, z którym zamierzasz używać synchronizacji plików Azure, w tym węzłach serwerów w klastrze trybu Failover wykonaj następujące czynności:

Dla każdego serwera, w tym węzłach serwerów w klastrze pracy awaryjnej zamierzasz korzystać z synchronizacji plików Azure, wykonaj następujące czynności:

1. Wyłącz Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer. To jest tylko wymagane dla serwera początkowego rejestracji i może być reenabled po na serwerze jest zarejestrowany.
    1. Otwórz Menedżera serwera.
    2. Kliknij przycisk **lokalnego serwera**:  
        !["Serwer lokalny" po lewej stronie interfejsu użytkownika Menedżera serwera](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Wybierz łącze dla **Konfiguracja zwiększonych zabezpieczeń** w okienku właściwości podrzędnej:  
        !["Konfiguracja zwiększonych zabezpieczeń" w Interfejsie użytkownika Menedżera serwera](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. Wybierz **poza** dla administratorów i użytkowników w wyskakującym oknie Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer:  
        ![Wybrana konfiguracja zwiększonych zabezpieczeń programu Internet Explorer pop okna z "Off"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Upewnij się, że uruchamiasz co najmniej 5.1 programu PowerShell.\* (Wartość domyślna w systemie Windows Server 2016 jest 5.1 programu PowerShell). Możesz sprawdzić, czy używasz 5.1 programu PowerShell. \* patrząc na wartość właściwości PSVersion obiektu $PSVersionTable:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    - Jeśli Twoje PSVersion jest mniejsza niż 5.1. \*, jak będzie w przypadku większości instalacji systemu Windows Server 2012 R2, można łatwo przeprowadzić uaktualnienie przez pobranie i zainstalowanie [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Odpowiedniego pakietu do pobrania i zainstalowania systemu Windows Server 2012 R2 jest **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Instalowanie i konfigurowanie programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Zaleca się zawsze przy użyciu najnowszej wersji modułów programu Azure PowerShell.

## <a name="install-the-azure-file-sync-agent"></a>Zainstaluj agenta synchronizacji plików Azure
Agent synchronizacji plików Azure jest pobrania pakietu, co pozwoli na systemu Windows Server do synchronizacji przy użyciu pliku Azure udziału. Agenta można pobrać z [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Po pobraniu, kliknij dwukrotnie pakiet MSI do rozpoczęcia instalacji agenta synchronizacji plików Azure.

> [!Important]  
> Jeśli zamierzasz używać synchronizacji plików Azure z klastrem trybu Failover, agent synchronizacji plików Azure należy można zainstalować na każdym węźle w klastrze.

Pakiet instalacyjny agenta synchronizacji plików Azure należy zainstalować względnie szybko bez zbyt wiele dodatkowych monitów. Zaleca się następujące czynności:
- Pozostaw domyślnej ścieżki instalacji `C:\Program Files\Azure\StorageSyncAgent`) aby uprościć zarządzanie Rozwiązywanie problemów i serwera.
- Włączanie usługi Microsoft Update do aktualności synchronizacji plików Azure. Wszystkie aktualizacje, w tym aktualizacje funkcji i poprawek, agent synchronizacji plików Azure nastąpi z witryny Microsoft Update. Zawsze zalecamy pobranie najnowszej aktualizacji do synchronizacji usługi Azure pliku. Zobacz [zasady aktualizacji synchronizacji plików Azure](storage-sync-files-planning.md#azure-file-sync-agent-update-policy) Aby uzyskać więcej informacji.

Po zakończeniu instalacji agenta synchronizacji plików Azure interfejsu użytkownika rejestracji serwera spowoduje automatyczne uruchamianie. Zobacz następnej sekcji opisano sposób zarejestrować ten serwer przy synchronizacji plików Azure.

## <a name="register-windows-server-with-storage-sync-service"></a>Rejestr systemu Windows Server z magazynu usługi synchronizacji
Rejestrowanie serwera z systemem Windows za pomocą usługi synchronizacji magazynu ustanawia relację zaufania między serwerem (lub klastra) i usługi synchronizacji magazynu. Interfejs użytkownika serwera rejestracji powinien automatyczne uruchamianie po instalacji agenta synchronizacji plików Azure, ale w takim przypadku można otworzyć go ręcznie z lokalizacji: `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`. Po otwarciu interfejsu użytkownika rejestracji serwera, kliknij przycisk **logowania** do rozpoczęcia.

Po zalogowaniu zostanie wyświetlony monit o następujące informacje:

![Zrzut ekranu interfejsu użytkownika serwera rejestracji](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Subskrypcja platformy Azure**: subskrypcji zawierająca usługę synchronizacji magazynu (zobacz [wdrożyć usługę synchronizacji magazynu](#deploy-the-storage-sync-service) powyżej). 
- **Grupa zasobów**: grupę zasobów, zawierającą magazynu usługi synchronizacji.
- **Usługa synchronizacji magazynu**: Nazwa usługi synchronizacji magazynu, z którym chcesz zarejestrować.

Po wybraniu odpowiednich informacji z listy rozwijane, kliknij przycisk **zarejestrować** do ukończenia rejestracji serwera. W ramach procesu rejestracji zostanie wyświetlony monit o dodatkowe logowania.

## <a name="create-a-sync-group"></a>Tworzenie grupy synchronizacji
Grupy synchronizacji definiuje topologia synchronizacji dla grupy plików. Punkty końcowe w ramach grupy synchronizacji zostaną zachowane w synchronizacji ze sobą. Grupy synchronizacji musi zawierać co najmniej jednej chmurze punktu końcowego, który reprezentuje udział plików Azure, i co serwer punktu końcowego, który reprezentuje ścieżkę, w systemie Windows Server. Aby utworzyć grupę synchronizacji, przejdź do magazynu usługi synchronizacji w [portalu Azure](https://portal.azure.com/)i kliknij przycisk **+ grupy synchronizacji**:

![Utwórz nową grupę synchronizacji w portalu Azure](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Wynikowa okienku wprowadza następujące informacje, aby utworzyć grupę synchronizacji z punktu końcowego w chmurze:

- **Nazwa grupy synchronizacji**: Nazwa grupy synchronizacji ma zostać utworzony. Ta nazwa musi być unikatowa w ramach usługi synchronizacji magazynu, ale może być dowolną nazwę logiczną dla Ciebie.
- **Subskrypcja**: subskrypcji wdrożono usługę synchronizacji magazynu w [wdrożyć usługę synchronizacji magazynu](#deploy-the-storage-sync-service) powyżej.
- **Konto magazynu**: kliknięcie przycisku "Wybierz konto magazynu" pop otwiera dodatkowe okienka stosowanie, wybierz konto magazynu zawierające udziału plików platformy Azure, z którym chcesz synchronizować.
- **Udział plików Azure**: Nazwa udziału plików platformy Azure, z którym chcesz synchronizować.

Aby dodać punktu końcowego serwera, przejdź do nowo utworzona grupa synchronizacji i kliknij przycisk "Dodaj punkt końcowy serwera".

![Dodaj nowy punkt końcowy serwera w okienku grupy synchronizacji](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

W okienku wynikowy "Dodawanie serwera punktu końcowego" wymaga następujących informacji do utworzenia punktu końcowego serwera:

- **Zarejestrowano serwer**: Nazwa serwera lub klastra, na którym chcesz utworzyć punkt końcowy serwera.
- **Ścieżka**: ścieżka w systemie Windows Server mają być synchronizowane w ramach grupy synchronizacji.
- **Obsługa poziomów w chmurze**: przełącznik Aby włączyć lub wyłączyć chmury, Obsługa poziomów w chmurze, które umożliwia rzadko używane lub uzyskać dostępu do plików do należeć do warstwy do usługi pliki Azure.
- **Wolne miejsce w woluminie**: ilość wolnego miejsca do zarezerwowania na woluminie, na którym znajduje się punkt końcowy serwera. Na przykład jeśli wolne miejsce w woluminie jest ustawiona na 50% na woluminie z jednego serwera punktu końcowego, około połowa ilość danych będzie warstwy do usługi pliki Azure. Należy pamiętać, że obsługa poziomów w chmurze niezależnie od tego, czy jest włączone, udziału plików Azure zawsze ma pełną kopię danych w grupie synchronizacji.

Kliknij przycisk Utwórz, aby dodać punkt końcowy serwera. Pliki będą teraz są synchronizowane na udziału plików platformy Azure i w systemie Windows Server. 

> [!Important]  
> Można wprowadzić zmiany w chmurze lub punkt końcowy serwera w grupie synchronizacji i zostały zsynchronizowane pliki do punktów końcowych w grupie synchronizacji. Jeśli bezpośrednio wprowadzić zmianę do punktu końcowego w chmurze (udział plików Azure), należy pamiętać, że zmiany muszą najpierw zostać wykryte przez zadanie wykrywania zmian synchronizacji plików Azure, czyli tylko initatiated dla punktu końcowego w chmurze, co 24 godziny. Zobacz [plików Azure — często zadawane pytania](storage-files-faq.md#afs-change-detection) Aby uzyskać więcej informacji.

## <a name="next-steps"></a>Następne kroki
- [Dodaj lub Usuń punkt końcowy serwera synchronizacji plików na platformę Azure](storage-sync-files-server-endpoint.md)
- [Rejestr/wyrejestrować serwera z funkcją synchronizacji plików Azure](storage-sync-files-server-registration.md)