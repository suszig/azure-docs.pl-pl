---
title: "Usługa DNS platformy Azure — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Często zadawane pytania dotyczące usługi Azure DNS"
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: kumud
ms.openlocfilehash: a8cff450730d05970398989ceb3e8585aefd6771
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2017
---
# <a name="azure-dns-faq"></a>Usługa DNS platformy Azure — często zadawane pytania

## <a name="about-azure-dns"></a>Usługa DNS platformy Azure — informacje

### <a name="what-is-azure-dns"></a>Co to jest system DNS platformy Azure?

System nazw domen lub DNS, jest odpowiedzialny za tłumaczenia (lub rozpoznawania) nazwę witryny sieci Web lub usługi na adres IP. Usługa DNS platformy Azure to Usługa hostingu domen DNS, rozpoznawania nazw przy użyciu infrastruktury Microsoft Azure. Dzięki hostowaniu swoich domen na platformie Azure możesz zarządzać rekordami DNS z zastosowaniem tych samych poświadczeń, interfejsów API, narzędzi i rozliczeń co w przypadku innych usług platformy Azure.

Domeny DNS w usłudze Azure DNS są hostowane w globalnej sieci platformy Azure serwerów nazw DNS. Używamy emisji dowolnej sieci, dzięki czemu każdy zapytanie DNS zostanie odebrane przez najbliższy dostępny serwer DNS. Zapewnia to zarówno duża wydajność i wysoka dostępność dla domeny.

Usługa Azure DNS jest oparta na usłudze Azure Resource Manager. W efekcie korzysta z funkcji Menedżera zasobów, takich jak kontrola dostępu oparta na rolach, dzienniki inspekcji i blokowanie zasobów. Twoje domenach i rekordach mogą być zarządzane za pośrednictwem portalu Azure, poleceń cmdlet programu PowerShell Azure i wiersza polecenia platformy Azure i platform. Aplikacje wymagające automatycznego zarządzania DNS można zintegrować z usługą za pośrednictwem interfejsu API REST i zestawy SDK.

### <a name="how-much-does-azure-dns-cost"></a>Ile kosztuje usługi Azure DNS

Modelu rozliczeń usługi Azure DNS opiera się na liczbę stref DNS udostępnianych w usłudze Azure DNS oraz liczby zapytań DNS, które otrzymują. Zniżki znajdują się na podstawie użycia.

Aby uzyskać więcej informacji, zobacz [usługi Azure DNS cennikiem](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Jaka jest umowa SLA dla usługi Azure DNS?

Firma Microsoft gwarantuje, że przez co najmniej 99,99% czasu odpowiedzi na prawidłowe zapytania DNS będą zwracane z co najmniej jednego serwera nazw usługi System DNS Azure.

Aby uzyskać więcej informacji, zobacz [strony umowy SLA platformy Azure DNS](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Co to jest strefy DNS? Czy to jest to samo co „domena DNS”? 

Domena jest unikatową nazwę w systemie nazw domen, na przykład "contoso.com".

Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Na przykład domena "contoso.com" może zawierać wiele rekordów DNS, takie jak "mail.contoso.com" (dla serwera poczty) i "www.contoso.com" (dla witryny sieci web). Te będą znajdować się w strefie DNS "contoso.com".

Nazwa domeny jest *tylko nazwa*, podczas gdy strefa DNS jest zasobem danych zawierającego rekordy DNS dla nazwy domeny. Usługa Azure DNS umożliwia hostowanie strefy DNS i zarządzanie rekordami DNS dla domeny na platformie Azure. Umożliwia także serwery DNS odpowiada na zapytania DNS z Internetu.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Czy muszę zakupić nazwę domeny DNS, aby używać usługi System DNS Azure? 

Niekoniecznie.

Nie trzeba kupować domeny, aby hostować strefę DNS w usłudze System DNS Azure. Strefę DNS można utworzyć w dowolnej chwili bez konieczności posiadania nazwy domeny. Zapytania DNS dla tej strefy rozwiąże tylko, jeśli są kierowane do usługi Azure DNS serwery nazw przypisane do strefy.

Musisz kupić nazwę domeny, jeśli chcesz połączyć swoją strefę DNS w hierarchii DNS globalnej — dzięki temu zapytania DNS z dowolnego miejsca na świecie można znaleźć strefy DNS i udzielić odpowiedzi z rekordów DNS.

## <a name="azure-dns-features"></a>Funkcje usługi Azure DNS

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Usługi Azure DNS obsługuje trybu failover routingu lub punkt końcowy usługi DNS ruchu?

Trybu failover routingu i punktu końcowego usługi DNS ruchu są dostarczane przez usługę Azure Traffic Manager. Jest to oddzielne usługa Azure, która może być używany razem z usługi Azure DNS. Aby uzyskać więcej informacji, zobacz [Omówienie Menedżera ruchu](../traffic-manager/traffic-manager-overview.md).

Usługa DNS platformy Azure obsługuje tylko hosting domeny DNS "static", gdy każdego zapytania DNS dotyczące rekordu DNS danego otrzyma zawsze tej samej odpowiedzi DNS.

### <a name="does-azure-dns-support-domain-name-registration"></a>Usługi Azure DNS obsługuje rejestracji nazwy jej domeny?

Nie. Usługa Azure DNS nie obsługuje obecnie zakupów nazw domen. Jeśli chcesz kupić domen, należy użyć Rejestratora nazw domen innych firm. Zazwyczaj rejestratora pobiera małych opłata. Następnie domeny może być hostowana w usłudze Azure DNS zarządzania rekordów DNS. Zobacz [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md) szczegółowe informacje.

Jest to funkcja, które Śledzimy naszej listy prac. Korzystając z witryny firmy Microsoft opinię do [rejestrowania programu obsługi dla tej funkcji](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Usługi Azure DNS obsługuje domen "private"?
Obsługa domen "private" jest implementowane za pomocą prywatnego strefy DNS.  Ta funkcja jest obecnie dostępna jako podgląd.  Strefy DNS prywatna są zarządzane przy użyciu tych samych narzędzi jako internetowy strefy DNS platformy Azure, ale są one tylko rozpoznawana z w określonej sieci wirtualne.  Zobacz [omówienie](private-dns-overview.md) szczegółowe informacje.

Informacje na temat innych wewnętrzny DNS na platformie Azure, zobacz [rozpoznawanie nazwy dla maszyn wirtualnych i wystąpień roli](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Usługa Azure DNS obsługuje rozszerzenia DNSSEC?

Nie. Usługa DNS platformy Azure aktualnie nie obsługuje rozszerzeń DNSSEC.

Jest to funkcja, które Śledzimy naszej listy prac. Korzystając z witryny firmy Microsoft opinię do [rejestrowania programu obsługi dla tej funkcji](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Usługa DNS platformy Azure obsługuje transferów stref (AXFR/IXFR)?

Nie. Usługa Azure DNS nie obsługuje obecnie transferów stref. Strefy DNS może być [zaimportować do usługi Azure DNS przy użyciu interfejsu wiersza polecenia Azure](dns-import-export.md). Zarządzanie rekordami DNS za pomocą [portalu zarządzania usługi Azure DNS](dns-operations-recordsets-portal.md), naszych [interfejsu API REST](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [poleceń cmdlet programu PowerShell](dns-operations-recordsets.md), lub [interfejsu wiersza polecenia narzędzia](dns-operations-recordsets-cli.md).

Jest to funkcja, które Śledzimy naszej listy prac. Korzystając z witryny firmy Microsoft opinię do [rejestrowania programu obsługi dla tej funkcji](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Usługi DNS platformy Azure obsługuje adres URL przekierowania?

Nie. Adres URL przekierowania usługi nie są faktycznie usługi DNS — działają na poziomie protokołu HTTP, a nie poziom DNS. Niektórzy dostawcy DNS pakietów adres URL przekierowania usługi w ramach ich ogólną oferty. Jest to obecnie nieobsługiwane przez usługę Azure DNS.

Ta funkcja jest śledzony w naszej listy prac. Korzystając z witryny firmy Microsoft opinię do [rejestrowania programu obsługi dla tej funkcji](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Za pomocą usługi Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Czy mogę hostowane przy użyciu usługi Azure DNS i dostawcy usługi DNS innej domeny?

Tak. Usługa DNS platformy Azure obsługuje wspólnej hostingu domeny z innymi usługami DNS.

Aby to zrobić, należy zmodyfikować rekordy NS dla domeny (które kontroli, których dostawców odbierania DNS wysyła zapytanie do domeny) aby wskazywał serwery nazw zarówno dostawców. Te rekordy NS muszą zostać zmodyfikowane w miejscach 3: w usłudze Azure DNS, inny dostawca i w strefie nadrzędnej (zazwyczaj skonfigurowany za pomocą rejestratora nazw domen). Aby uzyskać więcej informacji dotyczących delegowania usługi DNS, zobacz [Delegowanie domeny DNS](dns-domain-delegation.md).

Ponadto należy się upewnić, że rekordy DNS dla domeny są zsynchronizowane między zarówno dostawców DNS. Usługa Azure DNS nie obsługuje obecnie transferów stref DNS. Wymagana jest synchronizacja rekordów DNS przy użyciu [portalu zarządzania usługi Azure DNS](dns-operations-recordsets-portal.md), naszych [interfejsu API REST](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [poleceń cmdlet programu PowerShell](dns-operations-recordsets.md), lub [interfejsu wiersza polecenia narzędzia](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-4-azure-dns-name-servers"></a>Czy muszę delegować mojej domeny, aby wszystkie 4 serwery DNS platformy Azure?

Tak. Usługa Azure DNS przypisuje 4 serwery nazw do każdej strefy DNS izolacji błędów i zwiększonej odporności. Aby zakwalifikować się do umowy SLA platformy Azure DNS, należy delegować domenę do wszystkich serwerów nazw 4.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Jakie są limity użycia dla usługi Azure DNS?

Następujące domyślne limity stosowane podczas korzystania z usługi Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Strefy DNS platformy Azure można przenosić między grupami zasobów lub subskrypcji?

Tak. Strefy DNS można przenosić między grupami zasobów lub subskrypcji.

Nie ma żadnych wpływu na zapytania DNS podczas przenoszenia strefy DNS. Serwery nazw przypisane do strefy pozostają bez zmian i zapytań DNS są przetwarzane w zwykły w całym.

Aby uzyskać więcej informacji oraz instrukcje dotyczące przenoszenia stref DNS, zobacz [przenoszenia zasobów do nowej grupy zasobów lub subskrypcji](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Jak długo trwa DNS zmiany zaczęły obowiązywać?

Nowych stref DNS i rekordy DNS są zwykle odzwierciedlone na serwery nazw usługi Azure DNS szybkiego, w ciągu kilku sekund.

Zmiany w istniejących rekordów DNS może zająć nieco dłużej, ale nadal może mieć odzwierciedlenie w serwerów nazw DNS platformy Azure w ciągu 60 sekund. W takim przypadku DNS buforowania poza usługi Azure DNS (przez klientów DNS i rozpoznawania nazw DNS cykliczne) może również wpływ na czas poświęcony na zmiany w systemie DNS były skuteczne. Ten czas trwania pamięci podręcznej mogą być kontrolowane za pomocą właściwości Time To Live (TTL) każdego zestawu rekordów.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Jak chronić mój stref DNS przed przypadkowym usunięciem?

Usługa Azure DNS jest zarządzana przy użyciu usługi Azure Resource Manager i korzyści z kontroli dostępu do funkcji tego usługi Azure Resource Manager udostępnia. Kontrola dostępu oparta na rolach umożliwia kontrolowanie użytkowników, którzy mają odczytu lub zapisu do strefy DNS i zestawy rekordów. Aby zapobiec przypadkowemu modyfikacja lub usunięcie strefy DNS i zestawy rekordów można zastosować blokowania zasobów.

Aby uzyskać więcej informacji, zobacz [ochrona strefy DNS i rekordy](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Jak skonfigurować rekordy SPF w usłudze Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6-"></a>Czy Azure DNS Nameservers rozwiązać za pośrednictwem protokołu IPv6? 

Tak. Podwójny stos są Nameservers DNS platformy Azure (mają adresy IPv4 i IPv6). Aby znaleźć adres IPv6 nameservers usługi Azure DNS, przypisane do strefy DNS, można użyć narzędzia, takiego jak nslookup (na przykład `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Jak ustawić zapasowej międzynarodowej domeny nazwę (IDN) w usłudze Azure DNS?

Międzynarodowe nazwy domen (IDN) działają przez kodowanie, których używane są nazwa DNS "[punycode](https://en.wikipedia.org/wiki/Punycode)". Zapytania DNS są wykonywane przy użyciu tych nazw kodowany w formacie punycode.

Międzynarodowe nazwy domen (IDN) można skonfigurować w usłudze Azure DNS, konwertując pierwszy Nazwa strefy lub nazwy zestawu rekordów na ciąg punycode. Usługa Azure DNS nie obsługuje obecnie wbudowanej konwersję z/na ciąg punycode.

## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej o usłudze Azure DNS](dns-overview.md)
<br>
[Dowiedz się więcej o korzystaniu z usługi Azure DNS dla domen prywatnych](private-dns-overview.md)
<br>
[Więcej informacji na temat stref DNS i rekordów](dns-zones-records.md)
<br>
[Wprowadzenie do usługi Azure DNS](dns-getstarted-portal.md)

