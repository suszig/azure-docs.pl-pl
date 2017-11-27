---
title: "Wykonywanie operacji w usłudze Azure Blob Storage (magazynie obiektów) przy użyciu interfejsu wiersza polecenia platformy Azure| Microsoft Docs"
description: "Dowiedz się, jak przekazywać i pobierać obiekty blob w usłudze Azure Blob Storage, a także jak utworzyć sygnaturę dostępu współdzielonego (SAS) umożliwiającą zarządzanie dostępem do obiektu blob na koncie magazynu."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/15/2017
ms.author: marsma
ms.openlocfilehash: b4e99793d45b90411a068dd44a981cf24aa67d43
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="perform-blob-storage-operations-with-azure-cli"></a>Wykonywanie operacji w usłudze Blob Storage przy użyciu interfejsu wiersza polecenia platformy Azure

Azure Blob Storage to usługa do przechowywania dużych ilości danych obiektów niestrukturalnych, takich jak dane tekstowe lub binarne, do których można uzyskać dostęp z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. W tym samouczku opisano podstawowe operacje w usłudze Azure Blob Storage, takie jak przekazywanie, pobieranie i usuwanie obiektów blob. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie kontenera
> * Przekazywanie pliku (obiektu blob) do kontenera
> * Wyświetlanie listy obiektów blob w kontenerze
> * Pobieranie obiektu blob z kontenera
> * Kopiowanie obiektu blob między kontami magazynu
> * Usuwanie obiektu blob
> * Wyświetlanie i modyfikowanie właściwości i metadanych obiektów blob
> * Zarządzanie zabezpieczeniami za pomocą sygnatury dostępu współdzielonego (SAS)

Wykonanie instrukcji opisanych w tym samouczku wymaga użycia interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Tworzenie kontenera

Kontenery są podobne do katalogów znajdujących się na komputerze — grupy obiektów blob można organizować w kontenerze tak jak pliki w katalogu. Konto magazynu może zawierać dowolną liczbę kontenerów. W kontenerze można przechowywać nawet 500 TB danych obiektów blob, co stanowi maksymalną ilość danych na koncie magazynu.

Do tworzenia kontenera do przechowywania obiektów blob służy polecenie [az storage container create](/cli/azure/storage/container#create).

```azurecli-interactive
az storage container create --name mystoragecontainer
```

Nazwa kontenera musi zaczynać się literą lub cyfrą i może zawierać tylko litery, cyfry i znak łącznika (-). Aby uzyskać dodatkowe informacje o regułach nazewnictwa kontenerów i obiektów blob, zobacz temat [Naming and Referencing Containers, Blobs, and Metadata (Nazewnictwo i odwoływanie się do kontenerów, obiektów blob i metadanych)](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="enable-public-read-access-for-a-container"></a>Włączanie publicznego dostępu do odczytu do kontenera

Nowo utworzony kontener jest domyślnie prywatny. Oznacza to, że nie można korzystać z kontenera bez [sygnatury dostępu współdzielonego](#create-a-shared-access-signature-sas) lub kluczy dostępu do konta magazynu. Przy użyciu interfejsu wiersza polecenia platformy Azure można to zmienić, ustawiając jeden z trzech poziomów uprawnień kontenera:

| | |
|---|---|
| `--public-access off` | (Domyślnie) Brak publicznego dostępu do odczytu |
| `--public-access blob` | Publiczny dostęp do odczytu tylko do obiektów blob |
| `--public-access container` | Publiczny dostęp do odczytu do obiektów blob, uprawnienie do wyświetlania listy obiektów blob w kontenerze |

Ustawienie publicznego dostępu na wartość `blob` lub `container` powoduje włączenie dostępu tylko do odczytu dla wszystkich użytkowników Internetu. Jeśli na przykład chcesz wyświetlić obrazy przechowywane jako obiekty blob w witrynie internetowej, musisz włączyć publiczny dostęp do odczytu. Z kolei jeśli chcesz włączyć dostęp do odczytu i zapisu, musisz użyć [sygnatury dostępu współdzielonego (SAS)](#create-a-shared-access-signature-sas).

Do włączania publicznego dostępu do odczytu do kontenera służy polecenie [az storage container set-permission](/cli/azure/storage/container#create).

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access container
```

## <a name="upload-a-blob-to-a-container"></a>Przekazywanie obiektu blob do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Blokowe obiekty blob są najczęściej spotykanym typem obiektów blob przechowywanych w usłudze Azure Storage. Uzupełnialne obiekty blob są używane w razie konieczności dodania danych do istniejącego obiektu blob bez modyfikowania jego zawartości, na przykład w przypadku rejestrowania. Stronicowe obiekty blob obsługują pliki VHD maszyn wirtualnych IaaS.

W tym przykładzie do kontenera utworzonego w ostatnim kroku zostanie przekazany obiekt blob za pomocą polecenia [az storage blob upload](/cli/azure/storage/blob#upload).

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Jeśli obiekt blob nie istnieje, zostanie utworzony. W przeciwnym razie zostanie zastąpiony. Zostanie przekazanych kilka plików, tak aby lista obiektów blob, która zostanie wyświetlona w następnym kroku, zawierała kilka pozycji.

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Do wyświetlania listy obiektów blob w kontenerze służy polecenie [az storage blob list](/cli/azure/storage/blob#list).

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

Przykładowe dane wyjściowe:

```
Name            Blob Type      Length  Content Type              Last Modified
--------------  -----------  --------  ------------------------  -------------------------
ISSUE_TEMPLATE  BlockBlob         761  application/octet-stream  2017-04-21T18:29:50+00:00
LICENSE         BlockBlob       18653  application/octet-stream  2017-04-21T18:28:50+00:00
LICENSE-CODE    BlockBlob        1090  application/octet-stream  2017-04-21T18:29:02+00:00
README.md       BlockBlob        6700  application/octet-stream  2017-04-21T18:27:33+00:00
dir1/file1.txt  BlockBlob        6700  application/octet-stream  2017-04-21T18:32:51+00:00
```

## <a name="download-a-blob"></a>Pobieranie obiektu blob

Przy użyciu polecenia [az storage blob download](/cli/azure/storage/blob#download) zostanie pobrany obiekt blob przekazany w poprzednim kroku.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="copy-a-blob-between-storage-accounts"></a>Kopiowanie obiektu blob między kontami magazynu

Można asynchronicznie kopiować obiekty blob w obrębie konta magazynu i regionu lub między różnymi kontami magazynu i regionami.

W poniższym przykładzie pokazano sposób kopiowania obiektów blob z jednego konta magazynu do innego. Najpierw trzeba utworzyć kontener na źródłowym koncie magazynu, określając publiczny dostęp do odczytu do jego obiektów blob. Następnie plik zostanie przekazany do kontenera, a potem obiekt blob z tego kontenera zostanie skopiowany do kontenera na docelowym koncie magazynu.

Aby operacja kopiowania powiodła się, kontener docelowy opisany w tym przykładzie musi już istnieć na docelowym koncie magazynu. Ponadto źródłowy obiekt blob wskazany za pomocą argumentu `--source-uri` musi zawierać token sygnatury dostępu współdzielonego (SAS) albo musi być dostępny publicznie, jak w tym przykładzie.

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob to container in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/path/to/sourcefile \
    --name sourcefile

# Copy blob from source account to destination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

## <a name="delete-a-blob"></a>Usuwanie obiektu blob

Do usuwania obiektu blob z kontenera służy polecenie [az storage blob delete](/cli/azure/storage/blob#delete).

```azurecli-interactive
az storage blob delete \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="set-the-content-type"></a>Ustawianie typu zawartości

Typ zawartości, znany także jako typ MIME, określa format danych w obiekcie blob. Przeglądarki i inne oprogramowanie umożliwia określenie sposobu przetwarzania danych na podstawie typu zawartości. Poniższy przykład ustawia typ zawartości `image/png`.

```azurecli-interactive
# Set the content type
az storage blob update
    --container-name mystoragecontainer 
    --name blobName 
    --content-type image/png
```

## <a name="display-and-modify-blob-properties-and-metadata"></a>Wyświetlanie i modyfikowanie właściwości i metadanych obiektów blob

Każdy obiekt blob ma kilka właściwości zdefiniowanych przez usługę, takich jak np. nazwa, typ i długość, które można wyświetlić za pomocą polecenia [az storage blob show](/cli/azure/storage/blob#show). Za pomocą polecenia [az storage blob metadata update](/cli/azure/storage/blob/metadata#update) można również skonfigurować obiekt blob z niestandardowymi właściwościami, którym można przypisać własne wartości.

W tym przykładzie najpierw wyświetlimy właściwości obiektu blob zdefiniowane przez usługę, a następnie zaktualizujemy obiekt blob przy użyciu dwóch własnych właściwości metadanych. Na koniec za pomocą polecenia [az storage blob metadata show](/cli/azure/storage/blob/metadata#show) wyświetlimy właściwości metadanych obiektu blob oraz ich wartości.

```azurecli-interactive
# Show properties of a blob
az storage blob show \
    --container-name mystoragecontainer \
    --name blobName \
    --output table

# Set metadata properties of a blob (replaces all existing metadata)
az storage blob metadata update \
    --container-name mystoragecontainer \
    --name blobName \
    --metadata "key1=value1" "key2=value2"

# Show metadata properties of a blob
az storage blob metadata show \
    --container-name mystoragecontainer \
    --name blobName
```

## <a name="create-a-shared-access-signature-sas"></a>Tworzenie sygnatury dostępu współdzielonego (SAS)

Sygnatury dostępu współdzielonego (SAS) umożliwiają przyznawanie ograniczonego dostępu do obiektów na koncie magazynu bez udostępniania kluczy dostępu do konta magazynu. Aby wygenerować identyfikator URI umożliwiający dostęp do prywatnego zasobu, musisz utworzyć token sygnatury dostępu współdzielonego z odpowiednimi uprawnieniami i okresem ważności (efektywnym czasem trwania), a następnie dołączyć go jako ciąg zapytania do adresu URL zasobu, co pozwoli na utworzenie pełnego identyfikatora URI sygnatury dostępu współdzielonego. Dzięki temu wszyscy użytkownicy, którzy dysponują tym identyfikatorem URI sygnatury dostępu współdzielonego, będą mogli uzyskać dostęp do zasobu w okresie ważności tokenu sygnatury dostępu współdzielonego. Aby na przykład umożliwić komuś przejrzenie prywatnego obiektu blob, można przyznać dostęp do odczytu do niego na dwie minuty.

W poniższych krokach wyłączymy dostęp publiczny do kontenera, przetestujemy dostęp prywatny, a następnie wygenerujemy i przetestujemy identyfikator URI sygnatury dostępu współdzielonego.

### <a name="disable-container-public-access"></a>Wyłączanie dostępu publicznego do kontenera

Najpierw ustaw poziom dostępu do kontenera na wartość `off`. Gwarantuje to, że dostęp do kontenera lub jego obiektów blob nie będzie możliwy bez sygnatury dostępu współdzielonego lub klucza dostępu do konta magazynu.

```azurecli-interactive
az storage container set-permission \
    --name mystoragecontainer \
    --public-access off
```

### <a name="verify-private-access"></a>Weryfikowanie dostępu prywatnego

Aby potwierdzić brak publicznego dostępu do odczytu do obiektów blob w kontenerze, uzyskaj adres URL jednego z jego obiektów blob za pomocą polecenia [az storage blob url](/cli/azure/storage/blob#url).

```azurecli-interactive
az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv
```

Przejdź do adresu URL obiektu blob w prywatnym oknie przeglądarki. Pojawi się informacja o błędzie `ResourceNotFound`, ponieważ obiekt blob jest prywatny i nie dołączono sygnatury dostępu współdzielonego.

### <a name="create-a-sas-uri"></a>Tworzenie identyfikatora URI sygnatury dostępu współdzielonego

Teraz utworzymy identyfikator URI sygnatury dostępu współdzielonego, który umożliwia dostęp do obiektu blob. W poniższym przykładzie najpierw wypełnimy zmienną adresem URL obiektu blob za pomocą polecenia [az storage blob url](/cli/azure/storage/blob#url), a następnie wypełnimy inną zmienną tokenem sygnatury dostępu współdzielonego wygenerowanym za pomocą polecenia [az storage blob generate-sas](/cli/azure/storage/blob#generate-sas). Aby wygenerować pełny identyfikator URI sygnatury dostępu współdzielonego, połączymy te dwa elementy, oddzielając je separatorem ciągu zapytania `?`.

```azurecli-interactive
# Get UTC datetimes for SAS start and expiry (Example: 1994-11-05T13:15:30Z)
sas_start=`date -u +'%Y-%m-%dT%H:%M:%SZ'`
sas_expiry=`date -u +'%Y-%m-%dT%H:%M:%SZ' -d '+2 minute'`

# Get the URL for the blob
blob_url=`az storage blob url \
    --container-name mystoragecontainer \
    --name blobName \
    --output tsv`

# Obtain a SAS token granting read (r) access between the
# SAS start and expiry times
sas_token=`az storage blob generate-sas \
    --container-name mystoragecontainer \
    --name blobName \
    --start $sas_start \
    --expiry $sas_expiry \
    --permissions r \
    --output tsv`

# Display the full SAS URI for the blob
echo $blob_url?$sas_token
```

### <a name="test-the-sas-uri"></a>Testowanie identyfikatora URI sygnatury dostępu współdzielonego

W prywatnym oknie przeglądarki przejdź do pełnego identyfikatora URI sygnatury dostępu współdzielonego wyświetlonego za pomocą polecenia `echo` w poprzednim fragmencie kodu. Tym razem informacja o błędzie się nie pojawi i będzie można wyświetlić lub pobrać obiekt blob.

Poczekaj na wygaśnięcie adresu URL (w tym przykładzie dwie minuty), a następnie przejdź do identyfikatora URI w innym prywatnym oknie przeglądarki. Pojawi się informacja o błędzie `AuthenticationFailed`, ponieważ token sygnatury dostępu współdzielonego wygasł.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie są już potrzebne żadne zasoby z grupy zasobów, w tym utworzone konto magazynu i obiekty blob przekazane w ramach tego samouczka, usuń grupę zasobów za pomocą polecenia [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono podstawy pracy z obiektami blob w usłudze Azure Storage:

> [!div class="checklist"]
> * Tworzenie kontenera
> * Przekazywanie pliku (obiektu blob) do kontenera
> * Wyświetlanie listy obiektów blob w kontenerze
> * Pobieranie obiektu blob z kontenera
> * Kopiowanie obiektu blob między kontami magazynu
> * Usuwanie obiektu blob
> * Wyświetlanie i modyfikowanie właściwości i metadanych obiektów blob
> * Zarządzanie zabezpieczeniami za pomocą sygnatury dostępu współdzielonego (SAS)

Następujące zasoby zawierają dodatkowe informacje na temat korzystania z interfejsu wiersza polecenia platformy Azure oraz pracy z zasobami na koncie magazynu.

* Interfejs wiersza polecenia platformy Azure
  * [Logowanie za pomocą interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/authenticate-azure-cli) — informacje na temat różnych metod uwierzytelniania za pomocą interfejsu wiersza polecenia, w tym logowania nieinterakcyjnego za pośrednictwem [jednostki usługi](/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal), w celu uruchamiania nienadzorowanych skryptów interfejsu wiersza polecenia platformy Azure.
  * [Dokumentacja poleceń interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/)
* Microsoft Azure Storage Explorer
  * [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) jest bezpłatną, autonomiczną aplikacją oferowaną przez firmę Microsoft, która umożliwia wizualną pracę z danymi przechowywanymi w usłudze Azure Storage w systemach Windows, macOS i Linux.
