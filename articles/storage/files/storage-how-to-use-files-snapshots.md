---
title: "Praca z migawkami udziału (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Migawka udziału jest tylko do odczytu wersji udziału plików platformy Azure, która jest wykonywana w punkcie w czasie, aby utworzyć kopię zapasową udziału."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: renash
ms.openlocfilehash: eb5c7d0fcb9e1106dbc0aa577ba5ecfa1bf6bee7
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="work-with-share-snapshots-preview"></a>Praca z migawkami udziału (wersja zapoznawcza)
Migawka udziału (wersja zapoznawcza) jest tylko do odczytu wersji udziału plików platformy Azure, która jest wykonywana w punkcie w czasie. Po utworzeniu migawki udziału, to można można odczytać, kopiowane, lub usunięte, ale nie został zmodyfikowany. Migawki udziału umożliwia tworzenie kopii zapasowej do udziału, pojawiającą się w chwili. 

W tym artykule dowiesz się, jak do tworzenia, zarządzania i Usuń migawki udziału. Aby uzyskać więcej informacji, zobacz [udostępnić migawki omówienie](storage-snapshots-files.md) lub [migawki — często zadawane pytania](storage-files-faq.md).

## <a name="create-a-share-snapshot"></a>Utwórz migawkę udziału

Można utworzyć migawki udziału za pomocą portalu Azure, programu PowerShell, interfejsu wiersza polecenia Azure, interfejsu API REST lub dowolnego zestawu SDK usługi Magazyn Azure. W poniższych sekcjach opisano sposób tworzenia migawki udziału za pomocą portalu, interfejsu wiersza polecenia i programu PowerShell. 

Można utworzyć migawkę udziału udziału plików, gdy jest używany. Jednak migawki udziału przechwytywać tylko te dane, który został już zapisany do udziału plików w czasie, który wydano polecenie migawki udziału. To może wyłączyć wszystkie dane buforowane przez wszystkie aplikacje lub system operacyjny.

### <a name="create-a-share-snapshot-by-using-the-portal"></a>Utwórz migawkę udziału za pomocą portalu  
Aby utworzyć migawkę udziału w momencie, przejdź do pliku udziału w portalu, a następnie wybierz **utworzyć migawkę**.

>   ![Menu migawki w portalu](./media/storage-snapshots-create/portal-create-snapshot.png)


### <a name="create-a-share-snapshot-by-using-azure-cli-20"></a>Utwórz migawkę udziału przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0
Można utworzyć migawki udziału za pomocą `az storage share snapshot` polecenia:

```azurecli-interactive
az storage share snapshot -n <share name>
```

Przykładowe dane wyjściowe:
```json
{
  "metadata": {},
  "name": "<share name>",
  "properties": {
    "etag": "\"0x8D50B7F9A8D7F30\"",
    "lastModified": "2017-10-04T23:28:22+00:00",
    "quota": null
  },
  "snapshot": "2017-10-04T23:28:35.0000000Z"
}
```

### <a name="create-a-share-snapshot-by-using-powershell"></a>Utwórz migawkę udziału za pomocą programu PowerShell
Można utworzyć migawki udziału za pomocą `$share.Snapshot()` polecenia:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

```

## <a name="perform-common-share-snapshot-operations"></a>Wykonywanie typowych operacji migawki udziału

Można wyliczyć migawki udziału skojarzone z udziału plików przy użyciu **poprzednie wersje** kartę w systemie Windows za pośrednictwem REST, Biblioteka klienta programu PowerShell i portalu. Po udziału plików jest zainstalowany, można wyświetlić wszystkie poprzednie wersje pliku przy użyciu **poprzednie wersje** kartę w systemie Windows. 

W poniższych sekcjach opisano sposób korzystania z portalu Azure, Windows i Azure CLI 2.0 do listy, przejdź do i przywracania migawki udziału.

### <a name="share-snapshot-operations-in-the-portal"></a>Operacje migawki udziału w portalu

Można przyjrzeć się wszystkich migawek z udziału pliku udziału w portalu i przejdź do migawki udziału, aby wyświetlić jego zawartość.

#### <a name="view-a-share-snapshot"></a>Wyświetl migawkę udziału
W udziale plików w obszarze **migawki**, wybierz pozycję **wyświetlić migawki**.

![Polecenie "Wyświetl migawki" w portalu](./media/storage-snapshots-list-browse/snapshot-view-portal.png)

#### <a name="list-and-browse-to-share-snapshot-content"></a>Lista i Przeglądaj, aby udostępnić zawartość migawki
Wyświetl listę migawki udziału, a następnie przejdź do zawartości jedną migawkę bezpośrednio, wybierając odpowiednią sygnaturę.

![Migawki wybrane na liście sygnatury czasowe](./media/storage-snapshots-list-browse/snapshot-browsefiles-portal.png)

Możesz też wybrać **Connect** przycisk na liście migawki widoku w celu uzyskania `net use` polecenia i ścieżkę katalogu do określonego udziału migawki. Możesz następnie bezpośrednio przejść do tej migawki.


![Uzyskuj okienku command — pole](./media/storage-snapshots-list-browse/snapshot-connect-portal.png)

#### <a name="download-or-restore-from-a-share-snapshot"></a>Pobierz lub przywracanie z migawki udziału
Z portalu, pobieranie, lub przywrócić plik z migawki za pomocą **Pobierz** lub **przywrócić** przycisk odpowiednio.

![Pobierania i przywracania przycisków](./media/storage-snapshots-list-browse/snapshot-download-restore-portal.png)

### <a name="share-snapshot-operations-in-windows"></a>Operacje migawki udziału w systemie Windows
Jeśli została już wykonana migawki udziału udziału plików, można wyświetlić poprzednie wersje udział, określonego pliku lub katalogu z udziału plików zainstalowanych w systemie Windows. Na przykład poniżej przedstawiono sposób korzystania z funkcji poprzednich wersji do wyświetlania i przywrócić poprzednią wersję katalogu w systemie Windows.

> [!Note]  
> Można wykonać operacji na poziomie udziału i na poziomie pliku. Na liście wyświetlane jest tylko wersja zawiera zmiany do tego katalogu lub pliku. Jeśli określonego katalogu lub pliku nie została zmieniona między dwiema migawkami udziału, migawki udziału zostanie wyświetlony na liście poziomie poprzedniej wersji, ale nie w pliku lub katalogu w poprzedniej wersji listy.

#### <a name="mount-a-file-share"></a>Instalowanie udziału plików
Najpierw należy zainstalować udział plików przy użyciu `net use` polecenia.

#### <a name="open-a-mounted-share-in-file-explorer"></a>Otwórz zainstalowany udział w Eksploratorze plików
Przejdź do Eksploratora plików i znajdź zainstalowany udział.

![Zainstalowany udział w Eksploratorze plików](./media/storage-snapshots-list-browse/snapshot-windows-mount.png)

#### <a name="list-previous-versions"></a>Poprzednie wersje listy
Przejdź do elementu lub elementu nadrzędnego, który należy przywrócić. Kliknij dwukrotnie, aby przejść do żądanego katalogu. Kliknij prawym przyciskiem myszy i wybierz **właściwości** z menu.

![Kliknij prawym przyciskiem myszy menu dla wybranego katalogu](./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png)

Wybierz **poprzednie wersje** lista udziału migawek dla tego katalogu. Lista może potrwać kilka sekund, aby załadować, w zależności od szybkości sieci i liczby migawek udziału w katalogu.

![Karta poprzednie wersje](./media/storage-snapshots-list-browse/snapshot-windows-list.png)

Możesz wybrać **Otwórz** można otworzyć określonego migawki. 

![Otworzyć migawki](./media/storage-snapshots-list-browse/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Przywróć z poprzedniej wersji
Wybierz **przywrócić** Aby skopiować zawartość rekursywnie cały katalog w czasie tworzenia migawki udziału do oryginalnej lokalizacji.
 ![Przycisk w komunikacie ostrzegawczym przywracania](./media/storage-snapshots-list-browse/snapshot-windows-restore.png)

### <a name="share-snapshot-operations-in-azure-cli-20"></a>Operacje migawki udziału w programie Azure CLI 2.0
Azure CLI 2.0 umożliwia wykonywanie operacji takich jak lista udziału migawki, przeglądania, aby udostępnić zawartość migawki, przywracania lub pobieranie plików z udziału migawek lub usuwanie udział migawki.

#### <a name="list-share-snapshots"></a>Lista udziału migawki

Można wyświetlić listę migawki akcji przy użyciu [ `az storage share list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list) z `--include-snapshots`:

```azurecli-interactive 
az storage share list --include-snapshots
```

Polecenie wyświetla listę migawki udziału, wraz z jego powiązanych właściwości. Przykładem jest następujące dane wyjściowe:

```json
[
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:44:13.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:45:18.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": null
  }
]
```

#### <a name="browse-to-a-share-snapshot"></a>Przejdź do udziału migawki
Można przejść do migawki danego udziału i wyświetlania jej zawartości przy użyciu [ `az storage file list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Określ nazwę udziału i sygnaturę czasową, który chcesz przeglądać, jak pokazano w poniższym przykładzie:

```azurecli-interactive 
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

W danych wyjściowych, można zobaczyć, czy zawartość migawki udziału jest identyczna z zawartością udziału w momencie utworzenia tej migawki udziału:

```
Name            Content Length    Type    Last Modified
--------------  ----------------  ------  ---------------
HelloWorldDir/                    dir
IMG_0966.JPG    533568            file
IMG_1105.JPG    717711            file
IMG_1341.JPG    608459            file
IMG_1405.JPG    652156            file
IMG_1611.JPG    442671            file
IMG_1634.JPG    1495999           file
IMG_1635.JPG    974058            file

```
#### <a name="restore-from-a-share-snapshot"></a>Przywracanie z migawki udziału

Można przywrócić plik, kopiując lub pobrać z migawki udziału. Użyj `az storage file download` polecenia, jak pokazano w poniższym przykładzie:

```azurecli-interactive 
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```

W danych wyjściowych zobaczysz, że zawartość pobranego pliku i jego właściwości są takie same jak zawartość i właściwości w momencie, które współużytkują migawka została utworzona:

```json
{
  "content": null,
  "metadata": {},
  "name": "IMG_0966.JPG",
  "properties": {
    "contentLength": 533568,
    "contentRange": "bytes 0-533567/533568",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentType": "application/octet-stream"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D50B5F49F7ACDF\"",
    "lastModified": "2017-10-04T19:37:03+00:00",
    "serverEncrypted": true
  }
}
```

### <a name="file-share-snapshot-operations-in-azure-powershell"></a>Operacje migawki udziału plików w programie Azure PowerShell
Można użyć programu Azure Powershell do wykonywania tej samej operacji, takich jak lista udziału migawki, przeglądanie zawartości migawki udziału, przywracania lub pobieranie plików z migawki udziału lub usuwanie udziału migawek.

#### <a name="list-share-snapshots"></a>Lista udziału migawki

Możesz podać migawek udział w udziale określonym za pomocą `Get-AzureStorageShare`

```powershell
Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
```

#### <a name="browse-share-snapshots"></a>Przeglądaj migawki udziału
Może również przeglądania do migawki, aby wyświetlić jego zawartości przy użyciu akcji `Get-AzureStorageFile` z wartością `-Share` wskazujący określonego migawki

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
Get-AzureStorageFile -Share $snapshot
```

#### <a name="restore-from-share-snapshots"></a>Przywracanie z migawki udziału

Można przywrócić plik, kopiując lub pobierania pliku z udziału migawki za pomocą `Get-AzureStorageFileContent` polecenia

```powershell
$download='C:\Temp\Download'
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $download
```

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
$directory = Get-AzureStorageFile -ShareName "ContosoShare06" -Path "ContosoWorkingFolder" | Get-AzureStorageFile
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $directory
```

## <a name="delete-a-share-snapshot"></a>Usuń migawki udziału

Za pomocą portalu Azure, programu PowerShell, interfejsu wiersza polecenia, interfejsu API REST lub dowolnego zestawu SDK usługi Magazyn można usunąć udziału migawki. W poniższych sekcjach opisano sposób usuwania udziału migawki za pomocą portalu Azure, interfejsu wiersza polecenia i programu PowerShell.

Możesz przejść do udziału migawki i wyświetlanie różnic między dwiema migawkami przy użyciu narzędzia do porównywania. Następnie można określić migawki udziału, który chcesz usunąć. 

Nie można usunąć udziału udział migawki. Aby można było usunąć udział, należy najpierw usunąć wszystkie migawki udziału.

### <a name="delete-a-share-snapshot-by-using-the-portal"></a>Usuń migawki udziału za pomocą portalu  
W portalu przejdź do bloku udziału z pliku i użyć **usunąć** przycisk, aby usunąć co najmniej jednej migawki udziału.

>   ![Usuwanie przycisku](./media/storage-snapshots-delete/portal-snapshots-delete.png)


### <a name="delete-a-share-snapshot-by-using-azure-cli-20"></a>Usuń migawki udziału przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0
Można usunąć migawki udziału za pomocą `[az storage share delete]` polecenia. Użyj programu udziału migawki sygnaturę czasową `--snapshot '2017-10-04T23:28:35.0000000Z' ` parametr w następującym przykładzie:

```azurecli-interactive
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Przykładowe dane wyjściowe:
```json
{
  "deleted": true
}
```

### <a name="delete-a-share-snapshot-by-using-powershell"></a>Usuń migawki udziału za pomocą programu PowerShell
Można usunąć migawki udziału za pomocą `Remove-AzureStorageShare -Share` polecenia:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##Create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

##Delete snapshot
Remove-AzureStorageShare -Share $snapshot

```

## <a name="next-steps"></a>Kolejne kroki
* [Omówienie migawki](storage-snapshots-files.md)
* [Migawki — często zadawane pytania](storage-files-faq.md)
