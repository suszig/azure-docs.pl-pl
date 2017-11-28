---
title: "Migracja magazynu Azure — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Odpowiedzi na często zadawane pytania dotyczące migracji usługi Azure Storage"
services: storage
documentationcenter: na
author: genlin
manager: timlt
editor: tysonn
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 11/16/2017
ms.author: genli
ms.openlocfilehash: 516a0487afe11ef6915a002375661a23eaf13edc
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2017
---
# <a name="frequently-asked-questions-about-azure-storage-migration"></a>Często zadawane pytania dotyczące migracji usługi Azure Storage

Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące migracji usługi Azure Storage. 

## <a name="faq"></a>Często zadawane pytania

**Jak utworzyć skrypt, aby skopiować pliki z jednego kontenera do innego?**

Do kopiowania plików między kontenerów, można użyć narzędzia AzCopy. Zobacz poniższy przykład:

    AzCopy /Source:https://xxx.blob.core.windows.net/xxx
    /Dest:https://xxx.blob.core.windows.net/xxx /SourceKey:xxx /DestKey:xxx
    /S

Korzysta z narzędzia AzCopy [kopiowania obiektu Blob API](https://docs.microsoft.com/rest/api/storageservices/copy-blob) celu kopii każdego pliku w kontenerze.  
  
Można użyć dowolnej maszyny wirtualnej lub komputer lokalny, który ma dostęp do Internetu, aby uruchomić narzędzie AzCopy. Umożliwia także harmonogram usługi partia zadań Azure się to automatycznie, ale ma bardziej skomplikowane.  
  
Skrypt automatyzacji zaprojektowano pod kątem wdrożenia usługi Azure Resource Manager zamiast manipulowania zawartości magazynu. Aby uzyskać więcej informacji, zobacz [wdrażanie zasobów przy użyciu szablonów usługi Resource Manager i programu Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md).

**Zaangażowany kopiowania danych między dwoma udziałami inny plik w tym samym koncie magazynu w tym samym regionie jest bezpłatnie?**

Nie. Jest bezpłatna dla tego procesu.

**Jak wykonać kopię zapasową mojej całe konto magazynu na inne konto magazynu?**

Nie jest dostępna opcja do tworzenia kopii zapasowych całe konto magazynu bezpośrednio. Jednak można ręcznie przenieść kontenera na tym koncie magazynu na inne konto przy użyciu narzędzia AzCopy lub Eksploratora usługi Storage. W poniższej procedurze pokazano, jak używać narzędzia AzCopy można przenieść kontener:  
 

1.  Zainstaluj [AzCopy](storage-use-azcopy.md) narzędzia wiersza polecenia. To narzędzie ułatwia przenoszenie pliku wirtualnego dysku twardego między kontami magazynu.

2.  Po zainstalowaniu narzędzia AzCopy w systemie Windows za pomocą Instalatora, Otwórz okno polecenia POromprt, a następnie przejdź do folderu instalacji narzędzia AzCopy na komputerze. Domyślnie program AzCopy jest instalowana na **% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy** lub **%ProgramFiles%\Microsoft SDKs\Azure\AzCopy**.

3.   Uruchom następujące polecenie, aby przenieść kontenera. Należy zastąpić tekst z rzeczywistą wartością.   
     
            AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
            /Dest:https://destaccount.blob.core.windows.net/mycontainer2
            /SourceKey:key1 /DestKey:key2 /S

        - / source: Podaj identyfikator URI konta magazynu źródłowego (do kontenera)  
        - / dest: Podaj konto magazynu docelowego identyfikatora URI (do kontenera)  
        - /sourcekey: źródło przechowywanie klucza podstawowego konta, możesz skopiować ten klucz z portalu, wybierając konto magazynu.  
        - /destkey: Podaj docelowy magazyn klucza podstawowego konta, możesz skopiować ten klucz z portalu, wybierając konto magazynu.

Po wykonaniu tego polecenia pliki kontenera zostaną przeniesione na docelowe konto magazynu.

**Interfejsu wiersza polecenia AzCopy nie współdziała z przełącznikiem "Wzorzec" podczas kopiowania z jednego Azure blob do innego.**

Można bezpośrednio skopiuj i Edytuj AzCopy cmd i zweryfikować, aby upewnić się, że wzorzec jest zgodny źródła. Ponadto upewnij się, że **/S** obowiązują symboli wieloznacznych. Aby uzyskać więcej informacji, zobacz [parametrów narzędzia AzCopy](storage-use-azcopy.md).

**Jak przenieść dane z jednego kontenera magazynu do innego**

W tym celu wykonaj następujące kroki:

1.  Tworzenie kontenera (folder) w docelowym obiekcie blob.

2.  Użyj [Azcopy](https://azure.microsoft.com/en-us/blog/azcopy-5-1-release/) skopiować zawartość z oryginalnego kontenera obiektów blob do innego kontenera obiektów Blob.

**Jak utworzyć skrypt programu PowerShell, aby przenieść dane z jednego udziału plików na platformę Azure do innego magazynu platformy Azure?**

Użyj narzędzia AzCopy, aby przenieść dane z jednego udziału plików na platformę Azure do innego magazynu azure. Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).

**Jak przekazać dużych plików CSV do magazynu Azure?**

Użyj narzędzia AzCopy, aby przekazać dużych plików CSV do magazynu Azure. Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).

**Mam przenieść dzienniki z "dysku D" mojego konta magazynu Azure każdego dnia. Jak zautomatyzować to**

Można użyć narzędzia AzCopy i utworzyć zadanie w harmonogramie zadań. Przekazywanie plików do konta magazynu platformy Azure przy użyciu skryptu wsadowego AzCopy. Aby uzyskać więcej informacji, zobacz [sposób konfigurowania i uruchamiania zadań uruchamiania dla usługi w chmurze](../../cloud-services/cloud-services-startup-tasks.md).

**Jak przenieść moim koncie magazynu między subskrypcje?**

Użyj narzędzia AzCopy, aby przenieść konta magazynu między subskrypcjami. Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).

**Jak można przenieść 10 TB danych do magazynu w innym regionie?**

Przenoszenia danych, należy użyć narzędzia AzCopy. Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).

**Jak skopiować dane z lokalnej do magazynu Azure?**

Aby skopiować dane, należy użyć narzędzia AzCopy. Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).

**Jak I przenieść dane z lokalnego do usługi plików Azure?**

Użyj narzędzia AzCopy do przenoszenia danych. Aby uzyskać więcej informacji, aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).

**Sposób mapowania folderu kontenera na maszynie wirtualnej?**

Użyj udziału plików na platformę Azure.

**Jak wykonać kopię zapasową danych usługi Azure file storage?**

Istnieje rozwiązanie tworzenia kopii zapasowej. Jednak plików Azure obsługuje również kopia asynchronicznego. Tak, możesz skopiować pliki z udziału do innego udziału (w ramach konta magazynu lub innego konta magazynu) lub do kontenera obiektów blob (w ramach konta magazynu lub innego konta magazynu).
Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md).

**Jak przenieść zarządzanych dysków na inne konto magazynu?**

W tym celu wykonaj następujące kroki:

1.  Zatrzymaj maszynę wirtualną, podłączonego do dysków zarządzanych.

2.  Skopiuj zarządzanych dysku VHD z jednego obszaru do innego, uruchamiając następujący skrypt programu PowerShell systemu Azure:

    ```
    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId <ID>

    $sas = Grant-AzureRmDiskAccess -ResourceGroupName <RG name> -DiskName <Disk name> -DurationInSecond 3600 -Access Read

    $destContext = New-AzureStorageContext –StorageAccountName contosostorageav1 -StorageAccountKey <your account key>

    Start-AzureStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer 'vhds' -DestContext $destContext -DestBlob 'MyDestinationBlobName.vhd'
    ```

3.  Tworzenie dysków zarządzanych przy użyciu pliku wirtualnego dysku twardego w innym regionie, do którego została skopiowana wirtualnego dysku twardego. Aby to zrobić, uruchom następującą skrypt programu PowerShell systemu Azure:  

    ```
    $resourceGroupName = 'MDDemo'

    $diskName = 'contoso\_os\_disk1'

    $vhdUri = 'https://contoso.storageaccou.com.vhd

    $storageId = '/subscriptions/<ID>/resourceGroups/<RG name>/providers/Microsoft.Storage/storageAccounts/contosostorageaccount1'

    $location = 'westus'

    $storageType = 'StandardLRS'

    $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $vhdUri -StorageAccountId $storageId -DiskSizeGB 128

    $osDisk = New-AzureRmDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
    ``` 

Aby uzyskać więcej informacji na temat sposobu wdrażania maszyny wirtualnej z dyskiem zarządzanym, zobacz [CreateVmFromManagedOsDisk.ps1](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/blob/master/CreateVmFromManagedOsDisk.ps1).

**Jak pobrać o 1 – 2 TB danych z portalu Azure?**

Użyj narzędzia AzCopy można pobrać danych. Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).

**Jak zmienić lokalizacji dodatkowej, regionie Europy dla konta magazynu?**

Po utworzeniu konta magazynu, należy wybrać regionie podstawowym konta. Wybór dodatkowej region jest oparta na regionie podstawowym i nie można zmienić. Zobacz [replikacja usługi Azure Storage](storage-redundancy.md).

**Gdzie można uzyskać więcej informacji o szyfrowanie usługi Magazyn Azure (SSE)?**  
  
Zobacz następujące artykuły:

-  [Przewodnik po zabezpieczeniach magazynu Azure](storage-security-guide.md)

-   [Szyfrowanie usługi Magazyn Azure dla przechowywanych danych](storage-service-encryption.md)

**Jak przenieść lub pobrać dane z konta magazynu?**

Użyj narzędzia AzCopy można pobrać danych. Aby uzyskać więcej informacji, aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) i [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md).


**Jak zaszyfrować dane na koncie magazynu?**

Po włączeniu szyfrowania w ramach konta magazynu, istniejące dane nie są szyfrowane. Aby zaszyfrować istniejące dane, należy przesłać ponownie do danych na koncie magazynu.  W tym celu wykonaj następujące kroki:

Użyj narzędzia AZcopy, aby skopiować dane do innego konta magazynu, a następnie powrót do konta magazynu. Można również użyć [szyfrowanie magazynowanych](storage-service-encryption.md).

**W jaki sposób pobierać wirtualnego dysku twardego na komputerze lokalnym, inne niż przy użyciu opcji pobierania w portalu**

Można użyć [Eksploratora usługi Storage](https://azure.microsoft.com/features/storage-explorer/) do pobrania dysku VHD.

**Czy istnieją wymagania wstępne związane z zmianę replikacji konta magazynu LRS z GRS?**

Nie. 

**Jak uzyskać dostępu do usługi pliki Azure magazynu geograficznie nadmiarowego**

Dostęp do odczytu magazynu geograficznie nadmiarowego (RA-GRS) jest wymagany dostęp do nadmiarowego magazynu. Jednak usługi pliki Azure obsługuje tylko LRS i grs w warstwie standardowa, która nie zezwala na dostęp tylko do odczytu. 

**Jak przenieść z magazyn w warstwie Premium do magazynu w warstwie standardowa?**

W tym celu wykonaj następujące kroki:

1.  Utwórz nowe konto magazynu w warstwie standardowa (lub można użyć istniejącego konta Standard Storage w ramach subskrypcji).

2.  Pobierz narzędzia AzCopy. Uruchom jedno z następujących poleceń narzędzia AzCopy.
      
    Aby skopiować cały dysków na koncie magazynu:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /S 

    Aby skopiować tylko jeden dysk, podaj nazwę dysku we wzorcu

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /Pattern:abc.vhd

   
Może upłynąć kilka godzin, aby ukończyć operację.

Aby upewnić się, że transfer została ukończona pomyślnie, należy zbadać docelowy kontener konta magazynu w portalu Azure. Po dyski są kopiowane do konta standard storage, można dołączyć do maszyny wirtualnej, jako istniejącego dysku. Aby uzyskać więcej informacji, zobacz [jak można dołączyć dysku danych zarządzanych do maszyny wirtualnej systemu Windows w portalu Azure](../../virtual-machines/windows/attach-managed-disk-portal.md).  
  
**Jak przekonwertować na magazyn w warstwie Premium dla udziału plików?**

Magazyn w warstwie Premium nie jest dozwolona w udziale plików Azure.

**Jak uaktualnić z magazynu w warstwie standardowa do konta Premium Storage? Jak obniżyć z magazynu Premium do konta Standard Storage?**

- Musi utworzyć konto magazynu docelowego, skopiować dane z konta źródłowego do docelowego konta, a następnie usuń konto źródłowego.

- Aby wykonać kopię danych, można użyć narzędzia, takiego jak narzędzie AzCopy.

- Jeśli masz także maszyn wirtualnych, istnieje kilka dodatkowych kroków, które należy wykonać przed migracją danych konta magazynu. Aby uzyskać więcej informacji, zobacz [migracji do usługi Azure Premium Storage (niezarządzany dysków)](storage-migration-to-premium-storage.md).

**Jak przenieść z konta magazynu klasycznego do konta usługi Magazyn Azure Resource Manager?**

1.  Można użyć polecenia cmdlet Move-AzureStorageAccount.

2.  To polecenie cmdlet ma wiele kroków (weryfikacji, przygotowanie, Commit) i można sprawdzić przeniesienie, przed wprowadzeniem faktycznie.

3.  Jeśli masz także maszyn wirtualnych, istnieje kilka dodatkowych kroków, które należy wykonać przed migracją danych konta magazynu. Aby uzyskać więcej informacji, zobacz [migracji zasobów IaaS ze środowiska klasycznego do usługi Azure Resource Manager przy użyciu programu Azure PowerShell](../..//virtual-machines/windows/migration-classic-resource-manager-ps.md).

**Jak pobrać danych na komputerze z systemem Linux z konta magazynu platformy Azure, lub przekazania danych z maszyny z systemem Linux?**

Korzystając z wiersza polecenia platformy Azure.

-   Pobieranie pojedynczego obiektu blob

        azure storage blob download -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -b "<Remote File Name>" -d "<Local path where the file will be downloaded to>"

-   Przekaż pojedynczego obiektu blob: 

        azure storage blob upload -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -f "<Local File Name>"

**Jak można I umożliwić innym osobom dostęp do mojej zasobów magazynu?**

Aby umożliwić innym osobom dostęp do zasobów magazynu:

-   Token dostępu sygnatury dostępu Współdzielonego umożliwia uzyskanie dostępu do zasobu. 

-   Podaj użytkownika z kluczem podstawowym lub pomocniczym dla konta magazynu. Aby uzyskać więcej informacji, zobacz [Zarządzanie kontem magazynu](storage-create-storage-account.md#manage-your-storage-account).

-   Zmień zasady dostępu zezwala na dostęp anonimowy. Aby uzyskać więcej informacji, zobacz [udzielanie uprawnień użytkownikom anonimowym do kontenerów i obiektów blob](../blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

**Narzędzie AzCopy zainstalowanym?**

-   Jeśli dostęp do narzędzia AzCopy "wiersza polecenia usługi Magazyn Microsoft Azure", wpisz "AzCopy." Wiersz polecenia jest instalacja obok AzCopy.

-   Jeśli zainstalowano 32-bitowej wersji będzie znajduje się w tym miejscu: **% ProgramFiles(x86) %\\SDKs Microsoft\\Azure\\AzCopy.**

-   Jeśli zainstalowano wersję 64-bitowych, będzie on znajduje się tutaj: **% ProgramFiles %\\SDKs Microsoft\\Azure\\AzCopy**.

**Dla konta replikowanego magazynu (na przykład ZRS, GRS lub RA-GRS) jak uzyskać dostęp do danych przechowywanych w regionie pomocniczym?**

-   Jeśli używasz magazynu Strefowo nadmiarowy (ZRS) lub magazynu geograficznie nadmiarowego (GRS), chyba że do pracy awaryjnej nie dostępu danych z regionu pomocniczego. Aby uzyskać więcej informacji na temat procesu trybu failover, zobacz [czego można oczekiwać, jeśli magazyn pracy awaryjnej](storage-disaster-recovery-guidance.md#what-to-expect-if-a-storage-failover-occurs).

-   Jeśli używasz magazynu geograficznie nadmiarowego dostęp do odczytu (**RA-GRS**), dostępne dane z regionu pomocniczego w dowolnym momencie. Aby to zrobić, użyj jednej z następujących metod:  
      
    AzCopy: Dołącz "-dodatkowej" do nazwy konta magazynu w adresie URL do dodatkowej punktu końcowego. Na przykład:  
     
    https://storageaccountname-Secondary.blob.Core.Windows.NET/vhds/BlobName.VHD

    Token sygnatury dostępu Współdzielonego: Użyj tokenu sygnatury dostępu Współdzielonego uzyskują dostęp do danych z punktu końcowego. Aby uzyskać więcej informacji, zobacz [używanie udostępnionych sygnatur dostępu (SAS)](storage-dotnet-shared-access-signature-part-1.md).

**Jak używać domeny niestandardowej HTTPS z moim koncie magazynu? Na przykład, jak utworzyć "https://mystorageaccountname.blob.core.windows.net/images/image.gif" są wyświetlane jako "https://www.contoso.com/images/image.gif"?**

Protokół SSL nie jest obecnie obsługiwane na kontach magazynu z domen niestandardowych.
Jednak można użyć niestandardowej domeny z systemem innym niż HTTPS. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny dla punktu końcowego magazynu obiektów Blob](../blobs/storage-custom-domain-name.md).

**Jak używać FTP można uzyskać dostępu do danych jest konto magazynu?**

Nie istnieje sposób dostępu do konta magazynu bezpośrednio za pomocą FTP. Można jednak skonfigurować maszynę wirtualną platformy Azure, a następnie zainstaluj serwer FTP na maszynie wirtualnej. Program może przechowywać pliki w udziale plików Azure lub dysku danych, która jest dostępna dla maszyny wirtualnej serwera FTP.
Jeśli chcesz tylko do pobierania danych bez konieczności używania Eksploratora magazynu lub podobnej aplikacji, może być można używać tokenu sygnatury dostępu Współdzielonego. Aby uzyskać więcej informacji, zobacz [używanie udostępnionych sygnatur dostępu (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Skontaktuj się z pomocą techniczną.

Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskać szybkie rozwiązanie problemu.