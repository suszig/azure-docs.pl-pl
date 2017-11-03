---
title: "Sensor tag urządzenia & brama IoT Azure — Rozwiązywanie problemów z | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z strony Intel NUC bramy"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "problemy z iot, internet rzeczy problemów"
ROBOTS: NOINDEX
ms.assetid: 5f742c38-0e33-465a-9a0d-1e41e8d17187
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7e80951de55ade6b5140608dcff8ebb064f942ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting"></a>Rozwiązywanie problemów

## <a name="hardware-issues"></a>Problemy ze sprzętem

### <a name="ti-sensortag-cannot-be-connected"></a>Nie można połączyć analizy czasowej Sensor tag

Aby rozwiązać problemy z łącznością Sensor tag, użyj [aplikacji Sensor tag](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Występuje problem z Intel NUC

Do rozwiązywania problemów z rozruchem, zapoznaj się [problemów rozruchu nie na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).

Aby rozwiązać problemy z systemem operacyjnym, zobacz [problemów systemu operacyjnego na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).

Aby rozwiązać inne problemy, zapoznaj się [Blink i sygnalizuje kodów dla Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).

## <a name="nodejs-package-issues"></a>Problemy z pakietu node.js

### <a name="no-response-during-gulp-tasks"></a>Brak odpowiedzi podczas gulp zadań

Jeśli wystąpią problemy z uruchomionych zadań gulp, można dodać `--verbose` opcji do debugowania. Spróbuj zakończenie bieżącego zadania gulp przy użyciu `Ctrl + C`, a następnie uruchom następujące polecenie w oknie konsoli, aby wyświetlić komunikaty debugowania. Szczegółowe komunikaty o błędach mogą zobaczyć w danych wyjściowych konsoli.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Problemy dotyczące odnajdywania urządzenia

Aby uzyskać pomoc w rozwiązywaniu typowych problemów z `discover-sensortag` polecenia, wyboru [strona typu wiki](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>problemy z npm

Spróbuj zaktualizować pakiet npm, uruchamiając następujące polecenie:

```bash
npm install -g npm
```

Jeśli problem nadal występuje, zostaw komentarz na końcu tego artykułu lub utworzyć problem GitHub w naszym [repozytorium przykładowej](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).

## <a name="remote-debugging"></a>Debugowanie zdalne
> Poniżej instrukcje są przeznaczone do debugowania skryptów node.js używanych w tym samouczku.
### <a name="run-the-sample-application-in-debug-mode"></a>Uruchom przykładową aplikację w trybie debugowania

Uruchom przykładową aplikację w trybie debugowania, uruchamiając następujące polecenie:

```bash
gulp run --debug
```

Gdy aparat debugowania jest gotowy, powinny pojawić się `Debugger listening on port 5858` w danych wyjściowych konsoli.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Konfigurowanie programu Visual Studio Code do nawiązania połączenia z urządzeniem zdalnym

1. Otwórz **debugowania** panelu po lewej stronie.
2. Kliknij zielony **Rozpocznij debugowanie** przycisk (F5). Zostanie otwarty w programie Visual Studio Code `launch.json` pliku.
3. Aktualizacja `launch.json` pliku o następującej zawartości. Zastąp `[device hostname or IP address]` z rzeczywistego urządzenia IP adres lub nazwę hosta.

   ``` json
   {
     "version": "0.2.0",
     "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Konfiguracja zdalnego debugowania](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Dołączanie do zdalnej aplikacji

Kliknij zielony **Rozpocznij debugowanie** przycisk (F5) można rozpocząć debugowania.

Odczyt [JavaScript w kodzie VS](https://code.visualstudio.com/docs/languages/javascript#_debugging) Aby dowiedzieć się więcej na temat debugera.

![Debugowanie cz próbki](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Problemy z interfejsu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure (Azure CLI) jest kompilacji w wersji zapoznawczej. Aby wyszukiwać rozwiązania, można użyć [Przewodnik instalowania wersji zapoznawczej](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Jeśli wystąpią usterek za pomocą narzędzia plików [problem](https://github.com/Azure/azure-cli/issues) w **problemów** sekcji repozytorium GitHub.

Aby uzyskać pomoc w rozwiązywaniu typowych problemów, sprawdź [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

Jeśli spełniasz "Nie można odnaleźć wersji, który spełnia wymagania", uruchom następujące polecenie, aby uaktualnić narzędzie pip do najnowsza wersja.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Problemy z instalacją Python

### <a name="legacy-installation-issues-macos"></a>Problemy z instalacją starszej wersji (system macOS)

Podczas instalowania narzędzia pip, podczas instalowania starszej pakiety z zostanie zgłoszony błąd uprawnień **su** uprawnienia. Ta sytuacja występuje, ponieważ poprzednich instalacji języka Python za pomocą brew (macOS) nie został całkowicie odinstalowany. Niektóre narzędzia pip pakiety z poprzedniej instalacji zostały utworzone przez główny, co powoduje, że błąd uprawnień. Rozwiązanie, to usuń te pakiety zainstalowane przez główny. Aby zakończyć to zadanie, wykonaj następujące kroki:

1. Przejdź do strony `/usr/local/lib/python2.7/site-packages`
2. Lista pakietów utworzone przez główny:`ls -l | grep root`
3. Odinstaluj pakiety z kroku 2:`sudo rm -rf {package name}`
4. Zainstaluj ponownie Python.

## <a name="azure-iot-hub-issues"></a>Problemy z Centrum IoT Azure

Jeśli pomyślnie zostały udostępnione Centrum Azure IoT z wiersza polecenia platformy Azure i należy to narzędzie do zarządzania urządzeniami, łączących się z Centrum IoT, spróbuj następujących narzędzi.

### <a name="device-explorer"></a>Eksplorator urządzenia

[Eksplorator urządzenia](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) działa na komputerze lokalnym systemu Windows i łączy się z Centrum IoT na platformie Azure. Komunikuje się z następującymi [punkty końcowe Centrum IoT](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Zarządzanie tożsamościami urządzenia do obsługi administracyjnej urządzeń i zarządzanie nimi jest zarejestrowany w Centrum IoT.
- Otrzymywać urządzenia do chmury, więc można monitorować wiadomości wysyłane z urządzenia do Centrum IoT.
- Wyślij chmury do urządzenia, więc wiadomości można wysyłać do urządzenia z Centrum IoT.

Konfigurowanie parametrów połączenia Centrum IoT w to narzędzie, aby korzystać z jego możliwości.

### <a name="iothub-explorer"></a>Eksplorator Centrum iothub

[Centrum iothub explorer](https://github.com/Azure/iothub-explorer) to przykładowe narzędzie interfejsu wiersza polecenia dla wielu platform do zarządzania klientami urządzeń. Można użyć narzędzia do zarządzania urządzeniami w rejestrze tożsamości, monitorować urządzenia do chmury wiadomości i wysyłanie poleceń z chmury do urządzenia.

Aby zainstalować najnowszą wersję (wstępna) narzędzia Centrum iothub explorer, uruchom następujące polecenie:

```bash
npm install -g iothub-explorer@latest
```

Aby uzyskać dodatkową pomoc dotyczącą poleceń Centrum iothub explorer i ich parametrów, uruchom następujące polecenie:

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a>Portalu Azure

Pełne środowisko CLI ułatwia tworzenie i zarządzanie nimi wszystkich zasobów na platformie Azure. Można także użyć [portalu Azure](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) świadczenia pomocy, zarządzanie i debugowania zasobów platformy Azure.

## <a name="azure-storage-issues"></a>Problemy z usługą Azure Storage

[Eksploratora usługi Microsoft Azure Storage (wersja zapoznawcza)](http://storageexplorer.com/) jest aplikacją autonomiczną firmy Microsoft, który służy do pracy z danymi usługi Azure Storage w systemie Windows, macOS i Linux. Za pomocą tego narzędzia, możesz nawiązać połączenia z tabeli i wyświetlać dane w nim. To narzędzie służy do rozwiązywania problemów związanych z usługą Azure Storage.
