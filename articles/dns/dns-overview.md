---
title: "Omówienie usługi Azure DNS | Dokumentacja firmy Microsoft"
description: "Omówienie systemu DNS obsługującego usługę w systemie Microsoft Azure. Hosta domeny w systemie Microsoft Azure."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: f255fd9621ff90bfbb3ad193faa64495acf7ecd7
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="azure-dns-overview"></a>Omówienie usługi Azure DNS

System nazw domen lub DNS, jest odpowiedzialny za tłumaczenia (lub rozpoznawania) nazwę witryny sieci Web lub usługi na adres IP. Usługa DNS platformy Azure to Usługa hostingu domen DNS, rozpoznawania nazw przy użyciu infrastruktury Microsoft Azure. Dzięki hostowaniu swoich domen na platformie Azure możesz zarządzać rekordami DNS z zastosowaniem tych samych poświadczeń, interfejsów API, narzędzi i rozliczeń co w przypadku innych usług platformy Azure. Usługa Azure DNS teraz obsługuje również prywatnej domen DNS. Aby uzyskać więcej informacji, zobacz [przy użyciu usługi Azure DNS dla domen prywatnej](private-dns-overview.md).

![Omówienie systemu DNS](./media/dns-overview/scenario.png)

## <a name="features"></a>Funkcje

* **Niezawodność i wydajność** -domen DNS w usłudze Azure DNS są hostowane w globalnej sieci platformy Azure serwerów nazw DNS. Usługa Azure DNS korzysta emisji dowolnej sieci, dzięki czemu każdy zapytanie DNS zostanie odebrane przez najbliższy dostępny serwer DNS. Zapewnia to zarówno duża wydajność i wysoka dostępność dla domeny.

* **Integracja** — usługa Azure DNS może służyć do zarządzania rekordami DNS dla usług Azure i może służyć do zapewnienia DNS sieci zewnętrznych zasobów. Usługa Azure DNS jest zintegrowana w portalu Azure i używa tych samych poświadczeń, rozliczeń i pomocy technicznej jako innymi usługami Azure.

* **Zabezpieczenia** — usługa Azure DNS jest oparta na usłudze Azure Resource Manager. W efekcie korzysta z funkcji Menedżera zasobów, takich jak kontrola dostępu oparta na rolach, dzienniki inspekcji i blokowanie zasobów. Twoje domenach i rekordach mogą być zarządzane za pośrednictwem portalu Azure, poleceń cmdlet programu PowerShell Azure i wiersza polecenia platformy Azure i platform. Aplikacje wymagające automatycznego zarządzania DNS można zintegrować z usługą za pośrednictwem interfejsu API REST i zestawy SDK.

Usługa Azure DNS nie obsługuje obecnie zakupów nazw domen. Jeśli chcesz kupić domen, należy użyć Rejestratora nazw domen innych firm. Zazwyczaj rejestratora pobiera małych opłata. Następnie domeny może być hostowana w usłudze Azure DNS zarządzania rekordów DNS. Zobacz [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md) szczegółowe informacje.

## <a name="pricing"></a>Cennik

Karta DNS opiera się na liczbę stref DNS udostępnianych na platformie Azure i liczby zapytań DNS. Aby dowiedzieć się więcej na temat cen, odwiedź stronę [cennik usługi Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>Często zadawane pytania

Często zadawane pytania dotyczące usługi Azure DNS, zobacz [DNS platformy Azure — często zadawane pytania](dns-faq.md).

## <a name="next-steps"></a>Kolejne kroki

Więcej informacji na temat stref DNS i rekordy odwiedzając: [DNS strefy i rejestruje omówienie](dns-zones-records.md).

Dowiedz się, jak [utworzyć strefę DNS](./dns-getstarted-create-dnszone-portal.md) w usłudze Azure DNS.

Poznaj inne kluczowe [możliwości sieciowe](../networking/networking-overview.md) platformy Azure.

