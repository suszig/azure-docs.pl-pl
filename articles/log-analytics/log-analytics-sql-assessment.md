---
title: "Optymalizuj środowisko programu SQL Server z Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Z usługi Analiza dzienników Azure rozwiązanie do oceny SQL służy do oceny ryzyka i kondycji środowisk programu SQL server w regularnych odstępach czasu."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2aed3315fe60ace46dfb4176dc13aa417257b0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-sql-server-environment-with-the-sql-assessment-solution-in-log-analytics"></a>Optymalizuj środowisko programu SQL Server z rozwiązaniem oceny SQL w analizy dzienników

![Symbol oceny SQL](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

Rozwiązanie do oceny SQL można użyć do oceny ryzyka i kondycji środowisk serwera w regularnych odstępach czasu. Ten artykuł pomoże Ci zainstalować rozwiązania, dzięki czemu można wykonać działania naprawcze potencjalnych problemów.

To rozwiązanie zapewnia priorytetową listą zalecenia dotyczące infrastruktury serwera wdrożone. Zalecenia są podzielone na sześć obszarów fokus, które pomagają w szybkim świadomość ryzyka i podjęcia działań naprawczych.

Zalecenia są oparte na wiedzy i doświadczenia przez pracownicy firmy Microsoft z tysięcy wizyt klienta. Każde zalecenie znajdują się wskazówki dotyczące przyczyny problemu może być uwzględniana i implementowania sugerowane zmiany.

Można wybrać obszarach zainteresowań, które są najbardziej ważne dla organizacji i śledzić postęp w kierunku uruchomionym środowiskiem ryzyka bezpłatne i działa prawidłowo.

Po dodaniu rozwiązania i ocenę jest zakończone, podsumowanie informacji o obszarach zainteresowań jest wyświetlany na **oceny SQL** pulpitu nawigacyjnego dla infrastruktury w danym środowisku. W poniższych sekcjach opisano sposób użyć informacji na temat **oceny SQL** pulpitu nawigacyjnego, gdzie można przeglądać i wykonaj zalecane akcje dotyczące infrastruktury serwera SQL.

![Obraz kafelka oceny SQL](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Obraz pulpitu nawigacyjnego oceny SQL](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązania
Ocena SQL współpracuje z wszystkich aktualnie obsługiwanych wersjach programu SQL Server w wersjach Standard, Developer i Enterprise.

Skorzystaj z poniższych informacji, aby zainstalować i skonfigurować rozwiązania.

* Agenci musi być zainstalowany na serwerach, które mają zainstalowany program SQL Server.
* Rozwiązanie do oceny SQL wymaga obsługiwanej wersji programu .NET Framework 4 zainstalowane na każdym komputerze, na którym agent pakietu OMS.
* Aby można było zainstalować rozwiązania, użytkownik musi być administratorem lub współpracownikiem subskrypcji platformy Azure przy użyciu portalu Azure. Ponadto użytkownik musi być członkiem roli administratora lub współautora obszaru roboczego pakietu OMS w portalu pakietu OMS.
* Za pomocą agenta programu Operations Manager z oceną SQL, należy użyć konta Run-As menedżera operacji. Zobacz [konta programu Operations Manager, Uruchom jako dla OMS](#operations-manager-run-as-accounts-for-oms) poniżej Aby uzyskać więcej informacji.

  > [!NOTE]
  > MMA agent nie obsługuje programu Operations Manager Run-As kont.
  >
  >
* Dodaj rozwiązanie do oceny SQL na obszar roboczy OMS zastosowanie procesu opisanego w [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md). Nie są wymagane żadne dalsze czynności konfiguracyjne.

> [!NOTE]
> Po dodaniu rozwiązania, plik AdvisorAssessment.exe jest dodawany do serwerów z agentami. Dane konfiguracji jest do odczytu i następnie wysyłane do usługę w chmurze do przetwarzania. Logika jest stosowany do odebranych danych i usługi w chmurze rejestruje dane.

## <a name="sql-assessment-data-collection-details"></a>Szczegóły pobierania danych SQL do oceny
Ocena SQL służy do zbierania danych usługi WMI, dane rejestru dane dotyczące wydajności i wyników widoku dynamicznego zarządzania programu SQL Server za pomocą agentów, które mają włączone.

W poniższej tabeli przedstawiono metody zbierania danych dla agentów, czy Operations Manager (SCOM) jest wymagany i jak często dane są zbierane przez agenta.

| Platformy | Bezpośrednie agenta | Agenta programu SCOM | Azure Storage | SCOM wymagane? | Dane agenta programu SCOM wysyłane za pośrednictwem grupy zarządzania | Częstotliwość kolekcji |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  | &#8226; |7 dni |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Uruchom jako konta programu Operations Manager dla OMS
Analiza dzienników w OMS używa grupy zarządzania i agent programu Operations Manager w celu wysyłają i zbierają dane z usługą OMS. Kompilacje OMS na pakiety administracyjne dla obciążeń w celu zapewnienia wartość — Dodawanie usług. Poszczególnych obciążeń wymaga uprawnień specyficznego dla obciążenia uruchomione pakiety administracyjne w innym kontekście zabezpieczeń, takie jak konto domeny. Musisz podać poświadczenia informacji przez skonfigurowanie konta Operations Manager uruchom jako.

Skorzystaj z poniższych informacji, aby ustawić konto Uruchom jako programu Operations Manager w celu oceny SQL.

### <a name="set-the-run-as-account-for-sql-assessment"></a>Ustaw konto Uruchom jako w celu oceny SQL
 Jeśli korzystasz już z pakietu administracyjnego programu SQL Server, należy użyć tego konta Uruchom jako.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>Aby skonfigurować konto Uruchom jako SQL w konsoli operacje
> [!NOTE]
> Jeśli używasz bezpośredniej agent pakietu OMS zamiast agenta programu SCOM, pakiet zarządzania jest zawsze uruchamiany w kontekście zabezpieczeń konta systemu lokalnego. Pomiń kroki od 1 do 5 poniżej, a następnie uruchomić T-SQL lub próbki programu Powershell, określając NT AUTHORITY\SYSTEM z nazwą użytkownika.
>
>

1. W programie Operations Manager, otwórz konsolę operacje, a następnie kliknij przycisk **administracji**.
2. W obszarze **Konfiguracja Uruchom jako**, kliknij przycisk **profile**i Otwórz **OMS SQL oceny profilu Uruchom jako**.
3. Na **konta Uruchom jako** kliknij przycisk **Dodaj**.
4. Wybierz konto systemu Windows uruchom jako, które zawiera poświadczenia wymagane dla programu SQL Server, lub kliknij przycisk **nowy** go utworzyć.

   > [!NOTE]
   > Typ konta Uruchom jako musi być systemu Windows. Konta Uruchom jako musi także być częścią lokalnej grupy administratorów na wszystkich serwerach systemu Windows obsługującego wystąpienia programu SQL Server.
   >
   >
5. Kliknij pozycję **Zapisz**.
6. Modyfikuj, a następnie wykonaj w poniższym przykładzie T-SQL w każdym wystąpieniu serwera SQL udzielenia minimalne uprawnienia wymagane do konta Uruchom jako w celu wykonania oceny SQL. Jednak nie należy to zrobić, jeśli konto Uruchom jako jest już częścią roli serwera sysadmin na wystąpienia programu SQL Server.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Aby skonfigurować konto Uruchom jako SQL przy użyciu programu Windows PowerShell
Otwórz okno programu PowerShell i uruchom następujący skrypt po zaktualizowaniu z informacjami:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Opis sposobu mają pierwszeństwo zalecenia
Każdy zalecenie wprowadzone otrzymuje wartość wagi, która identyfikuje względnego zalecenia. 10 najważniejszych zalecenia są wyświetlane.

### <a name="how-weights-are-calculated"></a>Jak są obliczane wagi
Wagi są wartości zagregowanych oparte na trzech kluczowych czynników:

* *Prawdopodobieństwo* czy problemu może powodować problemy. Większe prawdopodobieństwo równa większych ogólny wynik dla zalecenia.
* *Wpływ* problemu w Twojej organizacji, jeśli spowodować problem. Większy wpływ równa większych ogólny wynik dla zalecenia.
* *Nakładu* wymagane do wykonania zalecenia. Wyższy nakładu równa mniejszych ogólny wynik dla zalecenia.

Wagi dla każde zalecenie jest wyrażona jako procent całkowitej wynik dostępne dla każdego obszaru fokus. Na przykład jeśli zalecenia w obszarze fokus zabezpieczeń i zgodności ma wynik % 5, wdrażanie tego zalecenia spowoduje zwiększenie ogólnej % wynik przez 5 zabezpieczeń i zgodności.

### <a name="focus-areas"></a>Obszarach zainteresowań
**Zabezpieczeń i zgodności** — w tym obszarze fokus zawiera zalecenia dotyczące potencjalnego zagrożenia bezpieczeństwa i naruszeń, zasad firmowych i wymagania techniczne, prawne i przepisami zgodności.

**Dostępność i ciągłość prowadzenia działalności biznesowej** — w tym obszarze fokus zawiera zalecenia dotyczące odporności infrastruktury i ochronę biznesowej, dostępności usług.

**Wydajność i skalowalność** — w tym obszarze fokus zawiera zalecenia ułatwiające organizacji infrastruktury IT zwiększania, upewnij się, że spełnia bieżące wymagania dotyczące wydajności środowiska informatycznego i jest w stanie reagować na zmieniające się infrastruktury wymagania związane z.

**Uaktualniania, wdrażania i migracji** — w tym obszarze fokus zawiera zalecenia ułatwiające uaktualniania, migracji i wdrożenia programu SQL Server w istniejącej infrastrukturze.

**Operacje i monitorowanie** — w tym obszarze fokus zawiera zalecenia ułatwiające usprawnić operacji IT, wdrożenia program prewencyjnej konserwacji i zmaksymalizować wydajność.

**Zarządzanie zmianami i konfiguracją** — w tym obszarze fokus zawiera zalecenia dotyczące ochrony codziennej pracy, należy zapewnić, że zmiany nie mieć negatywny wpływ na infrastrukturę, ustanowić procedury sterowania zmianami oraz do śledzenia i inspekcji konfiguracje systemu.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Należy dążyć wynik 100% w każdym obszarze fokus?
Niekoniecznie. Zalecenia są oparte na wiedzy i doświadczeń zdobytych Microsoft inżynierowie między tysięcy wizyt klienta. Jednak nie infrastruktury serwerowe są takie same i szczegółowe zalecenia mogą być bardziej lub mniej istotne dla Ciebie. Na przykład niektóre zalecenia dotyczące zabezpieczeń może być mniej ważne, jeśli maszyny wirtualne nie są widoczne w Internecie. Kilka zaleceń dostępności może być mniej istotne dla usług, które umożliwiają zbieranie danych ad hoc — niski priorytet i raportowania. Problemy, które są ważne dla dorosłych firm może być mniej ważne do rozruchu. Można zidentyfikować obszary fokus, które mają zebranych i przyjrzyj się jak zmienić wyniki wraz z upływem czasu.

Każdy zalecenie obejmuje wskazówek dotyczących Dlaczego ważne jest. Do oceny, czy wdrażanie zalecenie jest odpowiednie dla Ciebie, biorąc pod uwagę rodzaj usług informatycznych i potrzeb biznesowych w organizacji, należy skorzystać z poniższych wskazówek.

## <a name="use-assessment-focus-area-recommendations"></a>Użyj zaleceń obszaru fokus oceny
Zanim użyjesz rozwiązanie do oceny w pakietu OMS musi mieć zainstalowane oprogramowanie. Aby uzyskać więcej informacji na temat instalowania rozwiązań, zobacz [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md). Po jego zainstalowaniu, można wyświetlić podsumowanie zaleceń za pomocą kafelka oceny SQL na stronie Przegląd w OMS.

Wyświetl oceny zgodności podsumowania dla Twojej infrastruktury, a następnie wejdź do zalecenia.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aby wyświetlić zalecenia dla obszaru fokus i podjąć działania naprawcze
1. Na **omówienie** kliknij przycisk **oceny SQL** kafelka.
2. Na **oceny SQL** strony, sprawdź informacje w jednym z bloków obszaru fokus, a następnie kliknij przycisk jedna, aby wyświetlić zalecenia dla tego obszaru fokus.
3. Na wszystkich stronach obszaru fokus można wyświetlić priorytetową zalecenia dla danego środowiska. Kliknij zalecenie, w obszarze **dotyczy obiektów** Aby wyświetlić szczegóły dotyczące Dlaczego tworzone są zalecenia.  
    ![Obraz zalecenia oceny SQL](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Należy wykonać działania naprawcze sugerowane w **sugerowanych akcji**. Jeśli element został rozwiązany, ocen nowsze zarejestruje które zalecane akcje zostały pobrane i zwiększa wynik zgodności. Poprawione elementy są wyświetlane jako **przekazany obiektów**.

## <a name="ignore-recommendations"></a>Ignoruj zalecenia
Jeśli masz zaleceń, które chcesz zignorować, można utworzyć pliku tekstowego używanego OMS zapobiegające zaleceń znajdujących się w wynikach oceny.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Aby zidentyfikować zaleceń, które będzie ignorować
1. Zaloguj się do swojego obszaru roboczego i Otwórz dziennik wyszukiwania. Użyj następującego zapytania do listy zaleceń, które nie powiodły, na komputerach w danym środowisku.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Poniżej przedstawiono zrzut ekranu przedstawiający zapytania wyszukiwania dziennika: ![nie powiodło się zalecenia](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)
2. Wybierz zaleceń, które chcesz zignorować. Użyjesz wartości RecommendationId w następnej procedurze.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Tworzenie i używanie IgnoreRecommendations.txt pliku tekstowego
1. Utwórz plik o nazwie IgnoreRecommendations.txt.
2. Wklej lub wpisz każdego RecommendationId dla każde zalecenie, które mają OMS można zignorować w osobnym wierszu, a następnie zapisz i zamknij plik.
3. Umieść plik w następującym folderze na każdym komputerze, w którym ma OMS zignorowanie zalecenia.
   * Na komputerach z usługą Microsoft Monitoring Agent (połączony bezpośrednio lub za pośrednictwem programu Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorowania
   * Na serwerze zarządzania programu Operations Manager — *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft

### <a name="to-verify-that-recommendations-are-ignored"></a>Aby zweryfikować, że zalecenia są ignorowane
1. Po na następną zaplanowaną ocenę działa, domyślnie co 7 dni, określonego zalecenia są oznaczane ignorowany i nie będą wyświetlane na pulpicie nawigacyjnym oceny.
2. Następujące zapytania wyszukiwania dziennika służy do tworzenia listy wszystkich zignorowane zalecenia.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. Jeśli później zdecydujesz chcesz zobaczyć zignorowane zalecenia dotyczące, Usuń wszystkie pliki IgnoreRecommendations.txt lub RecommendationIDs można usunąć z nich.

## <a name="sql-assessment-solution-faq"></a>Rozwiązanie do oceny SQL — często zadawane pytania
*Częstotliwość oceny, czy działa?*

* Ocena jest uruchamiane co 7 dni.

*Czy istnieje sposób konfigurowania, jak często jest uruchamiana oceny?*

* Nie w tej chwili.

*Jeśli inny serwer zostanie odnaleziony po rozwiązanie do oceny SQL zostały dodane, zostanie on oceniane?*

* Tak, gdy okaże się, że jest oceniane z następnie, co 7 dni.

*Jeśli serwer jest likwidowany, gdy zostanie ono zostać usunięte z oceny?*

* Jeśli serwer nie przedstawi danych do 3 tygodni, zostanie ono usunięte.

*Co to jest nazwa procesu, który wykonuje zbierania danych?*

* AdvisorAssessment.exe

*Jak długo trwa do zebrania danych?*

* Kolekcja rzeczywistymi danymi na serwerze trwa około 1 godziny. Może trwać dłużej na serwerach, które mają wiele wystąpień programu SQL lub bazy danych.

*Jakiego typu dane są zbierane?*

* Zbierane są następujące typy danych:
  * USŁUGI WMI
  * Rejestru
  * Liczniki wydajności
  * SQL dynamicznych widoków zarządzania (DMV).

*Czy istnieje sposób konfigurowania, gdy dane są zbierane?*

* Nie w tej chwili.

*Dlaczego trzeba skonfigurować konto Uruchom jako?*

* Dla programu SQL Server są uruchamiane niewielkiej liczby zapytania SQL. Aby je uruchomić należy użyć konto Uruchom jako z uprawnieniami VIEW SERVER STATE do bazy danych SQL.  Ponadto aby można było wykonać kwerendę usługi WMI, wymagane są poświadczenia administratora lokalnego.

*Dlaczego są wyświetlane tylko zalecenia 10 pierwszych?*

* Zamiast daje utrudnione kompletnej zadań, zaleca się skupić się na najpierw adresowania priorytetową zalecenia. Po adresu im dodatkowe zalecenia staną się dostępne. Jeśli wolisz wyświetlić listę szczegółowych, można wyświetlić wszystkie zalecenia, używając przycisku Wyszukaj dziennika OMS.

*Czy istnieje sposób, aby zignorować zalecenie?*

* Tak, zobacz [zignorowanie zalecenia](#ignore-recommendations) powyższej sekcji.

## <a name="next-steps"></a>Następne kroki
* [Wyszukaj dzienniki](log-analytics-log-searches.md) do wyświetlenia szczegółowych danych SQL do oceny i zalecenia.
