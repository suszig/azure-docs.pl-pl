---
title: "Tworzenie strefy DNS przy użyciu interfejsu wiersza polecenia| Microsoft Docs"
description: "Dowiedz się, jak krok po kroku przy użyciu interfejsu wiersza polecenia utworzyć strefy DNS dla serwera usługi Azure DNS, aby rozpocząć hosting domeny DNS"
services: dns
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
ms.assetid: 1514426a-133c-491a-aa27-ee0962cea9dc
ms.service: dns
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: e2b93d8788f8f36ff6bc04a33d1f35a86cd49973


---
# <a name="create-an-azure-dns-zone-using-cli"></a>Tworzenie strefy DNS na platformie Azure przy użyciu interfejsu wiersza polecenia
> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-create-dnszone-portal.md)
> * [Program PowerShell](dns-getstarted-create-dnszone.md)
> * [Interfejs wiersza polecenia platformy Azure](dns-getstarted-create-dnszone-cli.md)
> 
> 

W tym artykule opisano kroki tworzenia strefy DNS przy użyciu interfejsu wiersza polecenia. Można również utworzyć strefę DNS za pomocą programu PowerShell lub witryny Azure Portal.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="before-you-begin"></a>Przed rozpoczęciem
Poniższe instrukcje korzystają z interfejsu wiersza polecenia platformy Microsoft Azure. Aby używać poleceń usługi Azure DNS, należy zaktualizować interfejs wiersza polecenia platformy Azure do najnowszej wersji (0.9.8 lub nowszej). Wpisz polecenie `azure -v` w celu sprawdzenia, która wersja interfejsu wiersza polecenia platformy Azure jest obecnie zainstalowana na komputerze.

## <a name="step-1-set-up-azure-cli"></a>Krok 1 — Konfigurowanie interfejsu wiersza polecenia platformy Azure
### <a name="1-install-azure-cli"></a>1. Zainstaluj interfejs wiersza polecenia platformy Azure
Możesz zainstalować interfejs wiersza polecenia platformy Azure dla systemu Windows, Linux lub Mac. Poniższe kroki muszą zostać wykonane, zanim będzie można zarządzać usługą Azure DNS za pomocą interfejsu wiersza polecenia platformy Azure. Więcej informacji znajduje się w temacie [Instalowanie interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md). Polecenia DNS wymagają interfejsu wiersza polecenia platformy Azure w wersji 0.9.8 lub nowszej.

Wszystkie polecenia dostawcy sieci w interfejsie wiersza polecenia można znaleźć przy użyciu następującego polecenia:

    azure network

### <a name="2-switch-cli-mode"></a>2. Przełącz tryb interfejsu wiersza polecenia
Usługa Azure DNS korzysta z usługi Azure Resource Manager. Pamiętaj, aby przełączyć tryb interfejsu wiersza polecenia w celu używania poleceń ARM.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. Zaloguj się do swojego konta platformy Azure
Otrzymasz monit o uwierzytelnienie się przy użyciu swoich poświadczeń. Należy pamiętać, że można używać tylko kont z identyfikatorem organizacji.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Wybierz subskrypcję
Wybierz subskrypcję platformy Azure do użycia.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. Utwórz grupę zasobów
Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Będzie ona używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Ponieważ jednak wszystkie zasoby DNS są globalne, a nie regionalne, wybór lokalizacji grupy zasobów nie ma wpływu na usługę Azure DNS.

Ten krok można pominąć, jeśli używasz istniejącej grupy zasobów.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. Zarejestruj subskrypcję
Usługa Azure DNS jest zarządzana przez dostawcę zasobów Microsoft.Network. Aby można było korzystać z usługi Azure DNS, musisz zarejestrować swoją subskrypcję platformy Azure w celu używania tego dostawcy zasobów. Jest to jednorazowa operacja dla każdej subskrypcji.

    azure provider register --namespace Microsoft.Network


## <a name="step-2-create-a-dns-zone"></a>Krok 2 — Tworzenie strefy DNS
Do tworzenia strefy DNS służy polecenie `azure network dns zone create`. Można opcjonalnie utworzyć strefę DNS wraz z tagami. Tagi to pary nazw i wartości używane przez usługę Azure Resource Manager do oznaczania zasobów etykietami na potrzeby rozliczeń lub grupowania. Aby uzyskać więcej informacji na temat tagów, zobacz [Porządkowanie zasobów na platformie Azure za pomocą tagów](../resource-group-using-tags.md).

W usłudze Azure DNS nazwy stref powinny być określane bez znaku **„.”** na końcu. Na przykład jako „**contoso.com**” zamiast „**contoso.com.**”.

### <a name="to-create-a-dns-zone"></a>Tworzenie strefy DNS
Poniższy przykład tworzy strefę DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*.

Skorzystaj z tego przykładu, aby utworzyć własną strefę DNS, podstawiając własne wartości.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Tworzenie strefy DNS i tagów
Interfejs wiersza polecenia platformy Azure obsługuje tagi stref DNS określone przy użyciu opcjonalnego parametru *-Tag*. Poniższy przykład pokazuje, jak utworzyć strefę DNS z dwoma tagami, project = demo i env = test.

Skorzystaj z tego przykładu, aby utworzyć strefę DNS i tagi, podstawiając własne wartości.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Wyświetlanie rekordów
Utworzenie strefy DNS powoduje również utworzenie następujących rekordów DNS:

* Rekord SOA. Jest obecny w katalogu głównym każdej strefy DNS.
* Autorytatywne rekordy serwera nazw (rekordy NS). Pokazują, które serwery nazw hostują strefę. Usługa Azure DNS korzysta z puli serwerów nazw, a więc różne serwery nazw mogą być przypisane do różnych stref w usłudze Azure DNS. Więcej informacji można znaleźć w temacie [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

Aby wyświetlić te rekordy, użyj polecenia `azure network dns-record-set show`.<BR>
*Składnia: network dns record-set show <grupa zasobów> <nazwa strefy dns><name> <type>*

W poniższym przykładzie w przypadku uruchomienia polecenia z grupą zasobów *myresourcegroup*, nazwą zestawu rekordów *"@"* (dla rekordu głównego) i typem *SOA* wygeneruje ono następujące dane wyjściowe:

    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Aby wyświetlić rekordy NS utworzone dla strefy, użyj następującego polecenia:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

> [!NOTE]
> Zestawy rekordów w katalogu głównym (*wierzchołku*) strefy DNS używają **@** jako nazwy zestawu rekordów.
> 
> 

## <a name="test"></a>Test
Strefę DNS można przetestować przy użyciu narzędzi DNS, takich jak nslookup, DIG lub polecenie cmdlet `Resolve-DnsName` programu PowerShell.

Jeśli domena nie została jeszcze delegowana do używania nowej strefy w usłudze Azure DNS, należy skierować zapytanie DNS bezpośrednio do jednego z serwerów nazw dla bieżącej strefy. Serwery nazw dla strefy są podane w rekordach NS, jak te wyświetlane przez polecenie „azure network dns record-set show” powyżej. Podstaw w poniższym poleceniu poprawne wartości dla swojej strefy.

Poniższy przykład korzysta z narzędzia DIG do wykonania zapytania na domenie contoso.com, używając serwerów nazw przypisanych do strefy DNS. Zapytanie musi wskazać serwer nazw, dla którego użyliśmy wartości *@<name server for the zone>*, i nazwę strefy z użyciem narzędzia DIG.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
     Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
     flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
     WARNING: recursion requested but not available

     OPT PSEUDOSECTION:
     EDNS: version: 0, flags:; udp: 4000
      QUESTION SECTION:
    contoso.com.                        IN      A

     AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Następne kroki
Po utworzeniu strefy DNS utwórz [zestawy rekordów i rekordy](dns-getstarted-create-recordset-cli.md), aby uruchomić rozpoznawanie nazw dla swojej domeny internetowej.




<!--HONumber=Nov16_HO2-->


