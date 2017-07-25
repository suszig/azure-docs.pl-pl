---
title: "Jak zarządzać usługą Azure File Storage w witrynie Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak zarządzać usługą Azure File Storage za pomocą witryny Azure Portal."
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
ms.openlocfilehash: d8ffb4359b0efe8da2f3bccb81c987bdeedf1a39
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---

# <a name="how-to-use-azure-file-storage-from-the-azure-portal"></a>Jak korzystać z usługi Azure File Storage w witrynie Azure Portal
Witryna [Azure Portal](https://portal.azure.com) udostępnia interfejs użytkownika umożliwiający zarządzanie usługą Azure File Storage. Za pomocą przeglądarki internetowej można wykonywać następujące czynności:

* Tworzenie udziału plików
* Przekazuj i pobieraj pliki do/z udziału plików.
* Monitoruj rzeczywiste użycie każdego udziału plików.
* Dostosowuj limit przydziału rozmiaru udziału plików.
* Kopiuj polecenia instalacji na potrzeby instalacji udziału plików z klienta systemu Windows lub Linux.

## <a name="create-file-share"></a>Tworzenie udziału plików
1. Zaloguj się do Portalu Azure.
2. W menu nawigacyjnym kliknij pozycję **Konta usługi Storage** lub **Konta usługi Storage (klasyczne)**.
    
    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Wybierz konto magazynu.

    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. Wybierz usługę „Pliki”.

    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. Kliknij pozycję „Udziały plików”, a następnie kliknij link w celu utworzenia pierwszego udziału plików.

    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. Wypełnij pola nazwy i rozmiaru udziału plików (do 5120 GB) w celu utworzenia pierwszego udziału plików. Po utworzeniu udziału plików możesz go zainstalować z dowolnego systemu plików, który obsługuje protokół SMB 2.1 lub SMB 3.0. Aby zmienić rozmiar pliku aż do 5120 GB, można kliknąć pozycję **Limit przydziału** dla udziału plików. W witrynie [Kalkulator cen platformy Azure](https://azure.microsoft.com/pricing/calculator/) można oszacować koszt magazynu związany z korzystaniem z usługi Azure File Storage.

    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Przekazywanie i pobieranie plików
1. Wybierz utworzony udział plików.

    ![Zrzut ekranu pokazujący sposób przekazywania i pobierania plików z portalu](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Kliknij przycisk **Przekaż**, aby otworzyć interfejs użytkownika funkcji przekazywania plików.

    ![Zrzut ekranu pokazujący sposób przekazywania plików z portalu](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-to-file-share"></a>Łączenie z udziałem plików
-  Kliknij przycisk **Połącz**, aby wyświetlić wiersz polecenia umożliwiający zainstalowanie udziału plików z systemu Windows lub Linux. Użytkownicy systemu Linux mogą także zapoznać się z artykułem [Jak używać usługi Azure File Storage z systemem Linux](storage-how-to-use-files-linux.md), aby uzyskać więcej instrukcji instalowania dla innych dystrybucji systemu Linux.

    ![Zrzut ekranu pokazujący sposób instalacji udziału plików](media/storage-file-how-to-use-files-portal/use-files-portal-connect.png)
-  Polecenia instalowania udziału plików w systemie Windows lub Linux można skopiować i uruchomić na maszynie wirtualnej platformy Azure lub komputerze lokalnym.

    ![Zrzut ekranu pokazujący polecenia instalacji dla systemu Windows i Linux](media/storage-file-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**Porada:**  
Aby znaleźć klucz dostępu konta magazynu na potrzeby instalowania, kliknij pozycję **Wyświetl klucze dostępu dla tego konta magazynu** w dolnej części strony połączenia.

## <a name="see-also"></a>Zobacz też
Poniższe linki umożliwiają uzyskanie dodatkowych informacji na temat usługi Magazyn plików Azure.

* [Często zadawane pytania](storage-files-faq.md)
* [Rozwiązywanie problemów](storage-troubleshoot-file-connection-problems.md)
