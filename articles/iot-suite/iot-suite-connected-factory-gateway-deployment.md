---
title: "Wdrażanie bramy sieci połączonych fabryki - Azure | Dokumentacja firmy Microsoft"
description: "Jak wdrożyć bramę Windows lub Linux, aby umożliwić łączność z połączonych fabryki wstępnie skonfigurowane rozwiązanie."
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
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: e99a7bc34ac5ed060100e5f5032513bf4b18b2eb
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="deploy-a-gateway-on-windows-or-linux-for-the-connected-factory-preconfigured-solution"></a>Wdrożyć bramę Windows lub Linux dla połączonych fabryki wstępnie skonfigurowane rozwiązanie

Oprogramowanie wymagane do wdrożenia bramy dla połączonych fabryki wstępnie skonfigurowane rozwiązanie zawiera dwa składniki:

* *OPC Proxy* nawiąże połączenie z Centrum IoT. *OPC Proxy* następnie czeka na polecenia i kontroli wiadomości z zintegrowane przeglądarki OPC, która jest uruchamiana w portalu rozwiązania połączonych fabryki.
* *Wydawcy OPC* łączy się z istniejącymi lokalnymi serwerami OPC UA i przekazuje komunikaty dane telemetryczne z nich do Centrum IoT.

Oba te składniki są open source i są dostępne jako źródło w serwisie GitHub i jak kontenery Docker:

| GitHub | DockerHub |
| ------ | --------- |
| [Wydawca OPC][lnk-publisher-github] | [Wydawca OPC][lnk-publisher-docker] |
| [Serwer Proxy OPC][lnk-proxy-github] | [Serwer Proxy OPC][lnk-proxy-docker] |

Nie publicznych adresów IP, lub luk w zaporze bramy są wymagane dla obu składnika. OPC serwera Proxy i wydawcy OPC należy używać tylko ruchu wychodzącego porty 443, 5671 i 8883.

Kroki opisane w tym artykule opisano, jak wdrożyć bramę przy użyciu rozwiązania Docker na albo [Windows](#windows-deployment) lub [Linux](#linux-deployment). Brama umożliwia łączność z połączonych fabryki wstępnie skonfigurowane rozwiązanie.

> [!NOTE]
> Oprogramowanie bramy działającą w kontenerze Docker jest [Azure IoT krawędzi].

## <a name="windows-deployment"></a>Wdrażanie systemu Windows

> [!NOTE]
> Jeśli nie masz jeszcze urządzenie bramy, firma Microsoft zaleca się, że kupić komercyjnych bramy w jednym z naszych partnerów. Odwiedź stronę [katalogu urządzenia Azure IoT] lista bramy urządzeń zgodnych z rozwiązania połączonych fabryki. Postępuj zgodnie z instrukcjami, które pochodzą z urządzeniem, aby skonfigurować bramę. Alternatywnie użyj poniższych instrukcji, aby ręcznie skonfigurować jedną z istniejących bram.

### <a name="install-docker"></a>Zainstaluj Docker

Zainstaluj [Docker dla systemu Windows] na urządzenie bramy z systemem Windows. Podczas instalacji systemu Windows Docker należy wybrać dysk na komputerze hosta, aby udostępnić Docker. Poniższy zrzut ekranu przedstawia udostępniania dysku D w systemie Windows:

![Zainstaluj Docker][img-install-docker]

Następnie utwórz folder o nazwie **docker** w folderze głównym dysku udostępnionego.
Ten krok można również wykonać po zainstalowaniu docker z **ustawienia** menu.

### <a name="configure-the-gateway"></a>Konfigurowanie bramy

1. Należy **iothubowner** parametry połączenia pakietu IoT Azure połączone fabryki wdrożenia do ukończenia wdrożenia bramy. W [portalu Azure], przejdź do Centrum IoT w grupie zasobów utworzone podczas wdrażania rozwiązania fabryka połączenia. Kliknij przycisk **zasady dostępu współużytkowanego** dostępu **iothubowner** ciąg połączenia:

    ![Znajdź parametry połączenia Centrum IoT][img-hub-connection]

    Kopiuj **parametry połączenia — klucz podstawowy** wartość.

1. Konfigurowanie bramy Centrum IoT, uruchamiając modułów dwa bramy **po** z wiersza polecenia, używając:

    `docker run -it --rm -h <ApplicationName> -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  to nazwa, która ma zostać przypisany do użytkownika wydawcy UA OPC w formacie **wydawcy.&lt; nazwę FQDN&gt;**. Na przykład, jeśli jest nazywany siecią fabryki **myfactorynetwork.com**, **ApplicationName** wartość jest **publisher.myfactorynetwork.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  jest **iothubowner** ciąg połączenia został skopiowany w poprzednim kroku. Ten ciąg połączenia jest używana tylko w tym kroku, nie będzie potrzebny w poniższych krokach:

    Użyj zamapowanych D:\\docker folder ( `-v` argument) później, aby utrwalić dwóch certyfikatów X.509, korzystających z modułów bramy.

### <a name="run-the-gateway"></a>Uruchom bramę

1. Uruchom ponownie bramę przy użyciu następujących poleceń:

    `docker run -it --rm -h <ApplicationName> --expose 62222 -p 62222:62222 -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/shared -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Ze względów bezpieczeństwa dwóch certyfikatów X.509 utrwalone w D:\\docker folder zawiera klucz prywatny. Ograniczanie dostępu do tego folderu do poświadczeń (zazwyczaj **Administratorzy**) używane do uruchamiania w kontenerze Docker. Kliknij prawym przyciskiem myszy D:\\docker folderu, wybierz **właściwości**, następnie **zabezpieczeń**, a następnie **Edytuj**. Nadaj **Administratorzy** Pełna kontrola i usunąć inne osoby:

    ![Udzielanie uprawnień do udziału Docker][img-docker-share]

1. Sprawdź łączność sieciową. W wierszu polecenia wpisz polecenie `ping publisher.<your fully qualified domain name>` na polecenie ping bramy. Jeśli docelowy jest nieosiągalny, Dodaj adres IP i nazwę bramy w pliku hosts na bramie. Plik hosts znajduje się w **Windows\\System32\\sterowniki\\itp** folderu.

1. Następnie próby połączenia z wydawcą zostało nawiązane przy użyciu lokalnego klienta OPC UA uruchomione w bramie. Adres URL punktu końcowego OPC UA `opc.tcp://publisher.<your fully qualified domain name>:62222`. Jeśli nie masz OPC Agent użytkownika klienta, możesz pobrać i użyć [open source OPC Agent użytkownika klienta].

1. Jeśli te testy lokalnego została ukończona pomyślnie, przejdź do **połączyć własny serwer UA OPC** w portalu rozwiązania fabryka połączenia. Wprowadź adres URL punktu końcowego wydawcy (`tcp://publisher.<your fully qualified domain name>:62222`) i kliknij przycisk **Connect**. Zostanie wyświetlone ostrzeżenie, certyfikatu, a następnie kliknij **Kontynuuj.** Następnie występuje błąd wydawcy nie ufaj Agent użytkownika klienta sieci Web. Aby rozwiązać ten problem, skopiuj **Agent użytkownika klienta sieci Web** certyfikat od **D:\\docker\\certyfikatów odrzucił\\certyfikaty** folder **D: \\docker\\aplikacji UA\\certyfikaty** folderu w bramie. Nie trzeba ponownie uruchomić bramy. Powtórz ten krok. Teraz można podłączyć do bramy z chmury, a wszystko będzie gotowe dodać serwery OPC UA do rozwiązania.

### <a name="add-your-opc-ua-servers"></a>Dodaj serwery OPC UA

1. Przejdź do **połączyć własny serwer UA OPC** w portalu rozwiązania fabryka połączenia. Wykonaj te same czynności, jak w poprzedniej sekcji, aby ustanowić zaufanie między portalem fabryki połączonych i OPC UA serwera. Ten krok określa wzajemnego zaufania certyfikatów z portalu fabryki połączonych i serwer OPC UA i tworzy połączenie.

1. Przeglądanie drzewa węzłów OPC UA OPC UA serwera, kliknij prawym przyciskiem myszy węzły OPC, a następnie wybierz **publikowania**. Do publikowania działają w ten sposób, serwer OPC UA i wydawcy musi być w tej samej sieci. Innymi słowy Jeśli jest w pełni kwalifikowaną nazwę wydawcy **publisher.mydomain.com** w pełni kwalifikowaną nazwę serwera OPC UA musi być, na przykład **myopcuaserver.mydomain.com**. Jeśli ustawienia są różne, można ręcznie dodać węzłów do znaleziono w pliku publishesnodes.json **D:\\docker** folderu. Plik publishesnodes.json jest generowana automatycznie na pierwszym pomyślnym publikowania OPC węzła.

1. Dane telemetryczne teraz wypływających z urządzeniem bramy. Można wyświetlić dane telemetryczne w **lokalizacje fabryki** widok portalu fabryki połączonych w obszarze **nowa fabryka**.

## <a name="linux-deployment"></a>Wdrożenie systemu Linux

> [!NOTE]
> Jeśli nie masz jeszcze urządzenie bramy, firma Microsoft zaleca się, że kupić komercyjnych bramy w jednym z naszych partnerów. Odwiedź stronę [katalogu urządzenia Azure IoT] lista bramy urządzeń zgodnych z rozwiązania połączonych fabryki. Postępuj zgodnie z instrukcjami, które pochodzą z urządzeniem, aby skonfigurować bramę. Alternatywnie użyj poniższych instrukcji, aby ręcznie skonfigurować jedną z istniejących bram.

[Zainstaluj Docker] na urządzeniu z systemem Linux bramy.

### <a name="configure-the-gateway"></a>Konfigurowanie bramy

1. Należy **iothubowner** parametry połączenia pakietu IoT Azure połączone fabryki wdrożenia do ukończenia wdrożenia bramy. W [portalu Azure], przejdź do Centrum IoT w grupie zasobów utworzone podczas wdrażania rozwiązania fabryka połączenia. Kliknij przycisk **zasady dostępu współużytkowanego** dostępu **iothubowner** ciąg połączenia:

    ![Znajdź parametry połączenia Centrum IoT][img-hub-connection]

    Kopiuj **parametry połączenia — klucz podstawowy** wartość.

1. Konfigurowanie bramy Centrum IoT, uruchamiając modułów dwa bramy **po** z powłoki z:

    `sudo docker run -it --rm -h <ApplicationName> -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/ -v /shared:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `sudo docker run --rm -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  jest nazwą aplikacji OPC UA bramy tworzy w formacie **wydawcy.&lt; nazwę FQDN&gt;**. Na przykład **publisher.microsoft.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  jest **iothubowner** ciąg połączenia został skopiowany w poprzednim kroku. Ten ciąg połączenia jest używana tylko w tym kroku, nie będzie potrzebny w poniższych krokach:

    Możesz użyć **/ shared** folder ( `-v` argument) później, aby utrwalić dwóch certyfikatów X.509, korzystających z modułów bramy.

### <a name="run-the-gateway"></a>Uruchom bramę

1. Uruchom ponownie bramę przy użyciu następujących poleceń:

    `sudo docker run -it -h <ApplicationName> --expose 62222 -p 62222:62222 --rm -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v /shared:/shared -v /shared:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `sudo docker run -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Ze względów bezpieczeństwa dwóch certyfikatów X.509 utrwalone w **/ shared** folder zawiera klucz prywatny. Ograniczanie dostępu do tego folderu poświadczeń, których używasz do wykonywania kontenera Docker. Aby ustawić uprawnienia dla **głównego** , użyj `chmod` powłoki polecenia w folderze.

1. Sprawdź łączność sieciową. Z powłoki, wprowadź polecenie `ping publisher.<your fully qualified domain name>` na polecenie ping bramy. Jeśli docelowy jest nieosiągalny, Dodaj adres IP i nazwę bramy w pliku hostów na bramie. Plik hosts znajduje się w **/itp** folderu.

1. Następnie próby połączenia z wydawcą zostało nawiązane przy użyciu lokalnego klienta OPC UA uruchomione w bramie. Adres URL punktu końcowego OPC UA `opc.tcp://publisher.<your fully qualified domain name>:62222`. Jeśli nie masz OPC Agent użytkownika klienta, możesz pobrać i użyć [open source OPC Agent użytkownika klienta].

1. Jeśli te testy lokalnego została ukończona pomyślnie, przejdź do **połączyć własny serwer UA OPC** w portalu rozwiązania fabryka połączenia. Wprowadź adres URL punktu końcowego wydawcy (`tcp://publisher.<your fully qualified domain name>:62222`) i kliknij przycisk **Connect**. Zostanie wyświetlone ostrzeżenie, certyfikatu, a następnie kliknij **Kontynuuj.** Następnie występuje błąd wydawcy nie ufaj Agent użytkownika klienta sieci Web. Aby rozwiązać ten problem, skopiuj **Agent użytkownika klienta sieci Web** certyfikat od **/udostępnione/odrzucone certyfikatów/certyfikaty** folder **/shared/UA aplikacji/certyfikaty** folderu na brama. Nie trzeba ponownie uruchomić bramy. Powtórz ten krok. Teraz można podłączyć do bramy z chmury, a wszystko będzie gotowe dodać serwery OPC UA do rozwiązania.

### <a name="add-your-opc-ua-servers"></a>Dodaj serwery OPC UA

1. Przejdź do **połączyć własny serwer UA OPC** w portalu rozwiązania fabryka połączenia. Wykonaj te same czynności, jak w poprzedniej sekcji, aby ustanowić zaufanie między portalem fabryki połączonych i OPC UA serwera. Ten krok określa wzajemnego zaufania certyfikatów z portalu fabryki połączonych i serwer OPC UA i tworzy połączenie.

1. Przeglądanie drzewa węzłów OPC UA OPC UA serwera, kliknij prawym przyciskiem myszy węzły OPC, a następnie wybierz **publikowania**. Do publikowania działają w ten sposób, serwer OPC UA i wydawcy musi być w tej samej sieci. Innymi słowy Jeśli jest w pełni kwalifikowaną nazwę wydawcy **publisher.mydomain.com** w pełni kwalifikowaną nazwę serwera OPC UA musi być, na przykład **myopcuaserver.mydomain.com**. Jeśli ustawienia są różne, można ręcznie dodać węzłów do znaleziono w pliku publishesnodes.json **/ shared** folderu. Publishesnodes.json jest generowana automatycznie na pierwszym pomyślnym publikowania OPC węzła.

1. Dane telemetryczne teraz wypływających z urządzeniem bramy. Można wyświetlić dane telemetryczne w **lokalizacje fabryki** widok portalu fabryki połączonych w obszarze **nowa fabryka**.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o architekturze połączonych fabryki wstępnie skonfigurowane rozwiązanie, zobacz [połączonych fabryki wstępnie skonfigurowane rozwiązanie wskazówki][lnk-walkthrough].

Dowiedz się więcej o [wydawcy OPC implementacji](iot-suite-connected-factory-publisher.md).

[img-install-docker]: ./media/iot-suite-connected-factory-gateway-deployment/image1.png
[img-hub-connection]: ./media/iot-suite-connected-factory-gateway-deployment/image2.png
[img-docker-share]: ./media/iot-suite-connected-factory-gateway-deployment/image3.png

[Docker dla systemu Windows]: https://www.docker.com/docker-windows
[katalogu urządzenia Azure IoT]: https://catalog.azureiotsuite.com/?q=opc
[portalu Azure]: http://portal.azure.com/
[open source OPC Agent użytkownika klienta]: https://github.com/OPCFoundation/UA-.NETStandardLibrary/tree/master/SampleApplications/Samples/Client.Net4
[Zainstaluj Docker]: https://www.docker.com/community-edition#/download
[lnk-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[Azure IoT krawędzi]: https://github.com/Azure/iot-edge

[lnk-publisher-github]: https://github.com/Azure/iot-edge-opc-publisher
[lnk-publisher-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua
[lnk-proxy-github]: https://github.com/Azure/iot-edge-opc-proxy
[lnk-proxy-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua-proxy