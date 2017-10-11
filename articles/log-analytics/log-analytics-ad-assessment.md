---
title: "Optymalizacja środowiskiem usługi Active Directory na platformie Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Rozwiązanie oceny usługi Active Directory można użyć do oceny ryzyka i kondycji środowisk serwera w regularnych odstępach czasu."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97368f0b9e89ffd0cd982b6e8670d5a1f62ad42c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optymalizowanie środowiska usługi Active Directory z rozwiązaniem oceny usługi Active Directory w analizy dzienników

![Symbol oceny usługi AD](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

Rozwiązanie oceny usługi Active Directory można użyć do oceny ryzyka i kondycji środowisk serwera w regularnych odstępach czasu. Ten artykuł pomoże Ci zainstalować i używać rozwiązania, dzięki czemu można podjąć działania naprawcze potencjalnych problemów z.

To rozwiązanie zapewnia priorytetową listą zalecenia dotyczące infrastruktury serwera wdrożone. Zalecenia są podzielone na cztery obszarach zainteresowań, które pomagają w szybkim świadomość ryzyka i podejmij akcję.

Zalecenia są oparte na wiedzy i doświadczenia przez pracownicy firmy Microsoft z tysięcy wizyt klienta. Każde zalecenie znajdują się wskazówki dotyczące przyczyny problemu może być uwzględniana i implementowania sugerowane zmiany.

Można wybrać obszarach zainteresowań, które są najbardziej ważne dla organizacji i śledzić postęp w kierunku uruchomionym środowiskiem ryzyka bezpłatne i działa prawidłowo.

Po dodaniu rozwiązania i ocenę jest zakończone, podsumowanie informacji o obszarach zainteresowań jest wyświetlany na **oceny AD** pulpitu nawigacyjnego dla infrastruktury w danym środowisku. W poniższych sekcjach opisano sposób użyć informacji na temat **oceny AD** pulpitu nawigacyjnego, gdzie można przeglądać i wykonaj zalecane akcje dotyczące infrastruktury serwera usługi Active Directory.

![Obraz kafelka oceny SQL](./media/log-analytics-ad-assessment/ad-tile.png)

![Obraz pulpitu nawigacyjnego oceny SQL](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązania
Skorzystaj z poniższych informacji, aby zainstalować i skonfigurować rozwiązania.

* Agenci musi być zainstalowany na kontrolerach domeny, które są członkami domeny, która ma zostać obliczone.
* Rozwiązanie oceny usługi Active Directory wymaga obsługiwanej wersji programu .NET Framework 4 (4.5.2 lub nowszej) zainstalowane na każdym komputerze, na którym agent pakietu OMS.
* Dodaj rozwiązanie oceny usługi Active Directory na obszar roboczy OMS z [witrynę Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) lub przy użyciu procesu opisanego w [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md).  Nie są wymagane żadne dalsze czynności konfiguracyjne.

  > [!NOTE]
  > Po dodaniu rozwiązania, plik AdvisorAssessment.exe jest dodawany do serwerów z agentami. Dane konfiguracji jest do odczytu i następnie wysyłane do usługę w chmurze do przetwarzania. Logika jest stosowany do odebranych danych i usługi w chmurze rejestruje dane.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Szczegóły kolekcji danych w usłudze Active Directory oceny

Active Directory oceny zbiera dane z następujących źródeł przy użyciu agentów, które mają włączone:

- Rejestr modułów zbierających dane
- Moduły zbierające LDAP
- .NET framework
- Dziennik zdarzeń modułów zbierających dane
- Usługa Active Directory interfejsy (ADSI)
- Windows PowerShell
- Moduły zbierające dane pliku
- Instrumentacja zarządzania Windows (WMI)
- Narzędzie DCDIAG interfejsu API
- Interfejs API usługi (NTFRS) replikacji plików
- Kod niestandardowy C#


W poniższej tabeli przedstawiono metody zbierania danych dla agentów, czy Operations Manager (SCOM) jest wymagany i jak często dane są zbierane przez agenta.

| Platformy | Bezpośrednie agenta | Agenta programu SCOM | Azure Storage | SCOM wymagane? | Dane agenta programu SCOM wysyłane za pośrednictwem grupy zarządzania | Częstotliwość kolekcji |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |7 dni |

## <a name="understanding-how-recommendations-are-prioritized"></a>Opis sposobu mają pierwszeństwo zalecenia
Każdy zalecenie wprowadzone otrzymuje wartość wagi, która identyfikuje względnego zalecenia. 10 najważniejszych zalecenia są wyświetlane.

### <a name="how-weights-are-calculated"></a>Jak są obliczane wagi
Wagi są wartości zagregowanych oparte na trzech kluczowych czynników:

* *Prawdopodobieństwo* czy problemu powoduje występowanie problemów. Większe prawdopodobieństwo równa większych ogólny wynik dla zalecenia.
* *Wpływ* problemu w Twojej organizacji, jeśli spowodować problem. Większy wpływ równa większych ogólny wynik dla zalecenia.
* *Nakładu* wymagane do wykonania zalecenia. Wyższy nakładu równa mniejszych ogólny wynik dla zalecenia.

Wagi dla każde zalecenie jest wyrażona jako procent całkowitej wynik dostępne dla każdego obszaru fokus. Na przykład jeśli zalecenia w obszarze fokus zabezpieczeń i zgodności ma wynik % 5, wdrażanie tego zalecenia zwiększa z ogólną % wynik przez 5 zabezpieczeń i zgodności.

### <a name="focus-areas"></a>Obszarach zainteresowań
**Zabezpieczeń i zgodności** — w tym obszarze fokus zawiera zalecenia dotyczące potencjalnego zagrożenia bezpieczeństwa i naruszeń, zasad firmowych i wymagania techniczne, prawne i przepisami zgodności.

**Dostępność i ciągłość prowadzenia działalności biznesowej** — w tym obszarze fokus zawiera zalecenia dotyczące odporności infrastruktury i ochronę biznesowej, dostępności usług.

**Wydajność i skalowalność** — w tym obszarze fokus zawiera zalecenia ułatwiające organizacji infrastruktury IT zwiększania, upewnij się, że spełnia bieżące wymagania dotyczące wydajności środowiska informatycznego i jest w stanie reagować na zmieniające się infrastruktury wymagania związane z.

**Uaktualniania, wdrażania i migracji** — w tym obszarze fokus zawiera zalecenia ułatwiające uaktualniania, migracji i wdrożyć istniejącą infrastrukturę usługi Active Directory.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Należy dążyć wynik 100% w każdym obszarze fokus?
Niekoniecznie. Zalecenia są oparte na wiedzy i doświadczeń zdobytych Microsoft inżynierowie między tysięcy wizyt klienta. Jednak nie infrastruktury serwerowe są takie same i szczegółowe zalecenia mogą być bardziej lub mniej istotne dla Ciebie. Na przykład niektóre zalecenia dotyczące zabezpieczeń może być mniej ważne, jeśli maszyny wirtualne nie są widoczne w Internecie. Kilka zaleceń dostępności może być mniej istotne dla usług, które umożliwiają zbieranie danych ad hoc — niski priorytet i raportowania. Problemy, które są ważne dla dorosłych firm może być mniej ważne do rozruchu. Można zidentyfikować obszary fokus, które mają zebranych i przyjrzyj się jak zmienić wyniki wraz z upływem czasu.

Każdy zalecenie obejmuje wskazówek dotyczących Dlaczego ważne jest. Do oceny, czy wdrażanie zalecenie jest odpowiednie dla Ciebie, biorąc pod uwagę rodzaj usług informatycznych i potrzeb biznesowych w organizacji, należy skorzystać z poniższych wskazówek.

## <a name="use-assessment-focus-area-recommendations"></a>Użyj zaleceń obszaru fokus oceny
Zanim użyjesz rozwiązanie do oceny w pakietu OMS musi mieć zainstalowane oprogramowanie. Aby uzyskać więcej informacji na temat instalowania rozwiązań, zobacz [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md). Po jego zainstalowaniu, można wyświetlić podsumowanie zaleceń za pomocą kafelka oceny na stronie Przegląd w OMS.

Wyświetl oceny zgodności podsumowania dla Twojej infrastruktury, a następnie wejdź do zalecenia.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aby wyświetlić zalecenia dla obszaru fokus i podjąć działania naprawcze
1. Na **omówienie** kliknij przycisk **oceny** kafelku infrastruktury serwera.
2. Na **oceny** strony, sprawdź informacje w jednym z bloków obszaru fokus, a następnie kliknij przycisk jedna, aby wyświetlić zalecenia dla tego obszaru fokus.
3. Na wszystkich stronach obszaru fokus można wyświetlić priorytetową zalecenia dla danego środowiska. Kliknij zalecenie, w obszarze **dotyczy obiektów** Aby wyświetlić szczegóły dotyczące Dlaczego tworzone są zalecenia.  
    ![Obraz zalecenia oceny](./media/log-analytics-ad-assessment/ad-focus.png)
4. Należy wykonać działania naprawcze sugerowane w **sugerowanych akcji**. Jeśli element został rozwiązany, nowsze rekordy oceny zalecane akcje zostały pobrane i zwiększy wynik zgodności. Poprawione elementy są wyświetlane jako **przekazany obiektów**.

## <a name="ignore-recommendations"></a>Ignoruj zalecenia
Jeśli masz zaleceń, które chcesz zignorować, można utworzyć pliku tekstowego używanego OMS zapobiegające zaleceń znajdujących się w wynikach oceny.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Aby zidentyfikować zaleceń, które będzie ignorować
1. Zaloguj się do swojego obszaru roboczego i Otwórz dziennik wyszukiwania. Użyj następującego zapytania do listy zaleceń, które nie powiodły, na komputerach w danym środowisku.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie zmienić powyższym zapytaniu następujące.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   Poniżej przedstawiono zrzut ekranu przedstawiający zapytania wyszukiwania dziennika: ![nie powiodło się zalecenia](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)
2. Wybierz zaleceń, które chcesz zignorować. Użyjesz wartości RecommendationId w następnej procedurze.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Tworzenie i używanie IgnoreRecommendations.txt pliku tekstowego
1. Utwórz plik o nazwie IgnoreRecommendations.txt.
2. Wklej lub wpisz każdego RecommendationId dla każde zalecenie, które mają analizy dzienników można zignorować w osobnym wierszu, a następnie zapisz i zamknij plik.
3. Umieść plik w następującym folderze na każdym komputerze, w którym ma OMS zignorowanie zalecenia.
   * Na komputerach z usługą Microsoft Monitoring Agent (połączony bezpośrednio lub za pośrednictwem programu Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorowania
   * Na serwerze zarządzania programu Operations Manager — *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft

### <a name="to-verify-that-recommendations-are-ignored"></a>Aby zweryfikować, że zalecenia są ignorowane
Po na następną zaplanowaną ocenę działa, domyślnie co 7 dni, określonego zalecenia są oznaczone *zignorowane* i nie będą wyświetlane na pulpicie nawigacyjnym oceny.

1. Następujące zapytania wyszukiwania dziennika służy do tworzenia listy wszystkich zignorowane zalecenia.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie zmienić powyższym zapytaniu następujące.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Jeśli później zdecydujesz chcesz zobaczyć zignorowane zalecenia dotyczące, Usuń wszystkie pliki IgnoreRecommendations.txt lub RecommendationIDs można usunąć z nich.

## <a name="ad-assessment-solutions-faq"></a>AD oceny rozwiązania — często zadawane pytania
*Częstotliwość oceny, czy działa?*

* Ocena jest uruchamiane co 7 dni.

*Czy istnieje sposób konfigurowania, jak często jest uruchamiana oceny?*

* Nie w tej chwili.

*Jeśli inny serwer zostanie odnaleziony po rozwiązanie do oceny zostały dodane, zostanie on oceniane?*

* Tak, gdy okaże się, że jest oceniane z następnie, co 7 dni.

*Jeśli serwer jest likwidowany, gdy zostanie ono zostać usunięte z oceny?*

* Jeśli serwer nie przedstawi danych do 3 tygodni, zostanie ono usunięte.

*Co to jest nazwa procesu, który wykonuje zbierania danych?*

* AdvisorAssessment.exe

*Jak długo trwa do zebrania danych?*

* Kolekcja rzeczywistymi danymi na serwerze trwa około 1 godziny. Może trwać dłużej na serwerach, które mają wiele serwerów usługi Active Directory.

*Czy istnieje sposób konfigurowania, gdy dane są zbierane?*

* Nie w tej chwili.

*Dlaczego są wyświetlane tylko zalecenia 10 pierwszych?*

* Zamiast daje utrudnione kompletnej zadań, zaleca się skupić się na najpierw adresowania priorytetową zalecenia. Po adresu im dodatkowe zalecenia staną się dostępne. Jeśli wolisz wyświetlić listę szczegółowych, można wyświetlić wszystkie zalecenia za pomocą wyszukiwania dziennika.

*Czy istnieje sposób, aby zignorować zalecenie?*

* Tak, zobacz [zignorowanie zalecenia](#ignore-recommendations) powyższej sekcji.

## <a name="next-steps"></a>Następne kroki
* Użyj [Zaloguj wyszukiwania analizy dzienników](log-analytics-log-searches.md) Aby wyświetlić szczegółowe dane oceny usługi AD i zalecenia.
