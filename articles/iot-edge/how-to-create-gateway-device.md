---
title: "Utwórz urządzenie bramy z krawędzią IoT Azure | Dokumentacja firmy Microsoft"
description: "Użyj Azure IoT Edge, aby utworzyć urządzenie bramy, które może przetworzyć informacji dla wielu urządzeń"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: c9f71a7e95ea8c1b2cbd9b74ef20f9b0342d00f8
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="create-an-iot-edge-gateway-device-to-process-data-from-other-iot-devices---preview"></a>Tworzenie urządzenia bramy IoT do przetwarzania danych przez inne urządzenia IoT — w wersji preview

Istnieją dwa sposoby używania urządzenia IoT krawędź jako bramy:

* Łączenie podrzędnego urządzeń, które używają [urządzenia Azure IoT SDK][lnk-devicesdk];
* Podłączania urządzeń, które nie korzystają z protokołu IoT Hub.

Gdy używasz urządzenia IoT jako bramy można uzyskać następujące korzyści:

* **Multipleksowanie połączenia**. Wszystkie urządzenia łączących się z Centrum IoT za pośrednictwem urządzenia IoT użyje tego samego podstawowego połączenia.
* **Wygładzanie ruchu**. Urządzenie brzegowe IoT automatycznie wykona wykładniczego wycofywania w przypadku ograniczania przepustowości podczas utrwalanie komunikaty lokalnie Centrum IoT. Spowoduje to rozwiązania odporne na największego ruchu.
* **Ograniczona obsługa w trybie offline**. Urządzenie bramy, które będą przechowywane lokalnie wiadomości, których nie można dostarczyć do Centrum IoT.

W tym artykule będzie nazywamy *brama brzegowa IoT* urządzenia IoT używany jako brama.

>[!NOTE]
>Obecnie:
> * Podrzędne urządzenia nie będą mogły uwierzytelniać się w bramie brama jest odłączona od centrum IoT; i
> * Urządzenia brzegowe IoT nie może połączyć się bram granicznych IoT.

## <a name="use-the-azure-iot-device-sdk"></a>Użyj zestawu SDK urządzenia Azure IoT

Koncentratora krawędzi, który jest zainstalowany na wszystkich urządzeniach krawędzi IoT udostępnia następujące podstawowych urządzeniom podrzędne:

* wiadomości urządzenia do chmury i chmury do urządzenia.
* bezpośrednie metod; i
* operacje dwie urządzenia.

>[!NOTE]
>Obecnie podrzędne urządzenia nie będą mogły używać przekazywania pliku, podczas łączenia za pośrednictwem bramy IoT krawędzi.

Po podłączeniu urządzenia do bramy usługi IoT krawędzi przy użyciu zestawu SDK urządzenia Azure IoT należy:

* Instalowanie podrzędne urządzenia przy użyciu parametrów połączenia, odwołujący się do nazwy hosta urządzenia bramy; i
* Upewnij się, że urządzenie podrzędne ufa certyfikat używany do akceptowania połączeń przez urządzenie bramy.

Po zainstalowaniu środowisko uruchomieniowe krawędzi IoT Azure przy użyciu skryptu kontroli utworzyć certyfikatu dla Centrum krawędzi, tak jak w programie IoT Edge zainstalować samouczka na symulowane urządzenie na [Windows] [ lnk-tutorial1-win] i [Linux][lnk-tutorial1-lin]. Ten certyfikat jest używany przez Centrum krawędzi do akceptowania przychodzących połączeń TLS, a musi być uważany za zaufany przez urządzenia podrzędne podczas nawiązywania połączenia z urządzeniem bramy.

Można utworzyć dowolnej infrastruktury certyfikatów, umożliwiającą zaufania wymagane dla topologii urządzenia bramy. W tym artykule przyjęto założenie, tę samą konfigurację certyfikatu, który ma zostać użyty do włączenia [zabezpieczeń urzędu certyfikacji X.509] [ lnk-iothub-x509] w Centrum IoT, który wymaga certyfikatu X.509 urzędu certyfikacji skojarzonego z określonym Centrum IoT (  *Właściciel Centrum IoT urzędu certyfikacji*) oraz serii certyfikatów, podpisanych tego urzędu certyfikacji, zainstalowane urządzenia IoT krawędzi.

>[!IMPORTANT]
>Obecnie krawędzi IoT i podrzędne urządzenia można używać tylko [tokeny sygnatury dostępu Współdzielonego] [ lnk-iothub-tokens] do uwierzytelniania za pomocą Centrum IoT. Certyfikaty będą używane tylko do zweryfikowania połączenia TLS między urządzeniem liścia i bramy.

Używa naszego konfiguracji **właściciela Centrum IoT urzędu certyfikacji** jako:
* certyfikat podpisywania dla ustawień środowiska uruchomieniowego krawędzi IoT na wszystkich urządzeniach krawędzi IoT i jako
* Certyfikat klucza publicznego zainstalowany na urządzeniach podrzędne.

Spowoduje to rozwiązanie umożliwiający wszystkich urządzeniach można używać dowolnego urządzenia IoT krawędź jako bramy, pod warunkiem, że są one połączone z tym samym Centrum IoT.

### <a name="create-the-certificates-for-test-scenarios"></a>Tworzenie certyfikatów dla scenariuszy testowania

Możesz użyć przykładu środowiska Powershell i skrypty powłoki systemowej opisanych w [Zarządzanie przykładowy certyfikat urzędu certyfikacji] [ lnk-ca-scripts] do wygenerowania z podpisem własnym **właściciela Centrum IoT urzędu certyfikacji** i certyfikaty urządzeń podpisany z nim.

1. Wykonaj krok 1 z [Zarządzanie przykładowy certyfikat urzędu certyfikacji] [ lnk-ca-scripts] zainstalować skrypty. Upewnij się, że sklonowana `modules-preview` gałęzi:
                
                git clone -b modules-preview https://github.com/Azure/azure-iot-sdk-c.git 
2. Wykonaj kroki od 2 do generowania **właściciela Centrum IoT urzędu certyfikacji**, ten plik będzie używany przez urządzenia podrzędne można zweryfikować połączenia.

Użyj poniższych instrukcji, aby wygenerować certyfikat dla urządzenia bramy.

#### <a name="bash"></a>Bash

* Uruchom `./certGen.sh create_edge_device_certificate myGateway` można utworzyć nowego certyfikatu z urządzenia.  
  Spowoduje to utworzenie.\certs\new-edge-device.* pliki zawierające klucz publiczny i PFX oraz.\private\new-edge-device.key.pem zawierający klucz prywatny urządzenia.  
* W `certs` katalogu Uruchom `cat ./new-edge-device.cert.pem ./azure-iot-test-only.intermediate.cert.pem ./azure-iot-test-only.root.ca.cert.pem > ./new-edge-device-full-chain.cert.pem` uzyskanie całego łańcucha klucza publicznego urządzenia.
* `./private/new-edge-device.cert.pem`zawiera klucz prywatny urządzenia.

#### <a name="powershell"></a>PowerShell

* Uruchom `New-CACertsEdgeDevice myGateway` można utworzyć nowego certyfikatu z urządzenia.
  Spowoduje to utworzenie plików myEdgeDevice * zawierające klucz publiczny, klucz prywatny i PFX tego certyfikatu.  Po wyświetleniu monitu o wprowadzenie hasła podczas podpisywania, wprowadź "1234".


>[!IMPORTANT]
>Ten przykład jest przeznaczony tylko do celów testowych. Scenariusze produkcji, można znaleźć w temacie [zabezpieczania wdrożenia IoT] [ lnk-iothub-secure-deployment] dla Azure IoT wytyczne dotyczące bezpiecznego rozwiązania IoT i odpowiednio obsługi administracyjnej certyfikatu.

### <a name="configure-an-iot-edge-device-as-a-gateway"></a>Konfigurowanie urządzenia IoT jako bramy

Aby skonfigurować urządzenia IoT krawędź jako bramy wystarczy skonfigurować do używania certyfikatu urządzenia utworzony w poprzedniej sekcji.

Przyjęto założenie, następujące nazwy plików z powyższych przykładowe skrypty:

| Dane wyjściowe | Skrypt bash | PowerShell |
| ------ | ----------- | ---------- |
| Certyfikat urządzenia | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| Klucza prywatnego na urządzeniu | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Łańcuch certyfikatów urządzenia | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| Właściciel Centrum IoT urzędu certyfikacji | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

 Analogously do instalacji opisane w wdrażanie Azure IoT Edge na symulowane urządzenie w [Windows] [ lnk-tutorial1-win] lub [Linux][lnk-tutorial1-lin], trzeba Podaj powyższych informacji do środowiska wykonawczego IoT krawędzi. 
 
 W systemie Linux:

        sudo iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem

W systemie Windows:

        iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/RootCA.pem

Domyślnie przykładowe skrypty nie ustawiono hasło klucza prywatnego na urządzeniu. Jeśli ustawisz hasło, Dodaj następujący parametr:

        --device-ca-passphrase {passphrase}

Skrypt wyświetli monit o można ustawić hasło dla certyfikatu agenta krawędzi.
Należy ponownie środowisko uruchomieniowe krawędzi IoT po wykonaniu tego polecenia.

### <a name="configure-an-iot-hub-device-application-as-a-downstream-device"></a>Skonfigurować aplikację Centrum IoT urządzenia jako urządzenia podrzędne

Podrzędne urządzenie może być dowolną aplikację przy użyciu [urządzenia Azure IoT SDK][lnk-devicesdk], takie jak opisano to proste w [Podłącz urządzenie do Centrum IoT przy użyciu platformy .NET] [ lnk-iothub-getstarted].

Najpierw aplikacji podrzędne urządzenie ma ufać **właściciela Centrum IoT urzędu certyfikacji** certyfikatu w celu weryfikowania połączeń TLS do urządzenia bramy. Ten krok, zwykle można przeprowadzić na dwa sposoby: na poziomie systemu operacyjnego lub (w przypadku niektórych języków) na poziomie aplikacji.

Na przykład aplikacji .NET można dodać następujący fragment kodu zaufania certyfikatu w formacie PEM składowanych w ścieżce `certPath`. Jeśli za pomocą skryptu powyżej ścieżki będą się odwoływać `certs/azure-iot-test-only.root.ca.cert.pem` (Bash) lub `RootCA.pem` (Powershell).

        using System.Security.Cryptography.X509Certificates;
        
        ...

        X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
        store.Open(OpenFlags.ReadWrite);
        store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
        store.Close();

Wykonanie tego kroku na poziomie systemu operacyjnego różni się od systemu Windows i między dystrybucje systemu Linux.

Drugim krokiem jest zainicjować sdk urządzenia IoT Hub parametrami połączenia odwołujących się do nazwy hosta z urządzeniem bramy.
Jest to realizowane przez dodanie `GatewayHostName` właściwości parametrów połączenia urządzenia. Na przykład poniżej przedstawiono przykładowe parametry połączenia urządzenia dla urządzeń, do którego możemy dołączany `GatewayHostName` właściwości:

        HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com

Następujące dwa kroki spowoduje włączenie aplikacji urządzenia do nawiązania połączenia z urządzeniem bramy.

## <a name="use-other-protocols"></a>Użyj innych protokołów

Jedną z głównych funkcji urządzenie bramy jest interpretować protokołów podrzędnych urządzeń. Do podłączania urządzeń, które nie korzystają z protokołu IoT Hub, możesz utworzyć moduł IoT krawędzi, która wykonuje tłumaczenie i łączy w imieniu podrzędne urządzenia IoT Hub.

Można zdecydować utworzyć *przezroczysty* lub *nieprzezroczyste* bramy:

| &nbsp; | Przezroczysty bramy | Brama nieprzezroczyste|
|--------|-------------|--------|
| Tożsamości, które są przechowywane w rejestrze tożsamości Centrum IoT | Tożsamości wszystkich połączonych urządzeń | Urządzenie bramy z tożsamością |
| Bliźniak urządzenia | Każdy podłączonego urządzenia ma własną dwie urządzenia | Tylko bramy ma twins modułu i urządzenia |
| Bezpośrednie metod i komunikaty chmury do urządzenia | Chmury mogą indywidualnie adresów każdego podłączonego urządzenia | Chmury można usunąć tylko z urządzeniem bramy |
| [Limity Centrum IoT i przydziały][lnk-iothub-throttles-quotas] | Zastosuj do każdego urządzenia | Zastosuj z urządzeniem bramy |

Gdy za pomocą wzorca nieprzezroczyste bramy wszystkich urządzeń łączących się za pośrednictwem tej bramy współużytkują tę samą kolejkę chmury do urządzenia, który może zawierać maksymalnie 50 wiadomości. Wynika, że wzorzec nieprzezroczyste bramy należy używać, tylko wtedy, gdy tylko kilka urządzeń jest nawiązywane za pośrednictwem bramy każdego pola i ich ruch z chmury do urządzenia jest niska.

Podczas implementowania bramę nieprzezroczyste, modułu translację protokołu używa modułu Parametry połączenia.

Podczas implementowania przezroczysty bramy, moduł tworzy wiele wystąpień klienta urządzenia IoT Hub, przy użyciu parametrów połączenia dla podrzędnego urządzeń.

[Azure IoT krawędzi Modbus modułu] [ lnk-modbus-module] jest otwarta implementacja modułu protokołu karty nieprzezroczyste bramy.

## <a name="next-steps"></a>Następne kroki

- [Zrozumieć wymagania i narzędzi do tworzenia modułów krawędzi IoT][lnk-module-dev].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens 
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/modules-preview/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus