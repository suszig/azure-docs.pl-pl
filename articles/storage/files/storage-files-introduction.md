---
title: "Wprowadzenie do usługi Azure Files | Microsoft Docs"
description: "Omówienie usługi Azure Files umożliwiającej tworzenie i używanie udziałów plików sieciowych w chmurze z wykorzystaniem standardowego protokołu SMB."
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
ms.date: 10/08/2017
ms.author: renash
ms.openlocfilehash: f96aa9fe12aba28e1ac3429f012419341bdf92c1
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-azure-files"></a>Wprowadzenie do usługi Azure Files
Usługa Pliki Azure oferuje w pełni zarządzane udziały plików w chmurze, dostępne za pośrednictwem [protokołu Server Message Block (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) będącego standardem branżowym (znanego również jako Common Internet File System, CIFS). Udziały plików platformy Azure można instalować współbieżne według chmurowych lub lokalnych wdrożeń systemów Windows, Linux i macOS. Ponadto udziały plików usługi Azure Files mogą być buforowane na serwerach z systemem Windows za pomocą usługi Azure File Sync (wersja zapoznawcza) w celu szybkiego dostępu blisko miejsca, w którym dane są używane.

## <a name="videos"></a>Filmy wideo
| Wprowadzenie do usługi Azure File Sync (2 m) | Usługa Azure Files z synchronizacją (konferencja Ignite 2017) (85 m)  |
|-|-|
| [![Zrzut ekranu wideo Wprowadzenie do usługi Azure File Sync — kliknij, aby odtworzyć](./media/storage-files-introduction/azure-file-sync-video-snapshot.png)](https://www.youtube.com/watch?v=Zm2w8-TRn-o) | [![Zrzut ekranu prezentacji usługi Azure Files z synchronizacją — kliknij, aby odtworzyć](./media/storage-files-introduction/azure-files-ignite-2017-video.png)](https://www.youtube.com/watch?v=r26jWDGF_rg) |

## <a name="why-azure-files-is-useful"></a>Na czym polega przydatność usługi Azure Files
Udziałów plików platformy Azure można używać w następujących celach:

* **Zastępowanie lub uzupełnianie lokalnych serwerów plików**:  
    Usługi Azure Files można użyć w celu całkowitego zastąpienia lub uzupełnienia tradycyjnych lokalnych serwerów plików lub urządzeń NAS. W popularnych systemach operacyjnych, takich jak Windows, macOS i Linux, udziały plików platformy Azure można bezpośrednio zainstalować bez względu na to, gdzie się one znajdują. Udziały plików platformy Azure można również replikować za pomocą usługi Azure File Sync na serwerach z systemem Windows, lokalnie lub w chmurze, w celu zapewnienia wydajnego i rozproszonego buforowania danych, gdziekolwiek są używane.

* **Migrowanie aplikacji metodą „lift and shift”**:  
    Usługa Azure File Storage ułatwia migrowanie do chmury metodą „lift and shift” tych aplikacji, które oczekują udziału plików do przechowywania danych aplikacji lub danych użytkownika. Usługa Azure Files umożliwia zarówno klasyczny scenariusz migracji metodą „lift and shift”, w którym aplikacja i jej dane są przenoszone do platformy Azure, jak i scenariusz hybrydowy migracji „lift and shift”, w którym dane aplikacji są przenoszone do usługi Azure Files, a aplikacja dalej działa lokalnie. 

* **Uproszczenie programowania aplikacji w chmurze**:  
    Usługi Azure Files można także używać na wiele różnych sposobów w celu uproszczenia nowych projektów projektowania aplikacji w chmurze. Na przykład:
    * **Współdzielone ustawienia aplikacji**:  
        W typowym wdrożeniu aplikacji rozproszonych pliki konfiguracji znajdują się w centralnej lokalizacji, skąd są dostępne dla wielu wystąpień aplikacji. Wystąpienia aplikacji mogą ładować swoje konfiguracje za pomocą interfejsu API REST plików i ludzie mogą uzyskiwać do nich dostęp w razie potrzeby przez zainstalowanie udziału SMB lokalnie.

    * **Udział diagnostyczny**:  
        Udział plików usługi Azure Files jest wygodnym miejscem do zapisywania dzienników, metryk i zrzutów awaryjnych aplikacji w chmurze. Dzienniki mogą być zapisywane przez wystąpienia aplikacji za pomocą interfejsu API REST pliku, a deweloperzy mogą uzyskiwać do nich dostęp przez zainstalowanie udziału plików na komputerze lokalnym. Zapewnia to dużą elastyczność, ponieważ deweloperzy mogą obsługiwać opracowywanie rozwiązań dla chmury bez konieczności porzucania istniejących narzędzi, które znają i lubią.

    * **Projektowanie/testowanie/debugowanie**:  
        Deweloperzy i administratorzy często w swojej pracy z maszynami wirtualnymi w chmurze muszą korzystać z zestawu narzędzi i programów narzędziowych. Kopiowanie tych narzędzi do każdej maszyny wirtualnej może być czasochłonne. Przez zainstalowanie udziału plików usługi Azure Files lokalnie na maszynach wirtualnych, deweloper i administrator mogą szybko uzyskać dostęp do swoich narzędzi bez konieczności kopiowania.

## <a name="key-benefits"></a>Najważniejsze korzyści
* **Dostęp współdzielony**. Udziały plików platformy Azure obsługują oparty na standardach branżowych protokół SMB, co oznacza, że można bezproblemowo zastąpić lokalne udziały plików udziałami plików platformy Azure bez obaw o zgodność aplikacji. Możliwość udostępnienia systemu plików na wielu komputerach, aplikacjach/wystąpieniach jest znaczącą korzyścią usługi Azure Files dla aplikacji wymagających możliwości pracy w środowisku współdzielonym. 
* **Pełne zarządzanie**. Udziały plików platformy Azure można tworzyć bez konieczności zarządzania sprzętem czy systemem operacyjnym. Oznacza to, że nie trzeba stosować poprawek systemu operacyjnego serwera w celu zastosowania krytycznych uaktualnień ochrony ani wymieniać uszkodzonych dysków twardych.
* **Skrypty i narzędzia**. Polecenia cmdlet programu PowerShell oraz interfejs wiersza polecenia platformy Azure umożliwiają tworzenie i instalowanie udziałów usługi Azure Files oraz zarządzanie nimi w ramach administracji aplikacjami platformy Azure. Udziały plików platformy Azure można tworzyć i zarządzać nimi za pomocą witryny Azure Portal i programu Azure Storage Explorer. 
* **Odporność**. Usługa Azure Files została zbudowana od podstaw w celu zapewnienia nieprzerwanej dostępności. Dzięki zastąpieniu lokalnych udziałów plików usługą Azure Files już nigdy nie będziesz musieć usuwać lokalnych awarii zasilania ani rozwiązywać problemów z siecią. 
* **Znajomy sposób programowania**. Aplikacje działające na platformie Azure mogą uzyskiwać dostęp do danych udziału za pomocą [interfejsów API We/Wy systemu plików](https://msdn.microsoft.com/library/system.io.file.aspx). Dzięki temu programiści mogą wykorzystać istniejący kod i własne umiejętności, aby zmigrować istniejące aplikacje. Oprócz systemowych interfejsów API We/Wy można używać [bibliotek klienckich usługi Azure Storage](https://msdn.microsoft.com/library/azure/dn261237.aspx) lub [interfejsu API REST usługi Azure Storage](/rest/api/storageservices/file-service-rest-api).

## <a name="next-steps"></a>Następne kroki
* [Create Azure File Share](storage-how-to-create-file-share.md) (Tworzenie udziału plików platformy Azure)
* [Connect and Mount on Windows](storage-how-to-use-files-windows.md) (Nawiązywanie połączenia i instalowanie w systemie Windows)
* [Connect and Mount on Linux](storage-how-to-use-files-linux.md) (Nawiązywanie połączenia i instalowanie w systemie Linux)
* [Connect and Mount on macOS](storage-how-to-use-files-mac.md) (Nawiązywanie połączenia i instalowanie w systemie macOS)
