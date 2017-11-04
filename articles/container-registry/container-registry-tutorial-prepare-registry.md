---
title: "Samouczek rejestru kontenera platformy Azure — przygotowanie rejestru kontenera platformy Azure z replikacją geograficzną"
description: "Tworzenie rejestru kontenera platformy Azure, skonfiguruj — replikacja geograficzna, przygotować obraz Docker i wdrożyć ją w rejestrze. Części, jeden z trzech części serii."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: acr, azure-container-registry, geo-replication
keywords: "Docker, kontenerów, rejestru, Azure"
ms.service: container-registry
ms.devlang: 
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 75408dc88b23b615971a23dc6235c563229d75aa
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="prepare-a-geo-replicated-azure-container-registry"></a>Przygotowanie rejestru kontenera platformy Azure z replikacją geograficzną

Rejestr kontenera platformy Azure jest prywatny rejestru Docker wdrożona na platformie Azure, które może być zamknięcie sieci w wdrożenia. W tym zestawie trzy artykułów Samouczek zostanie przedstawiony sposób umożliwia wdrażanie aplikacji sieci web platformy ASP.NET Core uruchomione w kontenerze systemu Linux do dwóch — replikacja geograficzna [aplikacji sieci Web dla kontenerów](../app-service/containers/index.yml) wystąpień. Zobaczysz, jak Azure automatycznie wdraża obraz do każdego wystąpienia aplikacji sieci Web z najbliższego repozytorium replikacją geograficzną.

W tym samouczku część w serii trzech części:

> [!div class="checklist"]
> * Tworzenie rejestru kontenera platformy Azure z replikacją geograficzną
> * Klonowanie kodu źródłowego aplikacji z usługi GitHub
> * Utwórz Docker kontenera obraz ze źródła aplikacji
> * Wypychanie obrazu kontenera do rejestru

W kolejnych samouczkach wdrożeniem kontenera z rejestru prywatnych w aplikacji sieci web działa w dwóch regionach platformy Azure. Następnie zaktualizuj kod w aplikacji i zaktualizuj oba wystąpienia aplikacji sieci Web za pomocą jednej `docker push` do rejestru.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.20 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).

Ten samouczek zakłada, że masz podstawową wiedzę na temat bazowych koncepcji usługi Docker, takich jak kontenery, obrazy kontenerów i podstawowe polecenia usługi Docker. W razie potrzeby zapoznaj się z tematem [Get starter with Docker (Rozpoczynanie pracy z platformą Docker)]( https://docs.docker.com/get-started/), aby uzyskać podstawowe informacje na temat kontenerów.

Do ukończenia tego samouczka konieczne będzie środowisko programowania Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Dlatego zaleca się instalacji lokalnej środowiska deweloperskiego wiersza polecenia platformy Azure i klastrem Docker.

> [!IMPORTANT]
> Funkcja — replikacja geograficzna rejestru kontenera Azure jest obecnie w **Podgląd**. Podglądy stają się dostępne dla Ciebie pod warunkiem, że zgadzasz się [uzupełniające warunki użytkowania](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Niektóre aspekty tej funkcji może zmienić przed ogólnodostępnej (GA).
>

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

Zaloguj się w witrynie [Azure Portal](http://portal.azure.com).

Wybierz **nowe** > **kontenery** > **rejestru kontenera platformy Azure**.

![Tworzenie rejestru kontenera w portalu Azure][tut-portal-01]

Skonfiguruj nowy rejestru z następującymi ustawieniami:

* **Nazwa rejestru**: Utwórz nazwę rejestru jest globalnie unikatowa w obrębie platformy Azure, która zawiera znaki alfanumeryczne 5 – 50
* **Grupa zasobów**: **Utwórz nowe** > `myResourceGroup`
* **Lokalizacja**:`West US`
* **Administrator**: `Enable` (wymagane dla aplikacji sieci Web dla kontenerów do ściągnięcia obrazy)
* **Jednostka SKU**: `Premium` (wymagane dla replikacji geograficznej)

Wybierz **Utwórz** wdrażania wystąpienia ACR.

![Tworzenie rejestru kontenera w portalu Azure][tut-portal-02]

W dalszej części tego samouczka, używamy `<acrName>` jako symbol zastępczy kontenera **nazwa rejestru** wybrany.

> [!TIP]
> Ponieważ rejestrów kontenera platformy Azure są typowo długiego okresu życia zasobów, które są używane na wielu hostach kontenera, zaleca się utworzenie rejestru w jego własnej grupy zasobów. Podczas konfigurowania replikacją geograficzną rejestrów i elementów webhook, te dodatkowe zasoby są umieszczane w tej samej grupie zasobów.
>

## <a name="configure-geo-replication"></a>Konfigurowanie replikacji geograficznej

Teraz, gdy masz rejestru Premium można skonfigurować replikację geograficzną. Aplikacja sieci web, które można skonfigurować w następnym samouczku do uruchamiania w dwóch regionach, mogą następnie kopiować obrazy kontenera z dokładnością rejestru.

Przejdź do rejestru nowego kontenera na platformie Azure, portalu i wybierz pozycję **replikacje** w obszarze **usług**:

![Replikacje w rejestrze kontenera portalu Azure, interfejsu użytkownika][tut-portal-03]

Mapa jest wyświetlany przedstawiający zielony Sześciokąty reprezentujący regiony platformy Azure, które są dostępne dla — replikacja geograficzna:

 ![Mapa regionów w portalu Azure][tut-map-01]

Replikowanie rejestru dla regionu wschodnie stany USA, wybierając jego sześciokąt zielony, a następnie wybierz **Utwórz** w obszarze **utworzyć replikacji**:

 ![Utwórz replikacji interfejsu użytkownika w portalu Azure][tut-portal-04]

Po zakończeniu replikacji portalu odzwierciedla *gotowe* dla obu regionów. Użyj **Odśwież** przycisk, aby odświeżyć stan replikacji; może potrwać kilka minut lub tak dla replik może być utworzona i zsynchronizowana.

![Stan replikacji interfejsu użytkownika w portalu Azure][tut-portal-05]

## <a name="container-registry-login"></a>Kontener rejestru logowania

Po skonfigurowaniu — replikacja geograficzna, Utwórz obraz kontenera i wypchnąć go do rejestru. Należy najpierw zalogować się wystąpieniu ACR przed wypchnięciem obrazów do niego. Z [podstawowa, standardowa i Premium jednostki SKU](container-registry-skus.md), można uwierzytelniać za pomocą tożsamości platformy Azure.

Użyj [logowania acr az](https://docs.microsoft.com/en-us/cli/azure/acr#az_acr_login) polecenia do uwierzytelniania i pamięci podręcznej poświadczeń dla rejestru. Zastąp `<acrName>` o nazwie rejestru utworzone w poprzednich krokach.

```azurecli
az acr login --name <acrName>
```

Polecenie zwraca `Login Succeeded` po zakończeniu.

## <a name="get-application-code"></a>Pobieranie kodu aplikacji

Przykładowe w tym samouczku obejmują aplikacji sieci web małej skompilowanej za pomocą [platformy ASP.NET Core](http://dot.net). Aplikacja służy strona HTML, która wyświetla region, z którego obraz został wdrożony przez rejestru kontenera platformy Azure.

![Samouczek aplikacji wyświetlony w przeglądarce][tut-app-01]

Użyj git Pobierz przykład do katalogu lokalnego i `cd` do tego katalogu:

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

## <a name="update-dockerfile"></a>Plik Dockerfile aktualizacji

Plik Dockerfile zawarte w próbce pokazuje, jak kontenera jest wbudowana. Rozpoczyna od oficjalne [aspnetcore](https://store.docker.com/community/images/microsoft/aspnetcore) obrazu, kopie aplikacji pliki w kontenerze, instaluje zależności, kompiluje dane wyjściowe na podstawie urzędnika [aspnetcore kompilacji](https://store.docker.com/community/images/microsoft/aspnetcore-build) obrazu, a na końcu Tworzy obraz aspnetcore zoptymalizowane.

Plik Dockerfile znajduje się pod adresem `./AcrHelloworld/Dockerfile` sklonowany źródła.

```dockerfile
FROM microsoft/aspnetcore:2.0 AS base
# Update <acrName> with the name of your registry
# Example: uniqueregistryname.azurecr.io
ENV DOCKER_REGISTRY <acrName>.azurecr.io
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY AcrHelloworld/AcrHelloworld.csproj AcrHelloworld/
RUN dotnet restore
COPY . .
WORKDIR /src/AcrHelloworld
RUN dotnet build -c Release -o /app

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AcrHelloworld.dll"]
```

Stosowanie w *acr helloworld* obrazu próbuje określić region, z którego wdrożono jego kontener, odpytując serwer DNS informacje dotyczące serwera logowania w rejestrze. Należy określić adres URL serwera logowania z rejestru w `DOCKER_REGISTRY` zmiennej środowiskowej w plik Dockerfile.

Najpierw pobierz adres URL serwera logowania w rejestrze z `az acr show` polecenia. Zastąp `<acrName>` o nazwie rejestru utworzone w poprzednich krokach.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Dane wyjściowe:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Następnie zaktualizuj `DOCKER_REGISTRY` wiersz zawierający adres URL serwera logowania z rejestru. W tym przykładzie możemy zaktualizować wiersza, aby odzwierciedlić naszym przykładzie nazwa rejestru, *uniqueregistryname*:

```dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Utwórz obraz kontenera

Po zaktualizowaniu plik Dockerfile z adresem URL rejestr, możesz za pomocą `docker build` do utworzenia obrazu kontenera. Uruchom następujące polecenie, aby utworzyć obraz i oznacz go za pomocą adresu URL prywatnych rejestru, ponownie zastępując `<acrName>` o nazwie rejestru:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

Kilka wierszy dane wyjściowe są wyświetlane jako obraz Docker jest tworzony (w tym miejscu pokazywane obcięty):

```bash
Sending build context to Docker daemon  523.8kB
Step 1/18 : FROM microsoft/aspnetcore:2.0 AS base
2.0: Pulling from microsoft/aspnetcore
3e17c6eae66c: Pulling fs layer
...
Step 18/18 : ENTRYPOINT dotnet AcrHelloworld.dll
 ---> Running in 6906d98c47a1
 ---> c9ca1763cfb1
Removing intermediate container 6906d98c47a1
Successfully built c9ca1763cfb1
Successfully tagged uniqueregistryname.azurecr.io/acr-helloworld:v1
```

Użyj `docker images` polecenie, aby wyświetlić wbudowanych obrazu:

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                                     TAG                 IMAGE ID            CREATED              SIZE
uniqueregistryname.azurecr.io/acr-helloworld   v1                  c9ca1763cfb1        About a minute ago   285MB
...
```

## <a name="push-image-to-azure-container-registry"></a>Obraz wypychania do rejestru kontenera platformy Azure

Na koniec użyj `docker push` polecenie, aby wypchnąć *acr helloworld* obrazu do rejestru. Zastąp `<acrName>` o nazwie rejestru.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Ponieważ skonfigurowaniu rejestru za replikację geograficzną obrazu są automatycznie replikowane do obu *zachodnie stany USA* i *wschodnie stany USA* regionów z tym jednym `docker push` polecenia.

Dane wyjściowe:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
9716cfe18412: Pushed
074867a942d5: Pushed
a77666945b96: Pushed
953ff32f2036: Pushed
aa2e77726d3c: Pushed
98b800c91d50: Pushed
a75caa09eb1f: Pushed
v1: digest: sha256:c515bcebf249b591b558318e2d0ec21d1320340dbf335730eb32372ff7d34255 size: 1792
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku utworzono rejestru kontenera prywatne i replikacją geograficzną, utworzony obraz kontenera, a następnie wypychana obrazu do rejestru. Wykonując kroki opisane w tym samouczku, można:

> [!div class="checklist"]
> * Utworzony rejestru kontenera platformy Azure z replikacją geograficzną
> * Sklonowana aplikacja kodu źródłowego z usługi GitHub
> * Utworzony obraz kontenera Docker ze źródła aplikacji
> * Wypychana obrazu kontenera do rejestru

Przejdź do następnego samouczek, aby dowiedzieć się więcej o wdrażaniu z kontenera do wielu aplikacji sieci Web dla wystąpień kontenerów do obsługi obrazów lokalnie przy użyciu — replikacja geograficzna.

> [!div class="nextstepaction"]
> [Wdrażanie aplikacji sieci web z rejestru kontenera platformy Azure](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png
