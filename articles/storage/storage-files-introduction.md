---
title: "Wprowadzenie do usługi Azure File Storage | Microsoft Docs"
description: "Omówienie usługi Azure File Storage umożliwiającej tworzenie i używanie udziałów plików sieciowych na platformie Microsoft Cloud zgodnie ze standardami branżowymi."
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
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: bae2e9825bf158bb015ec0affa56f15ce5baa201
ms.contentlocale: pl-pl
ms.lasthandoff: 07/26/2017

---

# <a name="introduction-to-azure-file-storage"></a>Wprowadzenie do usługi Azure File Storage
Usługa Azure File Storage oferuje udziały plików sieciowych w chmurze przy użyciu zgodnych ze standardami branżowymi protokołów [bloku komunikatów serwera (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) i [Common Internet File System (CIFS)](https://technet.microsoft.com/library/cc939973.aspx). Udziały plików platformy Azure mogą być instalowane równolegle przez klientów, takich jak wdrożenia lokalne systemu Windows, macOS i Linux, oraz przez usługę Azure Virtual Machines. Konto magazynu ogólnego przeznaczenia zapewnia dostęp do usługi Azure File Storage i innych usług, takich jak Blobs, Azure Virtual Machine Disks i Queues, w ramach jednego konta.



## <a name="videos"></a>Filmy wideo
| Wprowadzenie do usługi Azure File Storage (27 min) | Samouczek usługi Azure File Storage (5 minut)  |
|-|-|
| [![Zrzut ekranu przedstawiający film wideo Wprowadzenie do usługi Azure File Storage — kliknij, aby odtworzyć!](media/storage-file-storage/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Zrzut ekranu przedstawiający samouczek usługi Azure File Storage — kliknij, aby otworzyć!](media/storage-file-storage/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Na czym polega przydatność usługi Azure File Storage
Usługa Azure File Storage umożliwia zastąpienie hostowanych lokalnie lub w chmurze serwerów plików działających w systemach Windows Server, Linux lub w oparciu o technologię NAS pozbawionym systemu operacyjnego udziałem plików w chmurze. Takie rozwiązanie ma następujące korzyści:

* **Dostęp współdzielony**. Udziały plików platformy Azure obsługują oparty na standardach branżowych protokół SMB, co oznacza, że można bezproblemowo zastąpić lokalne udziały plików udziałami plików platformy Azure bez obaw o zgodność aplikacji. Możliwość udostępnienia systemu plików na wielu komputerach, aplikacjach/wystąpieniach jest znaczącą korzyścią usługi Azure File Storage dla aplikacji wymagających możliwości pracy w środowisku współdzielonym. 
* **Pełne zarządzanie**. Udziały plików platformy Azure można tworzyć bez konieczności zarządzania sprzętem czy systemem operacyjnym. Oznacza to, że nie trzeba stosować poprawek systemu operacyjnego serwera w celu zastosowania krytycznych uaktualnień ochrony ani wymieniać uszkodzonych dysków twardych.
* **Skrypty i narzędzia**. Polecenia cmdlet programu PowerShell oraz interfejs wiersza polecenia platformy Azure umożliwiają tworzenie i instalowanie udziałów usługi File Storage oraz zarządzanie nimi w ramach administracji aplikacjami platformy Azure. Udziały plików platformy Azure można tworzyć i zarządzać nimi za pomocą witryny Azure Portal i programu Azure Storage Explorer. 
* **Odporność**. Usługa Azure File Storage została zbudowana od podstaw w celu zapewnienia nieprzerwanej dostępności. Dzięki zastąpieniu lokalnych udziałów plików usługą Azure File Storage już nigdy nie będziesz musieć usuwać lokalnych awarii zasilania ani rozwiązywać problemów z siecią. 
* **Znajomy sposób programowania**. Aplikacje działające na platformie Azure mogą uzyskiwać dostęp do danych udziału za pomocą [interfejsów API We/Wy systemu plików](https://msdn.microsoft.com/library/system.io.file.aspx). Dzięki temu programiści mogą wykorzystać istniejący kod i własne umiejętności, aby zmigrować istniejące aplikacje. Oprócz systemowych interfejsów API We/Wy można używać [bibliotek klienckich usługi Azure Storage](https://msdn.microsoft.com/library/azure/dn261237.aspx) lub [interfejsu API REST usługi Azure Storage](/rest/api/storageservices/file-service-rest-api).

Udziałów plików platformy Azure można używać w następujących celach:

* **Zastąpienie lokalnych serwerów plików**:  
    Usługi Azure File Storage można użyć w celu całkowitego zastąpienia udziałów plików na tradycyjnych lokalnych serwerach plików lub urządzeniach NAS. W popularnych systemach operacyjnych, takich jak Windows, macOS i Linux, udziały plików platformy Azure można łatwo zainstalować bez względu na to, gdzie się one znajdują.

* **Migrowanie aplikacji metodą „lift and shift”**:  
    Usługa Azure File Storage ułatwia migrowanie do chmury metodą „lift and shift” tych aplikacji, które korzystają z lokalnych udziałów plików w celu współdzielenia danych między częściami aplikacji. Aby to osiągnąć, każda maszyna wirtualna łączy się z udziałem plików, dzięki czemu może odczytywać i zapisywać pliki podobnie jak w przypadku lokalnego udziału plików.

* **Uproszczenie projektowania aplikacji w chmurze**:  
    Usługi Azure File Storage można używać na kilka różnych sposobów w celu uproszczenia nowych projektów projektowania aplikacji w chmurze.
    * **Współdzielone ustawienia aplikacji**:  
        W typowym wdrożeniu aplikacji rozproszonych pliki konfiguracji znajdują się w centralnej lokalizacji, skąd są dostępne dla wielu różnych maszyn wirtualnych. Takie pliki konfiguracji można teraz przechowywać w udziale plików platformy Azure, skąd mogą być odczytywane przez wszystkie wystąpienia aplikacji. Te ustawienia mogą być także zarządzane za pośrednictwem interfejsu REST, co zapewnia globalny dostęp do plików konfiguracji.

    * **Udział diagnostyczny**:  
        Udziałów plików platformy Azure można również używać do zapisywania plików diagnostycznych, takich jak dzienniki, metryki i zrzuty awaryjne. Po ich udostępnieniu za pomocą protokołu SMB i interfejsu REST aplikacje mogą budować różne narzędzia analizy do przetwarzania i analizowania danych diagnostycznych oraz korzystać z tych narzędzi.

    * **Projektowanie/testowanie/debugowanie**:  
        Deweloperzy i administratorzy często w swojej pracy z maszynami wirtualnymi w chmurze muszą korzystać z zestawu narzędzi i programów narzędziowych. Instalowanie i rozpowszechnianie tych narzędzi na każdej maszynie wirtualnej, na której są one potrzebne, może być czynnością czasochłonną. Korzystając z usługi Azure File Storage, deweloper lub administrator może zapisać swoje ulubione narzędzia w udziale plików, z którym można nawiązać połączenie z dowolnej maszyny wirtualnej.
        
## <a name="how-does-it-work"></a>Jak to działa?
Zarządzanie udziałami plików platformy Azure jest znacznie prostsze niż zarządzanie lokalnymi udziałami plików. Na poniższym diagramie przedstawiono schemat zarządzania usługą Azure File Storage:

![Struktura plików](../../includes/media/storage-file-concepts-include/files-concepts.png)

* **Konto magazynu**: cały dostęp do usługi Azure Storage odbywa się przez konto magazynu. Aby uzyskać szczegółowe informacje na temat pojemności konta magazynu, zobacz temat Cele usługi Azure Storage dotyczące skalowalności i wydajności.
* **Udział**: udział usługi File Storage jest udziałem plików SMB na platformie Azure. Wszystkie pliki i katalogi muszą być tworzone w udziale nadrzędnym. Konto może zawierać nieograniczoną liczbę udziałów, a udział może obejmować nieograniczoną liczbę plików. Maksymalna całkowita objętość udziału plików to 5 TB.
* **Katalog**: opcjonalna hierarchia katalogów.
* **Plik**: plik w udziale. Plik może mieć maksymalnie 1 TB pojemności.
* **Format adresu URL**: adresy URL plików mają następujący format:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```
## <a name="next-steps"></a>Następne kroki
* [Create Azure File Share](storage-file-how-to-create-file-share.md) (Tworzenie udziału plików platformy Azure)
* [Connect and Mount on Windows](storage-file-how-to-use-files-windows.md) (Nawiązywanie połączenia i instalowanie w systemie Windows)
* [Connect and Mount on Linux](storage-how-to-use-files-linux.md) (Nawiązywanie połączenia i instalowanie w systemie Linux)
* [Connect and Mount on macOS](storage-file-how-to-use-files-mac.md) (Nawiązywanie połączenia i instalowanie w systemie macOS)
* [Często zadawane pytania](storage-files-faq.md)
* [Rozwiązywanie problemów](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Artykuły koncepcyjne i filmy
* [Azure File Storage: a frictionless cloud SMB file system for Windows and Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/) (Azure File Storage: płynnie działający system plików SMB w chmurze dla systemów Windows i Linux)

### <a name="tooling-support-for-azure-file-storage"></a>Narzędzia dostępne dla usługi Azure File Storage
* [Używanie programu Azure PowerShell z usługą Azure Storage](storage-powershell-guide-full.md)
* [How to use AzCopy with Microsoft Azure Storage](storage-use-azcopy.md) (Jak używać narzędzia AzCopy z usługą Microsoft Azure Storage)
* [Używanie interfejsu wiersza polecenia platformy Azure z usługą Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)

### <a name="blog-posts"></a>Wpisy na blogach
* [Azure File storage is now generally available](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/) (Usługa Azure File Storage została udostępniona publicznie)
* [Inside Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/) (Za kulisami usługi Azure File Storage)
* [Introducing Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx) (Wprowadzenie do usługi plików platformy Microsoft Azure)
* [Migrowanie danych do usługi Pliki Azure](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Dokumentacja
* [Dokumentacja biblioteki klienta usługi Storage dla programu .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Dokumentacja interfejsu API REST usługi plików](http://msdn.microsoft.com/library/azure/dn167006.aspx)

