---
title: "Połączone fabryki rozwiązania często zadawane pytania — Azure | Dokumentacja firmy Microsoft"
description: "Często zadawane pytania dla fabryki połączonych pakiet IoT"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: corywink
ms.openlocfilehash: d4cb452b34ddefc70dc1adcff0e5fead072aa16a
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Często zadawane pytania dla fabryki połączonych pakiet IoT wstępnie skonfigurowane rozwiązanie

Zobacz też ogólne [— często zadawane pytania](iot-suite-faq.md) pakietu IoT.

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solution"></a>Gdzie można znaleźć kodu źródłowego dla wstępnie skonfigurowane rozwiązanie?

Kod źródłowy jest przechowywane w następujących repozytorium GitHub:

* [Fabryka połączonych wstępnie skonfigurowane rozwiązanie](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>Co to jest OPC UA?

OPC Ujednolicona architektura (UA), wydane w ramach 2008, jest niezależne od platformy, zorientowane na usługę współdziałanie standardowa. OPC UA jest używany przez różnych systemów przemysłowych i urządzeń, takich jak czujniki branży komputerów i układy logiczne. OPC UA integruje funkcjonalność OPC klasycznego specyfikacji jeden rozszerzalne środowisko z wbudowanych zabezpieczeń. Jest to standardowy, który jest wymuszany przez OPC Foundation. [OPC Foundation](http://opcfoundation.org/) jest organizacją nie dla zysków z więcej niż 440 elementów członkowskich. Celem organizacji jest stosować specyfikacje OPC ułatwia współdziałanie wielu dostawców, obejmującego wiele platform, bezpieczny i niezawodny przy użyciu:

* Infrastruktura
* Specyfikacje
* Technologia
* Procesy

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-preconfigured-solution"></a>Dlaczego Microsoft został wybrany OPC UA dla połączonych fabryki wstępnie skonfigurowane rozwiązanie?

Microsoft postanowiono OPC UA, ponieważ jest to platforma otwarty, niezastrzeżonych, standard niezależne, branży i sprawdzone. Jest wymagane dla rozwiązań architektura odwołanie Industrie 4.0 (RAMI4.0) zapewniające współdziałanie szeroką gamę wytwarzania i urządzeń. Microsoft widzi żądania klientów do tworzenia rozwiązań Industrie 4.0. Obsługa OPC UA pomaga zmniejszyć bariery dla klientów do ich celach i udostępnia natychmiastowe wartość do nich.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Jak dodać publicznego adresu IP na symulacyjnych maszyny Wirtualnej?

Dostępne są dwie opcje, aby dodać adres IP:

* Użyj skryptu programu PowerShell `Simulation/Factory/Add-SimulationPublicIp.ps1` w [repozytorium](https://github.com/Azure/azure-iot-connected-factory). Podaj nazwę wdrożenia jako parametr. Dla wdrożenia lokalnego, należy użyć `<your username>ConnFactoryLocal`. Skrypt do drukowania adres IP maszyny wirtualnej.

* W portalu Azure Znajdź wdrożenia grupy zasobów. Z wyjątkiem wdrożenia lokalnego grupa zasobów ma nazwę określone jako rozwiązanie lub wdrożenia. W przypadku wdrożenia lokalnego przy użyciu skryptu kompilacji Nazwa grupy zasobów jest `<your username>ConnFactoryLocal`. Teraz Dodaj nową **publicznego adresu IP** zasobów do grupy zasobów.

> [!NOTE]
> W obu przypadkach upewnij się, zainstaluj najnowsze poprawki zgodnie z instrukcjami [Ubuntu witryny sieci Web](https://wiki.ubuntu.com/Security/Upgrades). Aktualizowanie instalacji dla tak długo, jak długo maszyny Wirtualnej jest dostępna za pośrednictwem publicznego adresu IP.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Jak usunąć publicznego adresu IP na symulacyjnych maszyny Wirtualnej?

Dostępne są dwie opcje, aby usunąć adres IP:

* Użyj programu PowerShell skryptu Simulation/Factory/Remove-SimulationPublicIp.ps1 z [repozytorium](https://github.com/Azure/azure-iot-connected-factory). Podaj nazwę wdrożenia jako parametr. Dla wdrożenia lokalnego, należy użyć `<your username>ConnFactoryLocal`. Skrypt do drukowania adres IP maszyny wirtualnej.

* W portalu Azure Znajdź wdrożenia grupy zasobów. Z wyjątkiem wdrożenia lokalnego grupa zasobów ma nazwę określone jako rozwiązanie lub wdrożenia. W przypadku wdrożenia lokalnego przy użyciu skryptu kompilacji Nazwa grupy zasobów jest `<your username>ConnFactoryLocal`. Teraz usunąć **publicznego adresu IP** zasobu z grupy zasobów.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Jak zalogować się na symulacyjnych maszyny Wirtualnej?

Logowanie do symulacji maszyny Wirtualnej jest obsługiwana tylko jeśli wdrożono rozwiązanie przy użyciu skryptu środowiska PowerShell `build.ps1` w [repozytorium](https://github.com/Azure/azure-iot-connected-factory).

Jeśli wdrożono rozwiązanie z www.azureiotsuite.com nie logowania się do maszyny Wirtualnej. Nie można zalogować się, ponieważ hasło jest generowany losowo i nie można zresetować go.

1. Dodaj publiczny adres IP do maszyny Wirtualnej. Zobacz [jak dodać publicznego adresu IP na symulacyjnych maszyny Wirtualnej?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Tworzenie sesji SSH maszyny Wirtualnej przy użyciu adresu IP maszyny wirtualnej.
1. Nazwa użytkownika do użycia: `docker`.
1. Hasło do użycia zależy od wersji, który został użyty do wdrożenia:
    * Wdrożenie za pomocą skryptu build.ps1 przed 1 czerwca 2017 rozwiązań, jest hasło: `Passw0rd`.
    * Wdrożenie za pomocą skryptu build.ps1 po 1 czerwca 2017 rozwiązania, można znaleźć hasło w `<name of your deployment>.config.user` pliku. Hasło jest zapisywane w **VmAdminPassword** ustawienie. Hasło jest generowane losowo w czasie wdrażania chyba że zostanie określony za pomocą `build.ps1` skryptu parametru`-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Jak zatrzymać i uruchomić wszystkie procesy docker w symulacji maszyny Wirtualnej?

1. Zaloguj się na symulacyjnych maszyny Wirtualnej. Zobacz [jak zarejestrować się celu symulacji maszyny Wirtualnej?](#how-do-i-sign-in-to-the-simulation-vm)
1. Aby sprawdzić, kontenery, które są aktywne, uruchom: `docker ps`.
1. Aby zatrzymać wszystkie kontenery symulacji, uruchom: `./stopsimulation`.
1. Aby uruchomić wszystkie kontenery symulacji:
    * Eksportuj powłoki zmiennej o nazwie **IOTHUB_CONNECTIONSTRING**. Należy użyć wartości **IotHubOwnerConnectionString** w `<name of your deployment>.config.user` pliku. Na przykład:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Uruchom polecenie `./startsimulation`.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>Jak zaktualizować symulacji w maszynie Wirtualnej?

Jeśli wprowadzono zmiany do symulacji, możesz użyć skryptu programu PowerShell `build.ps1` w [repozytorium](https://github.com/Azure/azure-iot-connected-factory) przy użyciu `updatedimulation` polecenia. Ten skrypt tworzy wszystkie składniki symulacji, zatrzymuje symulacji w maszynie Wirtualnej przekazuje, instaluje i można je uruchomić.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Jak sprawdzić parametry połączenia używane przez Moje rozwiązanie Centrum IoT

Jeśli wdrożono rozwiązanie z `build.ps1` skryptu w [repozytorium](https://github.com/Azure/azure-iot-connected-factory), ciąg połączenia jest wartość **IotHubOwnerConnectionString** w `<name of your deployment>.config.user` pliku.

Można również znaleźć parametrów połączenia przy użyciu portalu Azure. W zasobie Centrum IoT w grupie zasobów wdrożenia zlokalizuj ustawień parametrów połączenia.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Centrum IoT urządzeń, które używa symulacji fabryka połączenia?

Symulacja samodzielnie rejestruje następujące urządzenia:

* proxy.Beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.Mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.Rio.corp.contoso
* proxy.Seattle.corp.contoso
* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.Seattle.corp.contoso

Przy użyciu [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) lub [explorer Centrum iothub](https://github.com/azure/iothub-explorer) narzędzia, można sprawdzić, które urządzenia są zarejestrowane w usłudze Centrum IoT używa rozwiązania. Aby korzystać z tych narzędzi, należy parametry połączenia dla Centrum IoT w danym wdrożeniu.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Jak uzyskać dane dzienników ze składników symulacji

Wszystkie składniki w symulacji rejestrowania informacji plikach dziennika. Te pliki znajdują się w maszynie Wirtualnej w folderze `home/docker/Logs`. Aby pobrać dzienniki, można użyć skryptu programu PowerShell `Simulation/Factory/Get-SimulationLogs.ps1` w [repozytorium](https://github.com/Azure/azure-iot-connected-factory).

Ten skrypt należy zalogować się do maszyny Wirtualnej. Może być konieczne podanie poświadczeń przy logowaniu. Zobacz [jak zarejestrować się celu symulacji maszyny Wirtualnej?](#how-do-i-sign-in-to-the-simulation-vm) można znaleźć poświadczenia.

Skrypt dodaje i usuwa publiczny adres IP do maszyny Wirtualnej, jeśli jeszcze nie ma jeden i usuwa go. Skrypt umieszcza wszystkie pliki dziennika w archiwum i pobierane pliki archiwum na deweloperskiej stacji roboczej.

Możesz też zalogować się do maszyny Wirtualnej za pośrednictwem SSH i sprawdzić pliki dziennika w czasie wykonywania.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Jak można sprawdzić, czy symulacji wysyła dane w chmurze?

Z [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) lub [explorer Centrum iothub](https://github.com/azure/iothub-explorer) narzędzia, możesz sprawdzić dane wysyłane do Centrum IoT z określonymi urządzeniami. Aby korzystać z tych narzędzi, musisz wiedzieć, ciąg połączenia dla Centrum IoT w danym wdrożeniu. Zobacz [jak sprawdzić parametry połączenia Centrum IoT używane przez Moje rozwiązanie?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Sprawdź dane wysyłane przez jednego z urządzeń wydawcy:

* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.Seattle.corp.contoso

Jeśli zostanie wyświetlone żadne dane wysyłane do Centrum IoT, oznacza to, że wystąpił problem z symulacji. Pierwszym krokiem analizy powinien analizowanie plików dziennika składników symulacji. Zobacz [jak można uzyskać danych dziennika z składniki symulacji?](#how-can-i-get-log-data-from-the-simulation-components) Spróbuj dalej zatrzymać i uruchomić symulacji i jeśli nie ma jeszcze żadnych danych wysyłane, zaktualizuj symulacji całkowicie. Zobacz [jak zaktualizować symulacji w maszynie Wirtualnej?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="next-steps"></a>Następne kroki

Możesz także wypróbować niektóre inne funkcje i możliwości wstępnie skonfigurowanych rozwiązań Pakietu IoT:

* [Omówienie rozwiązania konserwacji predykcyjnej wstępnie](iot-suite-predictive-overview.md)
* [Fabryka połączonych wstępnie Omówienie rozwiązania](iot-suite-connected-factory-overview.md)
* [Zabezpieczenia IoT od podstaw](securing-iot-ground-up.md)