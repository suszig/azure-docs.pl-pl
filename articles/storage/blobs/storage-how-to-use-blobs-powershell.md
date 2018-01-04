---
title: "Wykonywanie operacji w magazynie obiektów Blob platformy Azure (obiekt magazynu) przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Samouczek — wykonywanie operacji w magazynie obiektów Blob platformy Azure (obiekt magazynu) przy użyciu programu PowerShell"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: robinsh
ms.openlocfilehash: 170c3091efc90f640792682377ed10e2eab0cab3
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="perform-azure-blob-storage-operations-with-azure-powershell"></a>Wykonywanie operacji magazynu obiektów Blob platformy Azure przy użyciu programu Azure PowerShell

Azure Blob Storage to usługa do przechowywania dużych ilości danych obiektów niestrukturalnych, takich jak dane tekstowe lub binarne, do których można uzyskać dostęp z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. W tym artykule omówiono podstawowe operacje w magazynie obiektów Blob platformy Azure, takich jak przekazywanie, pobieranie i usuwanie obiektów blob. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie kontenera 
> * Przekaż obiekty BLOB
> * Wyświetlanie listy obiektów blob w kontenerze 
> * Pobieranie obiektów blob
> * Kopiowanie obiektów blob
> * Usuwanie obiektów blob
> * Wyświetlanie i ustawianie właściwości i metadane obiektu blob
> * Zarządzanie zabezpieczeniami przy użyciu sygnatury dostępu współdzielonego

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie `Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Tworzenie kontenera

Obiekty blob są zawsze przesyłane do kontenera. Kontenery są podobne do katalogów znajdujących się na komputerze, umożliwiając organizować grupy obiektów blob w kontenerach, takich jak organizowanie plików w folderach na komputerze. Konto magazynu może zawierać dowolną liczbę kontenerów; jest ograniczona tylko według ilości miejsca zajmowanego na koncie magazynu (do 500TB). 

Po utworzeniu kontenera można ustawić poziomu dostępu, co pomaga zdefiniować, kto ma dostęp do obiektów blob w tym kontenerze. Na przykład może być prywatne (poziom dostępu = `Off`), co oznacza, nikt nie będzie można uzyskiwać do nich dostęp bez sygnatury dostępu współdzielonego i klucze dostępu dla konta magazynu. Jeśli nie zostanie określony poziom dostępu, podczas tworzenia kontenera, domyślnie prywatne.

Możesz obrazów w kontenerze użytkownika był dostępny publicznie. Na przykład jeśli mają być przechowywane obrazy do wyświetlenia w witrynie sieci Web, mają być publiczne. W takim przypadku należy ustawić dostępu do kontenera poziom `blob`, a każdy z adresem URL wskazującym obiektu blob w tym kontenerze mogą uzyskiwać dostęp do obiektu blob.

Aby utworzyć kontener, ustaw nazwę kontenera, a następnie utworzyć kontener, ustawianie uprawnień do obiektów "blob". Nazwy kontenerów muszą zaczynać się literą lub cyfrą i może zawierać tylko litery, cyfry i znaki łącznika (-). Więcej reguł o nazwach obiektów blob i kontenerów, zobacz [nazewnictwa i odwołuje się do kontenerów, obiektów blob i metadanych](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Utwórz nowy kontener z [AzureStorageContainer nowy](/powershell/module/azure.storage/new-azurestoragecontainer). Ustaw poziom dostępu publicznego. Nazwa kontenera, w tym przykładzie jest *howtoblobs*.

```powershell
$containerName = "howtoblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-into-a-container"></a>Przekaż obiekty BLOB do kontenera

Azure Blob Storage obsługuje blokowe obiekty BLOB, uzupełnialne obiekty BLOB i stronicowe obiekty BLOB.  Pliki VHD używane do tworzenia kopii maszyn wirtualnych IaaS są stronicowymi obiektami blob. Uzupełnialne obiekty blob są używane do rejestrowania, na przykład w sytuacji, w której konieczny jest zapis do pliku, a następnie dodawanie kolejnych informacji. Większość plików przechowywanych w usłudze Blob Storage to blokowe obiekty blob. 

Aby przekazać plik do blokowego obiektu blob, pobierz odwołanie do kontenera i uzyskaj odwołanie do blokowego obiektu blob w tym kontenerze. Po uzyskaniu odwołania do obiektu blob możesz przekazać do niego dane przy użyciu polecenia [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent). Ta operacja tworzy obiektu blob, jeśli nie istnieje lub zastąpiony, jeśli już istnieje.

Poniżej pokazano, jak przekazać obiekt blob do kontenera. Najpierw należy ustawić zmienne, które wskazują do katalogu na komputerze lokalnym, gdzie znajdują się pliki i ustawienia zmiennej dla nazwy pliku do przekazania. Jest to przydatne, gdy chcesz wielokrotnie wykonać operację. Przekaż kilka plików, więc może wyświetlać wiele wpisów, gdy lista obiektów blob w kontenerze.

Poniższe przykłady obciążenia rysunek001.jpg i Image002.png od D:\\_TestImages na dysku lokalnym w kontenerze, który został utworzony.

```powershell
$localFileDirectory = "D:\_TestImages\"

$blobName = "Image001.jpg"
$localFile = $localFileDirectory + $blobName
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx 
```

Przekaż inny plik. 

```powershell
$blobName = "Image002.png"
$localFile = $localFileDirectory + $blobName 
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx
```

Przed kontynuowaniem można przesłać dowolną liczbę plików.

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Pobierz listę obiektów blob w kontenerze, za pomocą [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) i wybierając nazwa obiektu blob, który będzie wyświetlany.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Pobieranie obiektów blob

Pobierz obiekty blob na swój dysk twardy. Najpierw należy ustawić zmiennej, która wskazuje na folder lokalny, do którego chcesz pobrać obiekty BLOB. Następnie dla każdego obiektu blob do pobrania, ustaw nazwę i wywołanie [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) można pobrać obiektu blob.

Ten przykładowy kod kopiuje obiekty BLOB do D:\\_TestImages\Downloads na dysku lokalnym. 

```powershell
# local directory to which to download the files
# example "D:\_TestImages\Downloads\"
$localTargetDirectory = "D:\_TestImages\Downloads\"

# download the first blob
$blobName = "Image001.jpg"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 

# download another blob
$blobName = "Image002.png"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 
```

## <a name="copy-blobs"></a>Kopiowanie obiektów blob

Istnieje kilka kwestii, które należy wziąć pod uwagę podczas kopiowania obiektów blob. Może być tylko kopiowane obiektu blob na nową nazwę w tej samej lokalizacji. Obiekt blob można kopiować do oddzielnego konta magazynu. Dużych obiektów blob (na przykład plik wirtualnego dysku twardego) można kopiować do innego konta magazynu. Te każdego wykona inaczej.

### <a name="simple-blob-copy"></a>Kopiuj prostego obiektu blob

Można utworzyć kopię jeden z obiektów blob w kontenerze, kopiując je do nowego obiektu blob. W tym przykładzie zostanie skopiuj go do tego samego kontenera o innej nazwie, ale może równie łatwo utworzyć kontener w innym i skopiowanie go zamiast tego. Ten przykład przedstawia sposób użycia [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) można skopiować obiektu blob. Należy określić zarówno źródło i nazwa docelowego obiektu blob i nazwa kontenera.

```powershell
$blobName = "Image002.png"
$newBlobName = "CopyOf_" + $blobName 

Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName `
  -DestBlob $newBlobName `
  -Context $ctx 

# get list of blobs to verify the new one has been created
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

### <a name="copy-a-blob-to-a-different-storage-account"></a>Kopiowanie obiektu blob do innego konta magazynu 

Można skopiować obiektu blob do oddzielnego konta magazynu. Jest jednym z przykładów w ten sposób można skopiować pliku wirtualnego dysku twardego, aby utworzyć kopię zapasową jednej z maszyn wirtualnych do innego konta magazynu w celu jego kopię zapasową. 

Konfigurowanie drugiego konta magazynu, pobrania kontekstu skonfigurować kontener na tym koncie magazynu i wykonać kopiowanie. Ta część skrypt jest niemal identyczny skryptu powyżej, z wyjątkiem za pomocą drugiego konta magazynu, zamiast pierwszy.

```powershell
#create new storage account, get context 
$storageAccount2Name = "blobstutorialtestcopy"
$storageAccount2 = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccount2Name `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx2 = $storageAccount2.Context

#create a container in the second storage account 
$containerName2 = "tutorialblobscopied"
New-AzureStorageContainer -Name $containerName2 `
  -Context $ctx2 `
  -Permission blob

# copy one of the blobs from the first storage account to the second one 
# specify the source and destination container, blob name, and context
Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -SrcContext $ctx `
  -DestContext $ctx2 

# list the blobs in the container in the second storage account
# to verify that the newly copied blob is there
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

### <a name="copying-very-large-blobs-asynchronously"></a>Kopiowanie asynchronicznie bardzo dużych obiektów blob

Jeśli kopiujesz bardzo dużych obiektów blob, który ma wiele GB, będzie można korzystać asynchroniczne kopii przez uruchomienie, a następnie powracające i sprawdzanie stanu, dopóki zostanie zakończone. Dzięki temu można nie jest powiązana podczas wykonywania operacji kopiowania.

Jeśli kopiujesz w ramach konta magazynu, kopia jest bardzo szybko. Jeśli kopiujesz dwóch kont magazynu w tym samym regionie, kopia jest dość szybkie. Jednak jeśli źródłowe i docelowe znajdują się w oddzielnych regionach, może zająć kilka godzin na zakończenie kopiowania, w zależności od odległość i rozmiar pliku. 

Ten skrypt używa tego samego zmienne zdefiniowane w ostatnim przykładzie kopiowania. Różnica polega na tym że tego przechwytuje stan kopii i zapytanie go wielokrotnie co 10 sekund, dopóki zostanie zakończone. Być może trzeba przekazać bardzo dużych obiektów blob na koncie magazynu, aby wyświetlić to zająć więcej niż jeden iteracji, aby zakończyć.

Użyj [Get AzureStorageBlobCopyState](/powershell/module/azure.storage/get-azurestorageblobcopystate) aby zbadać stan kopiowania. 

```powershell
# start the blob copy, and assign the result to $blobResult
$blobResult = Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -Context $ctx `
  -DestContext $ctx2

# get the status of the blob copy
$status = $blobResult | Get-AzureStorageBlobCopyState 
# show the value of the status
$status 

# loop until it finishes copying, pausing for 10 seconds after each iteration
# it is finished when the status is no longer 'Pending'
while ($status.Status -eq "Pending") {
    $status = $blobResult | Get-AzureStorageBlobCopyState 
    $status
    Start-Sleep 10
}

# get the list of blobs to see the new file in the second storage account 
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

## <a name="delete-blobs"></a>Usuwanie obiektów blob

Aby usunąć konto magazynu obiektów blob, użyj [AzureStorageBlob Usuń](/powershell/module/azure.storage/Remove-AzureStorageBlob). 

```powershell
$blobName = "Image001.jpg"
Remove-AzureStorageBlob -Blob $blobName -Container $containerName -Context $ctx

# get list of blobs to see the one you deleted is gone
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

## <a name="read-and-write-a-blobs-properties-and-metadata"></a>Odczytywanie i zapisywanie właściwości oraz metadane obiektu blob

Aby korzystać z tego zestawu właściwości i metod zwróconego **IListBlobItem**, należy rzutować go (lub przekonwertować go) do **CloudBlockBlob**, **CloudPageBlob**, lub **CloudBlobDirectory** obiektu. Jeśli typ jest nieznany, można zastosować sprawdzanie typu, aby określić, do którego obiektu rzutować obiekt. Poniższy kod przedstawia sposób pobierania i zwracania właściwości obiektów blob przekazać wcześniej przy użyciu [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) i rzutowanie wynik do obiektu CloudBlockBlob.

```powershell
# blob properties
# get a reference to the blob you uploaded -- this is an **IListBlobItem** -- then
# convert it to a CloudBlockBlob, giving you access to the properties 
# and methods of the blob 
$blobName = "Image001.jpg"
$blob = Get-AzureStorageBlob -Context $ctx `
   -Container $containerName `
   -Blob $blobName 

#convert $blob to a CloudBlockBlob object
$cloudBlockBlob = [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] $blob.ICloudBlob

# you can view the properties by typing in $cloudBlockBlob 
#   and hitting the period key; this brings up IntelliSense,
#   which shows you all of the available properties
Write-Host "blob type = " $cloudBlockBlob.BlobType
Write-Host "blob name = " $cloudBlockBlob.Name
Write-Host "blob uri = " $cloudBLockBlob.Uri
```

Wypełnienie właściwości systemu przez wywołanie metody FetchAttributes, a następnie wyświetlić właściwości. Niektóre właściwości są zapisywalne i ich wartości można zmienić. W tym przykładzie pokazano, jak można zmienić typu zawartości, znanej także jako typ MIME.

```powershell
# populate the system properties
$cloudBlockBlob.FetchAttributes()

# view some of the system properties
# contentType is commonly referred to as MIME type
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
Write-Host "size = " $cloudBlockBlob.Properties.Length 

# change the content type to image/png
$contentType = "image/jpg"
$cloudBlockBlob.Properties.ContentType = $contentType 
$cloudBlockBlob.SetProperties() 

# get the system properties again to show that the content type has changed
$cloudBlockBlob.FetchAttributes()
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
```

Każdy obiekt blob ma własną metadanych, które można ustawić. Na przykład mogą być przechowywane nazwy użytkownika, który przekazać obiekt blob lub daty/godziny, który najpierw został przekazany. Metadane składa się z pary klucz wartość. To ustawienie można zmodyfikować za pomocą programu PowerShell w następujący sposób.

```powershell
# "filename" is the key, "original file name" is the value
$cloudBlockBlob.Metadata["filename"] = "original file name"

# "owner" is the key, "RobinS" is the value
$cloudBlockBlob.Metadata["owner"] = "RobinS"

# save the metadata and then display it
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata

# clear the metadata, save it, and show it
$cloudBlockBlob.Metadata.Clear()
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata
```

## <a name="managing-security-for-blobs"></a>Zarządzanie zabezpieczeniami obiektów blob 

Domyślnie usługi Azure Storage chroni dane przez ograniczenie dostępu do właściciela konta, który znajduje się w posiadaniu kluczy dostępu do konta magazynu. W przypadku potrzeby udostępnienia danych obiektów blob na koncie magazynu ważne jest, aby zrobić to bez uszczerbku dla bezpieczeństwa kluczy dostępu do konta. Aby to zrobić, można udostępnionego dostępu podpisu adresu URL, czyli adres URL do zawartości, który zawiera parametry zapytania i token zabezpieczający, który umożliwia określony poziom uprawnień dla określonego przedziału czasu. Można na przykład daje dostęp do odczytu obiektu blob prywatnej 5 minut, ktoś mógł być wyświetlany. 

### <a name="set-the-access-level-of-the-container-and-its-blobs-to-private"></a>Ustaw poziom dostępu do kontenera i jego obiektów blob na prywatne

Najpierw należy ustawić poziom dostępu do kontenera do `Off`, który określa Brak dostępu bez sygnatury dostępu współdzielonego i klucza konta. Użyj [AzureStorageContainerAcl zestawu](/powershell/module/azure.storage/Set-AzureStorageContainerAcl).

```powershell
Set-AzureStorageContainerAcl -Name $containerName -Context $ctx -Permission Off 
```

### <a name="test-private-access"></a>Testowanie dostępu prywatnego

Aby zweryfikować, że nie masz dostępu do obiektów blob w tym kontenerze, konstruowania adresu URL do jednego z obiektów blob bez sygnatury dostępu współdzielonego i wyświetlenia obiektu blob. Przy użyciu protokołu HTTPS, adres URL będzie mieć następujący format:

    `https://storageaccountname.blob.core.windows.net/containername/blobname`

Ten przedstawia sposób tworzenia adresu URL obiektu blob.

```powershell
$blobUrl = "https://" `
  $storageAccountName ".blob.core.windows.net/" + `
  $containerName +  "/" $blobName

Write-Host "Blob URL = " $blobUrl 
```

Skopiuj adres URL obiektu blob i wklej go w oknie przeglądarki prywatnych — wyświetli błąd autoryzacji, ponieważ obiekt blob jest prywatny i nie zostały uwzględnione sygnatury dostępu współdzielonego. 

### <a name="create-the-sas-uri"></a>Utwórz identyfikator URI sygnatury dostępu Współdzielonego

Tworzenie identyfikatora URI połączenia SAS — jest to łącze do obiektu blob, takie jak parametry zapytania i tokenu zabezpieczającego, które tworzą sygnatury dostępu Współdzielonego. Wklej ten identyfikator URI do prywatnego okna przeglądania — przedstawiono w nim obraz. 

Najpierw utwórz Data/Godzina rozpoczęcia i Data/godzina wygaśnięcia dostępu. Ta metoda korzysta okna 2-minutowy. 

```powershell
# set the start time to the current date/time 
# set the end time to 2 minutes in the future
$StartTime = Get-Date
$EndTime = $StartTime.AddMinutes(2.0)
```

Utwórz identyfikator URI sygnatury dostępu Współdzielonego przy użyciu [AzureStorageBlobSASToken nowy](/powershell/module/azure.storage/new-azurestorageblobsastoken). Należy nazwę kontenera, nazwa obiektu blob, kontekst konta magazynu, uprawnienia, aby otrzymać (w tym przypadku do odczytu, zapisu i usuwania) oraz godzinę rozpoczęcia i zakończenia dla dostępu. 

```powershell
$SASURI = New-AzureStorageBlobSASToken -Container $containerName `
    -Blob $blobName `
    -Context $ctx `
    -Permission "rwd" `
    -StartTime $StartTime `
    -ExpiryTime $EndTime `
    -FullUri

Write-Host "URL with SAS = " $SASURI
```

Skopiuj identyfikator URI sygnatury dostępu Współdzielonego i umieszcza je w oknie przeglądarki prywatny; jaki będzie wyświetlana obrazu.

Poczekaj, aż wystarczająco długi adres URL, który wygaśnie (2 minuty w tym przykładzie), a następnie wklej adres URL do innego okna przeglądarki w trybie prywatnym. Ten czas, wystąpi błąd autoryzacji i nie będzie zawierać obrazu, ponieważ identyfikator URI sygnatury dostępu Współdzielonego utracił ważność.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Usuń wszystkie zasoby, które zostały utworzone. Aby to zrobić, można usunąć grupy zasobów, która spowoduje również usunięcie wszystkich zasobów zawartych w grupie. W takim przypadku spowoduje usunięcie wszystkich kont magazynu i samej grupy zasobów.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku przedstawiono informacje podstawowe zarządzanie magazynem obiektów Blob, np.:

> [!div class="checklist"]
> * Tworzenie kontenera 
> * Przekaż obiekty BLOB
> * Wyświetlanie listy obiektów blob w kontenerze 
> * Pobieranie obiektów blob
> * Kopiowanie obiektów blob
> * Usuwanie obiektów blob
> * Odczytywanie i zapisywanie właściwości i metadane obiektu blob
> * Zarządzanie zabezpieczeniami przy użyciu sygnatury dostępu współdzielonego

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Polecenia cmdlet usługi Magazyn Microsoft Azure PowerShell
* [Polecenia cmdlet programu PowerShell usługi Storage](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) jest bezpłatną aplikacją autonomiczną oferowaną przez firmę Microsoft, która umożliwia wizualną pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux.
