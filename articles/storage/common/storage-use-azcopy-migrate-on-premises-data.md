---
title: "Migracji danych lokalnych do usługi Azure Storage za pomocą narzędzia AzCopy | Dokumentacja firmy Microsoft"
description: "Użyj narzędzia AzCopy migracji danych ani skopiować dane do lub z obiektu blob, tabel i zawartości pliku. Łatwo przeprowadzić migrację danych z lokalnego magazynu do magazynu Azure."
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: azcopy
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: v-ruogun
ms.openlocfilehash: 3dbfb935ac0b134e127a5dccb7bc76716c688e8a
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Migrowanie danych lokalnych do magazynu w chmurze przy użyciu narzędzia AzCopy

Narzędzie AzCopy to narzędzie wiersza polecenia kopiowania danych do lub z magazynu obiektów Blob platformy Azure, Azure, plików i magazynu tabel Azure przy użyciu prostego polecenia. Polecenia zostały zaprojektowane dla uzyskania optymalnej wydajności. Możesz skopiować dane między systemem plików i konto magazynu lub kont magazynu.  

Możesz pobrać dwie wersje programu AzCopy:

* [Narzędzie AzCopy w systemie Linux](storage-use-azcopy.md) jest oparty na platformie .NET Core Framework. Platformy Linux jest przeznaczony oferując POSIX stylu opcji wiersza polecenia. 
* [Narzędzie AzCopy w systemie Windows](../storage-use-azcopy.md) jest oparty na platformie .NET Framework. Oferuje opcje wiersza polecenia Styl systemu Windows. 
 
Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz konto magazynu. 
> * Użyj narzędzia AzCopy, aby przekazać wszystkich danych.
> * Modyfikowanie danych do celów testowych.
> * Utwórz zaplanowane zadanie zadania lub usługi cron do identyfikowania nowych plików do przekazania.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka, Pobierz najnowszą wersję programu AzCopy na [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) lub [Windows](http://aka.ms/downloadazcopy). 

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Aby móc pobierać obiekty BLOB z regionu pomocniczego do lokalnego magazynu i na odwrót, ustaw **replikacji** do **odczytu access-magazynu geograficznie nadmiarowego**. Wybranie tej opcji spowoduje utworzenie [magazynu geograficznie nadmiarowego](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) konta. 
>
>

## <a name="create-a-container"></a>Tworzenie kontenera

Obiekty blob są zawsze przesyłane do kontenera. Kontenery służy do organizowania grup obiektów blob, takie jak organizowanie plików na komputerze w folderach. 

Wykonaj następujące kroki, aby utworzyć kontener:

1. Wybierz **kont magazynu** przycisk ze strony głównej i wybierz konto magazynu, który został utworzony.
2. Wybierz **obiekty BLOB** w obszarze **usług**, a następnie wybierz **kontenera**. 

   ![Tworzenie kontenera](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Nazwa kontenera musi zaczynać się literą lub cyfrą. Mogą one zawierać tylko litery, cyfry i znaki łącznika (-). Aby uzyskać dodatkowe informacje o regułach nazewnictwa kontenerów i obiektów blob, zobacz temat [Naming and Referencing Containers, Blobs, and Metadata (Nazewnictwo i odwoływanie się do kontenerów, obiektów blob i metadanych)](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="upload-all-files-in-a-folder-to-blob-storage"></a>Przekaż wszystkie pliki w folderze do magazynu obiektów Blob

Można użyć narzędzia AzCopy można przekazać wszystkie pliki w folderze do magazynu obiektów Blob na [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) lub [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Aby przekazać wszystkie obiekty BLOB w folderze, wprowadź następujące polecenie narzędzia AzCopy:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S
---

Zastąp `<key>` i `key` kluczem konta. W portalu Azure, możesz pobrać klucz konta, wybierając **klucze dostępu** w obszarze **ustawienia** na koncie magazynu. Wybierz klucz, a następnie wklej go do polecenia AzCopy. Jeśli docelowy określony kontener nie istnieje, AzCopy tworzy go i przekazuje plik do niego. Zaktualizuj ścieżkę źródłową do katalogu danych i Zastąp **myaccount** w adresie URL docelowego nazwą konta magazynu.

Aby przekazać zawartość określonego katalogu rekursywnie magazynu obiektów Blob, należy określić `--recursive` (Linux) lub `/S` opcji (system Windows). Po uruchomieniu narzędzia AzCopy z jednej z tych opcji, wszystkie podfoldery i pliki są także wysyłane.

## <a name="upload-modified-files-to-blob-storage"></a>Przekaż zmodyfikowane pliki do magazynu obiektów Blob
Można użyć narzędzia AzCopy do [przekazać pliki](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) na podstawie ich czasu ostatniej modyfikacji. Skorzystaj z tej, zmodyfikować lub utworzyć nowe pliki w katalogu źródłowym do celów testowych. Aby przekazać pliki tylko zaktualizowane lub nowsze, należy dodać `--exclude-older` (Linux) lub `/XO` do polecenia AzCopy parametr (system Windows).

Jeśli chcesz skopiować źródła zasobów, które nie istnieją w miejscu docelowym, należy określić zarówno `--exclude-older` i `--exclude-newer` (Linux) lub `/XO` i `/XN` parametrów (system Windows) w poleceniu AzCopy. Narzędzie AzCopy przekazuje tylko zaktualizowanych danych na podstawie sygnatury czasowej.
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S /XO
---

## <a name="create-a-scheduled-task-or-cron-job"></a>Utwórz zadanie zaplanowane zadania lub usługi cron 
Można utworzyć zaplanowane zadanie lub cron zadanie, które uruchamia skrypt polecenia AzCopy. Skrypt identyfikuje i przekazuje nowych danych lokalnych do magazynu w chmurze w określonym interwale. 

Skopiuj polecenia AzCopy do edytora tekstu. Zaktualizuj wartości parametrów polecenia AzCopy odpowiednie wartości. Zapisz plik jako `script.sh` (Linux) lub `script.bat` (system Windows) dla narzędzia AzCopy. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

Narzędzie AzCopy jest uruchamiana z pełne `--verbose` (Linux) lub `/V` opcji (system Windows). Dane wyjściowe zostanie przekierowany do pliku dziennika. 

W tym samouczku [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) służy do tworzenia zaplanowanych zadań w systemie Windows. [Crontab](http://crontab.org/) polecenie służy do tworzenia zadania cron w systemie Linux. 
 **Schtasks** umożliwia administratorowi utworzyć, usunąć zapytania, zmienić, uruchamianie i zatrzymywanie zaplanowanych zadań na komputerze lokalnym lub zdalnym. **Cron** umożliwia użytkownikom systemów Linux i Unix do uruchamiania poleceń lub skryptów w określonym dniu i godzinie za pomocą [wyrażenia cron](https://en.wikipedia.org/wiki/Cron#CRON_expression).


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Aby utworzyć zadanie cron w systemie Linux, wprowadź następujące polecenie na terminalu: 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Określanie wyrażenie cron `*/5 * * * * ` w poleceniu wskazuje, że skrypt powłoki `script.sh` powinno być ono uruchomione co pięć minut. Można zaplanować uruchomienie skryptu w określonym czasie codziennie, co miesiąc lub co rok. Aby dowiedzieć się więcej na temat ustawiania datę i godzinę do wykonywania zadań, zobacz [wyrażenia cron](https://en.wikipedia.org/wiki/Cron#CRON_expression). 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Aby utworzyć zaplanowane zadanie w systemie Windows, wpisz następujące polecenie w wierszu polecenia lub w programie PowerShell:

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Używa polecenia:
- `/SC` Parametr, aby określić harmonogram minuty.
- `/MO` Parametr, aby określić interwał pięć minut.
- `/TN` Parametr, aby określić nazwę zadania.
- `/TR` Parametr, aby określić ścieżkę do `script.bat` pliku. 

Aby dowiedzieć się więcej o tworzeniu zaplanowanego zadania w systemie Windows, zobacz [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---
 
Aby sprawdzić, czy zadanie zaplanowane zadanie/cron działa poprawnie, należy utworzyć nowe pliki w Twojej `myfolder` katalogu. Poczekaj pięć minut, aby upewnić się, że nowe pliki zostały przekazane do konta magazynu. Przejdź do katalogu dziennika, aby wyświetlić dane wyjściowe dzienników zaplanowanego zadania lub zadania cron. 

Aby dowiedzieć się więcej o sposobach przenoszenia danych lokalnych do magazynu Azure i na odwrót, zobacz [przenoszenie danych do i z usługi Magazyn Azure](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać więcej informacji na temat usługi Azure Storage i AzCopy zobacz następujące zasoby:

* [Wprowadzenie do usługi Azure Storage](../storage-introduction.md)
* [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) 
* [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md) 
* [Tworzenie konta magazynu](../storage-create-storage-account.md)
* [Zarządzanie obiektów blob z Eksploratora usługi Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

