---
title: "Tworzenie obrazów kontener dla sieci szkieletowej usług Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia obrazów kontener dla aplikacji sieci szkieletowej usług wielu kontenera."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Rozwiązanie docker kontenerów, Mikrousług, sieć szkieletowa usług Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 08b3cc4a52c09ee03818b563794ef9b009d12ef4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-container-images-for-service-fabric"></a>Tworzenie obrazów kontener dla sieci szkieletowej usług

W tym samouczku znajduje się jeden z samouczka serii, która pokazuje, jak używać kontenery w klastrze usługi sieć szkieletowa usług systemu Linux. W tym samouczku aplikacji kontenera wielu jest gotowy do użycia z sieci szkieletowej usług. W kolejnych samouczkach te obrazy są używane jako część aplikacji sieci szkieletowej usług. Ten samouczek zawiera informacje na temat wykonywania następujących czynności: 

> [!div class="checklist"]
> * Źródło powielania aplikacji z usługi GitHub  
> * Tworzenie obrazu kontenera ze źródła aplikacji
> * Wdróż wystąpienie rejestru kontenera platformy Azure (ACR)
> * Tag obrazu kontener dla ACR
> * Przekaż obraz do awaryjnego

W tej serii samouczka, dowiesz się, jak: 

> [!div class="checklist"]
> * Tworzenie obrazów kontener dla sieci szkieletowej usług
> * [Tworzenie i uruchamianie aplikacji sieci szkieletowej usług z kontenerami](service-fabric-tutorial-package-containers.md)
> * [Sposób obsługi trybu failover i skalowania w sieci szkieletowej usług](service-fabric-tutorial-containers-failover.md)

## <a name="prerequisites"></a>Wymagania wstępne

- Środowisko programistyczne systemu Linux dla platformy Service Fabric. Postępuj zgodnie z instrukcjami [tutaj](service-fabric-get-started-linux.md) do konfigurowania środowiska systemu Linux. 
- Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 
- Ponadto wymaga posiadania subskrypcji platformy Azure dostępna. Aby uzyskać więcej informacji dotyczących bezpłatną wersję próbną, przejdź [tutaj](https://azure.microsoft.com/free/).

## <a name="get-application-code"></a>Pobieranie kodu aplikacji

Przykładowa aplikacja używana w tym samouczku jest głosowania aplikacji. Aplikacja składa się z składników frontonu sieci web oraz wystąpienia pamięci podręcznej Redis zaplecza. Składniki są pakowane w kontenerze obrazów. 

Użyj git, aby pobrać kopię aplikacji w środowisku deweloperskim.

```bash
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

cd service-fabric-dotnet-containers/Linux/container-tutorial/
```

Katalog "kontener — samouczek" zawiera folder o nazwie "azure głosowanie". Ten folder "azure głosowanie" zawiera kod źródłowy frontonu i plik Dockerfile tworzenie frontonu. Katalog "kontener — samouczek" zawiera również katalog "redis", który zawiera plik Dockerfile, aby utworzyć obraz redis. Tych katalogach znajdują się zasoby niezbędne dla tego samouczka zestawu. 

## <a name="create-container-images"></a>Tworzenie kontenera obrazów

W katalogu "azure głosowanie" Uruchom następujące polecenie, aby utworzyć obraz dla składników frontonu sieci web. To polecenie używa plik Dockerfile w tym katalogu do utworzenia obrazu. 

```bash
docker build -t azure-vote-front .
```

Wewnątrz katalogu "redis", uruchom następujące polecenie, aby utworzyć obraz wewnętrznej bazy danych redis. To polecenie używa plik Dockerfile w katalogu, aby utworzyć obraz. 

```bash
docker build -t azure-vote-back .
```

Po zakończeniu użyj [obrazy usługi docker](https://docs.docker.com/engine/reference/commandline/images/) polecenie, aby wyświetlić utworzony obrazów.

```bash
docker images
```

Zwróć uwagę, że cztery obrazy zostały pobrane lub utworzony. *Azure głos początku* obraz zawiera aplikację. Został uzyskany z *python* obrazu z Centrum Docker. Obraz Redis pobranego z Centrum Docker.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Wdrażanie rejestru kontenera platformy Azure

Najpierw uruchom [logowania az](/cli/azure/login) polecenie, aby zalogować się do konta platformy Azure. 

Następnie użyj [konta az](/cli/azure/account#set) polecenie, aby wybrać subskrypcję do utworzenia kontenera Azure rejestru. 

```bash
az account set --subscription <subscription_id>
```

W przypadku wdrażania rejestru kontenera platformy Azure, musisz najpierw grupę zasobów. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). W tym przykładzie grupy zasobów o nazwie *myResourceGroup* jest tworzony w *westus* regionu. Wybierz grupy zasobów w regionie, w pobliżu. 

```bash
az group create --name myResourceGroup --location westus
```

Tworzenie kontenera Azure rejestru [az acr utworzyć](/cli/azure/acr#create) polecenia. Nazwa rejestru kontenera **muszą być unikatowe**.

```bash
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

W dalszej części tego samouczka używamy "acrname" jako symbolu zastępczego dla wybranej nazwy rejestru kontenera.

## <a name="log-in-to-your-container-registry"></a>Zaloguj się do rejestru kontenera

Zaloguj się do swojego wystąpienia ACR przed wypchnięciem obrazów do niego. Użyj [logowania acr az](/cli/azure/acr?view=azure-cli-latest#az_acr_login) polecenia do wykonania operacji. Podaj unikatową nazwę podane w rejestrze kontenera podczas jej tworzenia.

```bash
az acr login --name <acrName>
```

Polecenie zwraca komunikat "Pomyślnie logowania" po ukończeniu.

## <a name="tag-container-images"></a>Tag kontener obrazów

Obraz każdego kontenera musi być oznakowane o nazwie loginServer rejestru. Ten tag jest używane do przesyłania przypadku wypychania kontener obrazów w rejestrze obrazu.

Aby wyświetlić listę bieżącego obrazów, użyj [obrazy usługi docker](https://docs.docker.com/engine/reference/commandline/images/) polecenia.

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

Aby uzyskać nazwę loginServer, uruchom następujące polecenie:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Teraz, tag *azure głos początku* obrazu o loginServer rejestru kontenera. Ponadto Dodaj `:v1` na końcu nazwy obrazu. Znacznik określa wersję obrazu.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Następnie tagu *azure głos zwrotnego* obrazu o loginServer rejestru kontenera. Ponadto Dodaj `:v1` na końcu nazwy obrazu. Znacznik określa wersję obrazu.

```bash
docker tag azure-vote-back <acrLoginServer>/azure-vote-back:v1
```

Po oznakowany, uruchom "obrazy usługi docker" Aby zweryfikować działanie.


Dane wyjściowe:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-back                        latest              bf9a858a9269        22 minutes ago      107MB
<acrName>.azurecr.io/azure-vote-back    v1                  bf9a858a9269        22 minutes ago      107MB
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf        23 minutes ago      708MB
redis                                  latest              9813a7e8fcc0        2 days ago          107MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Wypychanie obrazów do rejestru

Wypychanie *azure głos początku* obrazu w rejestrze. 

Korzystając z następującego przykładu, Zamień nazwa ACR loginServer loginServer ze środowiska.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

Wypychanie *azure głos zwrotnego* obrazu w rejestrze. 

Korzystając z następującego przykładu, Zamień nazwa ACR loginServer loginServer ze środowiska.

```bash
docker push <acrLoginServer>/azure-vote-back:v1
```

Polecenia wypychania docker potrwać kilka minut.

## <a name="list-images-in-registry"></a>Listy obrazów w rejestrze

Aby powrócić do listy obrazów, do których został przypisany do rejestru kontenera platformy Azure, użyj [listy repozytorium acr az](/cli/azure/acr/repository#list) polecenia. Zaktualizuj polecenia o nazwie ACR.

```bash
az acr repository list --name <acrName> --output table
```

Dane wyjściowe:

```bash
Result
----------------
azure-vote-back
azure-vote-front
```

Po ukończeniu samouczka obrazu kontenera przechowywanych w prywatnej wystąpienia rejestru kontenera platformy Azure. Ten obraz jest wdrażana w kolejnych samouczkach z ACR do klastra usługi sieć szkieletowa usług.

## <a name="next-steps"></a>Następne kroki

W tym samouczku aplikacji ściągnięcia z usługi Github i kontener obrazów utworzonych i przypisany do rejestru. Wykonano następujące czynności:

> [!div class="checklist"]
> * Źródło powielania aplikacji z usługi GitHub  
> * Tworzenie obrazu kontenera ze źródła aplikacji
> * Wdróż wystąpienie rejestru kontenera platformy Azure (ACR)
> * Tag obrazu kontener dla ACR
> * Przekaż obraz do awaryjnego

Przejdź do następnego w samouczku Dowiedz się więcej o kontenery opakowania do aplikacji sieci szkieletowej usług za pomocą narzędzia Yeoman. 

> [!div class="nextstepaction"]
> [Pakiet i wdróż kontenery jako aplikacji sieci szkieletowej usług](service-fabric-tutorial-package-containers.md)
