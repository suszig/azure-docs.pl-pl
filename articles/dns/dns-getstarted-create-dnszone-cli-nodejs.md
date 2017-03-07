---
title: "Tworzenie strefy DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0| Microsoft Docs"
description: "Dowiedz się, jak tworzyć strefy DNS w usłudze Azure DNS. W tym szczegółowym przewodniku pokazano, jak utworzyć pierwszą strefę DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0 oraz jak nią zarządzać."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 1514426a-133c-491a-aa27-ee0962cea9dc
ms.service: dns
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 1e8645e8554bc8f8ec72a6df98bfe9ffc5be13b3
ms.lasthandoff: 02/27/2017

---

# <a name="create-an-azure-dns-zone-using-azure-cli-10"></a>Tworzenie strefy DNS na platformie Azure przy użyciu interfejsu wiersza polecenia platformy Azure 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-create-dnszone-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-create-dnszone-cli.md)

W tym artykule przedstawiono procedurę tworzenia strefy DNS przy użyciu interfejsu wiersza polecenia platformy Azure dla wielu platform, który jest dostępny dla systemów Windows, Mac i Linux. Można również utworzyć strefę DNS za pomocą środowiska [Azure PowerShell](dns-getstarted-create-dnszone.md) lub witryny [Azure Portal](dns-getstarted-create-dnszone-portal.md).

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

* [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-create-dnszone-cli-nodejs.md) — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami.
* [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-create-dnszone-cli.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami.

## <a name="introduction"></a>Wprowadzenie

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

Do tworzenia strefy DNS służy polecenie `azure network dns zone create`. Aby wyświetlić pomoc dla tego polecenia, wpisz `azure network dns zone create -h`.

Poniższy przykład tworzy strefę DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*. Skorzystaj z tego przykładu, aby utworzyć strefę DNS, podstawiając własne wartości.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

## <a name="verify-your-dns-zone"></a>Sprawdzanie poprawności strefy DNS

### <a name="view-records"></a>Wyświetlanie rekordów

Utworzenie strefy DNS powoduje również utworzenie następujących rekordów DNS:

* Rekord *SOA*. Ten rekord jest obecny w katalogu głównym każdej strefy DNS.
* Autorytatywne rekordy serwera nazw (rekordy NS). Te rekordy pokazują, które serwery nazw hostują strefę. Usługa Azure DNS korzysta z puli serwerów nazw, a więc różne serwery nazw mogą być przypisane do różnych stref w usłudze Azure DNS. Więcej informacji można znaleźć w temacie [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

Aby wyświetlić te rekordy, użyj polecenia `azure network dns-record-set list`:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com

info:    Executing command network dns record-set list
+ Looking up the DNS Record Sets
data:    Name                            : @
data:    Type                            : NS
data:    TTL                             : 172800
data:    Records:
data:      ns1-01.azure-dns.com.
data:      ns2-01.azure-dns.net.
data:      ns3-01.azure-dns.org.
data:      ns4-01.azure-dns.info.
data:
data:    Name                            : @
data:    Type                            : SOA
data:    TTL                             : 3600
data:    Email                           : azuredns-hostmaster.microsoft.com
data:    Host                            : ns1-01.azure-dns.com.
data:    Serial Number                   : 2
data:    Refresh Time                    : 3600
data:    Retry Time                      : 300
data:    Expire Time                     : 2419200
data:    Minimum TTL                     : 300
data:
info:    network dns record-set list command OK
```

> [!NOTE]
> Zestawy rekordów w katalogu głównym (*wierzchołku*) strefy DNS używają **@** jako nazwy zestawu rekordów.

### <a name="test-name-servers"></a>Testowanie serwerów nazw

Możesz sprawdzić, czy strefa DNS znajduje się na serwerach nazw usługi Azure DNS przy użyciu narzędzi, takich jak nslookup lub dig, albo polecenia cmdlet `Resolve-DnsName` programu PowerShell.

Jeśli domena nie została jeszcze delegowana do używania nowej strefy w usłudze Azure DNS, należy skierować zapytanie DNS bezpośrednio do jednego z serwerów nazw dla bieżącej strefy. Serwery nazw dla strefy są podane w rekordach NS, jak te wyświetlane przez polecenie `azure network dns record-set list`.

W poniższym przykładzie użyto narzędzia dig do wykonania zapytania dla domeny contoso.com za pomocą serwerów nazw przypisanych do strefy DNS. Pamiętaj, aby podstawić poprawne wartości dla swojej strefy.

```
  > dig @ns1-01.azure-dns.com contoso.com
  
  <<>> DiG 9.10.2-P2 <<>> @ns1-01.azure-dns.com contoso.com
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
contoso.com.         3600     IN      SOA     ns1-01.azure-dns.com. azuredns-hostmaster.microsoft.com. 1 3600 300 2419200 300

Query time: 93 msec
SERVER: 208.76.47.5#53(208.76.47.5)
WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
MSG SIZE  rcvd: 120
```

## <a name="next-steps"></a>Następne kroki

Po utworzeniu strefy DNS [utwórz w niej zestawy rekordów i rekordy DNS](dns-getstarted-create-recordset-cli.md).


