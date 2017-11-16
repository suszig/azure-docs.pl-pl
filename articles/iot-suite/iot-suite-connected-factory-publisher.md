---
title: "Za pomocą narzędzia wydawca OPC fabryki pakiet IoT Azure połączone | Dokumentacja firmy Microsoft"
description: "Sposób tworzenia i wdrażania implementacji odwołanie wydawcy OPC połączonych fabryki."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: dobett
ms.openlocfilehash: 934f4deb3139c136c871ab0125ba45267e1d1b05
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="opc-publisher-for-azure-iot-edge"></a>Wydawca OPC Edge IoT Azure

W tym artykule opisano sposób użycia implementacji odwołanie wydawcy OPC. Implementacja odniesienia przedstawiono sposób użycia krawędzi IoT Azure:

- Łączenie z istniejącymi serwerami OPC UA.
- Publikowanie kodowany w formacie JSON dane telemetryczne z tych serwerów w OPC UA *Pub/Sub* sformatować przy użyciu ładunek JSON, aby Centrum IoT Azure. Można użyć dowolnego z obsługiwanych przez usługę Azure IoT krawędzi protokołów transportu.

Ta aplikacja odwołanie obejmuje:

- UA OPC *klienta* do łączenia z istniejącymi serwerami OPC UA w sieci.
- UA OPC *serwera* nasłuchuje na porcie 62222 używanej do zarządzania, co jest opublikowana.

Aplikacja jest implementowane przy użyciu platformy .NET Core i można uruchomić na platformach obsługiwanych przez oprogramowanie .NET Core.

Wydawcy implementację logiki ponawiania próby podczas nawiązywania połączenia z punktami końcowymi. Wydawca oczekuje punkty końcowe odpowiedzieć w ciągu określonej liczby keep alive żądania. Tę logikę ponawiania umożliwia wydawcy do wykrywania warunków, takich jak zasilaniu OPC UA serwera.

Dla każdego różne publikowania na serwerze OPC UA tworzy oddzielnej subskrypcji, w którym są aktualizowane wszystkie węzły z tym Interwał publikowania.

Aby zmniejszyć obciążenie sieci, wydawcy obsługuje przetwarzanie wsadowe dane wysyłane do Centrum IoT. Plik wsadowy są wysyłane do Centrum IoT tylko wtedy, gdy osiągnie rozmiar pakietu skonfigurowany.

Ta aplikacja używa stosu odwołanie OPC UA podstaw OPC i w związku z tym licencjonowania ograniczenia są stosowane. Odwiedź stronę http://opcfoundation.github.io/UA-.NETStandardLibrary/ OPC UA dokumentacją i umowy licencyjnej.

Możesz znaleźć kod źródłowy OPC wydawcy w [wydawcy OPC Azure IoT Edge](https://github.com/Azure/iot-edge-opc-publisher) repozytorium GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Aby utworzyć aplikację, należy [.NET Core SDK 1.1.](https://docs.microsoft.com/dotnet/core/sdk) w systemie operacyjnym.

## <a name="build-the-application"></a>Kompilowanie aplikacji

### <a name="as-native-windows-application"></a>Jako natywnych aplikacji systemu Windows

Otwórz `OpcPublisher.sln` projektu z programu Visual Studio 2017 i Skompiluj rozwiązanie za pomocą F7.

### <a name="as-docker-container"></a>Jako kontener Docker

Aby utworzyć aplikację jako kontener Docker systemu Windows, należy użyć `Dockerfile.Windows` pliku konfiguracji.

Aby utworzyć aplikację jako kontener Linux Docker, użyj `Dockerfile` pliku konfiguracji.

Z katalogu głównego repozytorium na komputerze deweloperskim wpisz następujące polecenie w oknie konsoli:

`docker build -f <docker-configfile-to-use> -t <your-container-name> .`

`-f` Opcja dla `docker build` jest opcjonalny i domyślnie używany jest `Dockerfile` pliku konfiguracji.

Docker umożliwia również tworzenie bezpośrednio z repozytorium git. Można utworzyć kontener Docker systemu Linux przy użyciu następującego polecenia:

`docker build -t <your-container-name> .https://github.com/Azure/iot-edge-opc-publisher`

## <a name="configure-the-opc-ua-nodes-to-publish"></a>Konfigurowanie węzłów OPC UA publikowania

Aby skonfigurować OPC UA węzły, które powinny mieć wartości publikowane Centrum IoT Azure, należy utworzyć plik konfiguracji sformatowany JSON. Nazwa domyślnego dla tego pliku konfiguracji jest `publishednodes.json`. Aplikacja aktualizacji i zapisuje ten plik konfiguracji podczas używania OPC UA metody serwera **PublishNode** lub **UnpublishNode**.

Składnia pliku konfiguracji jest następująca:

```json
[
    {
        // example for an EnpointUrl is: opc.tcp://win10iot:51210/UA/SampleServer
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "OpcNodes": [
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised publishing interval).
            {
                // The identifier specifies the NamespaceUri and the node identifier in XML notation as specified in Part 6 of the OPC UA specification in the XML Mapping section.
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258"
            },
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with an OPC publishing interval of 4 seconds.
            // Publisher will use for each dinstinct publishing interval (of nodes on the same EndpointUrl) a separate subscription. All nodes without a publishing interval setting,
            // will be on the same subscription and the OPC UA stack will publish with the lowest sampling interval of a node.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                "OpcPublishingInterval": 4000
            },
            // Publisher will request the server at EndpointUrl to sample the node with the given sampling interval of 1 second
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised interval).
            // If the OPC publishing interval is set to a lower value, Publisher will adjust the OPC publishing interval of the subscription to the OPC sampling interval value.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                // the OPC sampling interval to use for this node.
                "OpcSamplingInterval": 1000
            }
        ]
    },

    // the format below is only supported for backward compatibility. you need to ensure that the
    // OPC UA server on the configured EndpointUrl has the namespaceindex you expect with your configuration.
    // please use the ExpandedNodeId syntax instead.
    {
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "NodeId": {
            "Identifier": "ns=0;i=2258"
        }
    }
    // please consult the OPC UA specification for details on how OPC monitored node sampling interval and OPC subscription publishing interval settings are handled by the OPC UA stack.
    // the publishing interval of the data to Azure IoT Hub is controlled by the command line settings (or the default: publish data to IoT Hub at least each 1 second).
]
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

### <a name="command-line-options"></a>Opcje wiersza polecenia

Aby wyświetlić pełną użycia aplikacji, użyj `--help` opcji wiersza polecenia. W poniższym przykładzie przedstawiono struktura polecenia:

```cmd/sh
OpcPublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]
```

`applicationname`jest to nazwa aplikacji OPC UA do użycia. Ten parametr jest wymagany. Nazwa aplikacji służy również do rejestrowania wydawcy w rejestrze urządzeń Centrum IoT.

`IoT Hubconnectionstring`to Centrum IoT ciąg połączenia właściciela. Ten parametr jest opcjonalny.

Obsługiwane są następujące opcje:

```cmd/sh
--pf, --publishfile=VALUE
  the filename to configure the nodes to publish.
    Default: './publishednodes.json'
--sd, --shopfloordomain=VALUE
  the domain of the shopfloor. if specified this
    domain is appended (delimited by a ':' to the '
    ApplicationURI' property when telemetry is
    sent to IoT Hub.
    The value must follow the syntactical rules of a
    DNS hostname.
    Default: not set
--sw, --sessionconnectwait=VALUE
  specify the wait time in seconds publisher is
    trying to connect to disconnected endpoints and
    starts monitoring unmonitored items
    Min: 10
    Default: 10
--vc, --verboseconsole=VALUE
  the output of publisher is shown on the console.
    Default: False
--ih, --IoT Hubprotocol=VALUE
  the protocol to use for communication with Azure
    IoT Hub (allowed values: Amqp, Http1, Amqp_
    WebSocket_Only, Amqp_Tcp_Only, Mqtt, Mqtt_
    WebSocket_Only, Mqtt_Tcp_Only).
    Default: Mqtt
--ms, --IoT Hubmessagesize=VALUE
  the max size of a message which can be send to
    IoT Hub. when telemetry of this size is available
    it will be sent.
    0 will enforce immediate send when telemetry is
    available
    Min: 0
    Max: 256 * 1024
    Default: 4096
--si, --IoT Hubsendinterval=VALUE
  the interval in seconds when telemetry should be
    send to IoT Hub. If 0, then only the
    IoT Hubmessagesize parameter controls when
    telemetry is sent.
    Default: '1'
--lf, --logfile=VALUE
  the filename of the logfile to use.
    Default: './Logs/<applicationname>.log.txt'
--pn, --portnum=VALUE
  the server port of the publisher OPC server
    endpoint.
    Default: 62222
--pa, --path=VALUE
  the endpoint URL path part of the publisher OPC
    server endpoint.
    Default: '/UA/Publisher'
--lr, --ldsreginterval=VALUE
  the LDS(-ME) registration interval in ms. If 0,
    then the registration is disabled.
    Default: 0
--ot, --operationtimeout=VALUE
  the operation timeout of the publisher OPC UA
    client in ms.
    Default: 120000
--oi, --opcsamplinginterval=VALUE
  the publisher is using this as default value in
    milliseconds to request the servers to sample
    the nodes with this interval
    this value might be revised by the OPC UA
    servers to a supported sampling interval.
    please check the OPC UA specification for
    details how this is handled by the OPC UA stack.
    a negative value will set the sampling interval
    to the publishing interval of the subscription
    this node is on.
    0 will configure the OPC UA server to sample in
    the highest possible resolution and should be
    taken with care.
    Default: 1000
--op, --opcpublishinginterval=VALUE
  the publisher is using this as default value in
    milliseconds for the publishing interval setting
    of the subscriptions established to the OPC UA
    servers.
    please check the OPC UA specification for
    details how this is handled by the OPC UA stack.
    a value less than or equal zero will let the
    server revise the publishing interval.
    Default: 0
--ct, --createsessiontimeout=VALUE
  specify the timeout in seconds used when creating
    a session to an endpoint. On unsuccessful
    connection attemps a backoff up to 5 times the
    specified timeout value is used.
    Min: 1
    Default: 10
--ki, --keepaliveinterval=VALUE
  specify the interval in seconds the publisher is
    sending keep alive messages to the OPC servers
    on the endpoints it is connected to.
    Min: 2
    Default: 2
--kt, --keepalivethreshold=VALUE
  specify the number of keep alive packets a server
    can miss, before the session is disconneced
    Min: 1
    Default: 5
--st, --opcstacktracemask=VALUE
  the trace mask for the OPC stack. See github OPC .
    NET stack for definitions.
    To enable IoT Hub telemetry tracing set it to 711.
    Default: 285  (645)
--as, --autotrustservercerts=VALUE
  the publisher trusts all servers it is
    establishing a connection to.
    Default: False
--tm, --trustmyself=VALUE
  the publisher certificate is put into the trusted
    certificate store automatically.
    Default: True
--fd, --fetchdisplayname=VALUE
  enable to read the display name of a published
    node from the server. this will increase the
    runtime.
    Default: False
--at, --appcertstoretype=VALUE
  the own application cert store type.
    (allowed values: Directory, X509Store)
    Default: 'X509Store'
--ap, --appcertstorepath=VALUE
  the path where the own application cert should be
    stored
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/own'
--tt, --trustedcertstoretype=VALUE
  the trusted cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--tp, --trustedcertstorepath=VALUE
  the path of the trusted cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/trusted'
--rt, --rejectedcertstoretype=VALUE
  the rejected cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--rp, --rejectedcertstorepath=VALUE
  the path of the rejected cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/rejected'
--it, --issuercertstoretype=VALUE
  the trusted issuer cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--ip, --issuercertstorepath=VALUE
  the path of the trusted issuer cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/issuers'
--dt, --devicecertstoretype=VALUE
  the IoT Hub device cert store type.
    (allowed values: Directory, X509Store)
    Default: X509Store
--dp, --devicecertstorepath=VALUE
  the path of the iot device cert store
    Default Default (depends on store type):
    X509Store: 'My'
    Directory: 'CertificateStores/IoT Hub'
-h, --help
  show this message and exit
```

Następujące zmienne środowiskowe służy do sterowania aplikacji:
- `_HUB_CS`: Ustawia parametry połączenia Centrum IoT właściciela
- `_GW_LOGP`: Określa nazwę pliku dziennika do użycia
- `_TPC_SP`: Ustawia ścieżkę do magazynu certyfikatów zaufanych stacji
- `_GW_PNFP`: Określa nazwę pliku konfiguracji publikowania

Argumenty wiersza polecenia zastępuje ustawień zmiennych środowiskowych.

Zwykle Określ ciąg połączenia Centrum IoT właściciela tylko przy pierwszym uruchomieniu aplikacji. Parametry połączenia są szyfrowane i przechowywane w magazynie certyfikatów platformy.

W kolejnych wywołaniach parametry połączenia są odczytu z magazynu certyfikatów platformy i użyć ponownie. Jeśli określisz parametry połączenia przy każdym uruchomieniu urządzenia w rejestrze urządzeń Centrum IoT jest usunięta i utworzona ponownie zawsze.

### <a name="native-on-windows"></a>Natywne w systemie Windows

Aby uruchomić aplikację w sposób macierzysty w systemie Windows, należy otworzyć `OpcPublisher.sln` projektu z programu Visual Studio 2017, Skompiluj rozwiązanie i opublikować go. Aplikację można uruchomić w **katalog docelowy** został opublikowany z:

```cmd
dotnet OpcPublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-self-built-container"></a>Użyj własnym wbudowanych kontenera

Aby uruchomić aplikację w kontenerze własnym wbudowanych, kompilacji, a następnie uruchom własne kontenera:

```cmd/sh
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-container-from-hubdockercom"></a>Użyj kontener z hub.docker.com

Brak dostępnej wbudowane kontenera na DockerHub. Aby uruchomić go, uruchom następujące polecenie:

```cmd/sh
docker run microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="important-when-using-a-container"></a>Ważne, korzystając z kontenera

#### <a name="access-to-the-publisher-opc-ua-server"></a>Dostęp do serwera wydawcy UA OPC

Serwer UA OPC wydawcy domyślnie nasłuchuje na porcie 62222. Aby aktywować tego portu wejściowego w kontenerze, należy użyć `docker run` opcji `-p`:

```cmd/sh
docker run -p 62222:62222 microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="enable-intercontainer-name-resolution"></a>Włączyć rozpoznawanie nazw intercontainer

Aby włączyć rozpoznawanie nazw z kontenera do innych kontenerów, należy:

- Utwórz sieć Mostek docker zdefiniowane przez użytkownika.
- Połącz kontenera do sieci przy użyciu `--network`opcji.
- Przypisz kontenera przy użyciu nazwy `--name` opcji.

W poniższym przykładzie przedstawiono te opcje konfiguracji:

```cmd/sh
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Teraz można osiągnąć kontenera przez inne kontenery za pośrednictwem sieci przy użyciu nazwy `publisher`.

#### <a name="assign-a-hostname"></a>Przypisanie nazwy hosta

Wydawca używa nazwy hosta komputerze, na którym jest uruchomiona w generowania certyfikatu i punktu końcowego. Docker wybierze losową nazwę hosta, chyba że zostanie ustawiony z `-h` opcji. Tutaj przykład wewnętrznej nazwy hosta kontenera, aby ustawić `publisher`:

```cmd/sh
docker run -h publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="using-bind-mounts-shared-filesystem"></a>Za pomocą powiązania instaluje (udostępnionych systemu plików)

W niektórych scenariuszach, dla których chcesz odczytać informacji o konfiguracji z lub zapisania plików dziennika do lokalizacji na hoście zamiast kontenera systemu plików. Aby skonfigurować to zachowanie, użyj `-v` opcji `docker run` w trybie instalacji powiązania. Na przykład:

```cmd/sh
-v //D/docker:/build/out/Logs
-v //D/docker:/build/out/CertificateStores
-v //D/docker:/shared
-v //D/docker:/root/.dotnet/corefx/cryptography/x509stores
```

#### <a name="store-for-x509-certificates"></a>Magazyn dla X509 certyfikatów

Przechowywanie X509 certyfikaty nie działa z instalacji powiązania, ponieważ uprawnienia ścieżkę do magazynu musi być `rw` właściciela. Zamiast tego należy użyć `-v` opcji `docker run` w trybie woluminu.

## <a name="debug-the-application"></a>Debugowanie aplikacji

### <a name="native-on-windows"></a>Natywne w systemie Windows

Otwórz `OpcPublisher.sln` projektu z programu Visual Studio 2017 r i rozpocząć debugowanie aplikacji przez naciskać klawisz F5.

### <a name="in-a-docker-container"></a>W kontenerze docker

Visual Studio 2017 obsługuje debugowanie aplikacji w kontenerze Docker przy użyciu `docker-compose`. Jednak ta metoda nie pozwalają na przekazywanie parametrów wiersza polecenia.

Zamiast opcji, która obsługuje VS2017 debugowania jest do debugowania za pośrednictwem `ssh`. Możesz użyć pliku konfiguracji kompilacji docker `Dockerfile.ssh` w folderze głównym repozytorium, aby utworzyć kontener włączone SSH:

```cmd/sh
docker build -f .\Dockerfile.ssh -t publisherssh .
```

Można teraz uruchomić kontenera do debugowania wydawcy:

```cmd/sh
docker run -it publisherssh
```

W kontenerze, należy uruchomić **ssh** demon ręcznie:

```cmd/sh
service ssh start
```

W tym momencie można utworzyć **ssh** sesji jako użytkownik `root` hasłem `Passw0rd`.

Aby przygotować się do debugowania aplikacji w kontenerze, należy wykonać następujące dodatkowe czynności:

1. Na stronie hosta, Utwórz `launch.json` pliku:

    ```json
    {
      "version": "0.2.0",
      "adapter": "<path>\\plink.exe",
      "adapterArgs": "root@localhost -pw Passw0rd -batch -T ~/vsdbg/vsdbg --interpreter=vscode",
      "languageMappings": {
        "C#": {
          "languageId": "3F5162F8-07C6-11D3-9053-00C04FA302A1",
          "extensions": [ "*" ]
        }
      },
      "exceptionCategoryMappings": {
        "CLR": "449EC4CC-30D2-4032-9256-EE18EB41B62B",
        "MDA": "6ECE07A9-0EDE-45C4-8296-818D8FC401D4"
      },
      "configurations": [
        {
          "name": ".NET Core Launch",
          "type": "coreclr",
          "cwd": "~/publisher",
          "program": "Opc.Ua.Publisher.dll",
          "args": "<put-the-publisher-command-line-options-here>",

          "request": "launch"
        }
      ]
    }
    ```

1. Skompilowanie projektu i opublikujesz go na katalog wybranych przez użytkownika.

1. Za pomocą narzędzia, takie jak `WinSCP` do skopiowania plików publikowanych w katalogu `/root/publisher` w kontenerze. Jeśli chcesz użyć innego katalogu, należy zaktualizować `cdw` właściwości w `launch.json` pliku.

Teraz możesz rozpocząć debugowanie przy użyciu następującego polecenia w oknie polecenia programu Visual Studio:

```cmd
DebugAdapterHost.Launch /LaunchJson:"<path-to-the-launch.json-file-you-saved>"
```

## <a name="next-steps"></a>Następne kroki

A sugerowane, następnym krokiem jest, aby dowiedzieć się, jak [wdrożyć bramę Windows lub Linux dla połączonych fabryki wstępnie skonfigurowane rozwiązanie](iot-suite-connected-factory-gateway-deployment.md).