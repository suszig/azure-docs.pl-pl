---
title: "Tworzenie funkcji w systemie Linux przy użyciu niestandardowego obrazu (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia usługi Azure Functions systemem niestandardowego obrazu systemu Linux."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 67ee02df2c42ba39c2f186cc95fa886a3d735ed2
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Tworzenie funkcji w systemie Linux przy użyciu niestandardowego obrazu (wersja zapoznawcza)

Azure Functions umożliwia hostowanie funkcji w systemie Linux w własne niestandardowe kontenera. Ta funkcja jest obecnie w przeglądzie. Możesz również [hosta w usłudze Azure App Service domyślny kontener](functions-create-first-azure-function-azure-cli-linux.md).  

Z tego samouczka dowiesz się sposobu wdrażania aplikacji funkcji jako obraz niestandardowy Docker. Ten wzorzec jest przydatne, gdy musisz dostosować wbudowanych obrazu kontenera usługi aplikacji. Można użyć niestandardowego obrazu funkcji wymagana jest wersja języka lub wymagają określonych zależności lub konfiguracji, który nie został podany w obrazie wbudowanych.

Ten samouczek przedstawia sposób użycia usługi Azure Functions do tworzenia i Wypchnij obrazu niestandardowego do Centrum Docker. Następnie użyć tego obrazu jako źródła wdrożenia dla aplikacji funkcja, która działa w systemie Linux. Docker służy do tworzenia i Wypchnij obrazu. Interfejsu wiersza polecenia Azure umożliwia tworzenie aplikacji funkcji i wdrażanie obrazu z Centrum Docker. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie niestandardowego obrazu przy użyciu rozwiązania Docker.
> * Opublikuj obraz niestandardowy w rejestrze kontenera. 
> * Tworzenie konta usługi Azure Storage. 
> * Tworzenie planu usługi aplikacji systemu Linux. 
> * Wdrażanie funkcji aplikacji z Centrum Docker.
> * Dodawanie ustawienia aplikacji do aplikacji funkcji. 

Poniższe kroki są obsługiwane na komputerze Mac, systemu Windows lub Linux.  

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

* [Git](https://git-scm.com/downloads)
* Aktywny [subskrypcji platformy Azure](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* A [konta Centrum Docker](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

W oknie terminalu uruchom następujące polecenie do klonowania repozytorium przykładowej aplikacji na komputerze lokalnym, a następnie przejdź do katalogu, który zawiera przykładowy kod.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>Utwórz obraz z pliku Docker

W tym repozytorium Git, Przyjrzyjmy się _plik Dockerfile_. Ten plik zawiera opis środowiska, które są wymagane do uruchomienia aplikacji funkcji w systemie Linux. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Odnośnie do hostowania obrazu w rejestrze kontener prywatnego, ustawienia połączenia należy dodać do aplikacji funkcji za pomocą **ENV** zmiennych w plik Dockerfile. Ponieważ w tym samouczku nie może zagwarantować używanie rejestru prywatnego, ustawienia połączenia są [dodawane po wdrożeniu przy użyciu interfejsu wiersza polecenia Azure](#configure-the-function-app) jako najlepszego rozwiązania bezpieczeństwa.   

### <a name="run-the-build-command"></a>Uruchom polecenie kompilacji
Aby utworzyć obraz Docker, uruchom `docker build` polecenia i podaj nazwę, `mydockerimage`i tagu `v1.0.0`. Zastąp `<docker-id>` w Centrum Docker konta identyfikatora.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Polecenie generuje dane wyjściowe podobne do następujących:

```bash
Sending build context to Docker daemon  169.5kB
Step 1/3 : FROM microsoft/azure-functions-runtime:v2.0.0-jessie
v2.0.0-jessie: Pulling from microsoft/azure-functions-runtime
b178b12f7913: Pull complete
2d9ce077a781: Pull complete
4775d4ba55c8: Pull complete
Digest: sha256:073f45fc167b3b5c6642ef4b3c99064430d6b17507095...
Status: Downloaded newer image for microsoft/azure-functions-runtime:v2.0.0-jessie
 ---> 217799efa500
Step 2/3 : ENV AzureWebJobsScriptRoot /home/site/wwwroot
 ---> Running in 528fa2077d17
 ---> 7cc6323b8ae0
Removing intermediate container 528fa2077d17
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5bdac9878423
Successfully built 5bdac9878423
Successfully tagged ggailey777/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Testowanie obrazu lokalnie
Sprawdź, czy obraz wbudowanych działa, uruchamiając Docker obrazu w kontenerze lokalnego. Problem [docker Uruchom](https://docs.docker.com/engine/reference/commandline/run/) poleceń i przekazywania do niej nazwy i tag obrazu. Należy określić przy użyciu portu `-p` argumentu.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Przy użyciu niestandardowego obrazu uruchomione w kontenerze Docker lokalnej, sprawdź aplikacji funkcji i kontener działają poprawnie przechodząc do <adresem http://localhost: 8080>.

![Testowanie aplikacji funkcji lokalnie.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Po zweryfikowaniu aplikacji funkcji w kontenerze hav można zatrzymać wykonywania. Teraz możesz wypchnąć niestandardowego obrazu na koncie Centrum Docker.

## <a name="push-the-custom-image-to-docker-hub"></a>Wypychanie niestandardowego obrazu do Centrum Docker

Rejestr jest aplikacja, która przechowuje obrazów i udostępnia obrazu i kontener usług. W celu udostępnienia obrazu, należy wypchnąć go do rejestru. Centrum docker to rejestru Docker obrazów, który umożliwia hostowanie własne repozytoriach publicznych lub prywatnych. 

Zanim można wypchnąć obrazu, należy zalogować się do Centrum Docker przy użyciu [logowania docker](https://docs.docker.com/engine/reference/commandline/login/) polecenia. Zastąp `<docker-id>` z nazwą konta i wpisz hasło do konsoli w wierszu. Inne opcje haseł Centrum Docker dla [docker logowania polecenie dokumentacji](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id> 
```

Komunikat "logowanie zakończyło się pomyślnie." potwierdza, że użytkownik jest zalogowany. Po zalogowaniu się można wypchnąć do Centrum Docker obrazu przy użyciu [wypychania docker](https://docs.docker.com/engine/reference/commandline/push/) polecenia.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Upewnij się, że wypychania zakończyło się pomyślnie, sprawdzając polecenia danych wyjściowych.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Teraz można użyć tego obrazu jako źródła wdrożenia dla nowej aplikacji funkcji na platformie Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym temacie, wymaga wiersza polecenia platformy Azure w wersji 2.0.21 lub nowszej. Uruchom `az --version` można znaleźć wersją. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Tworzenie planu usługi aplikacji systemu Linux

Linux hosting dla funkcji nie jest obecnie obsługiwane w planie zużycia. Należy uruchomić na plan usługi aplikacji systemu Linux. Aby dowiedzieć się więcej o hostingu, zobacz [hosting usługi Azure Functions plany porównanie](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Tworzenie i wdrażanie niestandardowego obrazu

Aplikacja funkcji obsługuje wykonywanie funkcji. Tworzenie aplikacji funkcji z obrazu Centrum Docker przy użyciu [az functionapp utworzyć](/cli/azure/functionapp#create) polecenia. 

W poniższym poleceniu Zastąp unikatowe funkcja Nazwa aplikacji, której występuje `<app_name>` symbolu zastępczego i konto magazynu nazwy `<storage_name>`. Nazwa `<app_name>` jest używana jako domyślna domena DNS aplikacji funkcji, więc nazwa ta musi być unikatowa wśród wszystkich aplikacji na platformie Azure. Jak wcześniej `<docker-id>` to nazwa konta Docker.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0 
```
Po utworzeniu aplikacji funkcji interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

_Wdrożenia kontenera obrazu nazwa-_ parametr wskazuje obrazu hostowanych w Centrum Docker na potrzeby tworzenia aplikacji funkcji. 


## <a name="configure-the-function-app"></a>Skonfiguruj aplikację — funkcja

Funkcja wymaga parametrów połączenia, aby nawiązać połączenie domyślne konto magazynu. Publikując niestandardowego obrazu do konta Kontener prywatny, zamiast tego należy wprowadzić te ustawienia aplikacji jako zmienne środowiskowe w plik Dockerfile przy użyciu [instrukcji ENV](https://docs.docker.com/engine/reference/builder/#env), lub równoważne. 

W takim przypadku `<storage_account>` jest nazwą utworzonego konta magazynu. Pobrać ciągu połączenia z [Pokaż parametry konta magazynu az](/cli/azure/storage/account#show-connection-string) polecenia. Dodaj te ustawienia aplikacji w aplikacji funkcji z [az functionapp config appsettings zestaw](/cli/azure/functionapp/config/appsettings#set) polecenia.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Teraz można przetestować funkcji systemem Linux na platformie Azure.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie niestandardowego obrazu przy użyciu rozwiązania Docker.
> * Opublikuj obraz niestandardowy w rejestrze kontenera. 
> * Tworzenie konta usługi Azure Storage. 
> * Tworzenie planu usługi aplikacji systemu Linux. 
> * Wdrażanie funkcji aplikacji z Centrum Docker.
> * Dodawanie ustawienia aplikacji do aplikacji funkcji.

Dowiedz się więcej o wdrażaniu usługi Azure Functions lokalnie za pomocą narzędzia podstawowych funkcji platformy Azure.

> [!div class="nextstepaction"] 
> [Kod testu Azure funkcji i lokalnie](functions-run-local.md)
