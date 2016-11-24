---
title: "Delegowanie domeny do usługi Azure DNS | Microsoft Docs"
description: "Dowiedz się, jak zmienić delegowanie domeny i korzystać z serwerów nazw usługi Azure DNS do zapewniania hostingu domeny."
services: dns
documentationcenter: na
author: sdwheeler
manager: carmonm
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/30/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 02d720a04fdc0fa302c2cb29b0af35ee92c14b3b
ms.openlocfilehash: 665e0684328538b61bb3eb05180d8d7d0e65ec49

---

# <a name="delegate-a-domain-to-azure-dns"></a>Delegowanie domeny do usługi Azure DNS

Usługa Azure DNS umożliwia hostowanie strefy DNS i zarządzanie rekordami DNS dla domeny na platformie Azure. Aby zapytania DNS dla domeny miały dostęp do usługi Azure DNS, należy delegować domenę do usługi Azure DNS z domeny nadrzędnej. Należy pamiętać, że usługa Azure DNS nie jest rejestratorem domen. W tym artykule wyjaśniono, jak działa delegowanie domeny i jak delegować domeny do usługi Azure DNS.

## <a name="how-dns-delegation-works"></a>Jak działa delegowanie DNS

### <a name="domains-and-zones"></a>Domeny i strefy

System nazw domen (DNS, Domain Name System) jest hierarchią domen. Hierarchia rozpoczyna się od domeny głównej, której nazwa to po prostu „**.**”.  Poniżej są domeny najwyższego poziomu, takie jak „com”, „net”, „org”, „uk” lub „jp”.  Pod nimi są domeny drugiego poziomu, takie jak „org.uk” lub „co.jp”.  I tak dalej. Domeny w hierarchii DNS są hostowane przy użyciu osobnych stref DNS. Te strefy są globalnie rozproszone, hostowane przez serwery DNS na całym świecie.

**Strefa DNS**

Domena jest unikatową nazwą w systemie nazw domen, na przykład „contoso.com”. Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Na przykład domena „contoso.com” może zawierać wiele rekordów DNS, takich jak „mail.contoso.com” (dla serwera poczty) i „www.contoso.com” (dla witryny sieci Web).

**Rejestrator domen**

Rejestrator domen to firma, która może udostępniać nazwy domen internetowych. Zweryfikuje ona, czy domena internetowa, której chcesz używać, jest dostępna, i umożliwi jej zakupienie. Po zarejestrowaniu nazwy domeny będziesz jej prawnym właścicielem. Jeśli masz już domenę internetową, będziesz korzystać z bieżącego rejestratora domeny do delegowania jej do usługi Azure DNS.

> [!NOTE]
> Aby dowiedzieć się, kto jest właścicielem danej nazwy domeny lub uzyskać informacje o tym, jak kupić domenę, zobacz [Zarządzanie domenami internetowymi w usłudze Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx).

### <a name="resolution-and-delegation"></a>Rozpoznawanie i delegowanie

Istnieją dwa typy serwerów DNS:

* *Autorytatywny* serwer DNS hostuje strefy DNS. Odpowiada na zapytania DNS dotyczące rekordów tylko w tych strefach.
* *Cykliczny* serwer DNS nie hostuje stref DNS. Odpowiada na zapytania DNS, wywołując autorytatywne serwery DNS w celu zebrania danych, których potrzebuje.

> [!NOTE]
> Usługa Azure DNS zapewnia autorytatywną usługę DNS.  Nie zapewnia cyklicznej usługi DNS.
>
> Usługa Cloud Services i maszyny wirtualne na platformie Azure są automatycznie skonfigurowane do korzystania z cyklicznych usług DNS zapewnianych oddzielnie w ramach infrastruktury platformy Azure.  Aby uzyskać informacje na temat zmiany tych ustawień DNS, zobacz [Name Resolution in Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) (Rozpoznawanie nazw na platformie Azure).

Klienci DNS na komputerach i urządzeniach przenośnych zwykle wywołują cykliczny serwer DNS w celu wykonywania wszelkich zapytań DNS potrzebnych aplikacjom klienckim.

Gdy cykliczny serwer DNS odbiera zapytanie dotyczące rekordu DNS takiego jak „www.contoso.com”, najpierw musi znaleźć serwer nazw hostujący strefę dla domeny „contoso.com”. W tym celu rozpoczyna od serwerów nazw głównych i wyszukuje serwery nazw hostujące strefę „com”. Następnie wykonuje zapytanie względem serwerów nazw „com”, aby znaleźć serwery nazw hostujące strefę „contoso.com”.  Na koniec jest w stanie wykonać względem tych serwerów nazw zapytanie dla „www.contoso.com”.

Ten proces jest nazywany rozpoznawaniem nazwy DNS. Mówiąc ściślej, rozpoznawanie nazw DNS obejmuje dodatkowe czynności, takie jak śledzenie rekordów CNAME, ale nie jest to istotne dla zrozumienia, jak działa delegowanie DNS.

W jaki sposób strefa nadrzędna wskazuje serwery nazw dla strefy podrzędnej? Robi to przy użyciu specjalnego rekordu DNS nazywanego rekordem NS (angielski akronim „NS” oznacza „name server”, czyli „serwer nazw”). Na przykład strefa główna zawiera rekordy NS dla strefy „com” i pokazuje serwery nazw strefy „com”. Z kolei strefa „com” zawiera rekordy NS dla strefy „contoso.com”, która wyświetla serwery nazw dla strefy „contoso.com”. Konfigurowanie rekordów NS dla strefy podrzędnej w strefie nadrzędnej nazywa się delegowaniem domeny.

![Dns-nameserver](./media/dns-domain-delegation/image1.png)

Każde delegowanie faktycznie zawiera dwie kopie rekordów NS — jedną w strefie nadrzędnej wskazującej strefę podrzędną i drugą w samej strefie podrzędnej. Strefa „contoso.com” zawiera rekordy NS dla strefy „contoso.com” (oprócz rekordów NS w strefie „com”). Są to tak zwane autorytatywne rekordy NS i znajdują się na wierzchołku strefy podrzędnej.

## <a name="delegating-a-domain-to-azure-dns"></a>Delegowanie domeny do usługi Azure DNS
Po utworzeniu strefy DNS w usłudze Azure DNS należy skonfigurować rekordy NS w strefie nadrzędnej, aby ustawić usługę Azure DNS jako autorytatywne źródło na potrzeby rozpoznawania nazw dla tej strefy. W przypadku domen zakupionych u rejestratora domen rejestrator zaoferuje opcję skonfigurowania tych rekordów NS.

> [!NOTE]
> Nie musisz być właścicielem domeny, aby utworzyć strefę DNS z tą nazwą domeny w usłudze Azure DNS. Musisz być jednak właścicielem domeny, aby skonfigurować delegowanie do usługi Azure DNS u rejestratora.

Załóżmy na przykład, że masz zakupioną domenę „contoso.com” i tworzysz strefę o nazwie „contoso.com” w usłudze Azure DNS. Jako właścicielowi domeny rejestrator zaoferuje Ci opcję skonfigurowania adresów serwerów nazw (czyli rekordów NS) dla domeny. Rejestrator będzie przechowywać te rekordy NS w domenie nadrzędnej, w tym przypadku „.com”. Klienci na całym świecie będą wówczas kierowani do Twojej domeny w strefie usługi Azure DNS podczas próby rozpoznania rekordów DNS w strefie „contoso.com”.

### <a name="finding-the-name-server-names"></a>Znajdowanie nazw serwerów nazw
Aby móc delegować swoją strefę DNS do usługi Azure DNS, musisz najpierw znać nazwy serwerów nazw dla swojej strefy. Usługa Azure DNS przydziela serwery nazw z puli za każdym razem, gdy tworzona jest strefa.

Najprostszym sposobem wyświetlenia serwerów nazw przypisanych do strefy jest skorzystanie z portalu Azure.  W tym przykładzie strefie „contoso.net” przypisano serwery nazw „ns1-01.azure-dns.com”, „ns2-01.azure-dns.net”, „ns3-01.azure-dns.org” i „ns4-01.azure-dns.info”:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Usługa Azure DNS automatycznie tworzy autorytatywne rekordy NS w strefie zawierającej przypisane serwery nazw.  Aby wyświetlić nazwy serwerów nazw za pomocą programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure, musisz po prostu pobrać te rekordy.

Przy użyciu programu Azure PowerShell autorytatywne rekordy NS można pobrać w sposób opisany poniżej. Należy pamiętać, że nazwa rekordu "@" służy do odwoływania się do rekordów na wierzchołku strefy.

    PS> $zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName MyResourceGroup
    PS> Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone

    Name              : @
    ZoneName          : contoso.net
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                        ns4-01.azure-dns.info}
    Tags              : {}

Można także użyć międzyplatformowego interfejsu wiersza polecenia platformy Azure, aby pobrać autorytatywne rekordy NS i tym samym odnaleźć serwery nazw przypisane do strefy:

    C:\> azure network dns record-set show MyResourceGroup contoso.net @ NS
    info:    Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
    data:    Id                              : /subscriptions/.../resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.net/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 172800
    data:    NS records
    data:        Name server domain name     : ns1-01.azure-dns.com.
    data:        Name server domain name     : ns2-01.azure-dns.net.
    data:        Name server domain name     : ns3-01.azure-dns.org.
    data:        Name server domain name     : ns4-01.azure-dns.info.
    data:
    info:    network dns record-set show command OK

### <a name="to-set-up-delegation"></a>Aby skonfigurować delegowanie

Każdy rejestrator ma swoje własne narzędzia do zarządzania systemem DNS służące do zmiany rekordów serwerów nazw dla domeny. Na stronie zarządzania systemem DNS rejestratora edytuj rekordy NS i zastąp je rekordami utworzonymi przez usługę Azure DNS.

Podczas delegowania domeny do usługi Azure DNS należy użyć nazw serwerów nazw udostępnionych przez usługę Azure DNS.  Należy zawsze używać wszystkich 4 nazw serwerów nazw, niezależnie od nazwy domeny.  Delegowanie domeny nie wymaga, aby nazwa serwera nazw korzystała z tej samej domeny najwyższego poziomu, co domena użytkownika.

Nie należy używać rekordów sklejki do wskazywania adresów IP serwerów nazw usługi Azure DNS, ponieważ te adresy IP mogą ulec zmianie w przyszłości. Delegowanie z wykorzystaniem nazw serwerów nazw we własnej strefie, niekiedy nazywanych „serwerami nazw znaczących”, nie jest obecnie obsługiwane w usłudze Azure DNS.

### <a name="to-verify-name-resolution-is-working"></a>Aby sprawdzić, czy rozpoznawanie nazw działa

Po zakończeniu delegowania możesz sprawdzić, czy rozpoznawanie nazw działa, uruchamiając zapytanie o rekord SOA dla swojej strefy (który również jest tworzony automatycznie podczas tworzenia strefy) za pomocą narzędzia takiego jak „nslookup”.

Pamiętaj, że nie musisz określać serwerów nazw usługi Azure DNS, ponieważ normalny proces rozpoznawania DNS znajdzie serwery nazw automatycznie, jeśli delegowanie zostało skonfigurowane prawidłowo.

    nslookup -type=SOA contoso.com

    Server: ns1-04.azure-dns.com
    Address: 208.76.47.4

    contoso.com
    primary name server = ns1-04.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)

## <a name="delegating-sub-domains-in-azure-dns"></a>Delegowanie domen podrzędnych w usłudze Azure DNS

Jeśli chcesz skonfigurować oddzielną strefę podrzędną, możesz delegować domenę podrzędną w usłudze Azure DNS. Załóżmy na przykład, że po skonfigurowaniu i delegowaniu domeny „contoso.com” w usłudze Azure DNS chcesz skonfigurować oddzielną strefę podrzędną „partners.contoso.com”.

Konfigurowanie domeny podrzędnej jest przeprowadzane za pomocą podobnej procedury jak normalne delegowanie. Jedyną różnicą jest to, że w kroku 3 rekordy NS muszą zostać utworzone w strefie nadrzędnej „contoso.com” w usłudze Azure DNS, a nie skonfigurowane za pomocą rejestratora domen.

1. Utwórz strefę podrzędną „partners.contoso.com” w usłudze Azure DNS.
2. Wyszukaj autorytatywne rekordy NS w strefie podrzędnej, aby uzyskać serwery nazw hostujące strefę podrzędną w usłudze Azure DNS.
3. Deleguj strefę podrzędną przez skonfigurowanie rekordów NS w strefie nadrzędnej wskazującej strefę podrzędną.

### <a name="to-delegate-a-sub-domain"></a>Aby delegować domenę podrzędną

W poniższym przykładzie programu PowerShell pokazano, jak to działa. Te same kroki można wykonać za pomocą portalu Azure lub międzyplatformowego interfejsu wiersza polecenia platformy Azure.

#### <a name="step-1-create-the-parent-and-child-zones"></a>Krok 1. Tworzenie stref nadrzędnych i podrzędnych
Najpierw utworzymy strefy nadrzędne i podrzędne. Mogą one znajdować się w tej samej grupie zasobów lub różnych grupach zasobów.

```powershell
$parent = New-AzureRmDnsZone -Name contoso.com -ResourceGroupName RG1
$child = New-AzureRmDnsZone -Name partners.contoso.com -ResourceGroupName RG1
```

#### <a name="step-2-retrieve-ns-records"></a>Krok 2. Pobieranie rekordów NS

Następnie pobieramy autorytatywne rekordy NS ze strefy podrzędnej, jak pokazano w następnym przykładzie.  Zawierają one serwery nazw przypisane do strefy podrzędnej.

```powershell
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS
```

#### <a name="step-3-delegate-the-child-zone"></a>Krok 3. Delegowanie strefy podrzędnej

Utwórz odpowiedni zestaw rekordów NS w strefie nadrzędnej, aby ukończyć delegowanie. Należy zauważyć, że nazwa zestawu rekordów w strefie nadrzędnej odpowiada nazwie strefy podrzędnej, w tym przypadku „partners”.

```powershell
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

### <a name="to-verify-name-resolution-is-working"></a>Aby sprawdzić, czy rozpoznawanie nazw działa

Aby sprawdzić, czy wszystko jest poprawnie skonfigurowane, wyszukaj rekord SOA strefy podrzędnej.

    nslookup -type=SOA partners.contoso.com

    Server: ns1-08.azure-dns.com
    Address: 208.76.47.8

    partners.contoso.com
        primary name server = ns1-08.azure-dns.com
        responsible mail addr = msnhst.microsoft.com
        serial = 1
        refresh = 900 (15 mins)
        retry = 300 (5 mins)
        expire = 604800 (7 days)
        default TTL = 300 (5 mins)

## <a name="next-steps"></a>Następne kroki

[Zarządzanie strefami DNS](dns-operations-dnszones.md)

[Zarządzanie rekordami DNS](dns-operations-recordsets.md)




<!--HONumber=Nov16_HO3-->


