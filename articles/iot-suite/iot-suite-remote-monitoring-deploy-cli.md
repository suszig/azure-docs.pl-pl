---
title: "Wdrażanie zdalne monitorowanie rozwiązania - Azure Java | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób udostępnienia zdalnego rozwiązanie monitorowania wstępnie skonfigurowane przy użyciu interfejsu wiersza polecenia."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 94c3db3286623264e9df7873962d10dd5cc662d4
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-using-the-cli"></a>Wdrażanie zdalne rozwiązanie monitorowania wstępnie skonfigurowane przy użyciu interfejsu wiersza polecenia

W tym samouczku przedstawiono sposób aprowizowania wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego. Możesz wdrożyć rozwiązanie przy użyciu interfejsu wiersza polecenia. Można także wdrożyć rozwiązanie przy użyciu interfejsu użytkownika sieci web na azureiotsuite.com, aby dowiedzieć się więcej o tej opcji, zobacz [wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania, należy aktywną subskrypcją platformy Azure.

Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](http://azure.microsoft.com/pricing/free-trial/).

Aby uruchomić interfejsu wiersza polecenia, należy [Node.js](https://nodejs.org/) zainstalowany na komputerze lokalnym.

## <a name="install-the-cli"></a>Instalowanie interfejsu wiersza polecenia

Aby zainstalować interfejsu wiersza polecenia, uruchom następujące polecenie w wierszu polecenia środowiska:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Zaloguj się do interfejsu wiersza polecenia

Przed wdrożeniem wstępnie skonfigurowane rozwiązanie, należy zalogować się do subskrypcji platformy Azure przy użyciu interfejsu wiersza polecenia w następujący sposób:

```cmd/sh
pcs login
```

Postępuj zgodnie z wyświetlanymi instrukcjami, aby ukończyć procesu logowania.

## <a name="deployment-options"></a>Opcje wdrażania

Podczas wdrażania wstępnie skonfigurowanych rozwiązań, dostępnych jest kilka opcji, które konfigurują procesu wdrażania:

| Opcja | Wartości | Opis |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | A _podstawowe_ wdrożenia jest przeznaczony dla testu i pokazów, wszystkie mikrousług jest wdrażana na jednej maszynie wirtualnej. A _standardowe_ wdrożenia jest przeznaczony dla środowiska produkcyjnego, mikrousług wdrażania wielu maszyn wirtualnych. A _lokalnego_ wdrożenia konfiguruje kontener Docker do uruchomienia mikrousług na komputerze lokalnym i używa usług Azure, takich jak magazyn i DB rozwiązania Cosmos w chmurze. |
| Środowisko uruchomieniowe | `dotnet`, `java` | Wybiera implementacja języka mikrousług. |

Aby dowiedzieć się więcej o sposobie używania lokalnego wdrożenia, zobacz [uruchomiony lokalnie zdalnego rozwiązanie monitorowania](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="deploy-the-preconfigured-solution"></a>Wdrażanie wstępnie skonfigurowane rozwiązanie

### <a name="example-deploy-net-version"></a>Przykład: wdrożenie wersji platformy .NET

Poniższy przykład przedstawia wdrożenie podstawowe, wersji .NET zdalnego wstępnie skonfigurowane rozwiązanie monitorowania:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Przykład: wdrożenie wersji języka Java

Poniższy przykład przedstawia sposób wdrażania standardową, wersję Java zdalnego wstępnie skonfigurowane rozwiązanie monitorowania:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opcje polecenia komputerów

Po uruchomieniu `pcs` polecenie, aby wdrożyć rozwiązanie, pojawi się monit dla:

- Nazwa rozwiązania. Ta nazwa musi być unikatowa.
- Subskrypcja platformy Azure, która ma być używana.
- Lokalizacja.
- Poświadczenia dla maszyn wirtualnych hostujących mikrousług. Można używać tych poświadczeń, uzyskiwać dostęp do maszyn wirtualnych do rozwiązywania problemów.

Gdy `pcs` polecenia zakończeniu on Wyświetla adres URL nowego wdrożenia wstępnie skonfigurowanych rozwiązań. `pcs` Polecenie powoduje utworzenie pliku `{deployment-name}-output.json` informacje dodatkowe, takie jak nazwa Centrum IoT udostępniony dla Ciebie.

Aby uzyskać więcej informacji na temat parametrów wiersza polecenia Uruchom polecenie:

```cmd/sh
pcs -h
```

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia, zobacz [sposób użycia interfejsu wiersza polecenia](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wstępnie skonfigurowanego rozwiązania
> * Wdrażanie wstępnie skonfigurowane rozwiązanie
> * Zaloguj się do wstępnie skonfigurowane rozwiązanie

Teraz, gdy wdrożono rozwiązanie monitorowania zdalnego następnym krokiem jest [Poznaj możliwości pulpit nawigacyjny rozwiązania](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->