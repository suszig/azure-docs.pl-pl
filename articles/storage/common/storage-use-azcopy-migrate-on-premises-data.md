---
title: "Migracja danych lokalnych do magazynu Azure z narzędzia AzCopy | Dokumentacja firmy Microsoft"
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
ms.openlocfilehash: f2c0b80248ef706394b3f84df4c3da26fb79026a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-with-azcopy"></a>Migrowanie danych lokalnych do magazynu w chmurze z narzędzia AzCopy

Narzędzie AzCopy to narzędzie wiersza polecenia przeznaczone do kopiowania danych z magazynu obiektów Blob Microsoft Azure, plików i tabeli, przy użyciu prostego polecenia przeznaczone do uzyskania optymalnej wydajności. Możesz skopiować dane między systemem plików i konto magazynu lub kont magazynu.  

Istnieją dwie wersje programu AzCopy, który można pobrać:

* [Narzędzie AzCopy w systemie Linux](storage-use-azcopy.md) wbudowana w .NET Framework Core, który jest przeznaczony dla platformy Linux oferty styl POSIX opcje wiersza polecenia. 
* [Narzędzie AzCopy w systemie Windows](../storage-use-azcopy.md) jest oparty na platformie .NET Framework i oferuje opcje wiersza polecenia Styl systemu Windows. 
 
Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta magazynu 
> * Użyj narzędzia AzCopy, aby przekazać wszystkich danych
> * Modyfikowanie danych do celów testowych
> * Utwórz zaplanowane zadanie zadania lub usługi cron do identyfikowania nowych plików do przekazania

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka: 

* Pobierz najnowszą wersję programu AzCopy na [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) lub [Windows](http://aka.ms/downloadazcopy). 

## <a name="log-in-to-the-azure-portalhttpsportalazurecom"></a>Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Aby móc pobierać obiekty BLOB z regionu pomocniczego do lokalnego magazynu i na odwrót, ustaw **replikacji** do **odczytu access-magazynu geograficznie nadmiarowego**. Wybranie tej opcji spowoduje utworzenie [magazynu geograficznie nadmiarowego](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) konta. 
>
>

## <a name="create-a-container"></a>Tworzenie kontenera

Obiekty blob są zawsze przesyłane do kontenera. Kontenery umożliwia organizowanie grup obiektów blob, takie jak organizowanie plików na komputerze w folderach. 

Wykonaj następujące kroki, aby utworzyć kontener:

1. Wybierz **kont magazynu** button na stronie głównej, a następnie wybierz utworzone konto magazynu.
2. Wybierz **obiekty BLOB** w obszarze **usług**, a następnie wybierz pozycję **kontenera**. 

![Tworzenie kontenera](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Nazwa kontenera musi zaczynać się literą lub cyfrą i może zawierać tylko litery, cyfry i znak łącznika (-). Aby uzyskać dodatkowe informacje o regułach nazewnictwa kontenerów i obiektów blob, zobacz temat [Naming and Referencing Containers, Blobs, and Metadata (Nazewnictwo i odwoływanie się do kontenerów, obiektów blob i metadanych)](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

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

Zastąp  **\<klucza\>**  kluczem konta. W portalu Azure, możesz pobrać klucz konta, wybierając **klucze dostępu** w obszarze Ustawienia na koncie magazynu. Wybierz klucz, a następnie wklej go do polecenia AzCopy. Jeśli docelowy określony kontener nie istnieje, AzCopy tworzy go i przekazuje plik do niego. Zaktualizuj ścieżkę źródłową do katalogu danych i Zastąp **myaccount** w docelowy adres URL do nazwy konta magazynu.

Określ `--recursive` i `/S` opcje, w systemie Linux i Windows, aby przekazać zawartość określonego katalogu do lokalizacji magazynu obiektów Blob. Po uruchomieniu narzędzia AzCopy z jednej z tych opcji, wszystkie podfoldery i pliki są także wysyłane.

## <a name="upload-modified-files-to-blob-storage"></a>Przekaż zmodyfikowane pliki do magazynu obiektów Blob
Można użyć narzędzia AzCopy do [przekazać pliki](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) na podstawie ich czasu ostatniej modyfikacji. Skorzystaj z tej, zmodyfikować lub utworzyć nowe pliki w katalogu źródłowym dla celów testowania. Aby przekazać pliki tylko zaktualizowane lub nowsze, należy dodać `--exclude-older`lub `/XO` parametru do systemu Windows i AzCopy Linux odpowiednio poleceń.

Jeśli chcesz skopiować źródła zasobów, które nie istnieją w miejscu docelowym, należy określić zarówno `--exclude-older` i `--exclude-newer` lub `/XO` i `/XN` parametrów w AzCopy systemu Linux i Windows odpowiednio poleceń. Narzędzie AzCopy przekazuje tylko zaktualizowanych danych oparte na ich sygnatury czasowej.
 
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
Możesz utworzyć zaplanowane zadanie/cron zadanie, które uruchamia skrypt polecenia AzCopy, który identyfikuje i przekazuje nowych danych lokalnych do magazynu w chmurze w określonym interwale. 

Skopiuj polecenia AzCopy do edytora tekstu. Zaktualizuj wartości parametrów polecenia programu AzCopy odpowiednie wartości. Zapisz plik jako `script.sh` lub `script.bat` dla narzędzia AzCopy w systemie Linux i Windows odpowiednio. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

Narzędzie AzCopy jest uruchamiana z pełne `--verbose` (Linux) i `/V` opcji (system Windows), a dane wyjściowe zostanie przekierowany do pliku dziennika. 

W tym samouczku [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) służy do tworzenia zaplanowanych zadań w systemie Windows, a [Crontab](http://crontab.org/) polecenie służy do tworzenia zadania cron w systemie Linux. 
 **Schtasks** umożliwia administratorowi utworzyć, usunąć zapytania, zmienić, uruchamianie i zatrzymywanie zaplanowanych zadań na komputerze lokalnym lub zdalnym. **Cron** umożliwia użytkownikom systemów Linux i Unix do uruchamiania poleceń lub skryptów na określony dzień i godzinę, przy użyciu [cron wyrażenia](https://en.wikipedia.org/wiki/Cron#CRON_expression)


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
**Aby utworzyć zadanie cron w systemie Linux**, wprowadź następujące polecenie na terminalu. 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Określanie wyrażenie cron `*/5 * * * * ` w poleceniu wskazuje skrypt powłoki `script.sh` powinien zostać wykonany co pięć minut. Skrypt może zostać zaplanowane do uruchomienia w określonym czasie codziennie, miesięcznego lub rocznego. Zobacz [wyrażenia cron](https://en.wikipedia.org/wiki/Cron#CRON_expression) Aby dowiedzieć się więcej na temat ustawiania datę i godzinę wykonywania zadania. 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
**Aby utworzyć zaplanowane zadanie w systemie Windows**, wprowadź następujące polecenie w wierszu polecenia lub środowiska PowerShell.

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Używa polecenia `/SC` parametr, aby określić harmonogram minute i `/MO` parametr, aby określić interwał pięć minut. `/TN` Parametr jest używany do określenia nazwy zadania i `/TR` parametr, aby określić ścieżkę do `script.bat` pliku. Odwiedź stronę [schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes) Aby dowiedzieć się więcej o tworzeniu zaplanowanego zadania w systemie Windows.

---
 
Można sprawdzić poprawności zaplanowane zadanie/cron zadania wykonuje się poprawnie, tworzyć nowe pliki w katalogu `myfolder`. Poczekaj przez pięć minut, aby potwierdzić, że nowe pliki zostały przekazane do konta magazynu. Przejdź do katalogu dziennika, aby wyświetlić dzienniki danych wyjściowych z zadania zaplanowanego zadania/cron. 

Odwiedź stronę [przeniesienia na lokalne dane](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) Dowiedz się więcej o sposobach przenoszenia danych lokalnych do magazynu Azure i na odwrót.  

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać więcej informacji na temat usługi Azure Storage i AzCopy zobacz następujące zasoby:

### <a name="azure-storage-documentation"></a>Dokumentacja magazynu Azure:
* [Wprowadzenie do usługi Azure Storage](../storage-introduction.md)
* [Transfer danych za pomocą narzędzia AzCopy w systemie Windows](storage-use-azcopy.md) 
* [Transfer danych za pomocą narzędzia AzCopy w systemie Linux](storage-use-azcopy-linux.md) 
* [Tworzenie konta magazynu](../storage-create-storage-account.md)
* [Zarządzanie obiektów blob z Eksploratora usługi Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

