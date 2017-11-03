---
title: "Register/unregister serwer synchronizacji plików Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak rejestrowanie i wyrejestrowywanie systemu Windows Server z usługą Azure pliku synchronizacji magazynu synchronizacji."
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
ms.openlocfilehash: 13a75d5cafd94435346660614721399f2d77919b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2017
---
# <a name="registerunregister-a-server-with-azure-file-sync-preview"></a>Register/unregister serwer synchronizacji plików Azure (wersja zapoznawcza)
Usługa Azure File Sync (wersja zapoznawcza) umożliwia scentralizowanie udziałów plików Twojej organizacji w usłudze Azure Files bez rezygnacji z elastyczności, wydajności i zgodności lokalnego serwera plików. Jest to realizowane poprzez przekształcanie systemów Windows Server w szybką pamięć podręczną udziału plików platformy Azure. Możesz użyć dowolnego dostępnego protokołu w systemie Windows Server w celu uzyskania lokalnego dostępu do danych (w tym protokołu SMB, systemu plików NFS i protokołu FTPS) i możesz mieć dowolną potrzebną Ci liczbę pamięci podręcznych na całym świecie.

Artykule przedstawiono sposób rejestrowanie i wyrejestrowywanie serwera z magazynu usługi synchronizacji. To może być wskazane, czy serwer jest likwidowany, czy nowy punkt końcowy serwera wymagane jest w grupie synchronizacji. Zobacz [Wdrażanie synchronizacji plików Azure (wersja zapoznawcza)](storage-sync-files-deployment-guide.md) informacji na temat wdrażania synchronizacji plików Azure end-to-end.

## <a name="prerequisites"></a>Wymagania wstępne
Aby zarejestrować systemu Windows Server z usługą synchronizacji magazynu, należy najpierw przygotować Windows Server z niezbędnych wymagań wstępnych:

* Upewnij się, że wdrożono usługę synchronizacji magazynu. Aby uzyskać więcej informacji na temat wdrażania usługi synchronizacji magazynu, zobacz [Wdrażanie synchronizacji plików Azure (wersja zapoznawcza)](storage-sync-files-deployment-guide.md).
* Upewnij się, że serwer ma połączenie z Internetem i czy Azure jest dostępna.
* Wyłącz Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer dla administratorów przy użyciu interfejsu użytkownika Menedżera serwera.
    
    ![Interfejs użytkownika Menedżera serwera z Konfiguracja zwiększonych zabezpieczeń wyróżnione](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Upewnij się, że moduł AzureRM PowerShell jest zainstalowany na serwerze. Jeśli serwer jest członkiem klastra pracy awaryjnej, każdy węzeł w klastrze wymaga modułu AzureRM. Więcej informacji na temat instalowania modułu AzureRM można znaleźć w [Instalowanie i konfigurowanie programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Zalecamy używanie najnowszej wersji modułu programu AzureRM PowerShell zarejestrować/wyrejestrować serwera. Jeśli pakiet AzureRM został wcześniej zainstalowany na tym serwerze (i 5.* jest wersja programu PowerShell na tym serwerze lub większej), można użyć `Update-Module` polecenia cmdlet tego pakietu aktualizacji. 

## <a name="register-a-server-with-storage-sync-service"></a>Zarejestruj serwer z magazynu usługi synchronizacji
Przed użyciem systemu Windows Server jako *punkt końcowy serwera* synchronizacji plików Azure *grupy synchronizacji*, musi być zarejestrowana w *Usługa synchronizacji magazynu*. Serwer można zarejestrować tylko jeden *Usługa synchronizacji magazynu* naraz.

### <a name="install-the-azure-file-sync-agent"></a>Zainstaluj agenta synchronizacji plików Azure
1. [Pobierz agenta synchronizacji plików Azure](https://go.microsoft.com/fwlink/?linkid=858257).
2. Uruchom Instalatora agenta synchronizacji plików Azure.
    
    ![W okienku pierwszej synchronizacji plików Azure Instalatora agenta](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Pamiętaj włączyć aktualizacje agenta synchronizacji plików Azure przy użyciu usługi Microsoft Update. Jest ważne, ponieważ poprawki błędów krytycznych i ulepszone funkcje umożliwiające pakiet serwera są wysyłane za pośrednictwem usługi Microsoft Update.

    ![Upewnij się, że usługa Microsoft Update jest włączona w okienku usługi Microsoft Update Instalatora agenta synchronizacji plików Azure](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Jeśli serwer nie został wcześniej zarejestrowany, rejestrowania serwera interfejsu użytkownika wyskakującym zostanie natychmiast po zakończeniu instalacji.

> [!Important]  
> Jeśli serwer jest członkiem klastra pracy awaryjnej, agent synchronizacji plików Azure musi można zainstalować na każdym węźle w klastrze.

### <a name="register-the-server-using-the-server-registration-ui"></a>Zarejestruj serwer przy użyciu rejestrowania serwera interfejsu użytkownika

> [!Important]  
> Subskrypcje dostawca rozwiązań w chmurze nie można użyć interfejsu użytkownika rejestrowania serwera. Zamiast tego należy użyć programu PowerShell (poniżej tej sekcji).

1. Jeśli rejestracja serwera interfejsu użytkownika nie zostały uruchomione bezpośrednio po zakończeniu instalacji agenta synchronizacji plików Azure, można było go uruchomić ręcznie, wykonując `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Kliknij przycisk *logowania* można uzyskać dostępu do Twojej subskrypcji platformy Azure. 

    ![Otwieranie okna dialogowego interfejsu użytkownika serwera rejestracji](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Wybierz poprawną subskrypcję, grupy zasobów i usługi synchronizacji magazynu z okna dialogowego.

    ![Informacje o synchronizacji usługi magazynu](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. W wersji zapoznawczej co więcej logowania jest wymagany do ukończenia procesu. 

    ![Zaloguj się w oknie dialogowym](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Jeśli serwer jest członkiem klastra pracy awaryjnej, każdy serwer musi obsługiwać rejestrowania serwera. Po wyświetleniu zarejestrowane serwery w portalu Azure synchronizacji plików Azure automatycznie rozpoznaje każdy węzeł jako członek tego samego klastra trybu Failover i grupuje je razem odpowiednio.

### <a name="register-the-server-with-powershell"></a>Zarejestruj serwer przy użyciu programu PowerShell
Można także przeprowadzić rejestracji serwera za pomocą programu PowerShell. To jest jedynym sposobem obsługiwanych serwera rejestracji subskrypcji dostawca rozwiązań w chmurze:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

## <a name="unregister-the-server-with-storage-sync-service"></a>Wyrejestruj serwer z magazynu usługi synchronizacji
Istnieje kilka czynności, które są wymagane do wyrejestrowania serwera z usługą synchronizacji magazynu. Spójrzmy how prawidłowo wyrejestrować serwera.

### <a name="optional-recall-all-tiered-data"></a>(Opcjonalnie) Odwołaj wszystkie dane warstwowych
Po włączeniu dla serwera punktu końcowego w chmurze będzie warstw *warstwy* plików do sieci udziały plików platformy Azure. Dzięki temu lokalnymi udziały plików do działania jako pamięci podręcznej, zamiast pełną kopię zestawu danych, aby wydajnie korzystać z miejsca na serwerze plików. Jednak jeśli punktu końcowego serwera zostanie usunięty z plikami warstwowych nadal lokalnie na serwerze, pliki staną się można niedostępne. W związku z tym dalsze dostęp do pliku, najpierw odwołać wszystkie pliki warstwowy z plików Azure przed kontynuowaniem wyrejestrowania. 

Można to zrobić za pomocą polecenia cmdlet programu PowerShell w sposób przedstawiony poniżej:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> W przypadku woluminu obsługującego serwer nie ma wystarczającej ilości wolnego miejsca, aby odwołać wszystkie dane warstwowych `Invoke-StorageSyncFileRecall` polecenia cmdlet zakończy się niepowodzeniem.  

### <a name="remove-the-server-from-all-sync-groups"></a>Usuń serwer z wszystkich grup synchronizacji
Przed wyrejestrowywaniem serwera na usługę synchronizacji magazynu, należy usunąć wszystkie serwera punkty końcowe dla tego serwera. Można to zrobić za pośrednictwem portalu:

1. Przejdź do magazynu usługi synchronizacji, gdy serwer jest zarejestrowany.
2. Usuń wszystkie serwera punkty końcowe dla tego serwera w każdej grupie synchronizacji przez usługę synchronizacji magazynu. Można to zrobić, klikając prawym przyciskiem myszy odpowiednie punkt końcowy serwera w okienku synchronizacji grupy.

    ![Usuwanie punktu końcowego serwera z grupy synchronizacji](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Ponadto można to zrobić za pomocą prostego skryptu PowerShell:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Login-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

### <a name="unregister-the-server"></a>Wyrejestruj serwer
Wszystkie dane zostały przypomnieć, a serwer został usunięty ze wszystkich grup synchronizacji, serwer można wyrejestrować. 

1. W portalu Azure Przejdź w sekcji "Zarejestrowane serwery" Usługa synchronizacji magazynu.
2. Kliknij prawym przyciskiem myszy na serwerze, który chcesz wyrejestrować, a następnie kliknij przycisk "Wyrejestrowywania serwera".

    ![Wyrejestrowanie serwera](media/storage-sync-files-server-registration/unregister-server-1.png)