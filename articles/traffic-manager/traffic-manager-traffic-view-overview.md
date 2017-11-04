---
title: "Ruch widoku w usługi Azure Traffic Manager | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do widoku ruchu Menedżera ruchu"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 0a07ff578c6afeedc6f3806d52bfe5aef6945c04
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="traffic-manager-traffic-view"></a>Widok ruchu Menedżera ruchu

>[!NOTE]
>Funkcja widoku ruchu w usłudze Traffic Manager znajduje się w publicznej wersji zapoznawczej i nie może mieć taki sam poziom dostępności i niezawodności jako funkcje, które są zwykle dostępności wersji. Funkcja nie jest obsługiwane, mogą mieć ograniczone możliwości i mogą nie być dostępne we wszystkich lokalizacjach Azure. Najbardziej aktualne powiadomień o dostępności i stan tej funkcji, sprawdź [aktualizacji usługi Azure Traffic Manager](https://azure.microsoft.com/updates/?product=traffic-manager) strony.

Menedżer ruchu dostarcza DNS poziomu routingu, tak aby użytkownicy końcowi są kierowane do dobrej kondycji punktów końcowych na podstawie metody routingu, były skonfigurowane podczas tworzenia profilu. Zapewnia to Menedżera ruchu z widokiem Twojej bazy użytkowników (na poziomie szczegółowości programu rozpoznawania nazw DNs) oraz ich wzorzec ruchu. Po włączeniu widoku ruchu, te informacje są przetwarzane dostarczają przydatnych wyników analiz. 

Przy użyciu widoku ruchu sieciowego, można:
- Dowiedz się, których Twojej bazy użytkowników znajdują się (do lokalnego DNS rozpoznawania poziomu szczegółowości).
- Wyświetl wielkość ruchu sieciowego (przestrzegane jako zapytania DNS obsługiwane przez usługę Azure Traffic Manager) pochodzące z tych obszarów.
-  Uzyskaj wgląd w nowości reprezentatywny czas oczekiwania wystąpił przez tych użytkowników.
- Nowości w wzorce ruchu z każdej z tych zasad użytkownika do regiony platformy Azure, gdzie masz punktów końcowych. 

Na przykład służy widok ruchu do zrozumienia regiony dużą liczbą ruchu, ale boryka się z większych opóźnień. Obok służą tych informacji do planowania użytkownika została zrozumiana. dzięki rozszerzania nowe regiony platformy Azure, aby te użytkownicy mogą mieć niższe środowisko opóźnienia.

## <a name="how-traffic-view-works"></a>Jak działa widok ruchu

Widok ruchu działa przez Menedżera ruchu przyjrzeć się zapytania przychodzące otrzymanych w ciągu ostatnich siedmiu dni przed profil, który ma ta funkcja jest włączona. Z tych informacji umożliwia wyodrębnianie źródłowy adres IP program rozpoznawania nazw DNS, który jest następnie używana jako reprezentacja lokalizacji użytkowników. Te są następnie grupowane podsumowanie poziomu programu rozpoznawania nazw DNS do utworzenia użytkownika podstawowego regionów, korzystając z informacji geograficzne obsługiwane przez usługę Traffic Manager adresów IP. Menedżer ruchu następnie przegląda regiony platformy Azure, do których został skierowany zapytania i tworzy mapę przepływu ruchu sieciowego dla użytkowników z tych regionów.
W następnym kroku Traffic Manager są powiązane z regionu podstawowego użytkownika do mapowania region platformy Azure z tabelami opóźnienia analizy sieci, które przechowuje inny użytkownik końcowy sieci zrozumienie Średni czas oczekiwania u użytkowników z tych regionów, gdy Łączenie z usługą regiony platformy Azure. Następnie wyszczególniono tych obliczeń na dla lokalnych DNS rozpoznawania adresu IP poziomu przed jego są prezentowane. Może przetworzyć tych informacji w przepływie pracy analytics wybranych przez użytkownika, możesz również pobrać tych informacji w pliku CSV.
Korzystając z widoku ruch, są rozliczane na podstawie liczby punktów danych używany do tworzenia analiz przedstawiony. Typ punktu tylko danych używany jest obecnie zapytań odebranych z profilu Menedżera ruchu. Aby uzyskać więcej informacji o cenach, odwiedź stronę [cennikiem usługi Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Następne kroki

- Dowiedz się [działania Menedżera ruchu](traffic-manager-overview.md)
- Dowiedz się więcej o [metody routingu ruchu](traffic-manager-routing-methods.md) obsługiwane przez Menedżera ruchu
- Dowiedz się, jak [Tworzenie profilu Menedżera ruchu](traffic-manager-create-profile.md)

