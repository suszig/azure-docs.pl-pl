---
title: "Często zadawane pytania dotyczące usługi pliki Azure | Dokumentacja firmy Microsoft"
description: "Odpowiedzi na często zadawane pytania dotyczące usługi pliki Azure."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/13/2017
ms.author: renash
ms.openlocfilehash: 91c46ea0897f83811cfa5c1a8b67677caff14569
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="frequently-asked-questions-about-azure-files"></a>Usługa pliki Azure — często zadawane pytania
[Usługa pliki Azure](storage-files-introduction.md) oferuje pełni zarządzanych udziałów plików w chmurze, które są dostępne za pośrednictwem branżowy standard [protokołu bloku komunikatów serwera (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (znanej także jako Common Internet File System lub CIFS). Udziały plików platformy Azure można instalować współbieżne według chmurowych lub lokalnych wdrożeń systemów Windows, Linux i macOS. Ponadto udziały plików platformy Azure mogą być buforowane na serwerach z systemem Windows z synchronizacji plików Azure (wersja zapoznawcza), zapewniania szybkiego dostępu bliskie gdzie jest używane dane.

W tym artykule opisano niektóre często zadawane pytania dotyczące funkcji plików Azure i funkcje, w tym synchronizacji plików Azure. Jeśli nie widzisz odpowiedzi na pytanie, w tym miejscu nie niechętnie dotrzeć do nas za pośrednictwem następujących kanałów (w coraz szybciej kolejności):

1. W sekcji uwag w tym artykule.
2. [Forum usługi Azure Storage](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Usługa pliki Azure UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: Aby utworzyć nową sprawę pomocy technicznej, przejdź do "Pomoc i obsługa techniczna" karcie w portalu Azure, a następnie kliknij przycisk "Nowy obsługują żądania".

## <a name="general"></a>Ogólne
* <a id="why-files-useful"></a>**Pliki Azure jest przydatne**  
   Usługa pliki Azure umożliwia tworzenie udziałów plików w chmurze bez konieczności zarządzania obciążenie serwera fizycznego lub urządzeń/urządzenia. Oznacza to, można poświęcają mniej czasu, stosowania aktualizacji systemu operacyjnego i wymiany uszkodzonych dysków — jak wszystkie monotonii pracę za Ciebie. Aby dowiedzieć się więcej o scenariuszach może ułatwić plików Azure, zobacz [dlaczego usługi pliki Azure jest przydatne](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>**Jakie są różne sposoby na dostęp do plików w plikach Azure?**  
    Udziałów plików na komputerze lokalnym przy użyciu protokołu SMB 3.0 lub użyj narzędzi, takich jak [Eksploratora usługi Storage](http://storageexplorer.com/) na dostęp do plików w udziale plików. Z aplikacji Aby uzyskać dostęp do plików w udziale plików Azure można użyć bibliotek klienckich magazynu, interfejsy API REST, programu PowerShell lub interfejsu wiersza polecenia.

* <a id="what-is-afs"></a>**Co to jest synchronizacja plików Azure?**  
    Synchronizacja programu Azure pliku umożliwia scentralizowanie udziałów plików w organizacji w plikach Azure bez zwiększanie elastyczność, wydajności i zgodności serwera plików lokalnych. Jest to realizowane poprzez przekształcanie systemów Windows Server w szybką pamięć podręczną udziału plików platformy Azure. Możesz użyć dowolnego dostępnego protokołu w systemie Windows Server w celu uzyskania lokalnego dostępu do danych (w tym protokołu SMB, systemu plików NFS i protokołu FTPS) i możesz mieć dowolną potrzebną Ci liczbę pamięci podręcznych na całym świecie.

* <a id="files-versus-blobs"></a>**Dlaczego użyje udział plików Azure w porównaniu z magazynu obiektów Blob platformy Azure dla danych**  
    Usługa pliki Azure i obiektów Blob platformy Azure magazynu zarówno możliwości przechowywania dużych ilości danych w chmurze, ale są przydatne do nieco innych celów. Magazyn obiektów Blob Azure jest przydatne w przypadku bardzo dużej skali, chmury natywnych aplikacji, które muszą do przechowywania danych bez struktury. Aby zmaksymalizować wydajność i skalę, magazynu obiektów Blob platformy Azure jest prostsze abstrakcji magazynu niż system plików wartość true. Ponadto magazynu obiektów Blob platformy Azure mogą być dostępne tylko za pomocą biblioteki klienta opartego na interfejsie REST (lub bezpośrednio za pomocą protokołu opartego na interfejsie REST).

    Usługa pliki Azure z drugiej strony w szczególności ma na celu jako system plików, biorąc pod uwagę abstract pliku możesz znają i lubią wieloletnim doświadczeniu lokalnych systemów operacyjnych. Przykład do magazynu obiektów Blob platformy Azure plików Azure oferuje interfejsu REST i biblioteki klienta opartego na interfejsie REST, ale w przeciwieństwie do magazynu obiektów Blob platformy Azure, plików Azure oferuje również SMB dostęp do udziałów plików Azure. Oznacza to, że można bezpośrednio zainstalować udział plików Azure systemu Windows, Linux lub system macOS — lokalnie lub w chmurze maszyn wirtualnych, bez konieczności pisania kodu lub dołączyć żadnych specjalnych sterowników w systemie plików. Ponadto udziały plików platformy Azure mogą być buforowane na serwerach plików lokalnych przy użyciu synchronizacji plików Azure umożliwiającą szybki dostęp bliskie gdzie jest używane dane. 
   
    Aby uzyskać bardziej szczegółowym omówieniem na temat różnic między plikami Azure i magazynu obiektów Blob platformy Azure, zobacz [decydowania, kiedy należy używać magazynu obiektów Blob platformy Azure, Azure plików lub dysków Azure](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Aby dowiedzieć się więcej na temat magazynu obiektów Blob platformy Azure, zobacz [wprowadzenie do magazynu obiektów Blob](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Dlaczego użyje udziału plików platformy Azure i dysków Azure?**  
    Dysk Azure jest po prostu tego dysku. Dysk autonomicznego przez siebie, nie jest bardzo przydatny — można pobrać wartości z dysku platformy Azure, należy dołączyć do maszyny wirtualnej, które działają na platformie Azure. Dyski Azure mogą być używane do wszelkich i wszystko używasz dysku na serwerze lokalnym: jako dysk systemu OS obszar wymiany w systemie operacyjnym lub jako dedykowanych dla magazynu dla aplikacji. Jedna interesujące dla dysków Azure polega na utworzeniu serwera plików w chmurze do użycia w dokładnie tak samo miejsca, w którym może używać udziału plików platformy Azure. Wdrażanie serwera plików na maszynach wirtualnych Azure to przyciągają, wysokiej wydajności sposób pozyskania magazyn plików na platformie Azure, jeśli wymagane opcje wdrażania (takie jak NFS protokołu pomocy technicznej lub premium magazynu), nie jest obecnie obsługiwany przez pliki Azure. 

    Z drugiej strony uruchomiony na serwerze plików z dysków Azure jako wewnętrznej bazy danych magazynu zazwyczaj będzie znacznie droższe niż przy użyciu udziału plików platformy Azure z kilku powodów. Po pierwsze oprócz płatności dla magazynu danych na dysku, musi również płacisz za wydatków uruchomionych maszyn wirtualnych platformy Azure. Po drugie musi również zarządzać maszyn wirtualnych, używany do uruchamiania serwera plików, takich jak jest odpowiedzialny za uaktualnień systemu operacyjnego itd. Ponadto jeśli ostatecznie wymaga się, że dane buforowane lokalnie, to do konfigurowania i zarządzania nią technologii replikacji (na przykład replikacji rozproszonego systemu plików) to zrobić.

    Interesujące sposobem najlepsze zarówno plików Azure, jak i hostowanych na maszynach wirtualnych Azure z dyskami Azure jako magazynu zaplecza, serwer plików jest zainstalowanie synchronizacji plików Azure na serwerze plików hostowanych maszyn wirtualnych chmury. Jeśli udział plików Azure znajduje się w tym samym regionie co serwer plików, można włączyć obsługę poziomów w chmurze i ustaw procent wolnego miejsca w woluminie na maksymalną (99%). Dzięki temu minimalnego duplikacji danych, można użyć w dowolnej aplikacji ma się na serwerach plików, takie niczego wymagających protokołu NFS obsługuje integrację.

    Aby uzyskać informacje na jednym ze sposobów Konfigurowanie wysokiej wydajności i serwera plików o wysokiej dostępności na platformie Azure, zobacz [wdrażanie IaaS klastry gości maszyny Wirtualnej w systemie Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Aby uzyskać bardziej szczegółowym omówieniem na temat różnic między plikami Azure i dysków Azure, zobacz [decydowania, kiedy należy używać magazynu obiektów Blob platformy Azure, Azure plików lub dysków Azure](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Aby dowiedzieć się więcej na temat dysków Azure, zobacz [omówienie dysków zarządzanych Azure](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>**Jak rozpocząć pracę przy użyciu plików Azure?**  
    Wprowadzenie do usługi pliki Azure jest łatwe: wystarczy [utworzyć udział plików](storage-how-to-create-file-share.md) i instalacji systemu operacyjnego preferowanych: 

    - [Zainstaluj w systemie Windows](storage-how-to-use-files-windows.md)
    - [Zainstaluj w systemie Linux](storage-how-to-use-files-linux.md)
    - [Zainstaluj na macOS](storage-how-to-use-files-mac.md)

    Aby uzyskać więcej informacji na temat Przewodnik na temat wdrażania udział plików Azure zastąpić produkcji udziałów plików w organizacji, zobacz [planowania wdrożenia usługi pliki Azure](storage-files-planning.md).

* <a id="redundancy-options"></a>**Jakie opcje nadmiarowość magazynu są obsługiwane przez usługi pliki Azure?**  
    Usługa pliki Azure aktualnie obsługuje tylko magazyn lokalnie nadmiarowy (LRS) i magazynu geograficznie nadmiarowego (GRS) w tej chwili. Firma Microsoft planuje obsługiwać magazyn strefowo nadmiarowy (ZRS) i dostęp do odczytu magazynu geograficznie nadmiarowego (RA-GRS) w przyszłości, ale nie posiadają osi czasu, aby udostępnić w tej chwili.

* <a id="tier-options"></a>**Warstwy magazynowania, które są obecnie obsługiwane przez usługi pliki Azure?**  
    Usługa pliki Azure aktualnie obsługuje tylko warstwy magazynu w warstwie standardowa. Obecnie nie ma osi czasu, aby udostępnić — wersja premium i chłodnych pomocy technicznej. Należy pamiętać, że nie można utworzyć udziały plików platformy Azure z konta magazynu tylko do obiektów Blob lub konta Premium magazynu.

* <a id="give-us-feedback"></a>**Czy na pewno chcesz zobaczyć *x* funkcja dodana do usługi pliki Azure, możesz dodać ją?**  
    Oczywiście zespół usługi pliki Azure jest zainteresowana wysłuchaniu wszystkie opinie, które masz o naszej usługi. Głosowania żądania funkcji na [UserVoice plików Azure](https://feedback.azure.com/forums/217298-storage/category/180670-files)! Firma Microsoft jest czekamy na delighting możesz z wielu nowych funkcji.

## <a name="azure-file-sync"></a>Synchronizacja plików na platformę Azure
* <a id="afs-region-availability"></a>**Jakie regiony są obecnie obsługiwane dla synchronizacji plików Azure (wersja zapoznawcza)?**  
    Synchronizacja programu Azure plik jest aktualnie dostępny w zachodnie stany USA, Europa Zachodnia, Australia Wschodnia i Azja południowo-wschodnia. Dodamy obsługę dodatkowych regionów, jak możemy dążyć do ogólnej dostępności. Aby uzyskać dodatkowe informacje, zobacz [dostępność w danym regionie](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>**W tej samej grupie synchronizacji może mieć przyłączony do domeny i serwery przyłączone do domeny?**  
    Tak, grupy synchronizacji może zawierać punktów końcowych serwera, mające różne członkostwa usługi Active Directory, łącznie z nie są przyłączone do domeny. Podczas konfiguracji technicznie działa, nie zaleca się to jako normalne konfiguracji list ACL definiowanych dla plików/folderów na jednym serwerze może nie być w stanie mają być egzekwowane przez inne serwery w grupie synchronizacji. Aby uzyskać najlepsze wyniki zaleca się synchronizacji między serwerami albo tego samego lasu usługi Active Directory, serwerami w różnych lasach usługi Active Directory z relacjami zaufania ustalonych lub serwerów nie znajduje się w domenie, ale nie kombinacji wszystkie powyższe.

* <a id="afs-change-detection"></a>**Utworzono plik bezpośrednio w mojej udziału plików platformy Azure przy użyciu protokołu SMB lub za pośrednictwem portalu. Jak długo, aż do pliku jest synchronizowany z serwerami w grupie synchronizacji?** 
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Gdy ten sam plik zostanie zmieniona na dwóch serwerach w tym samym czasie, co się stanie?**  
    Synchronizacja programu Azure pliku używa strategię rozpoznawania konfliktu prosty: Firma Microsoft zachować zmiany. Najbardziej ostatnio zapisany zachowuje oryginalna nazwa pliku. Starszy plik ma maszyny 'source' i numer konflikt dodanym na końcu nazwy z tym taksonomii: 
   
    `<FileNameWithoutExtension>-<MachineName>[-#].<ext>`  

    Na przykład pierwszy konflikt z `CompanyReport.docx` staje się `CompanyReport-CentralServer.docx` Jeśli `CentralServer` jest, gdzie wystąpił starsze zapisu. Drugi konflikt może być zidentyfikowany jako `CompanyReport-CentralServer-1.docx`.

* <a id="afs-storage-redundancy"></a>**Magazyn geograficznie nadmiarowy (GRS) jest obsługiwana dla synchronizacji plików Azure?**  
    Tak, zarówno magazyn lokalnie nadmiarowy (LRS), jak i Magazyn geograficznie nadmiarowy (GRS) są obsługiwane przez synchronizacji plików Azure. W przypadku pracy awaryjnej GRS między regionami sparowanego firma Microsoft zaleca, traktując nowy region jako kopia zapasowa tylko danych. Synchronizacja programu Azure pliku nie jest automatycznie uruchamiany synchronizowanie z nowego regionu podstawowego. 

* <a id="sizeondisk-versus-size"></a>**Dlaczego nie *rozmiaru na dysku* właściwości przeznaczony do porównywania plików *rozmiar* właściwości po użyciu synchronizacji plików Azure?**  
    Odszyfrowywanie udostępnia dwie właściwości **rozmiar** i **rozmiaru na dysku** do reprezentowania rozmiaru pliku. Te właściwości różnią się w niewielkim stopniu w rozumieniu; **Rozmiar** reprezentuje pełną rozmiar pliku, podczas gdy **rozmiaru na dysku** reprezentuje rozmiar strumienia pliku przechowywane na dysku. Te różnią się z różnych powodów, takich jak kompresja, za pomocą deduplikacji danych lub w tym przypadku, Obsługa poziomów z synchronizacji plików Azure w chmurze. Jeśli plik jest warstwowa do udziału plików Azure rozmiaru na dysku będzie zerem, ponieważ strumień pliku jest przechowywana w udziale plików platformy Azure, nie na dysku. Istnieje również możliwość dla pliku to częściowo warstwowych (lub częściowo odwołane), co oznacza, że część pliku znajduje się na dysku. Może to nastąpić, gdy pliki częściowo są odczytywane przez aplikacje, takie jak odtwarzacze multimedialne lub kompresja narzędzia. 

* <a id="is-my-file-tiered"></a>**Jak sprawdzić, czy plik został warstwowej?**  
    Istnieje kilka sposobów, aby sprawdzić, czy plik został warstwy do pliku Azure udziału:
    
    1. **Sprawdź atrybutów pliku.** Aby to zrobić, kliknij prawym przyciskiem myszy plik, przejdź do **szczegóły** i przewiń w dół do **atrybuty** właściwości. Warstwowych plik będzie miał następujący zestaw atrybutów:     
        
        | Atrybut list | Atrybut | Definicja |
        |:----------------:|-----------|------------|
        | A | Archiwum | Wskazuje, że plik kopii zapasowych za pomocą oprogramowania kopii zapasowej. Ten atrybut ma zawsze wartość niezależnie od tego, czy plik jest warstwowej lub pełni przechowywane na dysku. |
        | P | Plik rozrzedzony | Wskazuje, że plik jest plik rozrzedzony, który jest specjalistyczną odmianą pliku NTFS oferuje efektywne wykorzystanie, gdy plik na dysku strumienia przede wszystkim jest pusty. Synchronizacja programu Azure pliku używa plików rozrzedzonych, ponieważ plik jest w pełni do warstwy, co oznacza jego strumień pliku jest zapisywane tylko w chmurze albo częściowo odwołane, co oznacza część pliku znajduje się już na dysku. Jeśli plik jest w pełni przypomnieć na dysku, synchronizacji plików Azure przekonwertuje go z pliku rozrzedzonego zwykły plik. |
        | L | Punkt ponownej analizy | Wskazuje, że plik ma punkt ponownej analizy, który ma specjalne wskaźnika do użycia przez filtru systemu plików. Synchronizacja programu Azure pliku używa punktów ponownej analizy w celu zdefiniowania do synchronizacji plików Azure filtru systemu plików (StorageSync.sys) w chmurze plik przechowywania. Dzięki temu bezproblemowy dostęp bez użytkownika końcowego nawet znajomości służy synchronizacji plików Azure i sposobu uzyskania dostępu do pliku w udziale plików Azure. Gdy pełni przypomina plik, synchronizacji plików Azure usuwa punktu ponownej analizy z pliku. |
        | O | W trybie offline | Wskazuje, że niektóre lub wszystkie zawartość pliku nie są zapisywane na dysku. Pełni przypomina plik, synchronizacji plików Azure usuwa tego atrybutu. |

        ![Okno dialogowe właściwości dla pliku z wybraną kartą szczegóły](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Istnieje również możliwość wyświetlić wszystkie pliki w folderze atrybuty przez dodanie **atrybuty** do wyświetlania tabeli Exporer pliku. Aby to zrobić, kliknij prawym przyciskiem myszy istniejącą kolumnę (na przykład rozmiar), wybierz **więcej** i wybierz **atrybuty** z listy rozwijanej.
        
    2. **Użyj `fsutil` do sprawdzenia punkty ponownej analizy w pliku.** Jak wspomniano w poprzedniej opcji, warstwowy pliku zawsze będą mieć punktu ponownej analizy lub specjalne wskaźnika dla synchronizacji plików Azure filtr systemu plików (StorageSync.sys), należy ustawić. Można sprawdzić, czy plik ma ponownej analizy punktów z `fsutil` narzędzi w sesji programu PowerShell lub wiersz polecenia z podwyższonym poziomem uprawnień:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Jeśli plik zawiera punktu ponownej analizy, powinny pojawić się **ponownej analizy, wartość tagu: 0x8000001e**. Ta wartość szesnastkową jest wartość punktu ponownej analizy, należących do synchronizacji plików Azure. Dane wyjściowe zawiera również ponownej analizy, które udostępniać dane reprezentujące ścieżkę do pliku w pliku Azure.

        > [!Warning]  
        > `fsutil reparsepoint` Polecenia narzędzia zawiera także możliwość usuwania punktu ponownej analizy. Nie wykonuj tego polecenia, chyba że zdecyduje do zespołu inżynieryjnego synchronizacji plików Azure — może spowodować utratę danych. 

* <a id="afs-recall-file"></a>**Plik, który ma być użyty ma zostały warstwowy... jak I na dysku do użycia lokalnego odwołania?**  
    Najprostszym sposobem odwołanie pliku na dysku jest aby go otworzyć. Filtr systemu plików (StorageSync.sys) synchronizacji plików Azure bezproblemowo Pobierz plik z udziału plików Azure bez konieczności wykonywania pracy. Dla typów plików, które mogą być częściowo odczytu, takich jak multimedia lub pliki zip, otwieranie pliku nie spowoduje pobieranie całego pliku.

    Możliwe jest również możliwe życie pliku do przywołania przy użyciu programu PowerShell. Na przykład może to być przydatne, jeśli chcesz odwołać wielu plików jednocześnie (na przykład wszystkie pliki w folderze). Otwórz sesję programu PowerShell do węzła serwera zainstalowanym synchronizacji plików Azure i uruchom następujące polecenia programu PowerShell:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>**Jak można wymusić plik lub katalog do należeć do warstwy?**  
    Po włączeniu funkcji warstw chmury automatycznie warstwy plików według ostatniego dostępu i zmodyfikować razy, aby osiągnąć procent wolnego miejsca na wolumin określony dla punktu końcowego w chmurze, jednak czasami może chcesz wymusić plik do warstwy ręcznie. Na przykład może to być przydatne, jeśli zapiszesz dużych plików, które nie będą ponownie przez długi czas i wymagana ilość wolnego miejsca na woluminie teraz pliki/foldery. Można wymusić stosowanie warstw za pomocą następujących poleceń programu PowerShell:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-files-excluded"></a>**Które pliki i foldery, automatycznie są wyłączone przez synchronizacji plików Azure?**
    Domyślnie synchronizacja plików Azure nie obejmuje następujące pliki:
    
    - Desktop.ini
    - Thumbs.dB
    - ehthumbs.dB
    - ~$\*.\*
    - \*laccdb
    - \*TMP
    - 635D02A9D91C401B97884B82B3BCDAEA.\*

    Następujące foldery są również wykluczone domyślnie:

    - \System volume Information
    - \$ODTWÓRZ. BIN
    - \SyncShareState

* <a id="afs-os-support"></a>**Chcę użyć synchronizacji plików Azure z systemem Windows Server 2008 R2 z systemem Linux, lub z urządzeniem NAS - synchronizacji plików Azure obsługuje który?**
    Obecnie synchronizacji plików Azure obsługuje tylko systemu Windows Server 2016 i Windows Server 2012 R2. W tej chwili nie mamy żadnych innych planów, które firma Microsoft może udostępniać, ale jest otwarty i zainteresowanych do obsługi dodatkowych platform na życzenie klientów. Prosimy o kontakt platform chcesz nam obsługuje na [UserVoice plików Azure](https://feedback.azure.com/forums/217298-storage/category/180670-files).

## <a name="security-authentication-and-access-control"></a>Zabezpieczenia, uwierzytelniania i kontroli dostępu
* <a id="ad-support"></a>**To jest uwierzytelnianie oparte na usłudze Active Directory i kontroli dostępu, obsługiwane przez usługi pliki Azure?**  
    Usługa pliki Azure umożliwia zarządzanie kontrolą dostępu na dwa sposoby:

    - Za pomocą sygnatury dostępu Współdzielonego, można wygenerować tokeny z określonymi uprawnieniami, które są ważne przez ustalony czas. Można na przykład wygenerować token z dostępem tylko do odczytu do danego pliku o upływie 10 minut. Każdy, kto posiada token, gdy jest on prawidłowy ma dostęp tylko do odczytu do tego pliku dla tych 10 minut. Sygnatury dostępu Współdzielonego klucze są obsługiwane tylko za pośrednictwem interfejsu API REST lub bibliotek klienckich dzisiaj, należy zainstalować udział plików Azure za pośrednictwem protokołu SMB, z kluczy konta magazynu.

    - Synchronizacja programu Azure pliku zachować, a następnie replikację wszystkich list ACL (na podstawie usługi AD lub lokalny) do wszystkich punktów końcowych serwera, który synchronizowanych. Ponieważ systemu Windows Server mogą już uwierzytelniać za pomocą usługi Active Directory, synchronizacji plików Azure zapewnia dużą miary uzupełniające do pełną obsługę dla uwierzytelniania w usłudze AD i dociera obsługuje listy kontroli dostępu.

    Usługa pliki Azure bezpośrednio w tej chwili nie obsługuje usługi Active Directory.

* <a id="encryption-at-rest"></a>**Jak I Sprawdź, czy Moje udziału plików platformy Azure jest szyfrowane w rest?**  
    Szyfrowanie na rest trwa jest domyślnie włączone we wszystkich regionach. Dla tych regionów nie trzeba wykonywać żadnych czynności, aby włączyć szyfrowanie. Dla innych regionów, przejrzyj [szyfrowanie po stronie serwera](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>**Jak zapewniają dostęp do określonego pliku w przeglądarce sieci web**  
    Za pomocą sygnatury dostępu Współdzielonego, można wygenerować tokeny z określonymi uprawnieniami, które są ważne przez ustalony czas. Na przykład można wygenerować token z dostępem tylko do odczytu do konkretnego pliku przez określony czas. Każdy, kto posiada ten adres url dostępu do pliku bezpośrednio z dowolnej przeglądarki sieci web, gdy jest on nieprawidłowy. Klucze sygnatur dostępu współdzielonego można łatwo generować z interfejsu użytkownika, na przykład Eksploratora magazynu.

* <a id="file-level-permissions"></a>**Jest to, można określić tylko do odczytu lub uprawnienia tylko do zapisu w folderach w ramach udziału?**  
    W przypadku udziałów plików instalowanych za pomocą protokołu SMB nie masz takiej kontroli nad uprawnieniami. Jednak możesz to zrobić, tworząc sygnaturę dostępu współdzielonego za pośrednictwem interfejsu API REST lub bibliotek klienckich.

* <a id="ip-restrictions"></a>**Czy można zaimplementować ograniczenia adresów IP do udziału plików Azure?**  
    Tak, dostęp do udziału plików Azure można ograniczyć na poziomie konta magazynu. Aby uzyskać więcej informacji, zobacz zobacz [Konfigurowanie zapory magazynu Azure i sieci wirtualne](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>**Co to są zasady zgodności danych obsługiwane w przypadku plików Azure?**  
   Usługa pliki Azure działa w oparciu o takiej samej architekturze magazynu jako inne usługi magazynu w usłudze Azure Storage i stosuje się te same zasady zgodności danych. Więcej informacji na temat usługi Azure Storage danych zgodności, możesz pobrać i odwoływać się do [dokumentu ochrony danych Microsoft Azure](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409) i [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

## <a name="on-premises-access"></a>Dostęp do lokalnego
* <a id="expressroute-not-required"></a>**Czy masz łączenia do usługi pliki Azure za pomocą usługi Azure ExpressRoute lub użycie usługi synchronizacji programu Azure pliku lokalnego?**  
    Nie, usługi dostępu do udziału plików Azure nie jest wymagane. Jeśli udział plików Azure w przypadku instalowania bezpośrednio lokalnymi, jest wymagany jest port 445 (ruch wychodzący TCP) otwórz dostęp do Internetu (jest to port, który komunikuje się SMB). Jeśli używasz synchronizacji usługi Azure pliku wszystko wymaga jest port 443 (ruch wychodzący TCP) dla dostępu HTTPS (nie SMB wymagane). Jednak ExpressRoute może być używana z obu opcji dostępu, w razie potrzeby.

* <a id="mount-locally"></a>**Sposób instalacji udziału plików platformy Azure na komputerze lokalnym**  
    W przypadku zainstalowania udziału plików przy użyciu protokołu SMB, tak długo, jak jest otwarty port 445 (ruch wychodzący TCP) i klient obsługuje protokół SMB 3.0 (na przykład używasz systemu Windows 10 lub Windows Server 2016). Zablokowanie portu 445 w organizacji zasad lub przez Usługodawcę umożliwia synchronizację plików Azure dostęp do udziału plików platformy Azure.

## <a name="backup"></a>Tworzenie kopii zapasowych
* <a id="backup-share"></a>**Jak wykonywanie kopii zapasowych pliku Azure udostępnić?**  
    Można użyć okresowe [udostępnić migawki (wersja zapoznawcza)](storage-how-to-use-files-snapshots.md) ochrony przed przypadkowemu usuwaniu. Można użyć narzędzia AzCopy, RoboCopy, lub 3rd strony narzędzia kopii zapasowych, który można utworzyć kopię zapasową udziału zainstalowanego pliku. 

## <a name="share-snapshots"></a>Udostępnianie migawki
### <a name="share-snapshots---general"></a>Udostępnianie migawek — ogólne
* <a id="what-are-snaphots"></a>**Co to jest migawka udziału pliku?**  
    Migawki udział plików Azure umożliwia tworzenie tylko do odczytu wersji z udziałów plików. Umożliwia on również skopiować starszą wersję kopii zawartości do tego samego udziału lub innej lokalizacji w Azure lub lokalnie do dalszej modyfikacji. Aby dowiedzieć się więcej o udziału migawek, zobacz w [udostępnić migawki omówienie](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>**Gdzie są przechowywane Moje migawki udziału?**  
    Udziału migawki są przechowywane w tym samym kontem magazynu do udziału plików.

* <a id="snapshot-perf-impact"></a>**Czy istnieją wpływ na wydajność?**  
    Udział migawki nie mają żadnych zmniejszenie wydajności.

* <a id="snapshot-consistency"></a>**Udział migawki aplikacji są spójne?**  
    Nie. Migawki udziału nie są aplikacji spójne. Użytkownik będzie musiał opróżnić zapisami z aplikacji do udziału przed wykonaniem migawki udziału.

* <a id="snapshot-limits"></a>**Czy istnieje limit liczby migawek udziału?**  
    Ma limitu 200 migawek udziału, które można zachować pliki Azure. Migawki udziału nie wchodzą w skład przydziału udziału więc ma nr limitu udziału na całkowita ilość miejsca wykorzystana przez wszystkie migawki udziału. Limity konta magazynu jest nadal mają zastosowanie. Po migawki udziału 200 starsze migawki będzie musiał można usunąć, aby było możliwe utworzenie nowego udziału migawki.

### <a name="create-share-snapshots"></a>Tworzenie migawek udziału
* <a id="file-snaphsots"></a>**Można utworzyć migawki udziału poszczególnych plików?**  
    Udział migawki są tworzone na poziomie udziału plików. Możesz przywrócić pojedyncze pliki z migawki udziału pliku, ale nie można utworzyć migawki pliku na poziomie udziału. Jednak miały migawki udziału poziomu udziału należy wyświetlić migawki udziału, gdzie określony plik został zmieniony, możesz to zrobić z doświadczenia "Poprzednie wersje" w udziale zainstalowanego systemu Windows. Prosimy o kontakt Jeśli masz potrzeby funkcji migawki pliku [UserVoice plików Azure](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>**Można utworzyć migawki udziału zaszyfrowanego udziału plików?**  
    Należy utworzyć migawkę udziału udziały plików platformy Azure mający szyfrowania magazynowane włączone. Można przywrócić pliki z migawki udziału do udziału plików zaszyfrowanych. Jeśli na udział jest zaszyfrowany, również być szyfrowana migawki udziału.

* <a id="geo-redundant-snaphsots"></a>**Czy Moje migawki udziału geograficznie nadmiarowego?**
    Migawki udziału będą mieć tego samego nadmiarowość jako udziału plików platformy Azure, dla której zostały pobrane. Jeśli wybrano magazyn geograficznie nadmiarowy (GRS) dla konta migawki udziału również będą przechowywane nadmiarowo w parach regionie.

### <a name="manage-share-snapshots"></a>Zarządzanie migawki udziału
* <a id="browse-snapshots-linux"></a>**Czy można przeglądać Moje migawek udziału z systemem Linux?**  
    Azure CLI 2.0 umożliwia tworzenie listy, przeglądanie i przywracania w systemie Linux.

* <a id="copy-snapshots-to-other-storage-account"></a>**Czy można skopiować migawki udział do innego konta magazynu**  
    Możesz skopiować pliki z udziału migawek do innej lokalizacji, ale nie migawki udziału samodzielnie.

### <a name="restore-data-from-share-snapshots"></a>Przywróć dane z migawki udziału
* <a id="promote-share-snapshot"></a>**Czy można podwyższyć migawki udziału do udziału podstawowej?**  
    Możesz skopiować tylko dane z migawki udziału do dowolnego docelowej lokalizacji. Jednak nie można podwyższyć poziomu migawki udziału do udziału podstawowej.

* <a id="restore-snapshotted-file-to-other-share"></a>**Czy można przywrócić dane z mojej migawki udział do innego konta magazynu?**  
    Tak. Mogą zostać skopiowane pliki z migawki udziału do oryginalnej lub alternatywnej lokalizacji, w tym samym/innego konta magazynu w tej samej lub różnych regionach. Ponadto można kopiować pliki lokalnymi lub innymi chmury.    
  
### <a name="cleanup-share-snapshot"></a>Oczyszczanie udziału migawki
* <a id="delete-share-keep-snapshots"></a>**Można I Usuń Moje udział, ale nie współużytkują migawki?**
    Nie można usunąć z udziału, jeśli masz migawki aktywnego udziału w udziale. Interfejs API jest dostępny do usunięcia migawki udziału wraz z udziału i można uzyskać taki sam, jak również portalu Azure.

* <a id="delete-share-with-snapshots"></a>**Co się dzieje z mojej migawki udziału I usunięcia konta magazynu?**
    Usunięcie konta magazynu migawek udziału zostaną również usunięte.

## <a name="billing-and-pricing"></a>Rozliczeniach i cenach
* <a id="vm-file-share-network-traffic"></a>**Ruch sieciowy między maszyny Wirtualnej platformy Azure i udziału plików platformy Azure liczone jako zewnętrzne użycie przepustowości i rozliczany do subskrypcji?**  
    Jeśli udział plików i maszyna wirtualna znajdują się w tym samym regionie Azure, ruch między nimi jest bezpłatna. Jeśli są w różnych regionach, ruch między nimi będzie rozliczany jako zewnętrzne użycie przepustowości.

* <a id="share-snapshot-price"></a>**Ile korzystają koszt migawki?**
     Podczas udziału Podgląd pojemności migawki nie zostanie obciążona. Mają zastosowanie standardowe wciąż magazynu Wyjście i transakcji kosztów. Po udostępnieniu wersji ogólnodostępnej zarówno pojemności, jak i transakcji w migawce udziału będą naliczane opłaty.
     
     Udział migawki są przyrostowe charakter. Migawka podstawowego udziału jest udziału. Wszystkie migawki udziału kolejnych są przyrostowych i zapisze tylko różnic z wcześniejszej migawki udziału. Rozliczenie jest przeprowadzane tylko w przypadku zmiany zawartości. Jeśli masz Udostępnij 100 GiB danych, ale tylko 5 GiB uległ zmianie po ostatnim migawki udziału, zostanie migawki udziału wykorzystuje tylko 5 GiB dodatkowe i będzie rachunku 105 tylko GiB. Zobacz [strony cennik](https://azure.microsoft.com/pricing/details/storage/files/) uzyskać więcej informacji o transakcji, a opłaty za wyjście standardowe.

## <a name="scale-and-performance"></a>Skalowalność i wydajność
* <a id="files-scale-limits"></a>**Jakie są limity skalowania plików Azure?**  
    Aby uzyskać informacje na skalowalność i wydajność cele plików Azure, zobacz [elementy docelowe skalowalności i wydajności usługi pliki Azure](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>**Wymagany udział plików większych niż pliki Azure obecnie oferuje... może zwiększyć rozmiar Moje udziału plików platformy Azure?**  
    Nie, maksymalny rozmiar udziału plików platformy Azure jest 5 TiB. Ta funkcja jest obecnie nienaruszalne ograniczenie, które firma Microsoft nie można dostosować. Firma Microsoft pracuje nad rozwiązaniem do zwiększenia rozmiaru udziału do 100 TiBs, ale nie posiadają osi czasu, aby udostępnić w tej chwili.

* <a id="open-handles-quota"></a>**Ilu klientów można uzyskać dostępu do tego samego pliku równocześnie?**  
    Brak przydziału otwartych dojść 2000 w jednym pliku. Po utworzeniu 2000 otwartych dojść, wystąpi błąd który zostanie osiągnięty przydział.

* <a id="zip-slow-performance"></a>**Moje wydajności przebiegało powoli, rozpakowywanie plików do usługi pliki Azure. Co zrobić?**  
    Aby przetransferować dużą liczbę plików do usługi pliki Azure, zalecane jest użycie narzędzia AzCopy (z systemem Windows, Linux/Unix w wersji zapoznawczej) lub Azure Powershell jak te narzędzia są zoptymalizowane pod kątem transferu sieciowego.

* <a id="slow-perf-windows-81-2012r2"></a>**Już po zainstalowaniu mój udział plików Azure w systemie Windows Server 2012 R2 lub Windows 8.1 i Moje wydajność jest niska — Dlaczego?**  
    Istnieje znany problem w przypadku instalowania udział plików Azure w systemie Windows Server 2012 R2 i Windows 8.1, która została zastosowana poprawka w aktualizacji zbiorczej kwietnia 2014 r. dla Windows 8.1 i Windows Server 2012 R2. Upewnij się, że ta poprawka zostały zastosowane do uzyskania optymalnej wydajności wszystkich wystąpień systemu Windows Server 2012 R2 i Windows 8.1 (oczywiście zawsze zalecamy biorąc wszystkie poprawki systemu Windows za pośrednictwem usługi Windows Update). Aby uzyskać więcej informacji, zapoznaj się z odpowiednim artykułem KB [powolne działanie, gdy uzyskujesz dostęp do usługi pliki Azure z Windows 8.1 lub Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Funkcje i współdziałanie z innymi usługami
* <a id="cluster-witness"></a>**Można użyć Moje udziału plików platformy Azure jako *monitora udostępniania plików* dla moich klastra pracy awaryjnej systemu Windows Server?**  
    To nie jest obecnie obsługiwany dla udziału plików platformy Azure. Aby uzyskać więcej informacji na temat ustawiania to konto magazynu obiektów Blob platformy Azure, zobacz [wdrażanie monitora chmury dla klastra trybu Failover](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>**W wystąpieniu kontenera Azure mogą zainstalować udział plików Azure?** ?  
    Tak, udziały plików platformy Azure są przyciągają opcję, aby utrwalić informacji poza okres istnienia wystąpienia kontenera. Aby uzyskać więcej informacji, zobacz [instalowanie udziału plików na platformę Azure z wystąpień kontenera Azure](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>**Jest to operacja zmiany nazwy w interfejsie API REST?**  
    Nie w tej chwili.

* <a id="nested-shares"></a>**Czy można zagnieżdżać udziały, innymi słowy, udział w udziale?**  
    Nie. Udział plików to sterownik wirtualny z możliwością instalacji, dlatego zagnieżdżanie nie jest obsługiwane.

* <a id="ibm-mq"></a>**Przy użyciu plików platformy Azure z programem IBM MQ**  
    Firma IBM wydała dokument oprogramowania IBM mq podczas konfigurowania plików Azure z usługi. Aby uzyskać więcej informacji, zobacz: [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service) (Jak skonfigurować menedżera kolejki z wieloma wystąpieniami programu IBM MQ do działania z usługą Magazyn plików Azure).

## <a name="see-also"></a>Zobacz też
* [Rozwiązywanie problemów plików Azure w systemie Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Rozwiązywanie problemów plików Azure w systemie Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Rozwiązywanie problemów z synchronizacji plików Azure (wersja zapoznawcza)](storage-sync-files-troubleshoot.md)