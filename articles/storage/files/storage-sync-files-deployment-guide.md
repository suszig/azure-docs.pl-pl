---
title: "Wdrażanie synchronizacji plików Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć Azure plik synchronizacji od początku do końca."
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
ms.openlocfilehash: a9be43e6dd8ce2c8ba8326b06923677eb897765b
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2017
---
# <a name="deploy-azure-file-sync-preview"></a>Wdrażanie synchronizacji plików Azure (wersja zapoznawcza)
Umożliwia synchronizacji plików Azure (wersja zapoznawcza) scentralizowanie udziałów plików w organizacji w plikach Azure, przy zachowaniu elastyczności, wydajności i zgodności serwera plików lokalnych. Synchronizacja programu Azure pliku przy użyciu systemu Windows Server do szybkiego pamięci podręcznej udziału plików na platformę Azure. Można użyć każdego protokołu, który jest dostępny w systemie Windows Server dostępu do danych lokalnie, w tym protokołu SMB, systemu plików NFS i FTPS. Może mieć dowolną liczbę pamięci podręcznych zgodnie z potrzebami na całym świecie.

Zdecydowanie zaleca się przeczytanie [planowania wdrożenia usługi pliki Azure](storage-files-planning.md) i [planowania wdrożenia synchronizacji plików Azure](storage-sync-files-planning.md) przed wykonaniem kroków opisanych w tym artykule.

## <a name="prerequisites"></a>Wymagania wstępne
* Konto usługi Azure Storage i Azure udziału plików w tym samym regionie, który chcesz wdrożyć synchronizacji plików Azure. Aby uzyskać więcej informacji, zobacz:
    - [Dostępność w danym regionie](storage-sync-files-planning.md#region-availability) synchronizacji plików usługi Azure.
    - [Utwórz konto magazynu](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) krok opis sposobu tworzenia konta magazynu.
    - [Tworzenie udziału plików](storage-how-to-create-file-share.md) krok opis sposobu tworzenia udziału plików.
* Co najmniej jedno wystąpienie obsługiwanych systemu Windows Server lub klastra systemu Windows Server do synchronizacji z usługą Azure synchronizacji plików. Aby uzyskać więcej informacji o obsługiwanych wersjach systemu Windows Server, zobacz [współdziałanie z systemem Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability).

## <a name="deploy-the-storage-sync-service"></a>Wdrażanie usługi synchronizacji magazynu 
Usługa synchronizacji magazynu jest najwyższego poziomu zasobów platformy Azure dla synchronizacji plików Azure. Aby wdrożyć usługę synchronizacji magazynu, przejdź do [portalu Azure](https://portal.azure.com/), kliknij przycisk Nowy, a następnie wyszukaj synchronizacji plików Azure. W wynikach wyszukiwania wybierz **synchronizacji plików Azure (wersja zapoznawcza)**, a następnie wybierz **Utwórz** otworzyć **Wdrażanie synchronizacji magazynu** kartę.

W okienku otwartym wprowadź następujące informacje:

- **Nazwa**: unikatową nazwę usługi synchronizacji magazynu (na subskrypcję).
- **Subskrypcja**: subskrypcji, w którym chcesz utworzyć usługę synchronizacji magazynu. W zależności od strategii konfiguracji w organizacji możesz mieć dostępu do co najmniej jedna subskrypcja. Subskrypcja platformy Azure jest najbardziej podstawową kontenera rozliczeń dla każdej usługi w chmurze (takich jak pliki Azure).
- **Grupa zasobów**: Grupa zasobów to logiczne grupy zasobów platformy Azure, takich jak konta magazynu lub usługa synchronizacji magazynu. Można utworzyć nową grupę zasobów lub użyć istniejącej grupy zasobów dla synchronizacji plików Azure. (Zalecamy używanie grup zasobów jako kontenery do izolowania zasobów logicznie dla organizacji, takich jak grupowanie HR zasobów lub zasobów dla określonego projektu.)
- **Lokalizacja**: regionu, w której chcesz wdrożyć synchronizacji plików Azure. Tylko obsługiwane regiony są dostępne na tej liście.

Gdy skończysz, wybierz **Utwórz** wdrożenie usługi synchronizacji magazynu.

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Przygotowanie systemu Windows Server do użycia z synchronizacji plików Azure
Dla każdego serwera, który ma być używany z synchronizacji plików Azure, w tym węzłach serwerów w klastrze trybu Failover wykonaj następujące czynności:

1. Wyłącz **programu Internet Explorer Konfiguracja zwiększonych zabezpieczeń**. Jest to wymagane tylko w przypadku rejestracji serwera początkowego. Możesz je ponownie włączyć po zarejestrowaniu serwera.
    1. Otwórz Menedżera serwera.
    2. Kliknij przycisk **lokalnego serwera**:  
        !["Serwer lokalny" po lewej stronie interfejsu użytkownika Menedżera serwera](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Na **właściwości** subpane, wybierz link **Konfiguracja zwiększonych zabezpieczeń**.  
        ![W okienku "Konfiguracja zwiększonych zabezpieczeń" w Interfejsie użytkownika Menedżera serwera](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. W **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer** okno dialogowe, wybierz opcję **poza** dla **Administratorzy** i **użytkowników**:  
        ![Wybrana konfiguracja zwiększonych zabezpieczeń programu Internet Explorer pop okna z "Off"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Upewnij się, że uruchamiasz co najmniej 5.1 programu PowerShell.\* (Wartość domyślna w systemie Windows Server 2016 jest 5.1 programu PowerShell). Aby sprawdzić, czy korzystasz z programu PowerShell w wersji 5.1. \* sprawdzając wartość **PSVersion** właściwość **$PSVersionTable** obiektu:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Jeśli wartość PSVersion jest mniejsza niż 5.1. \*, jak będzie w przypadku większości instalacji systemu Windows Server 2012 R2, można łatwo przeprowadzić uaktualnienie przez pobranie i zainstalowanie [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Odpowiedniego pakietu do pobrania i zainstalowania systemu Windows Server 2012 R2 jest **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Instalowanie i konfigurowanie programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Zalecamy używanie najnowszej wersji modułów programu Azure PowerShell.

## <a name="install-the-azure-file-sync-agent"></a>Zainstaluj agenta synchronizacji plików Azure
Agent synchronizacji plików Azure jest pobrania pakietu, który umożliwia systemu Windows Server można synchronizować z udziałem plików na platformę Azure. Możesz pobrać agenta z [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Po zakończeniu pobierania kliknij dwukrotnie pakiet MSI do rozpoczęcia instalacji agenta synchronizacji plików Azure.

> [!Important]  
> Jeśli zamierzasz używać synchronizacji plików Azure z klastrem trybu Failover, musi być zainstalowany agent synchronizacji plików Azure w na każdym węźle w klastrze.

Pakiet instalacyjny agenta synchronizacji plików Azure należy zainstalować stosunkowo szybko i bez zbyt wiele dodatkowych monitów. Zaleca się wykonanie następujących czynności:
- Pozostaw domyślnej ścieżki instalacji (C:\Program Files\Azure\StorageSyncAgent), aby uprościć zarządzanie Rozwiązywanie problemów i serwera.
- Włączyć usługę Microsoft Update aktualności synchronizacji plików Azure. Wszystkie aktualizacje agenta synchronizacji plików Azure, w tym aktualizacje funkcji i poprawek, wystąpić z witryny Microsoft Update. Zaleca się zainstalowanie najnowszej aktualizacji do synchronizacji plików Azure. Aby uzyskać więcej informacji, zobacz [zasady aktualizacji synchronizacji plików Azure](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Po zakończeniu instalacji agenta synchronizacji plików Azure, interfejsu użytkownika rejestracji serwera jest automatycznie otwiera. Aby dowiedzieć się, jak zarejestrować ten serwer przy synchronizacji plików Azure, zobacz następną sekcję.

## <a name="register-windows-server-with-storage-sync-service"></a>Rejestr systemu Windows Server z magazynu usługi synchronizacji
Rejestrowanie serwera systemu Windows z usługą synchronizacji magazynu ustanawia relację zaufania między serwerem (lub klastra) i usługi synchronizacji magazynu. Interfejs użytkownika rejestracji serwera należy otwierać automatycznie po instalacji agenta synchronizacji plików Azure. Jeśli nie, można otworzyć go ręcznie z lokalizacji pliku: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Po uruchomieniu interfejsu użytkownika rejestracji serwera wybierz **logowania** do rozpoczęcia.

Po zalogowaniu, zostanie wyświetlony monit o następujące informacje:

![Zrzut ekranu interfejsu użytkownika serwera rejestracji](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Subskrypcja platformy Azure**: subskrypcji, który zawiera usługę synchronizacji magazynu (zobacz [wdrożyć usługę synchronizacji magazynu](#deploy-the-storage-sync-service)). 
- **Grupa zasobów**: grupy zasobów, który zawiera usługę synchronizacji magazynu.
- **Usługa synchronizacji magazynu**: Nazwa usługi synchronizacji magazynu, z którym chcesz zarejestrować.

Po wybraniu odpowiednie informacje, wybierz **zarejestrować** do ukończenia rejestracji serwera. W ramach procesu rejestracji zostanie wyświetlony monit o dodatkowe logowania.

## <a name="create-a-sync-group"></a>Tworzenie grupy synchronizacji
Grupy synchronizacji definiuje topologia synchronizacji dla grupy plików. Punkty końcowe w ramach grupy synchronizacji są zsynchronizowane ze sobą. Grupy synchronizacji musi zawierać co najmniej jedna chmura punktu końcowego, który reprezentuje udział plików na platformę Azure, i jeden serwer punktu końcowego, który reprezentuje ścieżkę, w systemie Windows Server. Aby utworzyć grupę synchronizacji w [portalu Azure](https://portal.azure.com/), przejdź do usługi synchronizacji magazynu, a następnie wybierz **+ grupy synchronizacji**:

![Utwórz nową grupę synchronizacji w portalu Azure](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

W okienku zostanie otwarty wprowadź następujące informacje, aby utworzyć grupę synchronizacji z punktu końcowego w chmurze:

- **Nazwa grupy synchronizacji**: Nazwa grupy synchronizacji ma zostać utworzony. Ta nazwa musi być unikatowa w ramach usługi synchronizacji magazynu, ale może być dowolną nazwę logiczną dla Ciebie.
- **Subskrypcja**: subskrypcji, w której wdrożono usługę synchronizacji magazynu w [wdrożyć usługę synchronizacji magazynu](#deploy-the-storage-sync-service).
- **Konto magazynu**: w przypadku wybrania **wybierz konto magazynu**, innego okienka pojawia się, w którym można wybrać konto magazynu, który ma udziału plików na platformę Azure, do którego mają być synchronizowane z.
- **Udział plików Azure**: Nazwa udziału plików na platformę Azure, z którym mają być synchronizowane.

Aby dodać punktu końcowego serwera, przejdź do grupy nowo utworzonych sync, a następnie wybierz **dodać punkt końcowy serwera**.

![Dodaj nowy punkt końcowy serwera w okienku grupy synchronizacji](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

W **dodać punkt końcowy serwera** okienku, wprowadź następujące informacje, aby utworzyć punktu końcowego serwera:

- **Zarejestrowanego serwera**: Nazwa serwera lub klastra, w której chcesz utworzyć punkt końcowy serwera.
- **Ścieżka**: ścieżka systemu Windows Server można synchronizować w ramach grupy synchronizacji.
- **Obsługa poziomów w chmurze**: przełącznik, aby włączyć lub wyłączyć chmury warstwy. Z chmurą, dodając funkcje warstw, rzadko używane lub uzyskać dostępu do plików może należeć do warstwy do usługi pliki Azure.
- **Wolne miejsce w woluminie**: ilość wolnego miejsca do zarezerwowania na woluminie, na którym znajduje się punkt końcowy serwera. Na przykład jeśli wolne miejsce w woluminie jest ustawiona na 50% na wolumin, który ma punkt końcowy pojedynczego serwera, około połowa ilości danych jest warstwowa do usługi pliki Azure. Niezależnie od tego, czy w chmurze, dodając funkcje warstw jest włączona, na udział plików na platformę Azure ma zawsze pełną kopię danych do grupy synchronizacji.

Aby dodać punkt końcowy serwera, wybierz **Utwórz**. Pliki są teraz zsynchronizowane na udział plików na platformę Azure i w systemie Windows Server. 

> [!Important]  
> Można wprowadzić zmiany do dowolnego punktu końcowego w chmurze lub punkt końcowy serwera w grupie synchronizacji i zsynchronizowaniu pliki do punktów końcowych w grupie synchronizacji. Jeśli bezpośrednio wprowadzić zmianę do punktu końcowego w chmurze (udział plików na platformę Azure), zmiany najpierw trzeba być odnajdowane przez zadanie wykrywania zmian synchronizacji plików Azure. Zadanie wykrywania zmian jest inicjowane dla punktu końcowego w chmurze tylko raz na 24 godziny. Aby uzyskać więcej informacji, zobacz [plików Azure — często zadawane pytania](storage-files-faq.md#afs-change-detection).

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>Migracja wdrożenia replikacji systemu plików DFS (DFS-R) do synchronizacji plików Azure
Migracja wdrożenia systemu plików DFS-R do synchronizacji usługi Azure pliku:

1. Utwórz grupę synchronizacji do reprezentowania topologii systemu plików DFS-R, który chcesz zastąpić.
2. Uruchom na serwerze, który ma pełny zestaw danych w topologii systemu plików DFS-R, aby przeprowadzić migrację. Instalowanie synchronizacji plików Azure na tym serwerze.
3. Zarejestruj tego serwera i utworzyć punktu końcowego serwera dla pierwszego serwera do migracji. Nie należy włączać chmury warstwy.
4. Poinformuj wszystkich synchronizacji danych do udziału plików na platformę Azure (punktu końcowego w chmurze).
5. Zainstaluj i zarejestruj agenta synchronizacji plików Azure na każdej z pozostałych serwerów systemu plików DFS-R.
6. Wyłącz R. systemu plików DFS 
7. Tworzenie punktu końcowego serwera na wszystkich serwerach systemu plików DFS-R. Nie należy włączać chmury warstwy.
8. Upewnij się, synchronizacji kończy i przetestować topologii zgodnie z potrzebami.
9. Wycofywanie R. systemu plików DFS
10. Chmury warstw może teraz można włączyć dla dowolnego punktu końcowego serwera zgodnie z potrzebami.

Aby uzyskać więcej informacji, zobacz [interop synchronizacji plików Azure z rozproszonego systemu plików (DFS)](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Następne kroki
- [Dodawanie lub usuwanie punktu końcowego serwera synchronizacji plików Azure](storage-sync-files-server-endpoint.md)
- [Zarejestruj lub Wyrejestruj serwer z synchronizacji plików Azure](storage-sync-files-server-registration.md)
