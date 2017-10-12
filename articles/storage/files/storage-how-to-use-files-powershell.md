---
title: "Jak zarządzać usługą Azure Files za pomocą programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak zarządzać usługą Azure Files za pomocą programu PowerShell."
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
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: f919e1880f709b416867a29de14f1dcc63a165fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-powershell-to-manage-azure-files"></a>Jak zarządzać usługą Azure Files za pomocą programu PowerShell
Przy użyciu programu Azure PowerShell można tworzyć udziały plików i zarządzać nimi.

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a>Instalowanie poleceń cmdlet programu PowerShell dla usługi Azure Storage
Aby przygotować się do użycia programu Azure PowerShell, pobierz i zainstaluj polecenia cmdlet tego programu. Aby uzyskać informacje o punkcie instalacji oraz instrukcje dotyczące instalacji, zobacz [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Jak zainstalować i skonfigurować program Azure PowerShell).

> [!NOTE]
> Zalecamy pobranie i zainstalowanie najnowszej wersji modułu Azure PowerShell (lub uaktualnienie do tej wersji).
> 
> 

Otwórz okno programu Azure PowerShell, klikając przycisk **Start** i wpisując polecenie **Windows PowerShell**. W oknie programu PowerShell zostanie załadowany moduł Azure PowerShell.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Tworzenie kontekstu konta magazynu i klucza
Utwórz kontekst konta magazynu. W kontekście zawarta jest nazwa konta magazynu oraz klucz konta. Aby uzyskać instrukcje dotyczące kopiowania klucza konta z witryny [Azure Portal](https://portal.azure.com), zobacz [Wyświetlanie i kopiowanie kluczy dostępu do magazynu](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

W poniższym przykładzie zastąp zmienne `storage-account-name` i `storage-account-key` nazwą konta magazynu i kluczem.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Tworzenie nowego udziału plików
Utwórz nowy udział o nazwie `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Masz już udział plików w usłudze Magazyn plików. Teraz dodamy katalog i plik.

> [!IMPORTANT]
> Nazwa udziału plików musi się składać z samych małych liter. Szczegółowe informacje o nazwach plików i udziałów plików można znaleźć w temacie [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Nazywanie i odwoływanie się do udziałów, katalogów, plików i metadanych).
> 
> 

## <a name="create-a-directory-in-the-file-share"></a>Tworzenie katalogu w udziale plików
Utwórz katalog w udziale. W poniższym przykładzie katalog nosi nazwę `CustomLogs`.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a>Przekazywanie pliku lokalnego do katalogu
Teraz przekażemy plik lokalny do katalogu. W poniższym przykładzie plik zostanie przekazany z lokalizacji `C:\temp\Log1.txt`. Zmień ścieżkę pliku tak, aby wskazywała prawidłowy plik na komputerze lokalnym.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a>Wyświetlanie listy plików w katalogu
Aby zobaczyć plik w katalogu, możesz wyświetlić listę wszystkich plików w tym katalogu. To polecenie zwraca pliki i podkatalogi (jeśli istnieją) w katalogu CustomLogs.

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Polecenie Get-AzureStorageFile zwraca listę plików i katalogów dla dowolnego przekazanego obiektu katalogu. Polecenie „Get-AzureStorageFile -Share $s” zwraca listę plików i katalogów w katalogu głównym. Aby uzyskać listę plików w podkatalogu, trzeba przekazać nazwę podkatalogu do polecenia Get-AzureStorageFile. Tak właśnie działa powyższy zapis — pierwsza część polecenia do kreski pionowej zwraca wystąpienie katalogu dla podkatalogu CustomLogs. Następnie jest ono przekazywane do polecenia Get-AzureStorageFile, które zwraca pliki i katalogi w podkatalogu CustomLogs.

## <a name="copy-files"></a>Kopiowanie plików
Począwszy od wersji 0.9.7 programu Azure PowerShell, można kopiować pliki do innych plików, pliki do obiektów blob oraz obiekty blob do plików. Poniżej przedstawiamy sposób wykonywania tych operacji kopiowania za pomocą poleceń cmdlet programu PowerShell.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Następne kroki
Poniższe linki umożliwiają uzyskanie dodatkowych informacji na temat usługi Azure Files.

* [Często zadawane pytania](../storage-files-faq.md)
* [Rozwiązywanie problemów w systemie Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Rozwiązywanie problemów w systemie Linux](storage-troubleshoot-linux-file-connection-problems.md)    