---
title: "Azure stos obsługi zasad | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat obsługi zasad i sposobu na utrzymanie zintegrowany system do stanu umożliwiającego stosu Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 498d0cdc3eac25b8efc7339e48381a4d167c0c7b
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-stack-servicing-policy"></a>Azure stos obsługi zasad

*Dotyczy: Azure stosu zintegrowane systemy*

W tym artykule opisano zasady obsługi w stosie Azure zintegrowanych systemów i co należy zrobić, aby zapewnić systemu do stanu umożliwiającego. 

## <a name="update-package-types"></a>Typy pakietów aktualizacji

Istnieją dwa typy pakietów aktualizacji dla zintegrowanych systemów. Aktualizacje oprogramowania i aktualizacje, które są specyficzne dla dostawcy sprzętu producenta sprzętu (OEM), takie jak sterowniki i oprogramowanie układowe. Te aktualizacje są dostarczane jako osobne pakiety aktualizacji stosu Azure i są zarządzane niezależnie.

- **Aktualizacje oprogramowania Microsoft**. Microsoft jest odpowiedzialny za end-to-end obsługi cyklu życia dla pakietów aktualizacji oprogramowania firmy Microsoft. Te pakiety mogą zawierać najnowsze aktualizacje zabezpieczeń systemu Windows Server, aktualizacje zabezpieczeń i aktualizacje funkcji Azure stosu. Możesz pobrać pakietów aktualizacji je bezpośrednio od firmy Microsoft.
- **Aktualizacje dostarczonym sprzętu OEM**. Azure stosu dostawców sprzętu będących partnerami są odpowiedzialne za end-to-end obsługi cyklu życia (w tym wskazówki) związanych ze sprzętem oprogramowania układowego i pakietów aktualizacji sterownika. Ponadto partnerów sprzętowych stosu Azure własne i obsługa wskazówki dotyczące wszystkich sprzętu na hoście cyklu życia sprzętu i oprogramowania. Producenta OEM obsługuje te pakiety na ich własnych witryny pobierania aktualizacji.

## <a name="update-package-release-cadence"></a>Szybko wersji pakietu aktualizacji

Microsoft oczekuje wersji pakietów aktualizacji oprogramowania w okresach miesięcznych. Jednak jest możliwe wersje aktualizacji wielokrotny lub nie w ciągu miesiąca. Dostawcy sprzętu OEM ich aktualizacje na zgodnie z potrzebami.

Pakiet aktualizacji firmy Microsoft ma następującą konwencją nazewnictwa pomaga łatwo zidentyfikować Data wydania:

*MajorProductVersion.MinorProductVersion.YYMMDD.BuildNumber*

Na przykład aktualizację oprogramowania firmy Microsoft, wydanej w dniu 15 czerwca 2017 musi wersji "1.0.170615.1".

## <a name="keep-your-system-under-support"></a>Aktualizowanie systemu jako pomoc techniczna

Uzyskanie pomocy technicznej dla systemu, należy dysponować stosu Azure zaktualizowane w określonym interwale. Nasze zasady dla odroczenia aktualizacji oprogramowania firmy Microsoft są trzy miesiące. System jest nieaktualny więcej niż trzy miesiące, są traktowane jako zgodne. Musisz zaktualizować system do co najmniej minimalna obsługiwana wersja, aby uzyskać pomoc techniczną. 

Pakiety aktualizacji oprogramowania firmy Microsoft są skumulowanych i wymagają poprzedniego pakietu aktualizacji jako warunek wstępny. Jeśli zdecydujesz się odroczenie co najmniej jednej aktualizacji, należy wziąć pod uwagę ogólną środowiska uruchomieniowego Jeśli chcesz uzyskać dostęp do najnowszej wersji.

W poniższej tabeli przedstawiono przykład aktualizacji pakietu wersji, ich wymagań wstępnych i minimalna obsługiwana wersja systemu musi wynosić do obsługi pomocy technicznej. Tabela jest oparta na początkowa wersja stosu Azure zintegrowanych systemów (kompilacja 1708), z pierwszej aktualizacji wersji pakietu (1709), w września 2017 r. 

| Najnowszy pakiet aktualizacji (*przykład*) | Wymagania wstępne | Minimalna obsługiwana wersja |
| -- | -- | -- |
| 1709 | Kompilacja 1708 | Nie dotyczy |
| 1710 | 1709 | Nie dotyczy |
| 1711 | 1710 | Nie dotyczy |
| 1712 | 1711 | 1709 |
| 1801 | 1712 | 1710 |
| 1802 | 1801 | 1711 |
| 1803 | 1802 | 1712 |
| 1804 | 1803 | 1801 |
| | | 

## <a name="next-steps"></a>Następne kroki

- [Zarządzanie aktualizacjami w stosie Azure](azure-stack-updates.md)


