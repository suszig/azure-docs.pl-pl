---
title: "Pomiary rzeczywistego użytkownika w usługi Azure Traffic Manager | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do pomiarów rzeczywistego użytkownika w usłudze Traffic Manager"
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
ms.openlocfilehash: a7e8ae605b6234341d9ab8b790f4c54d8627f29f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Omówienie rzeczywiste pomiary użytkownika Menedżera ruchu

>[!NOTE]
>Funkcja rzeczywiste pomiary użytkownika w usłudze Traffic Manager znajduje się w publicznej wersji zapoznawczej i nie może mieć taki sam poziom dostępności i niezawodności jako funkcje, które są zwykle dostępności wersji. Funkcja nie jest obsługiwane, mogą mieć ograniczone możliwości i mogą nie być dostępne we wszystkich lokalizacjach Azure. Najbardziej aktualne powiadomień o dostępności i stan tej funkcji, sprawdź [aktualizacji usługi Azure Traffic Manager](https://azure.microsoft.com/updates/?product=traffic-manager) strony.

Po skonfigurowaniu profilu usługi Traffic Manager przy użyciu metody routingu wydajności usługi analizuje gdzie żądań zapytań DNS pochodzą z i podejmuje decyzje dotyczące routingu do kierowania tych komputerom zgłaszającym żądania do regionu Azure, co umożliwia im uzyskać najmniejsze opóźnienia. Jest to osiągane przez wykorzystanie analizy opóźnienia sieci, które obsługuje Menedżera ruchu dla innego użytkownika końcowego sieci.

Rzeczywiste pomiary użytkownika umożliwia miarę pomiarów opóźnienia sieci do regiony platformy Azure za pośrednictwem aplikacji klienckich, które użytkownicy końcowi mogą używać, i mieć Menedżera ruchu należy wziąć pod uwagę także te informacje podczas podejmowania decyzji dotyczących routingu. Wybierając Użyj rzeczywiste pomiary użytkownika, można zwiększyć jego dokładność routingu dla żądań pochodzących z tych sieci, gdzie znajdują się użytkowników końcowych. 

## <a name="how-real-user-measurements-work"></a>Jak działają rzeczywiste pomiary użytkownika

Rzeczywiste pomiary użytkownika pracy dzięki użyciu opóźnienie miary aplikacje klienckie regiony platformy Azure, widziany przez użytkownika końcowego sieci, w których są używane. Na przykład jeśli masz strony sieci web, który jest dostępny przez użytkowników w różnych lokalizacjach (na przykład w Ameryce Północnej regionach), można wykorzystać możliwości rzeczywiste pomiary użytkownika korzystając z metody routingu wydajności można pobrać je do najlepszych region platformy Azure która jest hostowana aplikacja serwera.

Rozpocznie się osadzanie Azure podanego kodu JavaScript (z Unikatowy klucz w nim) na stronach sieci web. Po zakończeniu tej operacji, gdy użytkownik odwiedza tej strony sieci Web, JavaScript wysyła zapytanie do Menedżera ruchu, aby uzyskać informacje o regionach platformy Azure, że powinien on pomiaru. Usługa zwraca zestaw punktów końcowych do skryptu, który, a następnie miary tych regionów kolejno pobierając obraz piksela hostowanej w regionach platformy Azure i zapisując opóźnienia między czasem żądanie zostało wysłane i czasu otrzymania pierwszego bajtu . Pomiarów są następnie przekazywane do usługi Traffic Manager.

Wraz z upływem czasu dzieje się wiele razy, a w wielu sieciach prowadzące do usługi Traffic Manager uzyskiwanie dokładniejszych informacji na temat opóźnienia sieci, w którym znajdują się użytkownicy końcowi. Te informacje uruchamia wprowadzenie do uwzględnienia w decyzje dotyczące routingu wprowadzone przez Menedżera ruchu. W związku z tym prowadzi do zwiększenia dokładności tych decyzji na podstawie rzeczywistych pomiarów użytkownika wysyłane.

Gdy używasz rzeczywiste pomiary użytkownika są rozliczane na podstawie liczby pomiarów wysyłane do usługi Traffic Manager. Aby uzyskać więcej informacji o cenach, odwiedź stronę [cennikiem usługi Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="next-steps"></a>Następne kroki
- Dowiedz się, jak używać [rzeczywiste pomiary użytkownika ze stronami sieci web](traffic-manager-create-rum-web-pages.md)
- Dowiedz się [działania Menedżera ruchu](traffic-manager-overview.md)
- Dowiedz się więcej o [Mobile Center](https://docs.microsoft.com/mobile-center/)
- Dowiedz się więcej o [metody routingu ruchu](traffic-manager-routing-methods.md) obsługiwane przez Menedżera ruchu
- Dowiedz się, jak [Tworzenie profilu Menedżera ruchu](traffic-manager-create-profile.md)

