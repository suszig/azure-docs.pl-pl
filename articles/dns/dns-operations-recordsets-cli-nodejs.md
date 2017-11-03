---
title: "Zarządzanie rekordami DNS w usłudze Azure DNS przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Zarządzanie zestawów rekordów DNS i rekordy w usłudze Azure DNS, gdy hosting domeny w usłudze Azure DNS. Wszystkie polecenia interfejsu wiersza polecenia 1.0 dla operacji na zestawów rekordów i rekordów."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
ms.openlocfilehash: 307b327e4c04a0461e39930114eb193791cbda9a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-dns-records-in-azure-dns-using-the-azure-cli-10"></a>Zarządzanie rekordami DNS w usłudze Azure DNS przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

W tym artykule przedstawiono sposób zarządzania rekordy DNS dla strefy DNS przy użyciu wielu platform Azure interfejsu wiersza polecenia (CLI), która jest dostępna dla systemu Windows, Mac i Linux. Można również zarządzać rekordami DNS przy użyciu [programu Azure PowerShell](dns-operations-recordsets.md) lub [portalu Azure](dns-operations-recordsets-portal.md).

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

* [Interfejs wiersza polecenia platformy Azure 1.0](dns-operations-recordsets-cli-nodejs.md) — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami.
* [Interfejs wiersza polecenia platformy Azure 2.0](dns-operations-recordsets-cli.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami.

Przykłady w tym artykule założono, że masz już [zainstalowane 1.0 interfejsu wiersza polecenia Azure, zalogowany i utworzyć strefę DNS](dns-operations-dnszones-cli-nodejs.md).

## <a name="introduction"></a>Wprowadzenie

Przed utworzeniem rekordów DNS w usłudze Azure DNS należy najpierw zrozumieć, w jaki sposób usługa Azure DNS organizuje rekordy DNS w zestawy rekordów DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Aby uzyskać więcej informacji na temat rekordów DNS w usłudze Azure DNS, zobacz [Strefy i rekordy DNS](dns-zones-records.md).

## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

Aby utworzyć rekord DNS, użyj polecenia `azure network dns record-set add-record`. Aby uzyskać pomoc, zobacz `azure network dns record-set add-record -h`.

Podczas tworzenia rekordu należy określić nazwę grupy zasobów, nazwę strefy, nazwę zestawu rekordów, typ rekordu oraz szczegóły tworzonego rekordu. Podana nazwa zestawu rekordów musi być *względną* nazwy, co oznacza należy wykluczyć Nazwa strefy.

Jeśli zestaw rekordów jeszcze nie istnieje, to polecenie utworzy go dla Ciebie. Jeśli istnieje już zestaw rekordów, to polecenie adda przez użytkownika do istniejącego rekordu zestawu rekordów.

Jeśli jest tworzony nowy rekord, używany jest domyślny czas wygaśnięcia wynoszący 3600. Aby uzyskać instrukcje dotyczące sposobu używania różnych TTLs, zobacz [utworzyć zestaw rekordów DNS](#create-a-dns-record-set).

Poniższy przykład tworzy rekord A o nazwie *www* w strefie *contoso.com* w grupie zasobów *MyResourceGroup*. Adres IP rekordu A to *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Aby utworzyć rekord w wierzchołku strefy (w tym przypadku "contoso.com"), użyj nazwy rekordu "@", wraz z cudzysłowami:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>Tworzenie zestawu rekordów DNS

W powyższych przykładach rekord DNS albo został dodany do istniejącego zestawu rekordów lub zestawu rekordów została utworzona *niejawnie*. Można również utworzyć zestaw rekordów *jawnie* przed dodaniem do niej rekordów. Usługa DNS platformy Azure obsługuje "empty" zestawów rekordów, które mogą działać jako symbol zastępczy do zarezerwowania nazwy DNS przed utworzeniem rekordów DNS. Pusty zestawów rekordów są widoczne w płaszczyźnie kontroli usługi Azure DNS, ale nie są wyświetlane na serwery nazw usługi Azure DNS.

Zestawy rekordów są tworzone przy użyciu `azure network dns record-set create` polecenia. Aby uzyskać pomoc, zobacz `azure network dns record-set create -h`.

Tworzenie rekordu jawnie ustaw służy do określenia, takie jak właściwości zestawu rekordów [czasu wygaśnięcia (TTL, Time-To-Live)](dns-zones-records.md#time-to-live) i metadanych. [Metadane zestawu rekordów](dns-zones-records.md#tags-and-metadata) można skojarzyć dane specyficzne dla aplikacji z każdego zestawu rekordów jako pary klucz wartość.

Poniższy przykład tworzy rekord pusty ustawić TTL 60 sekund, przy użyciu `--ttl` parametr (forma krótka `-l`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

W poniższym przykładzie jest tworzony rekord z dwóch wpisów metadanych, "dział = not" i "środowiska produkcji =", za pomocą `--metadata` parametru (krótka wersja `-m`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

Posiadanie utworzony pusty zestaw rekordów, można dodawać rekordy przy użyciu `azure network dns record-set add-record` zgodnie z opisem w [Utwórz rekord DNS](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Tworzenie rekordów innych typów

O przedstawiono szczegółowo sposób utworzyć rekordy "", następujące przykłady przedstawiają sposób tworzenia rekordu innych typów rekordów obsługiwane przez usługę Azure DNS.

Parametry używane do określenia danych rekordu różnią się w zależności od typu rekordu. Na przykład w przypadku rekordu typu „A” należy podać adres IPv4 przy użyciu parametru `-a <IPv4 address>`. Parametry dla poszczególnych typów rekordów mogą być wyświetlane przy użyciu `azure network dns record-set add-record -h`.

W każdym przypadku zostanie przedstawiony sposób jeden rekord. Rekord jest dodawana do istniejącego zestawu rekordów lub niejawnie utworzyć zestaw rekordów. Aby uzyskać więcej informacji na temat tworzenia zestawów rekordów i rekordu definiujący parametr jawnie, zobacz [utworzyć zestaw rekordów DNS](#create-a-dns-record-set).

Firma Microsoft nie dają przykład można utworzyć zestawu rekordów SOA, ponieważ SOAs są tworzone i usunąć z każdej strefy DNS i nie można utworzyć ani usunąć oddzielnie. Jednak [SOA można modyfikować, jak pokazano w przykładzie nowsze](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record"></a>Utwórz rekord AAAA

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a>Utwórz rekord CNAME

> [!NOTE]
> Standardy usługi DNS nie zezwalają na rekordy CNAME w wierzchołku strefy (`-Name "@"`), ani akceptują zestawów rekordów zawierających więcej niż jeden rekord.
> 
> Aby uzyskać więcej informacji, zobacz [rekordy CNAME](dns-zones-records.md#cname-records).

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Utwórz rekord MX

W tym przykładzie używamy nazwy zestawu rekordów „@”, aby utworzyć rekord MX w wierzchołku strefy (w tym przypadku „contoso.com”).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Tworzenie rekordów NS

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>Utwórz rekord PTR

W takim przypadku "Mój-arpa-zone.com" reprezentuje strefy ARPA reprezentujący zakresowi adresów IP. Każdy rekord PTR w tej strefie odnosi się do adresu IP w tym zakresie adresów IP.  Nazwa rekordu "10" nie jest ostatni oktet adresu IP w zakresie adresów IP, to reprezentowany przez ten rekord.

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a>Tworzenie rekordów SRV

Podczas tworzenia [zestawu rekordów SRV](dns-zones-records.md#srv-records), określ  *\_usługi* i  *\_protokołu* w nazwie zestawu rekordów. Nie istnieje potrzeba do uwzględnienia "@" w nazwie zestawu rekordów, podczas tworzenia rekordów SRV zestawu w wierzchołku strefy.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a>Utwórz rekord TXT

Poniższy przykład pokazuje, jak utworzyć rekord TXT. Aby uzyskać więcej informacji na temat maksymalną długość ciągu obsługiwane w rekordach TXT, zobacz [rekordów TXT](dns-zones-records.md#txt-records).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a>Pobierz zestaw rekordów

Aby uzyskać dostęp do istniejącego zestawu rekordów, użyj `azure network dns record-set show`. Aby uzyskać pomoc, zobacz `azure network dns record-set show -h`.

Jak podczas tworzenia rekordu lub zestawu rekordów, musi być podana nazwa zestawu rekordów *względną* nazwy, co oznacza należy wykluczyć Nazwa strefy. Należy również określić typ rekordu strefy zawierającej zestawu rekordów i grupę zasobów, zawierającą strefy.

Poniższy przykład pobiera rekordu *www* a typu ze strefy *contoso.com* w grupie zasobów *MyResourceGroup*:

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a>Lista zestawów rekordów

Wyświetl listę wszystkich rekordów w strefie DNS przy użyciu `azure network dns record-set list` polecenia. Aby uzyskać pomoc, zobacz `azure network dns record-set list -h`.

W tym przykładzie zwraca zestawy wszystkich rekordów w strefie *contoso.com*, w grupie zasobów *MyResourceGroup*, niezależnie od tego, czy nazwa lub typ rekordu:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

W tym przykładzie zwraca wszystkie zestawy rekordów zgodne podanego typu rekordu (w tym przypadku rekordy ""):

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-to-an-existing-record-set"></a>Dodaj rekord do istniejącego zestawu rekordów

Można użyć `azure network dns record-set add-record` zarówno tworzenie rekordu w zestawie rekordów lub można dodać rekordu do istniejącego zestawu rekordów.

Aby uzyskać więcej informacji, zobacz [Utwórz rekord DNS](#create-a-dns-record) i [tworzenie rekordów innych typów](#create-records-of-other-types) powyżej.

## <a name="remove-a-record-from-an-existing-record-set"></a>Usuń rekord z istniejącego zestawu rekordów.

Aby usunąć rekord DNS z istniejącego zestawu rekordów, użyj `azure network dns record-set delete-record`. Aby uzyskać pomoc, zobacz `azure network dns record-set delete-record -h`.

To polecenie usuwa rekord DNS z zestawu rekordów. Usunięcie ostatniego rekordu w zestawie rekordów jest samego zestawu rekordów **nie** usunięte. Zamiast tego zostanie pozostawiony pusty zestaw rekordów. Aby usunąć rekord, ustaw zamiast tego, zobacz [Usuń zestaw rekordów](#delete-a-record-set).

Należy określić usunięcie rekordu i strefy należy ją usunąć, przy użyciu takich samych parametrach co przy tworzeniu przy użyciu rekordu `azure network dns record-set add-record`. Te parametry są opisane w [Utwórz rekord DNS](#create-a-dns-record) i [tworzenie rekordów innych typów](#create-records-of-other-types) powyżej.

To polecenie wyświetla monit o potwierdzenie. Ten monit można pomijać przy użyciu `--quiet` przełącznika (forma krótka `-q`).

Poniższy przykład umożliwia usunięcie rekordu A o wartości "1.2.3.4" z rekordu ustawić nazwane *www* w strefie *contoso.com*, w grupie zasobów *MyResourceGroup*. Nie jest wyświetlany monit o potwierdzenie.

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a>Modyfikowanie istniejącego zestawu rekordów

Każdy zestaw rekordów zawiera [czas wygaśnięcia (TTL)](dns-zones-records.md#time-to-live), [metadanych](dns-zones-records.md#tags-and-metadata)i rekordów DNS. W poniższych sekcjach opisano sposób modyfikowania każdej z tych właściwości.

### <a name="to-modify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a>Aby zmodyfikować rekord A, AAAA, MX, NS, PTR, SRV i TXT

Aby zmodyfikować istniejący rekord z typu A, AAAA, MX, NS, PTR, SRV i TXT, należy najpierw dodać nowy rekord i następnie usunąć istniejącego rekordu. Aby uzyskać szczegółowe instrukcje na temat usunąć i dodać rekordy Zobacz wcześniejszej części tego artykułu.

Poniższy przykład przedstawia sposób zmodyfikować rekord "", z adresu IP 1.2.3.4 adres IP 5.6.7.8:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="to-modify-a-cname-record"></a>Aby zmodyfikować rekord CNAME

Aby zmodyfikować rekord zasobu CNAME, użyj `azure network dns record-set add-record` nową wartość rekordu. W odróżnieniu od innych typów rekordów zestawu rekordów CNAME może zawierać tylko jeden rekord. W związku z tym jest istniejący rekord *zastąpione* po dodaniu nowego rekordu i nie trzeba usunąć oddzielnie.  Pojawi się monit zaakceptować tego zastąpienia.

Przykład modyfikuje zestawu rekordów CNAME *www* w strefie *contoso.com*, w grupie zasobów *MyResourceGroup*, aby wskazywał "www.fabrikam.net" zamiast jego istniejącej wartości:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a>Aby zmodyfikować rekord SOA

Użyj `azure network dns record-set set-soa-record` do modyfikowania SOA dla danej strefy DNS. Aby uzyskać pomoc, zobacz `azure network dns record-set set-soa-record -h`.

Poniższy przykład przedstawia sposób ustawiania właściwości "e-mail" rekord SOA strefy *contoso.com* w grupie zasobów *MyResourceGroup*:

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="to-modify-ns-records-at-the-zone-apex"></a>Aby zmodyfikować rekordy NS w wierzchołku strefy

Zestaw w wierzchołku strefy rekordów NS jest tworzony automatycznie w każdej strefie DNS. Zawiera ona nazwy serwerów nazw usługi Azure DNS, które są przypisane do strefy.

Możesz dodać dodatkowe serwery do tego rekordu NS ustawiona, do obsługi wspólnej hostingu domen z więcej niż jednego dostawcy usługi DNS. Można również zmodyfikować TTL i metadanych dla tego zestawu rekordów. Jednak nie można usunąć ani zmodyfikować wstępnie wypełnione serwerów nazw usługi Azure DNS.

Należy pamiętać, że dotyczy to tylko zestawu w wierzchołku strefy rekordów NS. Innymi zestawami rekordów NS w strefie (tak jak delegowania strefy podrzędnej) można modyfikować bez ograniczeń.

Poniższy przykład pokazuje, jak dodać serwer dodatkowe nazwy do zestawu w wierzchołku strefy rekordów NS:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a>Aby zmodyfikować TTL z istniejącego zestawu rekordów

Aby zmodyfikować TTL z istniejącego zestawu rekordów, użyj `azure network dns record-set set`. Aby uzyskać pomoc, zobacz `azure network dns record-set set -h`.

Poniższy przykład przedstawia sposób modyfikowania zestawu rekordów TTL, w tym przypadku do 60 sekund:

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a>Aby zmodyfikować metadane istniejącego zestawu rekordów

[Metadane zestawu rekordów](dns-zones-records.md#tags-and-metadata) można skojarzyć dane specyficzne dla aplikacji z każdego zestawu rekordów jako pary klucz wartość. Aby zmodyfikować metadane istniejącego zestawu rekordów, użyj `azure network dns record-set set`. Aby uzyskać pomoc, zobacz `azure network dns record-set set -h`.

Poniższy przykład przedstawia sposób modyfikowania rekordów dwa wpisy metadanych, "dział = not" i "środowiska produkcji =", za pomocą `--metadata` parametru (krótka wersja `-m`). Należy zauważyć, że metadane *zastąpione* przez podane wartości.

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a>Usuń zestaw rekordów

Zestawy rekordów można usunąć za pomocą `azure network dns record-set delete` polecenia. Aby uzyskać pomoc, zobacz `azure network dns record-set delete -h`. Usunięcie zestawu rekordów spowoduje również usunięcie wszystkich rekordów w zestawie rekordów.

> [!NOTE]
> Nie można usunąć SOA i zestawy rekordów NS w wierzchołku strefy (`-Name "@"`).  Te są tworzone automatycznie podczas strefy został utworzony i są usuwane automatycznie po usunięciu strefy.

Poniższy przykład powoduje usunięcie zestawu o nazwie rekordów *www* a typu ze strefy *contoso.com* w grupie zasobów *MyResourceGroup*:

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

Monit o potwierdzenie operacji usuwania. Aby pominąć ten monit, użyj `--quiet` przełącznika (forma krótka `-q`).

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [strefy i rekordy w usłudze Azure DNS](dns-zones-records.md).
<br>
Dowiedz się, jak [ochrony strefy i rekordy](dns-protect-zones-recordsets.md) przy użyciu usługi Azure DNS.
