---
title: "Optymalizacja środowiskiem programu System Center Operations Manager na platformie Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Rozwiązania System Center Operations Manager oceny można użyć do oceny ryzyka i kondycji środowisk serwera w regularnych odstępach czasu."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4992d98397da409f7c1cfbdeb40fdb0cdd0d2f19
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-assessment-preview-solution"></a>Optymalizuj środowisko z rozwiązań System Center Operations Manager oceny (wersja zapoznawcza)

![System Center Operations Manager oceny symbol](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

Rozwiązania System Center Operations Manager oceny można użyć do oceny ryzyka i kondycji środowisk serwera programu System Center Operations Manager w regularnych odstępach czasu. Ten artykuł pomaga zainstalować, skonfigurować i użycie rozwiązania, dzięki czemu można wykonać działania naprawcze potencjalnych problemów.

To rozwiązanie zapewnia priorytetową listą zalecenia dotyczące infrastruktury serwera wdrożone. Zalecenia są podzielone na cztery obszarach zainteresowań, które pomagają w szybkim świadomość ryzyka i podejmowanie działań naprawczych.

Zalecenia są oparte na wiedzy i doświadczenia przez pracownicy firmy Microsoft z tysięcy wizyt klienta. Każde zalecenie znajdują się wskazówki dotyczące przyczyny problemu może być uwzględniana i implementowania sugerowane zmiany.

Można wybrać obszarach zainteresowań, które są najbardziej ważne dla organizacji i śledzić postęp w kierunku uruchomionym środowiskiem ryzyka bezpłatne i działa prawidłowo.

Po dodaniu rozwiązania i ocenę jest zakończone, podsumowanie informacji o obszarach zainteresowań jest wyświetlany na **System Center Operations Manager oceny** pulpitu nawigacyjnego dla infrastruktury. W poniższych sekcjach opisano sposób użyć informacji na temat **System Center Operations Manager oceny** pulpitu nawigacyjnego, gdzie można przeglądać i wykonaj zalecane działania infrastruktury programu SCOM.

![Kafelek rozwiązania programu System Center Operations Manager](./media/log-analytics-scom-assessment/scom-tile.png)

![Pulpit nawigacyjny programu System Center Operations Manager oceny](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązania

Rozwiązanie współpracuje z programu Microsoft System Operations Manager 2012 R2 i 2012 z dodatkiem SP1.

Skorzystaj z poniższych informacji, aby zainstalować i skonfigurować rozwiązania.

 - Zanim użyjesz rozwiązanie do oceny w pakietu OMS musi mieć zainstalowane oprogramowanie. Instalowanie rozwiązania z [witrynę Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) lub zgodnie z instrukcjami w [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md).

 - Po dodaniu rozwiązania do obszaru roboczego, System Center Operations Manager oceny kafelka na pulpicie nawigacyjnym wyświetli komunikat wymagana dodatkowa konfiguracja. Kliknij Kafelek i wykonaj kroki konfiguracji wymienione na stronie

 ![Kafelka pulpitu nawigacyjnego programu System Center Operations Manager](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 Konfiguracji programu System Center Operations Manager można zrobić za pomocą skryptu, wykonując kroki wymienione na stronie Konfiguracja rozwiązania w OMS.

 Zamiast tego, aby skonfigurować oceny za pośrednictwem konsoli programu SCOM, wykonaj następujące czynności w tej samej kolejności
1. [Ustawianie konta Uruchom jako System Center Operations Manager oceny](#operations-manager-run-as-accounts-for-oms)  
2. [Skonfiguruj regułę System Center Operations Manager oceny](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>Szczegóły danych oceny System Center Operations Manager w kolekcji

Oceny programu System Center Operations Manager umożliwia zbieranie danych usługi WMI, rejestru danych, danych dziennika zdarzeń, danych programu Operations Manager za pomocą środowiska Windows PowerShell, zapytania SQL, za pomocą serwera, na którym włączono moduł zbierający informacje o pliku.

W poniższej tabeli przedstawiono metody kolekcji danych System Center Operations Manager oceny i jak często dane są zbierane przez agenta.

| Platformy | Bezpośrednie agenta | Agenta programu SCOM | Azure Storage | SCOM wymagane? | Dane agenta programu SCOM wysyłane za pośrednictwem grupy zarządzania | Częstotliwość kolekcji |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | | | | &#8226; | | 7 dni |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Uruchom jako konta programu Operations Manager dla OMS

Kompilacje OMS od pakietów administracyjnych dla obciążeń w celu zapewnienia wartość — Dodawanie usług. Poszczególnych obciążeń wymaga uprawnień specyficznego dla obciążenia uruchomione pakiety administracyjne w innym kontekście zabezpieczeń, takie jak konto domeny. Skonfiguruj konto Operations Manager uruchom jako, aby udostępnić informacji o poświadczeniach.

Skorzystaj z poniższych informacji, aby ustawić konto Uruchom jako programu Operations Manager dla System Center Operations Manager oceny.

### <a name="set-the-run-as-account"></a>Ustawianie konta Uruchom jako

1. W konsoli programu Operations Manager, przejdź do **administracji** kartę.
2. W obszarze **Konfiguracja Uruchom jako**, kliknij przycisk **kont**.
3. Utwórz konto Uruchom jako, po za pomocą Kreatora tworzenia konta systemu Windows. Konto używane jest identyfikowane i mających wszystkie wymagania wstępne poniżej:

    >[!NOTE]
    Konto Uruchom jako musi spełniać następujące wymagania:
    - Domena konta członkiem lokalnej grupy administratorów na wszystkich serwerach w środowisku (wszystkie operacje menedżera ról — Serwer zarządzania, bazy danych programu Operations Manager, Magazyn danych raportowania, konsoli sieci Web, bramy)
    - Rola administratora menedżera operacji dla grupy zarządzania są oceniane
    - Wykonanie [skryptu](#sql-script-to-grant-granular-permissions-to-the-run-as-account) udzielenia szczegółowe uprawnienia do konta w wystąpieniu programu SQL używane przez program Operations Manager.
      Uwaga: Jeśli to konto ma już uprawnienia administratora systemu, następnie przejdź wykonania skryptu.

4. W obszarze **zabezpieczenia dystrybucji**, wybierz pozycję **bezpieczniejsze**.
5. Określ serwer zarządzania, które jest dystrybuowane konta.
3. Przejdź wstecz, aby konfiguracja Uruchom jako, a następnie kliknij przycisk **profile**.
4. Wyszukaj *SCOM oceny profilu*.
5. Nazwa profilu powinna być: *programu Microsoft System Center Advisor SCOM oceny profilu Uruchom jako*.
6. Kliknij prawym przyciskiem myszy i zaktualizuj jego właściwości i dodać ostatnio utworzone konto Uruchom jako utworzonego w kroku 3.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>Skrypt SQL, aby udzielić szczegółowe uprawnienia do konta Uruchom jako

Uruchom następujący skrypt SQL można udzielić wymaganych uprawnień do konta Uruchom jako w wystąpieniu programu SQL używane przez program Operations Manager.

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-the-assessment-rule"></a>Skonfiguruj regułę oceny

Pakiet administracyjny rozwiązania System Center Operations Manager oceny zawiera reguły o nazwie *Microsoft System Center Advisor SCOM oceny Uruchom oceny reguły*. Ta reguła jest odpowiedzialna za uruchamianie oceny wydajności. Aby włączyć reguły i skonfigurować częstotliwość, użyj poniższej procedury.

Domyślnie program Microsoft System Center Advisor SCOM oceny Uruchom oceny reguła jest wyłączona. Aby uruchomić oceny, należy włączyć reguły na serwerze zarządzania. Wykonaj następujące kroki.

#### <a name="enable-the-rule-for-a-specific-management-server"></a>Włącz regułę dla serwera zarządzania określonymi

1. W **tworzenie** obszaru roboczego w konsoli programu Operations Manager, wyszukaj regułę *Microsoft System Center Advisor SCOM oceny Uruchom oceny reguły* w **reguły** okienko.
2. W wynikach wyszukiwania wybierz jedną, która zawiera tekst *typu: serwer zarządzania*.
3. Kliknij prawym przyciskiem myszy reguły, a następnie kliknij przycisk **zastępuje** > **dla konkretnego obiektu klasy: serwer zarządzania**.
4.  Na liście serwerów dostępnych zarządzania wybierz serwer zarządzania, w którym reguła ma zostać uruchomiona.
5.  Zmień wartość zastąpienia **True** dla **włączone** wartość parametru.  
    ![zastąpienie parametru](./media/log-analytics-scom-assessment/rule.png)

Podczas nadal w tym oknie skonfiguruj częstotliwość uruchamiania, wykonując następną procedurę.

#### <a name="configure-the-run-frequency"></a>Skonfiguruj częstotliwość uruchamiania

Ocena jest skonfigurowany do uruchamiania co 10 080 minut (lub siedem dni), domyślnym interwałem. Można zastąpić wartość minimalną wartość 1440 minut (lub jeden dzień). Wartość reprezentuje przerwę minimalny czas między oszacowanie działa. Aby zastąpić interwał, skorzystaj z poniższych czynności.

1. W **tworzenie** obszaru roboczego w konsoli programu Operations Manager, wyszukaj regułę *Microsoft System Center Advisor SCOM oceny Uruchom oceny reguły* w **reguły** okienko.
2. W wynikach wyszukiwania wybierz jedną, która zawiera tekst *typu: serwer zarządzania*.
3. Kliknij prawym przyciskiem myszy reguły, a następnie kliknij przycisk **Zastąp zasadę** > **dla wszystkich obiektów klasy: serwer zarządzania**.
4. Zmień **interwał** wartość parametru na wartość dla żądanego interwału. W poniższym przykładzie wartość jest równa 1440 minut (jeden dzień).  
    ![Parametr interwał](./media/log-analytics-scom-assessment/interval.png)  
    Jeśli wartość jest mniejsza niż 1440 minut, gdy zasada jest wykonywana w odstępach czasu w ciągu jednego dnia. W tym przykładzie reguła ignoruje wartość interwału i jest uruchamiany z częstotliwością jeden dzień.


## <a name="understanding-how-recommendations-are-prioritized"></a>Opis sposobu mają pierwszeństwo zalecenia

Każdy zalecenie wprowadzone otrzymuje wartość wagi, która identyfikuje względnego zalecenia. 10 najważniejszych zalecenia są wyświetlane.

### <a name="how-weights-are-calculated"></a>Jak są obliczane wagi

Wagi są wartości zagregowanych oparte na trzech kluczowych czynników:

- *Prawdopodobieństwo* czy problemu może powodować problemy. Większe prawdopodobieństwo równa większych ogólny wynik dla zalecenia.
- *Wpływ* problemu w Twojej organizacji, jeśli spowodować problem. Większy wpływ równa większych ogólny wynik dla zalecenia.
- *Nakładu* wymagane do wykonania zalecenia. Wyższy nakładu równa mniejszych ogólny wynik dla zalecenia.

Wagi dla każde zalecenie jest wyrażona jako procent całkowitej wynik dostępne dla każdego obszaru fokus. Na przykład jeśli zalecenia w obszarze fokus dostępność i ciągłość prowadzenia działalności biznesowej ma wynik % 5, wdrażanie tego zalecenia zwiększa z ogólną % wynik przez 5 dostępność i ciągłość prowadzenia działalności biznesowej.

### <a name="focus-areas"></a>Obszarach zainteresowań

**Dostępność i ciągłość prowadzenia działalności biznesowej** — w tym obszarze fokus zawiera zalecenia dotyczące odporności infrastruktury i ochronę biznesowej, dostępności usług.

**Wydajność i skalowalność** — w tym obszarze fokus zawiera zalecenia ułatwiające organizacji infrastruktury IT zwiększania, upewnij się, że spełnia bieżące wymagania dotyczące wydajności środowiska informatycznego i jest w stanie reagować na zmieniające się infrastruktury wymagania związane z.

**Uaktualnienie, migrację i wdrażanie** — w tym obszarze fokus zawiera zalecenia ułatwiające uaktualniania, migracji i wdrożenia programu SQL Server w istniejącej infrastrukturze.

**Operacje i monitorowanie** — w tym obszarze fokus zawiera zalecenia ułatwiające usprawnić operacji IT, wdrożenia program prewencyjnej konserwacji i zmaksymalizować wydajność.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Należy dążyć wynik 100% w każdym obszarze fokus?

Niekoniecznie. Zalecenia są oparte na wiedzy i doświadczeń zdobytych Microsoft inżynierowie między tysięcy wizyt klienta. Jednak nie infrastruktury serwerowe są takie same i szczegółowe zalecenia mogą być bardziej lub mniej istotne dla Ciebie. Na przykład niektóre zalecenia dotyczące zabezpieczeń może być mniej ważne, jeśli maszyny wirtualne nie są widoczne w Internecie. Kilka zaleceń dostępności może być mniej istotne dla usług, które umożliwiają zbieranie danych ad hoc — niski priorytet i raportowania. Problemy, które są ważne dla dorosłych firm może być mniej ważne do rozruchu. Można zidentyfikować obszary fokus, które mają zebranych i przyjrzyj się jak zmienić wyniki wraz z upływem czasu.

Każdy zalecenie obejmuje wskazówek dotyczących Dlaczego ważne jest. Skorzystać z poniższych wskazówek do oceny, czy wdrażanie zalecenie jest odpowiednie dla Ciebie, biorąc pod uwagę rodzaj usług informatycznych i potrzeb biznesowych w organizacji.

## <a name="use-assessment-focus-area-recommendations"></a>Użyj zaleceń obszaru fokus oceny

Zanim użyjesz rozwiązanie do oceny w pakietu OMS musi mieć zainstalowane oprogramowanie. Aby uzyskać więcej informacji na temat instalowania rozwiązań, zobacz [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md). Po jego zainstalowaniu, można wyświetlić podsumowanie zaleceń za pomocą kafelka System Center Operations Manager oceny na stronie Przegląd w OMS.

Wyświetl oceny zgodności podsumowania dla Twojej infrastruktury, a następnie wejdź do zalecenia.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aby wyświetlić zalecenia dla obszaru fokus i podjąć działania naprawcze

1. Na **omówienie** kliknij przycisk **System Center Operations Manager oceny** kafelka.
2. Na **System Center Operations Manager oceny** strony, sprawdź informacje w jednym z bloków obszaru fokus, a następnie kliknij przycisk jedna, aby wyświetlić zalecenia dla tego obszaru fokus.
3. Na wszystkich stronach obszaru fokus można wyświetlić priorytetową zalecenia dla danego środowiska. Kliknij zalecenie, w obszarze **dotyczy obiektów** Aby wyświetlić szczegóły dotyczące Dlaczego tworzone są zalecenia.  
    ![obszar fokus](./media/log-analytics-scom-assessment/focus-area.png)
4. Należy wykonać działania naprawcze sugerowane w **sugerowanych akcji**. Jeśli element został rozwiązany, ocen nowsze zarejestruje które zalecane akcje zostały pobrane i zwiększa wynik zgodności. Poprawione elementy są wyświetlane jako **przekazany obiektów**.

## <a name="ignore-recommendations"></a>Ignoruj zalecenia

Jeśli masz zaleceń, które chcesz zignorować, można utworzyć pliku tekstowego, który używa OMS zapobiegające zaleceń znajdujących się w wynikach oceny.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>Aby zidentyfikować zaleceń, które chcesz zignorować

1. Zaloguj się do swojego obszaru roboczego i Otwórz dziennik wyszukiwania. Użyj następującego zapytania do listy zaleceń, które nie powiodły, na komputerach w danym środowisku.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Poniżej przedstawiono zrzut ekranu przedstawiający zapytania wyszukiwania dziennika:  
    ![przeszukiwanie dzienników](./media/log-analytics-scom-assessment/scom-log-search.png)

2. Wybierz zaleceń, które chcesz zignorować. Użyjesz wartości RecommendationId w następnej procedurze.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Tworzenie i używanie IgnoreRecommendations.txt pliku tekstowego

1. Utwórz plik o nazwie IgnoreRecommendations.txt.
2. Wklej lub wpisz każdego RecommendationId dla każde zalecenie, które mają OMS można zignorować w osobnym wierszu, a następnie zapisz i zamknij plik.
3. Umieść plik w następującym folderze na każdym komputerze, w którym ma OMS zignorowanie zalecenia.
4. Na serwerze zarządzania programu Operations Manager — *SystemDrive*: System Center 2012 R2\Operations Manager\Server \Program Files\Microsoft.

### <a name="to-verify-that-recommendations-are-ignored"></a>Aby zweryfikować, że zalecenia są ignorowane

1. Po na następną zaplanowaną ocenę działa, domyślnie co siedem dni, określonego zalecenia są oznaczane ignorowany i nie będą wyświetlane na pulpicie nawigacyjnym oceny.
2. Następujące zapytania wyszukiwania dziennika służy do tworzenia listy wszystkich zignorowane zalecenia.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. Jeśli później zdecydujesz chcesz zobaczyć zignorowane zalecenia dotyczące, Usuń wszystkie pliki IgnoreRecommendations.txt lub RecommendationIDs można usunąć z nich.

## <a name="system-center-operations-manager-assessment-solution-faq"></a>System Center Operations Manager oceny rozwiązania — często zadawane pytania

*Rozwiązanie do oceny I dodane do mojego obszaru roboczego OMS. Nie widzę zalecenia. Dlaczego nie?* Po dodaniu rozwiązania, użyj następującego widoku kroki zaleceń na pulpicie nawigacyjnym OMS.  

- [Ustawianie konta Uruchom jako System Center Operations Manager oceny](#operations-manager-run-as-accounts-for-oms)  
- [Skonfiguruj regułę System Center Operations Manager oceny](#configure-the-assessment-rule)


*Czy istnieje sposób konfigurowania, jak często jest uruchamiana oceny?* Tak. Zobacz [skonfiguruj częstotliwość uruchamiania](#configure-the-run-frequency).

*Jeśli inny serwer zostanie odnaleziony po rozwiązaniu System Center Operations Manager oceny zostały dodane, zostanie on oceniane?* Tak, po odnajdywaniu, ocenia się od — domyślnie co siedem dni.

*Co to jest nazwa procesu, który wykonuje zbierania danych?* AdvisorAssessment.exe

*Gdy jest uruchamiany proces AdvisorAssessment.exe?* AdvisorAssessment.exe uruchamiana usługi kondycji serwera zarządzania, jeśli jest włączona zasada oceny. Za pomocą tego procesu, odnajdywania całego środowiska odbywa się za pośrednictwem zdalne zbieranie danych.

*Jak długo trwa zbierania danych?* Zbieranie danych na serwerze trwa około jednej godziny. Może trwać dłużej w środowiskach mających wiele wystąpień programu Operations Manager lub baz danych.

*Co zrobić, jeśli do mniej niż 1440 minut ustawić interwał oceny?* Ocena jest wstępnie skonfigurowana do uruchamiania na maksymalnie raz dziennie. Jeśli zastąpić wartość interwału do mniej niż 1440 minut, następnie oceny używa 1440 minut jako wartość interwału.

*Jak sprawdzić, czy występują błędy wstępnych?* Jeśli nie widzisz wyniki uruchomiono oceny, następnie jest prawdopodobne, że niektóre wymagania wstępne dotyczące oceny nie powiodło się. Można wykonać zapytania: `Type=Operation Solution=SCOMAssessment` i `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` dziennik wyszukiwania, aby sprawdzić wymagania wstępne nie powiodło się.

*Brak `Failed to connect to the SQL Instance (….).` komunikat wstępnych błędy. Co to jest problem?* AdvisorAssessment.exe, proces, która gromadzi dane, jest uruchamiana usługi kondycji serwera zarządzania. W ramach oceny proces próbuje połączyć się z serwerem SQL, gdzie występuje bazy danych programu Operations Manager. Ten błąd może wystąpić, jeśli reguły zapory blokowanie połączenia z wystąpieniem programu SQL Server.

*Jakiego typu dane są zbierane?* Zbierane są następujące typy danych: danych programu Operations Manager — dane usługi WMI — rejestru danych — dane w dzienniku zdarzeń — za pośrednictwem plików, zapytania SQL i programu Windows PowerShell moduł zbierający informacje o.

*Dlaczego trzeba skonfigurować konto Uruchom jako?* W przypadku serwera programu Operations Manager są uruchamiane różne zapytania SQL. Aby je uruchomić należy użyć konto Uruchom jako z niezbędne uprawnienia. Ponadto aby kwerenda WMI wymagane są poświadczenia administratora lokalnego.

*Dlaczego są wyświetlane tylko zalecenia 10 pierwszych?* Zamiast daje wyczerpująca, utrudnione listę zadań, zaleca się skupić się na najpierw adresowania priorytetową zalecenia. Po adresu im dodatkowe zalecenia staną się dostępne. Jeśli wolisz wyświetlić listę szczegółowych, można wyświetlić wszystkie zalecenia za pomocą wyszukiwania dziennika.

*Czy istnieje sposób, aby zignorować zalecenie?* Tak, zobacz [zignorowanie zalecenia](#Ignore-recommendations).


## <a name="next-steps"></a>Następne kroki

- [Wyszukaj dzienniki](log-analytics-log-searches.md) do wyświetlenia szczegółowych danych System Center Operations Manager oceny i zalecenia.
