---
title: "Instalowanie udziału plików platformy Azure za pomocą protokołu SMB w systemie macOS | Microsoft Docs"
description: "Dowiedz się, jak zainstalować udział plików platformy Azure za pomocą protokołu SMB w systemie macOS."
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
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 428086910273d10a68cb8193df377a4db267d6a3
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---

# <a name="mount-azure-file-share-over-smb-with-macos"></a>Instalowanie udziału plików platformy Azure za pomocą protokołu SMB w systemie macOS
[Azure File Storage](storage-dotnet-how-to-use-files.md) to usługa firmy Microsoft umożliwiająca tworzenie i używanie udziałów plików sieciowych na platformie Azure zgodnie ze standardami branżowymi. Udziały plików platformy Azure można instalować w systemach macOS Sierra (10.12) i El Capitan (10.11). W tym artykule przedstawiono dwa różne sposoby instalowania udziału plików platformy Azure w systemie macOS — za pomocą interfejsu użytkownika programu Finder i Terminala.

> [!Note]  
> Przed instalowaniem udziału plików platformy Azure przy użyciu protokołu SMB zaleca się wyłączenie podpisywanie pakietów SMB. Niezastosowanie się do tego zalecenia może spowodować zmniejszenie wydajności podczas uzyskiwania dostępu do udziału plików platformy Azure z systemu macOS. Połączenie SMB będzie szyfrowane, więc nie zostanie obniżone bezpieczeństwo Twojego połączenia. Następujące polecenia wykonane z poziomu terminala spowodują wyłączenie podpisywania pakietów SMB, jak opisano w tym [artykule pomocy technicznej firmy Apple dotyczącym wyłączania podpisywania pakietów SMB](https://support.apple.com/HT205926):  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a>Wymagania wstępne dotyczące instalowania udziału plików platformy Azure w systemie macOS
* **Nazwa konta magazynu**: Aby zainstalować udział plików platformy Azure, konieczne będzie podanie nazwy konta magazynu.

* **Klucz konta magazynu**: Aby zainstalować udział plików platformy Azure, konieczne będzie posiadanie podstawowego (lub dodatkowego) klucza magazynu. Klucze sygnatur dostępu współdzielonego nie są aktualnie obsługiwane na potrzeby instalowania.

* **Otwarty port 445**: Protokół SMB komunikuje się za pośrednictwem portu TCP 445. Na komputerze klienckim (komputerze Mac) upewnij się, że zapora nie blokuje portu TCP 445.

## <a name="mount-an-azure-file-share-via-finder"></a>Instalowanie udziału plików platformy Azure za pomocą programu Finder
1. **Otwórz program Finder**: Program Finder jest domyślnie otwarty w systemie macOS, ale możesz się upewnić, że jest on aktualnie wybraną aplikacją, klikając „ikonę twarzy systemu macOS” w Docku:  
    ![Ikona twarzy systemu macOS](media/storage-file-how-to-use-files-mac/mount-via-finder-1.png)

2. **W menu „Idź” wybierz pozycję „Połącz z serwerem”**: W ścieżce UNC z [wymagań wstępnych](#preq) przekształć początkowy podwójny ukośnik odwrotny (`\\`) w ciąg `smb://`, a wszystkie pozostałe ukośniki odwrotne (`\`) w ukośniki (`/`). Twój link powinien wyglądać podobnie do następującego: ![Okno dialogowe „Łączenie z serwerem”](./media/storage-file-how-to-use-files-mac/mount-via-finder-2.png)

3. **Użyj nazwy udziału i klucza konta magazynu w wyświetlonym monicie o podanie nazwy użytkownika i hasła**: Po kliknięciu przycisku „Połącz” w oknie dialogowym „Łączenie z serwerem” zostanie wyświetlony monit o podanie nazwy użytkownika i hasła (pole zostanie automatycznie wypełnione nazwą użytkownika systemu macOS). Istnieje możliwość umieszczenia nazwy udziału / klucza konta magazynu w pęku kluczy systemu macOS.

4. **Użyj udziału plików platformy Azure zgodnie z potrzebami**: Udział zostanie zainstalowany po podstawieniu nazwy udziału oraz klucza konta magazynu w polach nazwy użytkownika i hasła. Udziału można używać tak samo jak lokalnego folderu / udziału plików. Obsługiwane jest też przeciąganie i upuszczanie plików do udziału plików:

    ![Migawka przedstawiająca zainstalowany udział plików platformy Azure](./media/storage-file-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a>Instalowanie udziału plików platformy Azure za pomocą Terminala
1. Zastąp wartość `<storage-account-name>` nazwą konta magazynu. Jeśli zostanie wyświetlony monit, podaj klucz konta magazynu jako hasło. 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. **Użyj udziału plików platformy Azure zgodnie z potrzebami**: Udział plików platformy Azure zostanie zainstalowany w punkcie instalacji określonym w poprzednim poleceniu.  

    ![Migawka przedstawiająca zainstalowany udział plików platformy Azure](./media/storage-file-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a>Następne kroki
Poniższe linki umożliwiają uzyskanie dodatkowych informacji na temat usługi Magazyn plików Azure.

* [Artykuł pomocy technicznej firmy Apple — Łączenie z funkcją Udostępnianie plików na komputerze Mac](https://support.apple.com/HT204445)
* [Często zadawane pytania](storage-files-faq.md)
* [Rozwiązywanie problemów](storage-troubleshoot-file-connection-problems.md)
