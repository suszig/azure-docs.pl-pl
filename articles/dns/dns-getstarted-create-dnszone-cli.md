---
title: "Tworzenie strefy DNS przy użyciu interfejsu wiersza polecenia| Microsoft Docs"
description: "Dowiedz się, jak tworzyć strefy DNS w usłudze Azure DNS. W tym przewodniku krok po kroku pokazano, jak utworzyć pierwszą strefę DNS przy użyciu interfejsu wiersza polecenia platformy Azure oraz jak nią zarządzać."
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
ms.date: 12/05/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: 5bbd490925e5e25f10044af55af49daa494ee026

---

# <a name="create-an-azure-dns-zone-using-cli"></a>Tworzenie strefy DNS na platformie Azure przy użyciu interfejsu wiersza polecenia

> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-create-dnszone-portal.md)
> * [Program PowerShell](dns-getstarted-create-dnszone.md)
> * [Interfejs wiersza polecenia platformy Azure](dns-getstarted-create-dnszone-cli.md)

W tym artykule przedstawiono procedurę tworzenia strefy DNS przy użyciu interfejsu wiersza polecenia platformy Azure dla wielu platform, który jest dostępny dla systemów Windows, Mac i Linux. Można również utworzyć strefę DNS za pomocą programu PowerShell lub witryny Azure Portal.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami:

* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
* Musisz zainstalować najnowszą wersję interfejsu wiersza polecenia platformy Azure, który jest dostępny dla systemów Windows, Linux i MAC. Więcej informacji znajduje się w temacie [Instalowanie interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md).

## <a name="step-1---sign-in-and-create-a-resource-group"></a>Krok 1. Logowanie się i tworzenie grupy zasobów

### <a name="switch-cli-mode"></a>Przełącz tryb interfejsu wiersza polecenia

Usługa Azure DNS korzysta z usługi Azure Resource Manager. Pamiętaj, aby przełączyć tryb interfejsu wiersza polecenia w celu używania poleceń ARM.

```azurecli
azure config mode arm
```

### <a name="sign-in-to-your-azure-account"></a>Zaloguj się do swojego konta platformy Azure

Otrzymasz monit o uwierzytelnienie się przy użyciu swoich poświadczeń. Należy pamiętać, że można używać tylko kont z identyfikatorem organizacji.

```azurecli
azure login
```

### <a name="select-the-subscription"></a>Wybierz subskrypcję

Sprawdź subskrypcje dostępne na koncie.

```azurecli
azure account list
```

Wybierz subskrypcję platformy Azure do użycia.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Będzie ona używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Ponieważ jednak wszystkie zasoby DNS są globalne, a nie regionalne, wybór lokalizacji grupy zasobów nie ma wpływu na usługę Azure DNS.

Ten krok można pominąć, jeśli używasz istniejącej grupy zasobów.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Rejestrowanie dostawcy zasobów

Usługa Azure DNS jest zarządzana przez dostawcę zasobów Microsoft.Network. Aby można było korzystać z usługi Azure DNS, musisz zarejestrować swoją subskrypcję platformy Azure w celu używania tego dostawcy zasobów. Jest to jednorazowa operacja dla każdej subskrypcji.

```azurecli
azure provider register --namespace Microsoft.Network
```

## <a name="step-2---create-a-dns-zone"></a>Krok 2 — Tworzenie strefy DNS

Do tworzenia strefy DNS służy polecenie `azure network dns zone create`. Aby wyświetlić pomoc dla tego polecenia, wpisz `azure network dns zone create -h`.

Poniższy przykład tworzy strefę DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*. Skorzystaj z tego przykładu, aby utworzyć strefę DNS, podstawiając własne wartości.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

## <a name="step-3---verify"></a>Krok 3. Weryfikowanie

### <a name="view-records"></a>Wyświetlanie rekordów

Utworzenie strefy DNS powoduje również utworzenie następujących rekordów DNS:

* Rekord SOA. Jest obecny w katalogu głównym każdej strefy DNS.
* Autorytatywne rekordy serwera nazw (rekordy NS). Pokazują, które serwery nazw hostują strefę. Usługa Azure DNS korzysta z puli serwerów nazw, a więc różne serwery nazw mogą być przypisane do różnych stref w usłudze Azure DNS. Więcej informacji można znaleźć w temacie [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

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

Jeśli domena nie została jeszcze delegowana do używania nowej strefy w usłudze Azure DNS, należy skierować zapytanie DNS bezpośrednio do jednego z serwerów nazw dla bieżącej strefy. Serwery nazw dla strefy są podane w rekordach NS, jak te wyświetlane przez polecenie „azure network dns record-set show” powyżej. Podstaw w poniższym poleceniu poprawne wartości dla swojej strefy.

W poniższym przykładzie użyto narzędzia dig do wykonania zapytania dla domeny contoso.com za pomocą serwerów nazw przypisanych do strefy DNS. Zapytanie musi wskazywać serwer nazw, dla którego użyto wartości *@\<serwer nazw dla strefy\>*, i nazwę strefy z użyciem narzędzia dig.

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

## <a name="next-steps"></a>Następne kroki

Po utworzeniu strefy DNS [utwórz zestawy rekordów i rekordy](dns-getstarted-create-recordset-cli.md), aby utworzyć rekordy DNS dla swojej domeny internetowej.




<!--HONumber=Dec16_HO2-->


