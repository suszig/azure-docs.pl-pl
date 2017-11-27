---
title: "Alerty zabezpieczeń według typu w usłudze Azure Security Center | Microsoft Docs"
description: "W tym artykule omówiono różne rodzaje alertów zabezpieczeń dostępnych w usłudze Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2017
ms.author: yurid
ms.openlocfilehash: 829657664cf1e37b22d57c62614300a205b5e91c
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Informacje o alertach zabezpieczeń w usłudze Azure Security Center
Ten artykuł ułatwia zapoznanie się z różnymi typami alertów zabezpieczeń i powiązanych szczegółowych informacji dostępnych w usłudze Azure Security Center. Więcej informacji na temat zarządzania alertami i zdarzeniami znajduje się w artykule [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md).

Aby skonfigurować wykrywanie zaawansowane, przeprowadź uaktualnienie usługi Azure Security Center do wersji Standard. Dostępna jest bezpłatna 60-dniowa wersja próbna. W celu uaktualnienia wybierz pozycję **Warstwa cenowa** w obszarze [Zasady zabezpieczeń](security-center-policies.md). Aby dowiedzieć się więcej, zobacz [stronę z cennikiem](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> Usługa Security Center została wydana w ograniczonej wersji zapoznawczej z nowym zestawem funkcji wykrywania, które wykorzystują rekordy inspekcji, czyli wspólną platformę inspekcji, do wykrywania złośliwych zachowań na maszynach z systemem Linux. Wyślij [nam](mailto:ASC_linuxdetections@microsoft.com) wiadomość e-mail ze swoim identyfikatorem subskrypcji, aby dołączyć do tej wersji zapoznawczej.

## <a name="what-type-of-alerts-are-available"></a>Jakie typy alertów są dostępne?
Usługa Azure Security Center używa różnych [funkcji wykrywania](security-center-detection-capabilities.md), aby ostrzec klientów przed potencjalnymi atakami wymierzonymi w ich środowiska. Te alerty zawierają cenne informacje dotyczące przyczyny ich wyzwolenia, zasobów, których dotyczy atak, i źródła ataku. Informacje zawarte w alercie różnią się w zależności od typu analizy użytej do wykrycia zagrożenia. Zdarzenia mogą również zawierać dodatkowe informacje kontekstowe przydatne podczas badania zagrożenia.  Ten artykuł zawiera informacje na temat następujących typów alertów:

* Analiza zachowania maszyny wirtualnej (VMBA)
* Analiza sieci
* Analiza zasobów
* Informacje kontekstowe

## <a name="virtual-machine-behavioral-analysis"></a>Analiza zachowania maszyny wirtualnej
Usługa Azure Security Center może użyć analizy behawioralnej w celu identyfikacji zasobów, których bezpieczeństwo zostało naruszone, na podstawie analizy dzienników zdarzeń maszyny wirtualnej, na przykład zdarzeń tworzenia procesów i zdarzeń logowania. Ponadto istnieje korelacja z innymi sygnałami, które wykrywają dowody potwierdzające istnienie szeroko zakrojonej kampanii ataku.

> [!NOTE]
> Aby uzyskać więcej informacji na temat sposobu działania funkcji wykrywania usługi Security Center, zobacz [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md).
>

### <a name="crash-analysis"></a>Analiza awarii
Analiza awaryjnego zrzutu pamięci jest metodą używaną do wykrywania zaawansowanego złośliwego oprogramowania, które jest w stanie ominąć tradycyjne rozwiązania w zakresie zabezpieczeń. Różne rodzaje złośliwego oprogramowania próbują zmniejszyć prawdopodobieństwo wykrycia przez programy antywirusowe, rezygnując z zapisu na dysku lub szyfrując składniki oprogramowania zapisywane na dysku. Ta technika sprawia, że złośliwe oprogramowanie jest trudne do wykrycia przy użyciu tradycyjnych metod ochrony. Złośliwe oprogramowanie może jednak zostać wykryte za pomocą analizy pamięci, ponieważ aby mogło działać, musi zostawić ślady w pamięci maszyny.

Jeśli wystąpi awaria oprogramowania, zrzut awaryjny przechwytuje część pamięci w chwili pojawienia się awarii. Awaria może być spowodowana przez złośliwe oprogramowanie, powszechnie stosowane aplikacje lub problemy z systemem. Dzięki analizie pamięci w zrzucie awaryjnym usługa Security Center może wykryć metody, jakie zostały użyte w celu wykorzystania luk obecnych w oprogramowaniu, uzyskania dostępu do poufnych danych i dyskretnego pozostania na zainfekowanym komputerze. Jest to realizowane przy minimalnym wpływie na wydajność hostów, ponieważ analiza jest wykonywana przez zaplecze usługi Security Center.

Przykładowe alerty zrzutu awaryjnego, które zostały omówione w dalszej części tego artykułu, korzystają z następujących pól:

* DUMPFILE: nazwa pliku zrzutu awaryjnego.
* PROCESSNAME: nazwa procesu powodującego awarię.
* PROCESSVERSION: wersja procesu powodującego awarię.

### <a name="code-injection-discovered"></a>Wykryto iniekcję kodu
Iniekcja kodu to wstawianie modułów wykonywalnych do uruchomionych procesów lub wątków.  Ta metoda jest używana przez złośliwe oprogramowanie do uzyskiwania dostępu do danych, ukrywania swojej obecności lub zapobiegania usunięciu (np. trwałość). Ten alert wskazuje, że wprowadzony moduł znajduje się w zrzucie awaryjnym. Wiarygodni programiści czasami wykonują iniekcję kodu z niezłośliwych powodów, takich jak modyfikowanie lub rozbudowywanie istniejącej aplikacji albo składnika systemu operacyjnego.  Aby ułatwić odróżnienie niezłośliwych modułów od złośliwych, usługa Security Center sprawdza, czy wprowadzony moduł odpowiada profilowi podejrzanego zachowania. Wynik tej kontroli jest widoczny w polu alertu „SIGNATURE” i obejmuje ważność alertu, opis alertu oraz czynności zaradcze alertu. 

Ten alert zawiera następujące pola dodatkowe:

- ADDRESS: lokalizacja wprowadzonego modułu w pamięci
- IMAGENAME: nazwa wprowadzonego modułu. Zwróć uwagę, że to pole może być puste, jeśli obraz nie zawiera nazwy obrazu.
- SIGNATURE: pokazuje, czy wprowadzony moduł jest zgodny z profilem podejrzanego zachowania. 

W poniższej tabeli przedstawiono przykłady wyników i ich opisy:

| Wartość podpisu                      | Opis                                                                                                       |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Podejrzane wykorzystanie odpowiedniego modułu ładującego | To podejrzane zachowanie często jest skorelowane z ładowaniem wprowadzonego kodu niezależnie od modułu ładującego systemu operacyjnego |
| Podejrzane wykorzystanie wprowadzonego kodu          | Oznacza złośliwe działanie, które często jest skorelowane z wprowadzeniem kodu do pamięci                                       |
| Podejrzane wykorzystanie iniekcji         | Oznacza złośliwe działanie, które często jest skorelowane z użyciem wprowadzonego kodu w pamięci                                   |
| Podejrzane wykorzystanie wprowadzonego debugera | Oznacza złośliwe działanie, które często jest skorelowane z wykryciem lub obejściem debugera                         |
| Podejrzane zdalne wykorzystanie wprowadzonego kodu   | Oznacza złośliwe działanie, które często jest skorelowane ze scenariuszami kontrolki polecenia n (C2)                                 |

Oto przykład tego typu alertu:

![Alert o iniekcji kodu](./media/security-center-alerts-type/security-center-alerts-type-fig21.png)

### <a name="suspicious-code-segment"></a>Podejrzany segment kodu
Podejrzany segment kodu wskazuje, że segment kodu został przydzielony przy użyciu niestandardowych metod, takich jak metody używane przez iniekcję odbijającą i zamianę pamięci procesu.  Ponadto ten alert przetwarza dodatkową charakterystykę segment kodu, aby zapewnić kontekst dotyczący możliwości i zachowań zgłoszonego segmentu kodu.

Ten alert zawiera następujące pola dodatkowe:

- ADDRESS: lokalizacja wprowadzonego modułu w pamięci
- SIZE: rozmiar podejrzanego segmentu kodu
- STRINGSIGNATURES: to pole zawiera możliwości interfejsów API, których nazwy funkcji znajdują się w segmencie kodu. Przykładowe możliwości mogą obejmować:
    - Deskryptory sekcji obrazów, dynamiczne wykonanie kodu dla architektury x64, alokacja pamięci i możliwość modułu ładującego, możliwość zdalnej iniekcji kodu, możliwość sterowania przejęciami, zmienne środowiskowe odczytu, umowna pamięć procesu odczytu, wykonywanie zapytań lub modyfikowanie uprawnień tokenu, komunikacja sieci HTTP/HTTPS i komunikacja gniazd sieci.
- IMAGEDETECTED: to pole wskazuje, czy wprowadzono obraz PE do procesu, w którym wykryto podejrzany segment kodu, oraz adres początku wprowadzonego modułu.
- SHELLCODE: to pole wskazuje obecność zachowania powszechnie używanego przez złośliwe ładunki w celu uzyskania dostępu do dodatkowych funkcji systemu operacyjnego dotyczących zabezpieczeń. 

Oto przykład tego typu alertu:

![Alert dotyczący podejrzanego segmentu kodu](./media/security-center-alerts-type/security-center-alerts-type-fig22.png)

### <a name="shellcode-discovered"></a>Wykryto kod powłoki
Kod powłoki to ładunek uruchamiany po wykorzystaniu przez złośliwe oprogramowanie luki w zabezpieczeniach oprogramowania. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła zachowanie kodu wykonywalnego typowe dla złośliwych ładunków. Wprawdzie niezłośliwe oprogramowanie może zachowywać się podobnie, jednak nie jest to typowe w przypadku zwykłych metod tworzenia oprogramowania.

Alert kodu powłoki zawiera następujące pole dodatkowe:

* ADDRESS: lokalizacja kodu powłoki w pamięci.

Oto przykład tego typu alertu:

![Alert kodu powłoki](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Wykryto przejęcie modułu
System Windows używa bibliotek dołączanych dynamicznie (DLL, Dynamic Link Library), aby umożliwić oprogramowaniu korzystanie z typowych funkcji systemu Windows. Przejęcie biblioteki DLL ma miejsce, gdy złośliwe oprogramowanie zmienia kolejność ładowania bibliotek DLL, aby załadować złośliwy ładunek do pamięci, w której może zostać wykonany dowolny kod. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła moduł o podobnej nazwie, który jest ładowany z dwóch różnych ścieżek. Jedna z nich odpowiada typowej lokalizacji plików binarnych systemu Windows.

Wiarygodni programiści czasami zmieniają kolejność ładowania bibliotek DLL z niezłośliwych powodów, takich jak instrumentacja albo rozbudowa systemu operacyjnego Windows lub aplikacji. Aby ułatwić rozróżnienie złośliwych i potencjalnie niegroźnych zmian kolejności ładowania bibliotek DLL, usługa Azure Security Center sprawdza, czy załadowany moduł jest zgodny z podejrzanym profilem. Wynik tej kontroli jest widoczny w polu alertu „SIGNATURE” i obejmuje ważność alertu, opis alertu oraz czynności zaradcze alertu. Aby zbadać, czy moduł jest wiarygodny czy złośliwy, przeprowadź analizę kopii dyskowej przejmującego modułu. Możesz na przykład sprawdzić podpis cyfrowy pliku lub uruchomić skanowanie antywirusowe.

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola:

* SIGNATURE: pokazuje, czy przejmujący moduł jest zgodny z profilem podejrzanego zachowania.
* HIJACKEDMODULE: nazwa przejętego modułu systemu Windows.
* HIJACKEDMODULEPATH: ścieżka przejętego modułu systemu Windows.
* HIJACKINGMODULEPATH: ścieżka przejmującego modułu.

Oto przykład tego typu alertu:

![Alert o przejęciu modułu](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Wykryto zamaskowany moduł systemu Windows
Złośliwe oprogramowanie może używać typowych nazw plików binarnych systemu Windows (np. SVCHOST.EXE) lub modułów (np. NTDLL.DLL) w celu *ukrycia* charakteru złośliwego oprogramowania przed administratorami systemu. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła w pliku zrzutu awaryjnego moduły używające nazw modułów systemu Windows, które nie spełniają innych kryteriów typowych dla tego rodzaju modułów. Analiza kopii dyskowej zamaskowanego modułu może dostarczyć dodatkowych informacji na temat wiarygodności lub złośliwości tego modułu. Analiza może obejmować:

* Potwierdzenie, że dany plik jest dostarczany jako część wiarygodnego pakietu oprogramowania.
* Sprawdzenie podpisu cyfrowego pliku.
* Uruchomienie skanowania antywirusowego pliku.

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola dodatkowe:

* DETAILS: opisuje, czy metadane modułów są prawidłowe i czy moduł został załadowany ze ścieżki systemowej.
* NAME: nazwa zamaskowanego modułu systemu Windows.
* PATH: ścieżka zamaskowanego modułu systemu Windows

Ten alert wyodrębnia i wyświetla także niektóre pola z nagłówka PE modułu, takie jak „CHECKSUM” i „TIMESTAMP”. Te pola są wyświetlane tylko wtedy, gdy występują w module. Aby uzyskać szczegółowe informacje na temat tych pól, zobacz [specyfikację Microsoft PE i COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Oto przykład tego typu alertu:

![Alert o zamaskowanym elemencie systemu Windows](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Wykryto zmodyfikowany plik binarny systemu
Złośliwe oprogramowanie może modyfikować podstawowe pliki binarne systemu, aby potajemnie uzyskać dostęp do danych lub niezauważenie przetrwać w zaatakowanym systemie. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła modyfikację podstawowych plików binarnych systemu operacyjnego Windows w pamięci lub na dysku.

Wiarygodni programiści czasami modyfikują moduły systemu w pamięci z niezłośliwych powodów, na przykład w celu obejścia lub uzyskania zgodności aplikacji. Aby ułatwić rozróżnienie złośliwych i potencjalnie wiarygodnych modułów, usługa Azure Security Center sprawdza, czy zmodyfikowany moduł jest zgodny z podejrzanym profilem. Wynik tej kontroli jest wskazywany przez ważność alertu, opis alertu oraz czynności zaradcze alertu.

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola dodatkowe:

* MODULENAME: nazwa zmodyfikowanego pliku binarnego systemu.
* MODULEVERSION: wersja zmodyfikowanego pliku binarnego systemu.

Oto przykład tego typu alertu:

![Alert o zmodyfikowanym pliku binarnym systemu](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Wykonanie podejrzanego procesu
Usługa Security Center identyfikuje podejrzany proces uruchomiony na docelowej maszynie wirtualnej, a następnie wyzwala alert. Mechanizm wykrywania nie szuka określonej nazwy, ale działa z uwzględnieniem parametru pliku wykonywalnego. W związku z tym nawet jeśli osoba atakująca zmieni nazwę pliku wykonywalnego, usługa Security Center będzie w stanie wykryć podejrzany proces.

Oto przykład tego typu alertu:

![Alert o wykonaniu podejrzanego procesu](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domains-accounts-queried"></a>Wiele zapytań do kont domeny
Usługa Security Center może wykryć wielokrotne próby zapytań do kont domeny usługi Active Directory, które są zazwyczaj wykonywane przez osoby atakujące podczas czynności rozpoznawczych sieci. Osoby atakujące mogą korzystać z tej techniki wysyłania zapytań do domeny w celu identyfikacji użytkowników, kont administratorów domeny, komputerów, które są kontrolerami domeny, oraz potencjalnych relacji zaufania z innymi domenami.

Oto przykład tego typu alertu:

![Alert o wielokrotnych zapytaniach do konta domeny](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

### <a name="local-administrators-group-members-were-enumerated"></a>Wyliczono członków grupy administratorów lokalnych

Usługa Security Center wyzwoli alert, kiedy w systemie Windows Server 2016 i Windows 10 zostanie wyzwolone zdarzenie zabezpieczeń 4798. Dzieje się tak, kiedy zostają wyliczone grupy administratorów lokalnych, co jest zazwyczaj wykonywane przez osoby atakujące podczas czynności rozpoznawczych sieci. Osoby atakujące mogą korzystać z tej techniki w celu wykonania zapytań o tożsamości użytkowników z uprawnieniami administracyjnymi.

Oto przykład tego typu alertu:

![Administrator lokalny](./media/security-center-alerts-type/security-center-alerts-type-fig14-new.png)

### <a name="anomalous-mix-of-upper-and-lower-case-characters"></a>Nietypowa kombinacja wielkich i małych liter

Usługa Security Center wyzwoli alert, kiedy wykryje użycie kombinacji wielkich i małych liter w wierszu polecenia. Niektóre osoby atakujące mogą użyć tej techniki w celu obejścia zasad komputera uwzględniających wielkość liter lub bazujących na skrótach.

Oto przykład tego typu alertu:

![Nietypowa kombinacja](./media/security-center-alerts-type/security-center-alerts-type-fig15-new.png)

### <a name="suspected-kerberos-golden-ticket-attack"></a>Podejrzenie ataku na złoty bilet protokołu Kerberos

Naruszony klucz [krbtgt](https://technet.microsoft.com/library/dn745899.aspx) może zostać użyty przez osobę atakującą do utworzenia „złotego biletu” protokołu Kerberos, który umożliwiłby osobie atakującej podszycie się pod dowolnego użytkownika. Usługa Security Center wyzwoli alert, kiedy wykryje ten rodzaj aktywności.

> [!NOTE] 
> Aby uzyskać więcej informacji o złotym bilecie protokołu Kerberos, przeczytaj przewodnik [Windows 10 credential theft mitigation guide](http://download.microsoft.com/download/C/1/4/C14579CA-E564-4743-8B51-61C0882662AC/Windows%2010%20credential%20theft%20mitigation%20guide.docx) (Przewodnik ograniczania przypadków kradzieży poświadczeń w systemie Windows 10).

Oto przykład tego typu alertu:

![Złoty bilet](./media/security-center-alerts-type/security-center-alerts-type-fig16-new.png)

### <a name="suspicious-account-created"></a>Utworzono podejrzane konto

Usługa Security Center wyzwoli alert, kiedy zostanie utworzone konto bardzo podobne do istniejącego wbudowanego konta z uprawnieniami administracyjnymi. Ta technika może zostać użyta przez osoby atakujące do utworzenia nieautoryzowanego konta, które pozostanie niezauważone podczas weryfikacji dokonywanej przez człowieka.
 
Oto przykład tego typu alertu:

![Podejrzane konto](./media/security-center-alerts-type/security-center-alerts-type-fig17-new.png)

### <a name="suspicious-firewall-rule-created"></a>Utworzono podejrzaną regułę zapory

Osoby atakujące mogą próbować obejść zabezpieczenia hosta przez utworzenie niestandardowych reguł zapory umożliwiających złośliwym aplikacjom komunikowanie się na potrzeby poleceń i kontroli lub podejmowanie ataków przez sieć za pośrednictwem naruszonego hosta. Usługa Security Center wyzwoli alert, kiedy wykryje, że utworzono nową regułę zapory przy użyciu pliku wykonywalnego w podejrzanej lokalizacji.
 
Oto przykład tego typu alertu:

![Reguła zapory](./media/security-center-alerts-type/security-center-alerts-type-fig18-new.png)

### <a name="suspicious-combination-of-hta-and-powershell"></a>Podejrzana kombinacja hosta HTA i programu PowerShell

Usługa Security Center wyzwoli alert, kiedy wykryje, że narzędzie Microsoft HTML Application Host (HTA) uruchamia polecenia programu PowerShell. Jest to technika używana przez osoby atakujące do uruchamiania złośliwych skryptów programu PowerShell.
 
Oto przykład tego typu alertu:

![HTA i PS](./media/security-center-alerts-type/security-center-alerts-type-fig19-new.png)


## <a name="network-analysis"></a>Analiza sieci
Wykrywanie zagrożeń sieci za pomocą usługi Security Center polega na automatycznym zbieraniu informacji o zabezpieczeniach uzyskanych na podstawie ruchu protokołu IPFIX (Internet Protocol Flow Information Export) na platformie Azure. Analizuje ona te informacje, często zestawiając informacje z wielu źródeł, aby zidentyfikować zagrożenia.

### <a name="suspicious-outgoing-traffic-detected"></a>Wykryto podejrzany ruch wychodzący
Urządzenia sieciowe mogą być wykrywane i profilowane w podobny sposób jak innego rodzaju systemy. Osoby atakujące zazwyczaj zaczynają od skanowania portów. W poniższym przykładzie mamy podejrzany ruch SSH z maszyny wirtualnej. W tym scenariuszu możliwy jest siłowy atak SSH lub atak polegający na sprawdzaniu, czy dany port jest otwarty w zasobie zewnętrznym.

![Alert o podejrzanym ruchu wychodzącym](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Ten alert zawiera informacje, które umożliwiają identyfikację zasobu użytego do zainicjowania ataku. Dostępne są również informacje pozwalające wskazać zagrożoną maszynę, czas wykrycia oraz użyty protokół i port. Ta strona zawiera także listę środków zaradczych, które można zastosować, aby zminimalizować problem.

### <a name="network-communication-with-a-malicious-machine"></a>Komunikacja sieciowa ze złośliwą maszyną
Wykorzystując źródła analizy zagrożeń firmy Microsoft, usługa Azure Security Center może wykryć zagrożone maszyny, które komunikują się ze złośliwym adresem IP — w wielu przypadkach centrum poleceń i kontroli. W tym przypadku usługa Security Center wykryła, że komunikacja odbywała się przy użyciu złośliwego oprogramowania Pony Loader (znanego także jako [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![alert o komunikacji sieciowej](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zasobu użytego do zainicjowania tego ataku, zaatakowanego zasobu, adresu IP ofiary, adresu IP osoby atakującej oraz czasu wykrycia.

> [!NOTE]
> W celu zachowania poufności informacji prawdziwe adresy IP zostały usunięte z tego zrzutu ekranu.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Możliwy wychodzący atak typu „odmowa usługi”
Nienormalny ruch sieciowy pochodzący z jednej maszyny wirtualnej może spowodować zasygnalizowanie przez usługę Security Center możliwości ataku typu „odmowa usługi”.

Oto przykład tego typu alertu:

![Wychodzący atak typu „odmowa usługi”](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Analiza zasobów
Analiza zasobów usługi Security Center koncentruje się na usługach PaaS, takich jak integracja z funkcją [wykrywania zagrożeń bazy danych Azure SQL Database](../sql-database/sql-database-threat-detection.md). Na podstawie analizy wyników z tych obszarów usługa Security Center generuje alert związany z zasobami.

### <a name="potential-sql-injection"></a>Potencjalna iniekcja SQL
Iniekcja SQL to atak polegający na wstawieniu złośliwego kodu do ciągów, które są następnie przekazywane do wystąpienia programu SQL Server w celu ich przeanalizowania i wykonania. Każda procedura tworząca instrukcje SQL powinna zostać przejrzana pod kątem zagrożenia iniekcją, ponieważ oprogramowanie SQL Server wykonuje wszystkie otrzymane zapytania, które mają poprawną składnię. Wykrywanie zagrożenia SQL wykorzystuje maszynowe uczenie się, analizę behawioralną i wykrywanie anomalii w celu określenia podejrzanych zdarzeń, które mogą mieć miejsce w bazach danych SQL Azure. Na przykład:

* Próba dostępu do bazy danych przez byłego pracownika
* Ataki polegające na iniekcji SQL
* Nieoczekiwany dostęp do produkcyjnej bazy danych przez użytkownika w domu

![Alert o potencjalnej iniekcji SQL](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zaatakowanego zasobu, czasu wykrycia i stanu ataku. Udostępnia także link do dalszych kroków analizowania problemu.

### <a name="vulnerability-to-sql-injection"></a>Podatność na iniekcję SQL
Ten alert jest wyzwalany, gdy w bazie danych zostanie wykryty błąd aplikacji, który może oznaczać możliwe luki w zabezpieczeniach umożliwiające ataki przez iniekcję kodu SQL.

![Alert o potencjalnej iniekcji SQL](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Nietypowy dostęp z nieznanej lokalizacji
Ten alert jest wyzwalany, gdy na serwerze zostanie wykryte zdarzenie dostępu z nieznanego adresu IP, które nie występowało w ostatnim okresie.

![Alert o nietypowym dostępie](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="contextual-information"></a>Informacje kontekstowe
Podczas badania analitycy potrzebują dodatkowego kontekstu w celu określenia rodzaju zagrożenia i sposobu jego wyeliminowania.  Przykład: wykryto anomalię dotyczącą sieci, ale bez zrozumienia tego, jakie inne akcje mają miejsce w sieci lub są związane z zasobem, którego dotyczy zagrożenie, bardzo trudno określić, jakie działania należy podjąć. Aby ułatwić rozwiązanie tego problemu, zdarzenie naruszenia zabezpieczeń może zawierać artefakty, powiązane zdarzenia i informacje, które mogą pomóc analitykom. Dostępność dodatkowych informacji będzie różna zależnie od typu wykrytego zagrożenia i konfiguracji środowiska — nie będą one dostępne dla wszystkich zdarzeń naruszenia zabezpieczeń.

Jeśli dostępne są dodatkowe informacje, zostaną one wyświetlone w zdarzeniu naruszenia zabezpieczeń poniżej listy alertów. Może ono zawierać informacje, takie jak:

- Zdarzenia czyszczenia dziennika
- Urządzenie PNP podłączone z nieznanego urządzenia
- Alerty, względem których nie można wykonać żadnych działań 

![Alert o nietypowym dostępie](./media/security-center-alerts-type/security-center-alerts-type-fig20.png) 


## <a name="see-also"></a>Zobacz też
W tym artykule opisano różne typy alertów zabezpieczeń w usłudze Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Obsługa zdarzeń naruszenia zabezpieczeń w usłudze Azure Security Center](security-center-incident.md)
* [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md)
* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.
