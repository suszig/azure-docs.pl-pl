---
title: "Wdrożenie zdalne monitorowania lokalnie - Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób wdrażania na komputerze lokalnym do testowania i rozwoju zdalnego wstępnie skonfigurowane rozwiązanie monitorowania."
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 03/07/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 77f40e2f10cbdb9930a22a4248e19bb3356af7af
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-locally"></a>Wdrażanie zdalne wstępnie skonfigurowane rozwiązanie monitorowania lokalnie

W tym artykule przedstawiono sposób wdrażania na komputerze lokalnym do testowania i rozwoju zdalnego wstępnie skonfigurowane rozwiązanie monitorowania. Takie podejście wdraża mikrousług lokalnego kontenerze Docker i używa Centrum IoT, rozwiązania Cosmos bazę danych i usług Azure storage w chmurze. Możesz użyć wstępnie skonfigurowanych rozwiązań (komputery) interfejsu wiersza polecenia do wdrożenia usługi w chmurze Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Aby wdrożyć używane przez zdalne wstępnie skonfigurowane rozwiązanie monitorowania usług Azure, musisz mieć aktywną subskrypcją platformy Azure.

Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](http://azure.microsoft.com/pricing/free-trial/).

Do wykonania lokalnego wdrożenia, potrzebne są następujące narzędzia zainstalowane na komputerze deweloperskim lokalnego:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Rozwiązania docker compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) — to oprogramowanie jest wymagane w przypadku komputerów z interfejsu wiersza polecenia.
* KOMPUTERY INTERFEJSU WIERSZA POLECENIA

> [!NOTE]
> Te narzędzia są dostępne na wiele platform, w tym Windows, Linux lub z systemem iOS.

### <a name="install-the-pcs-cli"></a>Zainstaluj interfejs wiersza polecenia komputerów

Aby zainstalować interfejsu wiersza polecenia, uruchom następujące polecenie w wierszu polecenia środowiska:

```cmd/sh
npm install iot-solutions -g
```

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia, zobacz [sposób użycia interfejsu wiersza polecenia](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="deploy-the-azure-services"></a>Wdrażanie usług Azure

Mimo że w tym artykule pokazano, jak uruchomić mikrousług lokalnie, zależą one od trzech usług platformy Azure w chmurze. Wdrożenia tych usług Azure ręcznie za pośrednictwem portalu Azure lub użyj interfejsu wiersza polecenia komputerów. W tym artykule przedstawiono sposób użycia `pcs` narzędzia.

### <a name="sign-in-to-the-cli"></a>Zaloguj się do interfejsu wiersza polecenia

Przed wdrożeniem wstępnie skonfigurowane rozwiązanie, należy zalogować się do subskrypcji platformy Azure przy użyciu interfejsu wiersza polecenia w następujący sposób:

```cmd/sh
pcs login
```

Postępuj zgodnie z wyświetlanymi instrukcjami, aby ukończyć procesu logowania.

### <a name="run-a-local-deployment"></a>Uruchamianie lokalnego wdrożenia

Użyj następującego polecenia można uruchomić lokalnego wdrożenia:

```cmd/pcs
pcs -s local
```

Skrypt wyświetla monit o podanie poniższych informacji:

* Nazwa rozwiązania.
* Subskrypcja platformy Azure, która ma być używana.
* Lokalizacja centrum danych Azure do użycia.

Skrypt tworzy Centrum IoT wystąpienia, wystąpienie rozwiązania Cosmos bazy danych i konto magazynu platformy Azure w grupie zasobów w Twojej subskrypcji platformy Azure. Nazwa grupy zasobów jest nazwą rozwiązania wybrano podczas uruchomienia `pcs` narzędzia.

Skrypt może zająć kilka minut do uruchomienia. Po zakończeniu instalacji zostanie wyświetlony komunikat z `Copy the following environment variables to /scripts/local/.env file:`. Skopiuj definicje zmiennych środowiska po wiadomości, można użyć w kolejnym kroku.

## <a name="download-the-source-code"></a>Pobieranie kodu źródłowego

Zdalne repozytorium kodu źródłowego monitorowania obejmuje Docker pliki konfiguracji, które należy pobrać, skonfigurować i uruchomić obrazów Docker, które zawierają mikrousług. Klonowanie repozytorium, przejdź do odpowiedniego folderu na komputerze lokalnym, a następnie uruchom jedno z następujących poleceń:

Aby zainstalować implementacje Java mikrousług, uruchom polecenie:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Aby zainstalować implementacje .net mikrousług, uruchom polecenie:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Te polecenia Pobierz kod źródłowy dla wszystkich mikrousług. Mimo że nie ma potrzeby kodu źródłowego, aby uruchomić mikrousług w Docker, kod źródłowy jest przydatne, jeśli planujesz później zmodyfikować wstępnie skonfigurowane rozwiązanie i przetestuj zmiany lokalnie.

## <a name="run-the-microservices-in-docker"></a>Uruchom mikrousług w Docker

Aby uruchomić mikrousług Docker, należy najpierw zmodyfikować **skryptów\\lokalnego\\.env** pliku w lokalnej kopii repozytorium. Zamień całą zawartość pliku definicje zmiennych środowiska należy zanotować za `pcs` polecenia wcześniej. Te zmienne środowiskowe umożliwić mikrousług w kontenerze Docker do łączenia się z usługami Azure utworzonych przez `pcs` narzędzia.

Aby uruchomić wstępnie skonfigurowane rozwiązanie, przejdź do **scripts\local** folderu w środowisku wiersza polecenia i uruchom następujące polecenie:

```cmd\sh
docker-compose up
```

Przy pierwszym uruchomieniu tego polecenia Docker pobiera obrazy mikrousługi z Centrum Docker tworzenie kontenerów lokalnie. W kolejnych uruchomieniach Docker uruchamia natychmiast kontenerów.

Oddzielne powłoki służy do wyświetlania dzienników z kontenera. Najpierw znaleźć przy użyciu Identyfikatora kontenera `docker ps -a` polecenia. Następnie użyj `docker logs {container-id} --tail 1000` do wyświetlania ostatnich wpisów dziennika 1000 określonego kontenera.

Aby uzyskać dostęp do zdalnego pulpitu nawigacyjnego monitorowania rozwiązania, przejdź do [adresem http://localhost: 8080](http://localhost:8080) w przeglądarce.

## <a name="tidy-up"></a>Uporządkować

Aby uniknąć niepotrzebnych kosztów, po zakończeniu testowania, należy usunąć usługi w chmurze z subskrypcją platformy Azure. Najprostszym sposobem usunięcia usług jest użycie portalu Azure można usunąć grupy zasobów utworzonej przez `pcs` narzędzia.

Użyj `docker-compose down --rmi all` polecenie, aby usunąć obrazy usługi Docker i wolnego miejsca na komputerze lokalnym. Możesz także usunąć lokalną kopię zdalnego repozytorium monitorowania utworzenia sklonowanego kodu źródłowego z usługi GitHub.

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wstępnie skonfigurowanego rozwiązania
> * Wdrażanie wstępnie skonfigurowane rozwiązanie
> * Zaloguj się do wstępnie skonfigurowane rozwiązanie

Teraz, gdy wdrożono rozwiązanie monitorowania zdalnego następnym krokiem jest [Poznaj możliwości pulpit nawigacyjny rozwiązania](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->