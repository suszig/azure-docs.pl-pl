---
title: "Ocena linii bazowej sieci Web w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite | Microsoft Docs"
description: "W tym dokumencie wyjaśniono, jak korzystać z rozwiązania Zabezpieczenia i inspekcja w pakiecie OMS w celu oceny linii bazowej sieci Web wszystkich monitorowanych serwerów sieci Web pod kątem zgodności i zabezpieczeń."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 0d4707dc0c0ffbf40d0d10a6d12b9709a9655258
ms.contentlocale: pl-pl
ms.lasthandoff: 05/03/2017


---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Ocena linii bazowej sieci Web w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite
Ten dokument ułatwia korzystanie z funkcji oceny linii bazowej sieci Web w [rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite (OMS)](operations-management-suite-overview.md) w celu zapewnienia bezpieczeństwa monitorowanych zasobów.

## <a name="what-is-web-baseline-assessment"></a>Co to jest ocena linii bazowej sieci Web?
Obecnie rozwiązanie Zabezpieczenia w pakiecie OMS umożliwia ocenę podstawy linii bazowej zabezpieczeń systemów operacyjnych. Skanuje ono ustawienia systemu operacyjnego serwerów co 24 godziny i zapewnia wgląd w ich potencjalnie zagrożone ustawienia. Przeczytaj artykuł [Ocena linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-baseline.md), aby uzyskać więcej informacji na ten temat.

Ocena linii bazowej sieci Web ma na celu znalezienie potencjalnie zagrożonych ustawień serwera sieci Web. Trzy podstawowe źródła konfiguracji linii bazowej sieci Web to konfiguracja platformy .NET, platformy ASP.NET i usług IIS.  Podobnie jak w przypadku oceny linii bazowej systemu operacyjnego rozwiązanie Zabezpieczenia w pakiecie OMS skanuje serwery sieci Web co 24 godziny i zapewnia wgląd w ich stan zabezpieczeń.  W przypadku usługi Internet Information Service (IIS) konfiguracje mogą być w znaczącym stopniu dostosowywane, co pozwala na zastępowanie różnych poziomów witryny i aplikacji. Skaner sprawdza ustawienia na każdym poziomie witryny/aplikacji oraz na domyślnym poziomie głównym. Ułatwia to znajdowanie lokalizacji potencjalnie zagrożonych ustawień i szybkie rozwiązywanie problemów.


## <a name="web-security-baseline-assessment"></a>Ocena linii bazowej zabezpieczeń sieci Web
Dostęp do tej wersji zapoznawczej funkcji będzie można uzyskać za pomocą opcji wyszukiwania w pakiecie OMS. Aby uruchomić odpowiednie zapytanie, wykonaj następujące czynności:

1. Na głównym pulpicie nawigacyjnym pakietu **Microsoft Operations Management Suite** kliknij kafelek **Zabezpieczenia i inspekcja**.
2. Na pulpicie nawigacyjnym **Zabezpieczenia i inspekcja** kliknij przycisk **Przeszukiwanie dzienników**.
3. Pierwszym zapytaniem, którego możesz użyć, jest zapytanie **podsumowania oceny linii bazowej sieci Web**. W polu **Rozpocznij wyszukiwanie tutaj** wpisz to zapytanie: Type*=SecurityBaselineSummary BaselineType=web*. Następujący ekran zawiera przykładowe dane wyjściowe:

![Podsumowanie oceny linii bazowej sieci Web](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> W tym zapytaniu każdy rekord wskazuje podsumowanie oceny na jednym serwerze.

Po otwarciu **przeszukiwania dzienników** możesz wpisywać różne zapytania, aby uzyskać więcej informacji o ocenie linii bazowej sieci Web. Oprócz poprzedniego zapytania w tej wersji zapoznawczej można użyć poniższych zapytań.

**Ocena reguły linii bazowej sieci Web**: każdy rekord reprezentuje jedną ocenę reguły linii bazowej sieci Web na jednym serwerze. Zawiera wszystkie dane dotyczące reguły, lokalizacji, oczekiwanego wyniku i rzeczywistego wyniku.

**Zapytanie**: Type*=SecurityBaseline BaselineType=web*

![Ocena reguły linii bazowej sieci Web](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Pokaż wszystkie wyniki dla określonego serwera**: to zapytanie pozwala wyświetlić wyniki z określonego serwera.

**Zapytanie**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Wszystkie wyniki](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Te rekordy/zapytania umożliwiają również tworzenie własnych pulpitów nawigacyjnych, raportów i alertów. Na poniższym ekranie pokazano przykładową kontrolkę interfejsu użytkownika, którą można dodać do pulpitu nawigacyjnego. Aby dowiedzieć się, jak wizualizować dane przy użyciu projektanta widoku w pakiecie OMS, kliknij [tutaj](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). Na poniższym ekranie pokazano przykładowy wygląd kafelka po wprowadzeniu tego dostosowania.

![Przykładowy interfejs użytkownika](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Aby dowiedzieć się, które ustawienia są sprawdzane pod kątem oceny linii bazowej, pobierz [ten arkusz kalkulacyjny programu Excel](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) z informacjami o tych ustawieniach.

## <a name="see-also"></a>Zobacz też
Ten dokument przedstawia informacje na temat oceny linii bazowej sieci Web w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS. Więcej informacji na temat zabezpieczeń w pakiecie OMS zawierają następujące artykuły:

* [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorowanie alertów zabezpieczeń i reagowanie na nie w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorowanie zasobów w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-monitoring-resources.md)


