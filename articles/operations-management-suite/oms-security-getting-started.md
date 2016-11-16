---
title: "Wprowadzenie do rozwiązania Zabezpieczenia i inspekcja w pakiecie Operations Management Suite | Microsoft Docs"
description: "Ten dokument ułatwia rozpoczęcie korzystania z rozwiązania Zabezpieczenia i inspekcja w pakiecie Operations Management Suite w celu monitorowania chmury hybrydowej."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 754796ef-a43e-468a-86c9-04a2eda55b5b
ms.service: operations-management-suite
ms.topic: get-started-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: fe043cd4438612ada9a31ae895aae75b913b0a37


---
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Wprowadzenie do korzystania z rozwiązania Zabezpieczenia i inspekcja w pakiecie Operations Management Suite
Ten dokument pomaga szybko rozpocząć korzystanie z rozwiązania Zabezpieczenia i inspekcja w pakiecie Operations Management Suite (OMS), objaśniając działanie poszczególnych opcji.

## <a name="what-is-oms"></a>Co to jest pakiet OMS?
Pakiet Microsoft Operations Management Suite (OMS) to oparte na chmurze rozwiązanie firmy Microsoft do zarządzania systemami IT, które ułatwia zarządzanie infrastrukturą lokalną i chmurową oraz jej ochronę. Aby uzyskać więcej informacji na temat pakietu OMS, przeczytaj artykuł [Omówienie pakietu Operations Management Suite (OMS)](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Pulpit nawigacyjny Zabezpieczenia i inspekcja w pakiecie OMS
Rozwiązanie Zabezpieczenia i inspekcja w pakiecie OMS zapewnia kompleksowy wgląd w stan zabezpieczeń IT organizacji za pomocą wbudowanych zapytań wyszukujących istotne problemy, które wymagają uwagi. Pulpit nawigacyjny **Zabezpieczenia i inspekcja** jest głównym ekranem prezentujące wszystkie funkcje pakietu OMS związane z bezpieczeństwem. Zawiera ogólne informacje o stanie zabezpieczeń komputerów. Daje również możliwość wyświetlenia wszystkich zdarzeń z ostatnich 24 godzin, 7 dni lub dowolnego innego przedziału czasu. Aby uzyskać dostęp do pulpitu nawigacyjnego **Zabezpieczenia i inspekcja**, wykonaj następujące kroki:

1. Na głównym pulpicie nawigacyjnym pakietu **Microsoft Operations Management Suite** kliknij kafelek **Ustawienia** po lewej stronie.
2. W bloku **Ustawienia**, w obszarze **Rozwiązania** kliknij opcję **Zabezpieczenia i inspekcja**.
3. Pojawi się pulpit nawigacyjny **Zabezpieczenia i inspekcja**:
   
    ![Pulpit nawigacyjny Zabezpieczenia i inspekcja w pakiecie OMS](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Jeśli korzystasz z tego pulpitu nawigacyjnego po raz pierwszy i nie monitorujesz jeszcze żadnych urządzeń przy użyciu rozwiązania OMS, kafelki nie będą wypełnione danymi uzyskanymi od agenta. Po zainstalowaniu agenta może minąć trochę czasu, zanim dane pojawią się na pulpicie nawigacyjnym. W związku z tym początkowo niektóre informacje nie będą widoczne, dopóki nie zostaną przesłane do chmury.  W takiej sytuacji jest normalnym zjawiskiem, że niektóre kafelki nie zawierają wymiernych informacji. Artykuł [Connect Windows computers directly to OMS](https://technet.microsoft.com/library/mt484108.aspx) (Podłączanie komputerów z systemem Windows bezpośrednio do pakietu OMS) zawiera więcej informacji na temat sposobu instalowania agenta OMS w systemie Windows. Artykuł [Connect Linux computers to OMS](https://technet.microsoft.com/library/mt622052.aspx) (Podłączanie komputerów z systemem Linux do pakietu OMS) zawiera więcej informacji dotyczących wykonywania tego zadania w systemie Linux.

> [!NOTE]
> Agent będzie zbierać informacje oparte na bieżących włączonych zdarzeniach, takie jak nazwa komputera, nazwa użytkownika i adres IP. Nie są jednak gromadzone żadne dokumenty bądź pliki, nazwy baz danych ani dane prywatne.   
> 
> 

Rozwiązania to zestawy reguł logiki, wizualizacji i gromadzenia danych opracowane w celu pokonywania kluczowych wyzwań klienta. Zabezpieczenia i inspekcja to jedno z takich rozwiązań — inne można dodać oddzielnie. Przeczytaj artykuł [Add solutions](https://technet.microsoft.com/library/mt674635.aspx) (Dodawanie rozwiązań), aby uzyskać więcej informacji na temat dodawania nowego rozwiązania.

Pulpit nawigacyjny Zabezpieczenia i inspekcja w pakiecie OMS jest podzielony na cztery główne kategorie:

* **Domeny zabezpieczeń**: w tym obszarze można bardziej szczegółowo przeglądać historię zabezpieczeń. Zapewnia on też dostęp do ocen złośliwego oprogramowania i aktualizacji, zabezpieczeń sieci, informacji o tożsamości i dostępie, komputerów ze zdarzeniami bezpieczeństwa, a także szybki dostęp do pulpitu nawigacyjnego Azure Security Center.
* **Problemy godne uwagi**: ta opcja umożliwia szybkie ustalenie liczby aktywnych problemów i ich ważności.
* **Wykrycia (podgląd)**: umożliwia określenie schematów ataku przez wizualizację alertów zabezpieczeń dotyczących poszczególnych zasobów.
* **Analiza zagrożeń**: umożliwia określenie schematów ataku przez wizualizację całkowitej liczby serwerów z wychodzącym złośliwym ruchem IP, typu złośliwych zagrożeń oraz mapy pochodzenia tych adresów IP. 
* **Typowe zapytania dotyczące zabezpieczeń**: ta opcja zawiera listę najbardziej typowych zapytań zabezpieczeń, których można używać do monitorowania środowiska. Po kliknięciu jednego z zapytań pojawia się blok **Wyszukiwanie** z wynikami tego zapytania.

> [!NOTE]
> Aby uzyskać więcej informacji o sposobach zapewniania bezpieczeństwa danych przez pakiet OMS, przeczytaj artykuł How OMS secures your data (W jaki sposób pakiet OMS chroni dane?).
> 
> 

## <a name="security-domains"></a>Domeny zabezpieczeń
Podczas monitorowania zasobów ważna jest możliwość szybkiej oceny bieżącego stanu środowiska. Jednak warto również śledzić zdarzenia, które wystąpiły w przeszłości, a mogą prowadzić do lepszego zrozumienia tego, co dzieje się w środowisku w danym momencie. 

> [!NOTE]
> Przechowywanie danych jest realizowane zgodnie z cennikiem pakietu OMS. Aby uzyskać więcej informacji, odwiedź stronę z informacjami o cenach pakietu [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx).
> 
> 

Scenariusze reagowania na zdarzenia i ich analizowania odniosą bezpośrednie korzyści z informacji dostępnych na kafelku **Rekordy zabezpieczeń w czasie**.

![Rekordy zabezpieczeń w czasie](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Po kliknięciu tego kafelka pojawi się blok **Wyszukiwanie** przedstawiający wyniki zapytania **Zdarzenia zabezpieczeń** (Type=SecurityEvents) na podstawie danych z ostatnich siedmiu dni, jak pokazano poniżej:

![Rekordy zabezpieczeń w czasie](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Wyniki wyszukiwania są podzielone na dwa okienka: okienko po lewej stronie zawiera zestawienie liczby znalezionych zdarzeń zabezpieczeń, komputerów, na których zostały one wykryte, liczby kont odnalezionych na tych komputerach oraz typów działań. Prawe okienko zawiera całkowitą liczbę wyników i chronologiczny widok zdarzeń zabezpieczeń z nazwą komputera i działaniem zdarzenia. Możesz również kliknąć opcję **Pokaż więcej**, aby wyświetlić więcej szczegółów na temat danego zdarzenia, takich jak dane dotyczące zdarzenia, identyfikator zdarzenia i źródło zdarzenia.

> [!NOTE]
> Więcej informacji na temat zapytania wyszukiwania pakietu OMS zawiera artykuł [OMS search reference](https://technet.microsoft.com/library/mt450427.aspx) (Wprowadzenie do wyszukiwania w pakiecie OMS).
> 
> 

### <a name="antimalware-assessment"></a>Ocena oprogramowania chroniącego przed złośliwym kodem
Ta opcja umożliwia szybką identyfikację komputerów z niedostateczną ochroną i komputerów, których zabezpieczenia zostały naruszone na skutek działania złośliwego kodu. Informacje o stanie oceny złośliwego oprogramowania i wykrytych zagrożeń na monitorowanych serwerach są odczytywane i przesyłane do usługi OMS w chmurze w celu przetworzenia. Serwery z wykrytymi zagrożeniami lub niedostateczną ochroną są widoczne na pulpicie nawigacyjnym oceny oprogramowania chroniącego przed złośliwym kodem, który jest dostępny po kliknięciu kafelka **Ocena oprogramowania chroniącego przed złośliwym kodem**. 

![Ocena oprogramowania chroniącego przed złośliwym kodem](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Podobnie jak w przypadku innych kafelków dynamicznych dostępnych na pulpicie nawigacyjnym pakietu OMS, po kliknięciu kafelka pojawia się blok **Wyszukiwanie** z wynikami zapytania. Jeśli klikniesz opcję **Brak zgłaszania** w obszarze **Stan ochrony**, zostanie wyświetlony wynik zapytania z jednym wpisem zawierającym nazwę i rangę komputera, jak widać poniżej:

![Wynik wyszukiwania](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [!NOTE]
> *Ranga* to wartość podsumowująca stan ochrony (włączona, wyłączona, zaktualizowana itp.) oraz znalezione zagrożenia. Taka liczba ułatwia agregację danych.
> 
> 

Gdy klikniesz nazwę komputera, pojawi się chronologiczny widok stanu ochrony danego komputera. Jest to bardzo przydatne w scenariuszach, w których należy ustalić, czy oprogramowanie chroniące przed złośliwym kodem było wcześniej zainstalowane, a w pewnym momencie zostało usunięte.   

### <a name="update-assessment"></a>Ocena aktualizacji
Ta opcja umożliwia szybkie ustalenie ogólnego narażenia na potencjalne problemy z zabezpieczeniami oraz stopnia krytyczności tych problemów dla danego środowiska. Rozwiązanie Zabezpieczenia i inspekcja w pakiecie OMS zapewnia tylko wizualizację tych aktualizacji. Rzeczywiste dane pochodzą z rozwiązania [Aktualizacje systemu](https://technet.microsoft.com/library/mt484096.aspx), które jest oddzielnym modułem pakietu OMS. Oto przykład aktualizacji:

![Aktualizacje systemu](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [!NOTE]
> Więcej informacji na temat rozwiązania Aktualizacje systemu zawiera artykuł [Update servers with the System Updates solution](https://technet.microsoft.com/library/mt484096.aspx) (Aktualizowanie serwerów przy użyciu rozwiązania Aktualizacje systemu).
> 
> 

### <a name="identity-and-access"></a>Tożsamość i dostęp
Tożsamość powinna być warstwą kontroli w przedsiębiorstwie. Ochrona tożsamości powinna stanowić najwyższy priorytet. W przeszłości organizacja była otoczona granicami pełniącymi rolę podstawowych stref obronnych. Obecnie jednak coraz więcej danych i aplikacji jest przenoszonych do chmury, przez co nową strefą ochronną staje się tożsamość. 

> [!NOTE]
> Obecnie dane gromadzone są tylko na podstawie danych logowania zdarzeń zabezpieczeń (zdarzenie o identyfikatorze 4624). W przyszłości uwzględniane będą też dane logowania usług Office 365 i Azure AD.
> 
> 

Dzięki monitorowaniu działań związanych z tożsamością można podejmować prewencyjne działania przed wystąpieniem zdarzenia i reagować na próby ataku. Pulpit nawigacyjny **Tożsamość i dostęp** zawiera przegląd stanu tożsamości, w tym liczbę nieudanych prób logowania, konta użytkowników wykorzystywane podczas tych prób, konta zablokowane, konta, dla których zmieniono lub zresetowano hasło, a także liczbę kont aktualnie zalogowanych użytkowników. 

Po kliknięciu kafelka **Tożsamość i dostęp** pojawi się następujący pulpit nawigacyjny:

![Tożsamość i dostęp](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Informacje zawarte na tym pulpicie nawigacyjnym mogą bezpośrednio pomóc w zidentyfikowaniu potencjalne podejrzanych działań. Widać na przykład 338 prób logowania jako **Administrator**, z których 100% zakończyło się niepowodzeniem. Może to być spowodowane atakiem siłowym na to konto. Po kliknięciu tego konta zostanie wyświetlonych więcej informacji, które mogą pomóc w ustaleniu zasobu docelowego tego potencjalnego ataku:

![Wyniki wyszukiwania](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Raport szczegółowy zawiera ważne informacje dotyczące tego zdarzenia, w tym: komputer docelowy, typ logowania (w tym przypadku logowanie sieciowe), działanie (w tym przypadku zdarzenie o identyfikatorze 4625) oraz szczegółowy przebieg prób w czasie. 

### <a name="computers"></a>Komputery
Ten kafelek umożliwia dostęp do wszystkich komputerów, na których istnieją aktywne zdarzenia zabezpieczeń. Po kliknięciu tego kafelka pojawia się lista komputerów ze zdarzeniami zabezpieczeń oraz liczba zdarzeń na każdym komputerze:

![Komputery](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Można kontynuować badania, klikając wybrany komputer i przeglądając oznaczone zdarzenia zabezpieczeń.

### <a name="azure-security-center"></a>Azure Security Center
Ten kafelek jest po prostu skrótem do pulpitu nawigacyjnego Azure Security Center. Więcej informacji na temat tego rozwiązania zawiera artykuł [Przewodnik Szybki start dotyczący Centrum zabezpieczeń Azure](../security-center/security-center-get-started.md).

## <a name="notable-issues"></a>Problemy godne uwagi
Głównym przeznaczeniem tej grupy opcji jest zapewnianie szybkiego wglądu w problemy występujące w danym środowisku oraz ich podział na problemy krytyczne, ostrzegawcze oraz informacyjne. Kafelek typu aktywnego problemu oferuje wizualizację tych problemów, ale nie podaje szczegółowych informacji. Służy do tego dolna część tego kafelka, na której widać nazwę problemu (NAZWA), liczbę obiektów, których dotyczy (LICZBA) i stopień ich ważności (WAŻNOŚĆ).

![Problemy godne uwagi](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Widać, że te problemy zostały już przedstawione w innych obszarach grupy **Domeny zabezpieczeń**, co podkreśla przeznaczenie tego widoku, jakim jest wizualizacja najważniejszych problemów w środowisku w jednym miejscu.

## <a name="detections-preview"></a>Wykrycia (podgląd)
Ta opcja służy przede wszystkim do szybkiego identyfikowania potencjalnych zagrożeń w środowisku oraz określania ich ważności.

![Analiza zagrożeń](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Z tej opcji można również skorzystać podczas analizy reakcji na zdarzenia w celu przeprowadzenia oceny i uzyskania dalszych informacji o ataku.

> [!NOTE]
> Aby uzyskać więcej informacji na temat reagowania na zdarzenia przy użyciu pakietu OMS, obejrzyj prezentację [How to Leverage the Azure Security Center & Microsoft Operations Management Suite for an Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) (Jak reagować na zdarzenia przy użyciu rozwiązania Azure Security Center i pakietu Microsoft Operations Management Suite).
> 
> 

## <a name="threat-intelligence"></a>Analiza zagrożeń
Nowa sekcja analizy zagrożeń w rozwiązaniu Zabezpieczenia i inspekcja umożliwia wizualizację możliwych schematów ataku na kilka sposobów: całkowita liczba serwerów z wychodzącym złośliwym ruchem IP, typ złośliwego kodu oraz mapa pochodzenia tych adresów IP. Mapa jest interaktywna: klikając adres IP, można uzyskać więcej informacji.

Żółte pinezki na mapie wskazują ruch przychodzący ze złośliwych adresów IP. Nie jest niczym niezwykłym, że serwery połączone z Internetem odbierają złośliwy ruch przychodzący, ale warto przeglądać te próby, aby upewnić się, że żadna z nich nie zakończyła się pomyślnie. Wskaźniki te są oparte na dziennikach usług IIS i zapory systemu Windows oraz danych WireData.  

![Analiza zagrożeń](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Typowe zapytania dotyczące zabezpieczeń
Lista typowych zapytań dotyczących zabezpieczeń może ułatwiać szybki dostęp do informacji o zasobach i dostosowanie ich do wymagań danego środowiska. Typowe zapytania obejmują:

* Wszystkie działania dotyczące zabezpieczeń
* Działania dotyczące zabezpieczeń na komputerze „computer01.contoso.com” (zastąp własną nazwą komputera)
* Działania dotyczące zabezpieczeń na komputerze „computer01.contoso.com” dla konta „Administrator” (zastąp własnymi nazwami komputera i konta)
* Działania logowania według komputera
* Konta, przy użyciu których przerwano pracę oprogramowania firmy Microsoft chroniącego przed złośliwym kodem na dowolnym komputerze
* Komputery, na których proces oprogramowania firmy Microsoft chroniącego przed złośliwym kodem został przerwany
* Komputery, na których wykonano plik „hash.exe” (zastąp inną nazwą procesu)
* Nazwy wszystkich wykonanych procesów
* Działania logowania według konta
* Konta, przy użyciu których wykonano zdalne logowanie do komputera „computer01.contoso.com” (zastąp własną nazwą komputera)

## <a name="see-also"></a>Zobacz też
Ten dokument przedstawia wprowadzenie do rozwiązania Zabezpieczenia i inspekcja w pakiecie OMS. Więcej informacji na temat zabezpieczeń w pakiecie OMS zawierają następujące artykuły:

* [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorowanie alertów zabezpieczeń i reagowanie na nie w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorowanie zasobów w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-monitoring-resources.md)




<!--HONumber=Nov16_HO2-->


