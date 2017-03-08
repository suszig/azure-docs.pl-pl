---
title: "Tworzenie strefy DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0| Microsoft Docs"
description: "Dowiedz się, jak tworzyć strefy DNS w usłudze Azure DNS. W tym szczegółowym przewodniku pokazano, jak utworzyć pierwszą strefę DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0 oraz jak nią zarządzać."
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
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: 00a4dfbe070129ce6dc84f006d793007d55dba3f
ms.lasthandoff: 02/28/2017

---

# <a name="create-an-azure-dns-zone-using-azure-cli-20"></a>Tworzenie strefy DNS na platformie Azure przy użyciu interfejsu wiersza polecenia platformy Azure 2.0

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

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Konfigurowanie interfejsu wiersza polecenia platformy Azure 2.0 dla usługi Azure DNS

### <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami:

* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

* Zainstaluj najnowszą wersję interfejsu wiersza polecenia platformy Azure, który jest dostępny dla systemów Windows, Linux i MAC. Więcej informacji znajduje się w temacie [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

### <a name="sign-in-to-your-azure-account"></a>Zaloguj się do swojego konta platformy Azure

Otwórz okno konsoli i uwierzytelnij się przy użyciu swoich poświadczeń. Aby uzyskać więcej informacji, zobacz Log in to Azure from the Azure CLI (Logowanie do platformy Azure z poziomu interfejsu wiersza polecenia platformy Azure).

```azurecli
az login
```

### <a name="select-the-subscription"></a>Wybierz subskrypcję

Sprawdź subskrypcje dostępne na koncie.

```azurecli
az account list
```

### <a name="choose-which-of-your-azure-subscriptions-to-use"></a>Wybierz subskrypcję platformy Azure do użycia.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Będzie ona używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Ponieważ jednak wszystkie zasoby DNS są globalne, a nie regionalne, wybór lokalizacji grupy zasobów nie ma wpływu na usługę Azure DNS.

Ten krok można pominąć, jeśli używasz istniejącej grupy zasobów.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

Do tworzenia strefy DNS służy polecenie `az network dns zone create`. Aby wyświetlić pomoc dla tego polecenia, wpisz `az network dns zone create --help`.

Poniższy przykład tworzy strefę DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*. Skorzystaj z tego przykładu, aby utworzyć strefę DNS, podstawiając własne wartości.

```azurecli
az network dns zone create --resource-group myresourcegroup --name contoso.com
```

## <a name="verify-your-dns-zone"></a>Sprawdzanie poprawności strefy DNS

### <a name="view-records"></a>Wyświetlanie rekordów

Utworzenie strefy DNS powoduje również utworzenie następujących rekordów DNS:

* Rekord *SOA*. Ten rekord jest obecny w katalogu głównym każdej strefy DNS.
* Autorytatywne rekordy serwera nazw (rekordy NS). Te rekordy pokazują, które serwery nazw hostują strefę. Usługa Azure DNS korzysta z puli serwerów nazw, a więc różne serwery nazw mogą być przypisane do różnych stref w usłudze Azure DNS. Więcej informacji można znaleźć w temacie [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

Aby wyświetlić te rekordy, użyj polecenia `az network dns record-set list`:

```azurecli
az network dns record-set list --resource-group MyResourceGroup --zone-name contoso.com
```

Poniżej przedstawiono odpowiedź na polecenie `az network dns record-set list`:

```json
[
  {
    "etag": "510f4711-8b7f-414e-b8d5-c0383ea3d62e",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@",
    "metadata": null,
    "name": "@",
    "nsRecords": [
      {
        "nsdname": "ns1-04.azure-dns.com."
      },
      {
        "nsdname": "ns2-04.azure-dns.net."
      },
      {
        "nsdname": "ns3-04.azure-dns.org."
      },
      {
        "nsdname": "ns4-04.azure-dns.info."
      }
    ],
    "resourceGroup": "myresourcegroup",
    "ttl": 172800,
    "type": "Microsoft.Network/dnszones/NS"
  },
  {
    "etag": "0e48e82f-f194-4d3e-a864-7e109a95c73a",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@",
    "metadata": null,
    "name": "@",
    "resourceGroup": "myresourcegroup",
    "soaRecord": {
      "email": "azuredns-hostmaster.microsoft.com",
      "expireTime": 2419200,
      "host": "ns1-04.azure-dns.com.",
      "minimumTtl": 300,
      "refreshTime": 3600,
      "retryTime": 300,
      "serialNumber": 1
    },
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/SOA"
  }
]
```

> [!NOTE]
> Zestawy rekordów w katalogu głównym (*wierzchołku*) strefy DNS używają **@** jako nazwy zestawu rekordów.

### <a name="test-name-servers"></a>Testowanie serwerów nazw

Możesz sprawdzić, czy strefa DNS znajduje się na serwerach nazw usługi Azure DNS przy użyciu narzędzi, takich jak nslookup lub dig, albo polecenia cmdlet `Resolve-DnsName` programu PowerShell.

Jeśli domena nie została jeszcze delegowana do używania nowej strefy w usłudze Azure DNS, należy skierować zapytanie DNS bezpośrednio do jednego z serwerów nazw dla bieżącej strefy. Serwery nazw dla strefy są podane w rekordach NS, jak te wyświetlane przez polecenie `az network dns record-set list`.

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


