---
title: "Szybki Start Azure - obiektów transferu do/z magazynu obiektów Blob platformy Azure przy użyciu języka Python | Dokumentacja firmy Microsoft"
description: "Szybko poznać, aby przenieść obiekty z magazynu obiektów Blob platformy Azure przy użyciu języka Python"
services: storage
documentationcenter: storage
author: ruthogunnnaike
manager: cwatson
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/12/2017
ms.author: v-ruogun
ms.openlocfilehash: 44ec416a814ff6a5fef79ef21e2f54ce4ce4da17
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-python"></a>Obiekty transferu do/z magazynu obiektów Blob platformy Azure przy użyciu języka Python
Z tego przewodnika Szybki Start dowiesz się przekazywanie, pobieranie i listy blokowych obiektów blob w kontenerze w magazynie obiektów Blob platformy Azure przy użyciu języka Python. 

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start: 
* Zainstaluj [języka Python](https://www.python.org/downloads/)
* Pobierz i zainstaluj [zestawu SDK usługi Magazyn Azure dla języka Python](storage-python-how-to-use-blob-storage.md#download-and-install-azure-storage-sdk-for-python). 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-storage-account-using-the-azure-portal"></a>Utwórz konto magazynu przy użyciu portalu Azure

Najpierw utwórz nowe konto magazynu ogólnego przeznaczenia na potrzeby tego przewodnika Szybki Start. 

1. Przejdź do [portalu Azure](https://portal.azure.com) i zaloguj się za pomocą konta platformy Azure. 
2. W menu Centrum wybierz **nowy** > **magazynu** > **konta magazynu — obiekt blob, plików, tabeli, kolejki**. 
3. Wprowadź nazwę konta magazynu. Nazwa musi mieć od 3 do 24 znaków i może zawierać tylko cyfry i małe litery. Również musi być unikatowa.
4. Ustaw `Deployment model` do **Menedżera zasobów**.
5. Ustaw `Account kind` do **ogólnego przeznaczenia**.
6. Ustaw `Performance` do **standardowe**. 
7. Ustaw `Replication` do **magazyn lokalnie nadmiarowy (LRS)**.
8. Ustaw `Storage service encryption` do **wyłączone**.
9. Ustaw `Secure transfer required` do **wyłączone**.
10. Wybierz subskrypcję. 
11. Aby uzyskać `resource group`, Utwórz nową i nadaj unikatową nazwę. 
12. Wybierz `Location` dla konta magazynu.
13. Sprawdź **Przypnij do pulpitu nawigacyjnego** i kliknij przycisk **Utwórz** można utworzyć konta magazynu. 

Po utworzeniu konta magazynu jest przypięta do pulpitu nawigacyjnego. Kliknij go, aby go otworzyć. W obszarze **ustawienia**, kliknij przycisk **klucze dostępu**. Wybierz klucz i skopiuj nazwę konta magazynu do Schowka, a następnie wklej go do Notatnika do późniejszego użycia.

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji
[Przykładowa aplikacja](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) używane w tym szybkiego startu jest podstawowej aplikacji Python.  

Użyj [git](https://git-scm.com/) do pobrania kopii aplikacji w środowisku deweloperskim. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

To polecenie klonów repozytorium do folderu lokalnego git. Aby otworzyć Python program, odszukaj folder magazynu — obiekty BLOB python — Szybki Start i example.py pliku.  

## <a name="configure-your-storage-connection-string"></a>Konfigurowanie parametrów połączenia magazynu
W aplikacji, należy podać klucz konta magazynu nazwy i konta można utworzyć `BlockBlobService` obiektu. Otwórz `example.py` pliku z Eksploratora rozwiązań w środowiskiem IDE. Zastąp **accountname** i **accountkey** wartości z nazwą konta i klucz. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>Uruchom próbki
Ten przykład tworzy plik testowy w folderze "Dokumenty". Przykładowy program plik testu zostanie przesłany do magazynu obiektów Blob, zawiera listę obiektów blob w kontenerze i pobiera plik z nową nazwą. 

Uruchom przykład. Następujące dane wyjściowe to przykładowe dane wyjściowe zwrócone w przypadku uruchamiania aplikacji:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Po naciśnięciu klawisza, aby kontynuować, program przykładowy usuwa kontenera magazynu i pliki. Przed kontynuowaniem sprawdź folder "Dokumenty" dla dwóch plików. Można je otworzyć i sprawdzić, czy są one takie same.

Można również użyć narzędzia takie jak [Eksploratora usługi Storage Azure](http://storageexplorer.com) do wyświetlania plików w magazynie obiektów Blob. Eksplorator usługi Storage platformy Azure to bezpłatne narzędzie i platform umożliwiający dostęp do danych konta magazynu. 

Po zweryfikowaniu pliki naciśnij dowolny klawisz, aby zakończyć pokaz i usuwanie plików testowych. Teraz, znając prezentowanym przykładzie, otwórz plik example.py, aby przyjrzeć się kodu. 

## <a name="get-references-to-the-storage-objects"></a>Pobierz odwołania do obiektów magazynu
Najpierw musisz jest utworzyć odwołania do obiektów używane do uzyskania dostępu i zarządzania magazynem obiektów Blob. Te obiekty kompilacji na każdym z nich i są używane przez kolejnego na liście.

* Utwórz wystąpienie **BlockBlobService** obiektu, który wskazuje usługi obiektów Blob na koncie magazynu. 

* Utwórz wystąpienie **CloudBlobContainer** obiektu, który reprezentuje uzyskują dostęp do kontenera. Kontenery są używane do organizowania obiektów blob, jak używać folderów na komputerze do organizowania plików.

Po utworzeniu kontenera obiektów Blob w chmurze, można utworzyć wystąpienia **CloudBlockBlob** obiekt, który wskazuje konkretnego obiektu blob, w którym interesuje i wykonywanie operacji takich jak przekazywanie, pobieranie i kopii.

W tej sekcji wystąpienia obiektów, Utwórz nowy kontener i następnie ustawić uprawnień dla kontenera, obiekty BLOB są publiczne. Kontener jest nazywany **quickstartblobs**. 


```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
## <a name="upload-blobs-to-the-container"></a>Przekaż obiekty BLOB do kontenera

Usługa Blob Storage obsługuje blokowe, uzupełnialne i stronicowe obiekty blob. Blokowe obiekty BLOB są najczęściej używane i która jest używana w tym Szybki Start.  

Można przekazać pliku do obiektu blob, Pobierz pełną ścieżkę pliku przez dołączenie nazwy katalogu i nazwa pliku na dysku lokalnym. Następnie można przekazać pliku do określonej ścieżki przy użyciu **utworzyć\_obiektu blob\_z\_ścieżki** — metoda. 

Przykładowy kod tworzy plik lokalny do zastosowania w przypadku przekazywania i pobierania, przechowywanie plików do przekazania jako **pliku\_ścieżki\_do\_pliku** i nazwa obiektu blob jako **lokalnej\_pliku\_nazwa**. Poniższy przykład powoduje przekazanie pliku z kontenera o nazwie **quickstartblobs**.

```python
# Create a file in Documents to test the upload and download.
local_path=os.path.expanduser("~\Documents")
local_file_name ="QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file =os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name
block_blob_service.create_blob_from_path(container_name, local_file_name, full_path_to_file)
```

Istnieje kilka metod przekazywania, korzystających z magazynu obiektów Blob. Na przykład, jeśli masz strumienia pamięci, można użyć **utworzyć\_obiektu blob\_z\_strumienia** — metoda zamiast **utworzyć\_obiektu blob\_z\_ścieżki**. 

Blokowe obiekty BLOB może być możliwie jak 4,7 TB i może być dowolna z arkuszy programu Excel do dużych plików wideo. Stronicowe obiekty BLOB są głównie używane dla pliki VHD używane do tworzenia kopii maszyn wirtualnych IaaS. Dołącz obiekty BLOB są używane do logowania, takie jak kiedy zachodzi potrzeba zapisane do pliku i następnie dodać więcej informacji. Większość obiektów przechowywanych w magazynie obiektów Blob są blokowych obiektów blob.

## <a name="list-the-blobs-in-a-container"></a>Wyświetlanie listy obiektów blob w kontenerze

Pobierz listę plików za pomocą kontenera **list_blobs** metody. Ta metoda zwraca generator. Poniższy kod pobiera listę obiektów blob, a następnie w pętli, przedstawiający nazwy obiektów blob znalezionych w kontenerze.  

```python
# List the blobs in the container
print("\nList blobs in the container")
    generator = block_blob_service.list_blobs(container_name)
    for blob in generator:
        print("\t Blob name: " + blob.name)
```

## <a name="download-the-blobs"></a>Pobieranie obiektów blob

Pobierać obiekty BLOB przy użyciu dysku lokalnym **uzyskać\_obiektów blob\_do\_ścieżki** metody. Poniższy kod pobiera blob przekazany w poprzedniej sekcji. "_DOWNLOADED" jest dodawany jako sufiks nazwy obiektów blob pozwala zobaczyć, oba pliki na dysku lokalnym. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Przekazane w tego przewodnika Szybki Start obiektów blob nie są już potrzebne, można usunąć za pomocą całego kontenera **usunąć\_kontenera**. Jeśli pliki utworzone nie są już potrzebne, możesz użyć **usunąć\_obiektu blob** metody, aby usunąć pliki.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="next-steps"></a>Następne kroki
 
W tym szybkiego startu przedstawiono sposób przesyłania plików między magazynem obiektów blob platformy Azure przy użyciu języka Python i dysku lokalnego. Aby dowiedzieć się więcej na temat pracy z magazynem obiektów blob, nadal do magazynu obiektów Blob, jak to zrobić.

> [!div class="nextstepaction"]
> [Porada operacje magazynu obiektów blob](./storage-python-how-to-use-blob-storage.md)
 

Aby uzyskać więcej informacji na temat Eksploratora usługi Storage i obiektów blob, zobacz [zasobami magazynu obiektów Blob platformy Azure zarządzać za pomocą Eksploratora usługi Storage](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
