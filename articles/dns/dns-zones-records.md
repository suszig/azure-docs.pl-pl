---
title: "Przegląd stref DNS i rekordy — usługi Azure DNS | Dokumentacja firmy Microsoft"
description: "Przegląd pomocy technicznej do hostowania strefy DNS i rekordy DNS firmy Microsoft Azure."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: jonatul
ms.openlocfilehash: 00f6309114039db23a1d22f1eb70076b842dadca
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="overview-of-dns-zones-and-records"></a>Przegląd stref DNS i rekordów

Ta strona wyjaśnia podstawowe pojęcia domen, strefy DNS i rekordy DNS i zestawy rekordów i jak są one obsługiwane w usłudze Azure DNS.

## <a name="domain-names"></a>Nazwy domen

System nazw domen (DNS, Domain Name System) jest hierarchią domen. Hierarchia rozpoczyna się od domeny głównej, której nazwa to po prostu „**.**”.  Poniżej są domeny najwyższego poziomu, takie jak „com”, „net”, „org”, „uk” lub „jp”.  Pod nimi są domeny drugiego poziomu, takie jak „org.uk” lub „co.jp”. Domeny w hierarchii DNS są globalnie rozproszone, hostowane przez serwery DNS na całym świecie.

Rejestratora nazw domen jest organizacja, która umożliwia uzyskanie nazwy domeny, takie jak "contoso.com".  Zakup nazwy domeny daje prawo do kontrolowania hierarchię DNS w ramach tej samej nazwie, na przykład umożliwia bezpośrednie nazwy "www.contoso.com" do witryny sieci web firmy. Rejestratora może obsługiwać domeny w jego własnej nazwy serwerów w Twoim imieniu lub umożliwiają określenie alternatywnej nazwy serwerów.

Usługa DNS platformy Azure udostępnia globalnie rozproszone, wysokiej dostępności nazwy infrastruktury serwera, które służy do obsługi domeny. Hosting domeny w usłudze Azure DNS, można zarządzać rekordami DNS z tymi samymi poświadczeniami, interfejsy API, narzędzia, rozliczeń i pomocy technicznej jako innymi usługami Azure.

Usługa Azure DNS nie obsługuje obecnie zakupów nazw domen. Jeśli chcesz kupić nazwę domeny, musisz użyć Rejestratora nazw domen innych firm. Zazwyczaj rejestratora pobiera małych opłata. Następnie domeny może być hostowana w usłudze Azure DNS zarządzania rekordów DNS. Zobacz [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md) szczegółowe informacje.

## <a name="dns-zones"></a>Strefy DNS

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>Rekordy DNS

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Czas wygaśnięcia

Czas wygaśnięcia (TTL) określa, jak długo każdy rekord jest buforowany przez klientów przed ponownym uruchomieniem zapytania. W powyższym przykładzie czas wygaśnięcia wynosi 3600 sekund lub 1 godzina.

W usłudze Azure DNS czas wygaśnięcia jest określany dla zestawu rekordów, a nie dla każdego rekordu, więc taką samą wartość jest używana dla wszystkich rekordów w danym zestawie rekordów.  Można określić dowolną wartość TTL od 1 do 2 147 483 647 sekund.

### <a name="wildcard-records"></a>Symbol wieloznaczny rekordów

Usługa DNS platformy Azure obsługuje [rekordy z użyciem symboli wieloznacznych](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Symbol wieloznaczny rekordów są zwracane w odpowiedzi na każde zapytanie o pasującej nazwie (chyba że istnieje lepsze dopasowanie pochodzące zestawu rekordów bez symboli wieloznacznych). Usługa DNS platformy Azure obsługuje zestawy rekordów z użyciem symboli wieloznacznych dla wszystkich typów rekordów z wyjątkiem NS i SOA.

Aby utworzyć zestaw rekordów symboli wieloznacznych, użyj nazwy zestawu rekordów "\*". Można również również użyć nazwy z '\*"jako jego skrajnej lewej etykiecie, na przykład"\*.foo ".

### <a name="caa-records"></a>Rejestruje CAA

Rejestruje CAA Zezwalaj właścicieli domeny określić, które urzędy certyfikacji (CA) są autoryzowane do wystawiania certyfikatów dla danej domeny. Dzięki temu urzędy certyfikacji w celu uniknięcia źle wystawianie certyfikatów w pewnych okolicznościach. Rejestruje CAA ma trzy właściwości:
* **Flagi**: jest liczbą całkowitą z zakresu od 0 do 255, używany do reprezentowania flagi stanu krytycznego, który ma specjalne znaczenie na [RFC](https://tools.ietf.org/html/rfc6844#section-3)
* **Tag**: ciąg ASCII, co może być jedną z następujących czynności:
    * **problem**: Użyj, jeśli chcesz określić urzędów certyfikacji, które mogą wystawiać certyfikaty (wszystkie typy)
    * **issuewild**: Użyj, jeśli chcesz określić urzędów certyfikacji, które mogą wystawiać certyfikaty (tylko certyfikaty symboli wieloznacznych)
    * **iodef**: Określ adres e-mail lub nazwa hosta, do którego urzędy certyfikacji mogą wyświetlać powiadomienia dla nieautoryzowanych certyfikatów problem żądań
* **Wartość**: wartość dla konkretnego znacznika wybranego

### <a name="cname-records"></a>Rekordy CNAME

Zestawy rekordów CNAME nie mogą współistnieć z innymi zestawami rekordów o tej samej nazwie. Na przykład nie można utworzyć zestawu rekordów CNAME o nazwie względnej "www" oraz rekordu A o nazwie względnej "www" w tym samym czasie.

Ponieważ wierzchołku strefy (nazwa = "@") zawsze zawiera zestawy, które zostały utworzone podczas tworzenia strefy rekordów NS i SOA, nie można utworzyć rekord CNAME w wierzchołku strefy.

Te ograniczenia wynikają z standardy usługi DNS i nie są ograniczenia usługi Azure DNS.

### <a name="ns-records"></a>Rekordy NS

Zestawu w wierzchołku strefy rekordów NS (nazwy "@") jest tworzony automatycznie w każdej strefie DNS i usuwane automatycznie po usunięciu strefy (nie można go usunąć osobno).

Ten zestaw rekordów zawiera nazwy serwerów nazw usługi Azure DNS, które są przypisane do strefy. Możesz dodać dodatkowe serwery do tego rekordu NS ustawiona, do obsługi wspólnej hostingu domen z więcej niż jednego dostawcy usługi DNS. Można również zmodyfikować TTL i metadanych dla tego zestawu rekordów. Jednak nie można usunąć ani zmodyfikować wstępnie wypełnione serwerów nazw usługi Azure DNS. 

Należy pamiętać, że dotyczy to tylko zestawu w wierzchołku strefy rekordów NS. Innymi zestawami rekordów NS w strefie (tak jak delegowania strefy podrzędnej) można tworzyć, modyfikować i usunięte bez ograniczeń.

### <a name="soa-records"></a>Rekordów SOA

Zestaw rekordów SOA jest tworzony automatycznie w wierzchołku każdej strefy (nazwa = "@") i usuwane automatycznie po usunięciu strefy.  Rekordów SOA nie można utworzyć ani usunąć oddzielnie.

Można zmodyfikować wszystkie właściwości rekord SOA, z wyjątkiem właściwości "host", która jest wstępnie skonfigurowana do odwoływania się do nazwę serwera podstawowa nazwa podana przez usługę Azure DNS.

### <a name="spf-records"></a>Rekordy SPF

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>Rekordy SRV

[Rekordy SRV](https://en.wikipedia.org/wiki/SRV_record) są używane przez różnych usług w celu określenia lokalizacji serwera. Podczas określania rekordów SRV w usłudze Azure DNS:

* *Usługi* i *protokołu* musi być określony jako część nazwy zestawu rekordów i jest poprzedzony prefiksem znaki podkreślenia.  Na przykład "\_sip.\_ TCP.name ".  Dla rekord w wierzchołku strefy, nie istnieje potrzeba do określenia "@" w nazwie rekordu, po prostu użyć usługę i protokół, na przykład "\_sip.\_ TCP ".
* *Priorytet*, *wagi*, *portu*, i *docelowej* są określone jako parametry każdego rekordu w zestawie rekordów.

### <a name="txt-records"></a>Rekordów TXT

Rekordów TXT są używane do mapowania nazw domen na dowolne ciągi. Są one używane przez różne aplikacje, w szczególności związane z konfiguracją poczty e-mail, takich jak [Framework zasad nadawcy (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) i [DomainKeys zidentyfikowane poczty (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

Standardy usługi DNS zezwala na pojedynczy rekord TXT zawierać wielu ciągów, z których każdy może być do 254 znaków. W przypadku wielu ciągów są łączone przez klientów i traktowane jako pojedynczy ciąg.

Podczas wywoływania metody interfejsu API REST usługi Azure DNS, należy określić osobno każdy ciąg TXT.  Korzystając z portalu Azure, interfejsy programu PowerShell lub interfejsu wiersza polecenia należy określić pojedynczy ciąg na rekord, który automatycznie jest podzielony na segmenty 254 znaków, jeśli to konieczne.

Nie należy mylić wielu ciągów w rekordzie DNS z wielu rekordów TXT w zestawie rekordów TXT.  Zestaw rekordów TXT może zawierać wiele rekordów *z których każdy* może zawierać wielu ciągów.  Usługa DNS platformy Azure obsługuje długość całkowita ciągu maksymalnie 1024 znaki wszystkich rekordów TXT (we wszystkich rekordów połączone).

## <a name="tags-and-metadata"></a>Znaczniki i metadane

### <a name="tags"></a>Tagi

Znaczniki są listą par nazwa wartość i są używane przez usługi Azure Resource Manager do oznaczania zasobów.  Usługa Azure Resource Manager używa tagów, aby umożliwić filtrowanie widoków rachunku Azure i umożliwia także ustawić zasady, na którym tagi są wymagane. Aby uzyskać więcej informacji na temat tagów, zobacz [Porządkowanie zasobów na platformie Azure za pomocą tagów](../azure-resource-manager/resource-group-using-tags.md).

Usługa DNS platformy Azure obsługuje przy użyciu usługi Azure Resource Manager tagów zasobów strefy DNS.  Nie obsługuje tagi zestawów rekordów DNS, mimo że jako alternatywę "metadanych" jest obsługiwana na zestawów rekordów DNS, jak wyjaśniono poniżej.

### <a name="metadata"></a>Metadane

Alternatywą dla tagów zestawu rekordów DNS platformy Azure obsługuje dodawanie adnotacji do zestawów rekordów przy użyciu "metadanych".  Podobnie jak tagi, metadanych umożliwia kojarzenie pary nazwa wartość z każdego zestawu rekordów.  Może to być przydatne na przykład do rekordu przeznaczenia każdego zestawu rekordów.  W przeciwieństwie do tagów metadanych nie może służyć do zapewnienia filtrowany widok rachunku Azure i nie można określić w zasadach usługi Azure Resource Manager.

## <a name="etags"></a>Elementy etag

Załóżmy, że dwie osoby lub dwoma procesami spróbuj zmodyfikować rekord DNS, w tym samym czasie. Który wins? I będzie wygrał użytkownik wiadomo, że zostały one zastąpione zmiany utworzone przez innego użytkownika?

Usługa Azure DNS korzysta elementy etag bezpiecznie obsłużyć równoczesnych zmian do tego samego zasobu. Elementy etag są niezależne od [usługi Azure Resource Manager "Tagi"](#tags). Każdy zasób DNS (strefy lub zestawu rekordów) ma element Etag skojarzone z nim. Zawsze, gdy zasób jest pobierana, również są pobierane jego Etag. Podczas aktualizowania zasobu, można przesłać tag Etag dzięki usłudze Azure DNS można sprawdzić, czy tag Etag na dopasowań serwera. Ponieważ każda aktualizacja do zasobu powoduje Etag ponownie generowane, element etag wskazuje, że nastąpiła zmiana współbieżnych. Elementy etag można również podczas tworzenia nowego zasobu upewnij się, że zasób nie istnieje.

Domyślnie programu PowerShell usługi Azure DNS korzysta elementy etag zablokować równoczesnych zmian do stref i zestawy rekordów. Opcjonalny *-zastąpić* przełącznika mogą być używane do pomijania sprawdzania Etag, w tym przypadku wszystkie równoczesnych zmian zostaną zastąpione.

Na poziomie interfejsu API REST usługi Azure DNS elementy etag określono korzystanie z nagłówków HTTP.  W poniższej tabeli znajduje się ich zachowanie:

| Nagłówek | Zachowanie |
| --- | --- |
| Brak |Umieść zawsze powiedzie się (nie są sprawdzane Etag) |
| IF-match<etag> |Umieść powiedzie się tylko, jeśli zasób istnieje i element Etag odpowiada |
| IF-match * |Umieść powiedzie się tylko, jeśli istnieje zasób |
| IF-none-match * |Umieść powiedzie się tylko, jeśli zasób nie istnieje. |


## <a name="limits"></a>Limity

Następujące domyślne limity stosowane podczas korzystania z usługi Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Następne kroki

* Aby rozpocząć korzystanie z usługi Azure DNS, Dowiedz się, jak [utworzyć strefę DNS](dns-getstarted-create-dnszone-portal.md) i [utworzyć rekordy DNS](dns-getstarted-create-recordset-portal.md).
* Aby przeprowadzić migrację istniejącej strefy DNS, Dowiedz się, jak [importowanie i eksportowanie pliku strefy DNS](dns-import-export.md).
