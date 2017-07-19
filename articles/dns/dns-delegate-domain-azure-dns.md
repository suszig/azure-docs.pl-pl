---
title: "Delegowanie domeny do usługi Azure DNS | Microsoft Docs"
description: "Dowiedz się, jak zmienić delegowanie domeny i korzystać z serwerów nazw usługi Azure DNS do zapewniania hostingu domeny."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 33b3ec24432ff1268860b9a2e9d5098600a8dedc
ms.contentlocale: pl-pl
ms.lasthandoff: 06/16/2017

---

# <a name="delegate-a-domain-to-azure-dns"></a>Delegowanie domeny do usługi Azure DNS

Usługa Azure DNS umożliwia hostowanie strefy DNS i zarządzanie rekordami DNS dla domeny na platformie Azure. Aby zapytania DNS dla domeny miały dostęp do usługi Azure DNS, należy delegować domenę do usługi Azure DNS z domeny nadrzędnej. Należy pamiętać, że usługa Azure DNS nie jest rejestratorem domen. W tym artykule wyjaśniono, jak delegować domenę do usługi Azure DNS.

W przypadku domen zakupionych u rejestratora domen rejestrator zaoferuje opcję skonfigurowania tych rekordów NS. Nie musisz być właścicielem domeny, aby utworzyć strefę DNS z tą nazwą domeny w usłudze Azure DNS. Musisz być jednak właścicielem domeny, aby skonfigurować delegowanie do usługi Azure DNS u rejestratora.

Załóżmy na przykład, że masz zakupioną domenę „contoso.net” i tworzysz strefę o nazwie „contoso.net” w usłudze Azure DNS. Jako właścicielowi domeny rejestrator oferuje Ci opcję skonfigurowania adresów serwerów nazw (czyli rekordów NS) dla domeny. Rejestrator przechowuje te rekordy NS w domenie nadrzędnej (w tym przypadku „.net”). Klienci na całym świecie mogą być kierowani do Twojej domeny w strefie usługi Azure DNS podczas próby rozpoznania rekordów DNS w strefie „contoso.net”.

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

1. Logowanie się do witryny Azure Portal
1. W menu Centrum kliknij pozycję **Nowa > Sieć >** , a następnie kliknij pozycję **Strefa DNS**, aby otworzyć blok Tworzenie strefy DNS.

    ![Strefa DNS](./media/dns-domain-delegation/dns.png)

1. W bloku **Tworzenie strefy DNS** wprowadź następujące wartości, a następnie kliknij pozycję **Utwórz**:

   | **Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Nazwa**|contoso.net|Nazwa strefy DNS|
   |**Subskrypcja**|[Twoja subskrypcja]|Wybierz subskrypcję, w której chcesz utworzyć bramę aplikacji.|
   |**Grupa zasobów**|**Utwórz nową:** contosoRG|Utwórz grupę zasobów. Nazwa grupy zasobów musi być unikatowa w obrębie wybranej subskrypcji. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Lokalizacja**|Zachodnie stany USA||

> [!NOTE]
> Ustawienie Grupa zasobów dotyczy lokalizacji grupy zasobów i nie ma wpływu na strefę DNS. Lokalizacja strefy DNS jest zawsze „globalna” i nie jest wyświetlana.

## <a name="retrieve-name-servers"></a>Pobieranie serwerów nazw

Aby móc delegować swoją strefę DNS do usługi Azure DNS, musisz najpierw znać nazwy serwerów nazw dla swojej strefy. Usługa Azure DNS przydziela serwery nazw z puli za każdym razem, gdy tworzona jest strefa.

1. Gdy utworzysz strefę DNS, w okienku **Ulubione** witryny Azure Portal kliknij pozycję **Wszystkie zasoby**. W bloku **Wszystkie zasoby** kliknij strefę DNS **contoso.net**. Jeśli wybrana subskrypcja zawiera kilka zasobów, możesz wpisać **contoso.net** w polu Filtruj według nazwy..., aby łatwo uzyskać dostęp do bramy aplikacji. 

1. W bloku Strefa DNS pobierz serwery nazw. W tym przykładzie strefie „contoso.net” przypisano serwery nazw „ns1-01.azure-dns.com”, „ns2-01.azure-dns.net”, „ns3-01.azure-dns.org” i „ns4-01.azure-dns.info”:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Usługa Azure DNS automatycznie tworzy autorytatywne rekordy NS w strefie zawierającej przypisane serwery nazw.  Aby wyświetlić nazwy serwerów nazw za pomocą programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure, musisz po prostu pobrać te rekordy.

W poniższych przykładach udostępniono również instrukcje pobierania serwerów nazw dla strefy w usłudze Azure DNS przy użyciu programu PowerShell i interfejsu wiersza polecenia platformy Azure.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Odpowiedzią jest poniższy przykład.

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

Odpowiedzią jest poniższy przykład.

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-the-domain"></a>Delegowanie domeny

Po utworzeniu strefy DNS i pobraniu serwerów nazw domena nadrzędna powinna zostać zaktualizowana przy użyciu informacji o serwerach nazw usługi Azure DNS. Każdy rejestrator ma swoje własne narzędzia do zarządzania systemem DNS służące do zmiany rekordów serwerów nazw dla domeny. Na stronie zarządzania systemem DNS rejestratora edytuj rekordy NS i zastąp je rekordami utworzonymi przez usługę Azure DNS.

Podczas delegowania domeny do usługi Azure DNS należy użyć nazw serwerów nazw udostępnionych przez usługę Azure DNS. Zaleca się używanie wszystkich czterech nazw serwerów nazw, niezależnie od nazwy domeny. Delegowanie domeny nie wymaga, aby nazwa serwera nazw korzystała z tej samej domeny najwyższego poziomu, co domena użytkownika.

Nie należy używać rekordów sklejki do wskazywania adresów IP serwerów nazw usługi Azure DNS, ponieważ te adresy IP mogą ulec zmianie w przyszłości. Delegowanie z wykorzystaniem nazw serwerów nazw we własnej strefie, niekiedy nazywanych „serwerami nazw znaczących”, nie jest obecnie obsługiwane w usłudze Azure DNS.

## <a name="verify-name-resolution-is-working"></a>Sprawdzanie prawidłowego rozpoznawania nazw

Po zakończeniu delegowania możesz sprawdzić, czy rozpoznawanie nazw działa, uruchamiając zapytanie o rekord SOA dla swojej strefy (który również jest tworzony automatycznie podczas tworzenia strefy) za pomocą narzędzia takiego jak „nslookup”.

Nie musisz określać serwerów nazw usługi Azure DNS, jeśli delegowanie zostało skonfigurowane prawidłowo, ponieważ normalny proces rozpoznawania nazw DNS znajdzie serwery nazw automatycznie.

```
nslookup -type=SOA contoso.com
```

Poniżej zamieszczono przykładową odpowiedź na poprzednie polecenie:

```
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
```

## <a name="delegate-sub-domains-in-azure-dns"></a>Delegowanie domen podrzędnych w usłudze Azure DNS

Jeśli chcesz skonfigurować oddzielną strefę podrzędną, możesz delegować domenę podrzędną w usłudze Azure DNS. Załóżmy na przykład, że po skonfigurowaniu i delegowaniu domeny „contoso.net” w usłudze Azure DNS chcesz skonfigurować oddzielną strefę podrzędną „partners.contoso.net”.

1. Utwórz strefę podrzędną „partners.contoso.net” w usłudze Azure DNS.
2. Wyszukaj autorytatywne rekordy NS w strefie podrzędnej, aby uzyskać serwery nazw hostujące strefę podrzędną w usłudze Azure DNS.
3. Deleguj strefę podrzędną przez skonfigurowanie rekordów NS w strefie nadrzędnej wskazującej strefę podrzędną.

### <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

1. Logowanie się do witryny Azure Portal
1. W menu Centrum kliknij pozycję **Nowa > Sieć >** , a następnie kliknij pozycję **Strefa DNS**, aby otworzyć blok Tworzenie strefy DNS.

    ![Strefa DNS](./media/dns-domain-delegation/dns.png)

1. W bloku **Tworzenie strefy DNS** wprowadź następujące wartości, a następnie kliknij pozycję **Utwórz**:

   | **Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Nazwa**|partners.contoso.net|Nazwa strefy DNS|
   |**Subskrypcja**|[Twoja subskrypcja]|Wybierz subskrypcję, w której chcesz utworzyć bramę aplikacji.|
   |**Grupa zasobów**|**Użyj istniejącej:** contosoRG|Utwórz grupę zasobów. Nazwa grupy zasobów musi być unikatowa w obrębie wybranej subskrypcji. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Lokalizacja**|Zachodnie stany USA||

> [!NOTE]
> Ustawienie Grupa zasobów dotyczy lokalizacji grupy zasobów i nie ma wpływu na strefę DNS. Lokalizacja strefy DNS jest zawsze „globalna” i nie jest wyświetlana.

### <a name="retrieve-name-servers"></a>Pobieranie serwerów nazw

1. Gdy utworzysz strefę DNS, w okienku **Ulubione** witryny Azure Portal kliknij pozycję **Wszystkie zasoby**. W bloku **Wszystkie zasoby** kliknij strefę DNS **partners.contoso.net**. Jeśli wybrana subskrypcja zawiera kilka zasobów, możesz wpisać **partners.contoso.net** w polu Filtruj według nazwy..., aby łatwo uzyskać dostęp do strefy DNS.

1. W bloku Strefa DNS pobierz serwery nazw. W tym przykładzie strefie „contoso.net” przypisano serwery nazw „ns1-01.azure-dns.com”, „ns2-01.azure-dns.net”, „ns3-01.azure-dns.org” i „ns4-01.azure-dns.info”:

 ![Dns-nameserver](./media/dns-domain-delegation/viewzonens500.png)

Usługa Azure DNS automatycznie tworzy autorytatywne rekordy NS w strefie zawierającej przypisane serwery nazw.  Aby wyświetlić nazwy serwerów nazw za pomocą programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure, musisz po prostu pobrać te rekordy.

### <a name="create-name-server-record-in-parent-zone"></a>Tworzenie rekordu serwera nazw w strefie nadrzędnej

1. W witrynie Azure Portal przejdź do strefy DNS **contoso.net**.
1. Kliknij pozycję **+ Zestaw rekordów**.
1. W bloku **Dodaj zestaw rekordów** wprowadź następujące wartości, a następnie kliknij przycisk **OK**:

   | **Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Nazwa**|partners|Nazwa podrzędnej strefy DNS|
   |**Typ**|NS|Użyj wartości NS w przypadku rekordów serwera nazw.|
   |**Czas wygaśnięcia**|1|Czas wygaśnięcia.|
   |**Jednostka czasu wygaśnięcia**|Godziny|Ustawia jednostkę czasu wygaśnięcia na godziny|
   |**SERWER NAZW**|{serwery nazw ze strefy partners.contoso.net}|Wprowadź wszystkie 4 serwery nazw ze strefy partners.contoso.net. |

   ![Dns-nameserver](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Delegowanie domen podrzędnych w usłudze Azure DNS za pomocą innych narzędzi

Poniższe przykłady zawierają instrukcje delegowania domen podrzędnych w usłudze Azure DNS przy użyciu programu PowerShell i interfejsu wiersza polecenia:

#### <a name="powershell"></a>PowerShell

W poniższym przykładzie programu PowerShell pokazano, jak to działa. Te same czynności można wykonać w witrynie Azure Portal lub międzyplatformowym interfejsie wiersza polecenia platformy Azure.

```powershell
# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Użyj polecenia `nslookup` w celu sprawdzenia poprawności skonfigurowania wszystkich elementów przez wyszukanie rekordu SOA strefy podrzędnej.

```
nslookup -type=SOA partners.contoso.com
```

```
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
```

#### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Z danych wyjściowych pobierz serwery nazw dla strefy `partners.contoso.net`.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Utwórz zestaw rekordów i rekordy NS dla poszczególnych serwerów nazw.

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Usuwanie wszystkich zasobów

Aby usunąć wszystkie zasoby utworzone w tym artykule, wykonaj następujące czynności:

1. W okienku **Ulubione** witryny Azure Portal kliknij pozycję **Wszystkie zasoby**. W bloku Wszystkie zasoby kliknij grupę zasobów **contosorg**. Jeśli wybrana subskrypcja zawiera kilka zasobów, możesz wpisać **contosorg** w polu **Filtruj według nazwy...**, aby łatwo uzyskać dostęp do grupy zasobów.
1. W bloku **contosorg** kliknij przycisk **Usuń**.
1. Portal wymaga wpisania nazwy grupy zasobów w celu potwierdzenia zamiaru jej usunięcia. Wpisz nazwę grupy zasobów *contosorg*, po czym kliknij przycisk **Usuń**. Usunięcie grupy zasobów powoduje usunięcie wszystkich zasobów w niej zawartych, dlatego zawsze należy sprawdzić zawartość grupy zasobów przed jej usunięciem. Portal usuwa wszystkie zasoby zawarte w grupie zasobów, a następnie usuwa tę grupę zasobów. Ten proces trwa kilka minut.

## <a name="next-steps"></a>Następne kroki

[Zarządzanie strefami DNS](dns-operations-dnszones.md)

[Zarządzanie rekordami DNS](dns-operations-recordsets.md)

