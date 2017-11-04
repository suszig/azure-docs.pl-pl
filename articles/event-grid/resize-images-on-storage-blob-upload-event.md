---
title: "Użyj Azure zdarzeń siatki można zautomatyzować, zmiana rozmiaru przekazać obrazów | Dokumentacja firmy Microsoft"
description: "Azure siatki zdarzeń może wyzwolić na przekazywanie obiektu blob w usłudze Azure Storage. Służy to do wysyłania plików obrazu przekazane do magazynu Azure do innych usług, takich jak usługi Azure Functions do zmiany rozmiaru i inne usprawnienia."
services: event-grid
author: ggailey777
manager: cfowler
editor: 
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 709d23ab590c06d5da9b03e2767bc0be5905355b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Automatyzowanie zmiana rozmiaru obrazów przekazane za pomocą zdarzeń siatki

[Azure siatki zdarzeń](overview.md) to usługa obsługi zdarzeń dla chmury. Siatka zdarzeń można tworzyć subskrypcje zdarzenia generowane przez usług platformy Azure lub innych zasobów.  

W tym samouczku jest częścią dwóch serii samouczków magazynu. Rozszerza [poprzedniego samouczek magazynu] [ previous-tutorial] można dodać przy użyciu siatki zdarzeń platformy Azure i usługi Azure Functions niekorzystającą automatyczne generowanie miniatur. Włącza siatki zdarzeń [usługi Azure Functions](..\azure-functions\functions-overview.md) odpowiedzieć na [magazynu obiektów Blob Azure](..\storage\blobs\storage-blobs-introduction.md) zdarzeń oraz generowanie miniatur przekazane obrazy. Tworzenia subskrypcji zdarzeń dla obiektu Blob magazynu utworzyć zdarzenia. Gdy obiekt blob jest dodawany do określonego kontenera magazynu obiektów Blob, nosi nazwę punktu końcowego funkcji. Dane przekazane do powiązanie funkcji z siatki zdarzeń umożliwia dostęp do obiektu blob i Wygeneruj obraz miniatury. 

Dodawanie funkcji zmiany rozmiaru do istniejącej aplikacji przekazywanie obrazu używasz interfejsu wiersza polecenia Azure i portalu Azure.

![Opublikowana aplikacja sieci web w przeglądarce Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz konto magazynu Azure ogólne
> * Wdrażanie kodu bez serwera przy użyciu usługi Azure Functions
> * Utwórz subskrypcję zdarzenia magazynu obiektów Blob w siatce zdarzeń

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

+ Należy wykonać poprzednie samouczek magazynu obiektów Blob: [przekazać dane obrazu w chmurze za pomocą usługi Azure Storage][previous-tutorial]. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym temacie, wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.14 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

Jeśli nie korzystasz z usługi Cloud Shell, musisz się najpierw zalogować za pomocą polecenia `az login`.

## <a name="create-an-azure-storage-account"></a>Tworzenie konta usługi Azure Storage

Środowisko Azure Functions wymaga konta magazynu ogólnego. Utwórz oddzielnego ogólne konta magazynu w grupie zasobów za pomocą [Tworzenie konta magazynu az](/cli/azure/storage/account#create) polecenia.

Nazwy kont usługi Magazyn muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery. 

W poniższym poleceniu zastąp własne globalnie unikatowej nazwy dla konta magazynu ogólne, w której występuje `<general_storage_account>` symbolu zastępczego. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Tworzenie aplikacji funkcji  

Musi mieć aplikacji funkcji do wykonywania funkcji obsługi. Aplikacja funkcji zapewnia środowisko do bezserwerowego wykonywania kodu funkcji. Utwórz aplikację funkcji przy użyciu polecenia [az functionapp create](/cli/azure/functionapp#create). 

W poniższym poleceniu zastąp własną nazwą aplikacji unique — funkcja, której występuje `<function_app>` symbolu zastępczego. Nazwa `<function_app>` jest używana jako domyślna domena DNS aplikacji funkcji, więc nazwa ta musi być unikatowa wśród wszystkich aplikacji na platformie Azure. W takim przypadku `<general_storage_account>` jest nazwą utworzonego konta magazynu ogólnego.  

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Teraz należy skonfigurować aplikację funkcji do połączenia z magazynem obiektów blob. 

## <a name="configure-the-function-app"></a>Skonfiguruj aplikację — funkcja

Funkcja wymaga parametrów połączenia, aby nawiązać połączenie z kontem magazynu obiektów blob. W takim przypadku `<blob_storage_account>` to nazwa konta magazynu obiektów Blob, utworzony w poprzednim samouczka. Pobrać ciągu połączenia z [Pokaż parametry konta magazynu az](/cli/azure/storage/account#show-connection-string) polecenia. Nazwa kontenera miniaturę również musi być ustawiona na `thumbs`. Dodaj te ustawienia aplikacji w aplikacji funkcji z [az functionapp config appsettings zestaw](/cli/azure/functionapp/config/appsettings#set) polecenia.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbs
```

Teraz można wdrożyć projektu kodu funkcji do tej funkcji aplikacji.

## <a name="deploy-the-function-code"></a>Wdrażanie kodu — funkcja 

Funkcji języka C#, która wykonuje rozmiaru obrazu jest dostępna w tym [repozytorium GitHub próbki](https://github.com/Azure-Samples/function-image-upload-resize). Wdrożenie projektu kodu funkcje do aplikacji funkcji za pomocą [konfiguracji źródła wdrożenia functionapp az](/cli/azure/functionapp/deployment/source#config) polecenia. 

W poniższym poleceniu `<function_app>` jest ta sama aplikacja funkcja utworzony w poprzednim skryptu.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

Funkcja zmiany rozmiaru obrazu jest wyzwalany przez zdarzenie subskrybowanie zdarzeń utworzony obiekt Blob. Dane przekazane do wyzwalacza zawiera adres URL obiektu blob, który z kolei jest przekazywany do powiązania wejściowego uzyskanie załadowanego obrazu z magazynu obiektów Blob. Funkcja generuje obraz miniatury i zapisuje wynikowy strumienia oddzielne kontenera w magazynie obiektów Blob. Aby dowiedzieć się więcej na temat tej funkcji, zobacz [plik readme w repozytorium przykładowej](https://github.com/Azure-Samples/function-image-upload-resize/blob/master/README.md).
 
Funkcja kod projektu jest wdrażany bezpośrednio z repozytorium próbki publicznego. Aby dowiedzieć się więcej na temat Opcje wdrażania usługi Azure Functions, zobacz [ciągłego wdrażania usługi Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-your-event-subscription"></a>Tworzenie subskrypcji zdarzeń

Subskrypcję zdarzeń wskazuje zdarzenia generowane przez dostawcę ma wysyłane do określonego punktu końcowego. W takim przypadku punktu końcowego jest uwidaczniany przez funkcji. Poniższe kroki umożliwiają utworzenie subskrypcji zdarzeń z funkcji w portalu Azure: 

1. W [portalu Azure](https://portal.azure.com), kliknij strzałkę w lewym dolnym rogu rozszerzenia wszystkich usług, wpisz `functions` w **filtru** pola, a następnie wybierz pozycję **aplikacji funkcji**. 

    ![Przejdź do funkcji aplikacji w portalu Azure](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Rozwiń funkcji aplikacji, wybierz **imageresizerfunc** funkcji, a następnie wybierz **subskrypcji dodać siatki zdarzeń**.

    ![Przejdź do funkcji aplikacji w portalu Azure](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Użyj ustawień subskrypcji zdarzeń określony w tabeli.

    ![Tworzenie subskrypcji zdarzeń z funkcji w portalu Azure](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Ustawienie      | Sugerowana wartość  | Opis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nazwa** | imageresizersub | Nazwa identyfikująca nowej subskrypcji zdarzeń. | 
    | **Typ tematu** |  Konta magazynu | Wybierz dostawcę zdarzeń konta magazynu. | 
    | **Subskrypcja** | Twoja subskrypcja | Domyślnie można wybrać Twojej bieżącej subskrypcji.   |
    | **Grupa zasobów** | myResourceGroup | Wybierz **Użyj istniejącego** i wybierz grupę zasobów, które były używane w tym temacie.  |
    | **Wystąpienie** |  `<blob_storage_account>` |  Wybierz utworzone konto magazynu obiektów Blob. |
    | **Typy zdarzeń** | Utworzony obiekt blob | Usuń zaznaczenie wszystkich typów innych niż **utworzony obiekt Blob**. Tylko typy zdarzeń z `Microsoft.Storage.BlobCreated` są przekazywane do funkcji.| 
    | **Punkt końcowy subskrybenta** | automatycznie wygenerowany | Użyj adresu URL punktu końcowego, który zostanie wygenerowany. | 
    | **Prefiks filtru** | / blobServices domyślne/kontenery/obrazów/blob / | Filtruje zdarzenia magazynu tylko do tych na **obrazów** kontenera.| 

4. Kliknij przycisk **Utwórz** można dodać subskrypcji zdarzeń. Spowoduje to utworzenie subskrypcji zdarzeń, które wyzwala **imageresizerfunc** po dodaniu do obiektu blob **obrazów** kontenera. Po zmianie rozmiaru obrazów są dodawane do **Kciuki** kontenera.

Teraz, gdy usługi wewnętrznej bazy danych są skonfigurowane, można przetestować funkcji zmiany rozmiaru obrazu w przykładowej aplikacji sieci web. 

## <a name="test-the-sample-app"></a>Testowanie aplikacji przykładowych

Aby przetestować obrazu zmiana rozmiaru w aplikacji sieci web, przejdź do adresu URL opublikowanej aplikacji. Domyślnym adresem URL aplikacji sieci Web jest `https://<web_app>.azurewebsites.net`.

Kliknij przycisk **przekazać zdjęć** regionu, aby wybrać i przekazać plik. Możesz także przeciągnąć zdjęcie do tego regionu. 

Zauważ, że po zniknie załadowanego obrazu, kopię załadowanego obrazu jest wyświetlana w **wygenerowany miniatur** Karuzela. Ten obraz został zmiany rozmiaru przez funkcję, dodać do kontenera Kciuki i pobrana przez klienta sieci web. 

![Opublikowana aplikacja sieci web w przeglądarce Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz konto magazynu Azure ogólne
> * Wdrażanie kodu bez serwera przy użyciu usługi Azure Functions
> * Utwórz subskrypcję zdarzenia magazynu obiektów Blob w siatce zdarzeń

Przejście do trzech części serii samouczek magazynu, aby dowiedzieć się, jak zabezpieczyć dostęp do konta magazynu.

> [!div class="nextstepaction"]
> [Bezpieczny dostęp do danych aplikacji w chmurze](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)


+ Aby dowiedzieć się więcej na temat zdarzeń siatki, zobacz [wprowadzenie do usługi Azure Event siatki](overview.md). 
+ Aby wypróbować inną samouczek, że funkcje usługi Azure Functions, zobacz [tworzy funkcję, która integruje się z usługą Azure Logic Apps](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md