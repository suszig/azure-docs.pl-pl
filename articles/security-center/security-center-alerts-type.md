<properties
   pageTitle="Alerty zabezpieczeń według typu w usłudze Azure Security Center | Microsoft Azure"
   description="Ten dokument ułatwia zrozumienie typów alertów zabezpieczeń dostępnych w usłudze Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>


# Alerty zabezpieczeń według typu w usłudze Azure Security Center
Ten dokument ułatwia zrozumienie różnych typów alertów zabezpieczeń dostępnych w usłudze Azure Security Center. Więcej informacji na temat zarządzania alertami znajduje się w artykule [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md).

> [AZURE.NOTE] Aby włączyć wykrywanie zaawansowane, przeprowadź uaktualnienie usługi Azure Security Center do wersji Standard. Dostępna jest bezpłatna 90-dniowa wersja próbna. W celu uaktualnienia wybierz warstwę cenową w [Zasadach zabezpieczeń](security-center-policies.md). Aby dowiedzieć się więcej, zobacz [stronę cen](https://azure.microsoft.com/pricing/details/security-center/).


## Jakie typy alertów są dostępne?
Azure Security Center oferuje szereg alertów zharmonizowanych z etapami zwalczania cyberataków. Na poniższym rysunku przedstawiono kilka przykładów różnych alertów odnoszących się do niektórych z tych etapów.

![Łańcuch zagrożeń](./media/security-center-alerts-type/security-center-alerts-type-fig1.png)


**Określenie celu i atak**

- Ataki za pośrednictwem przychodzących połączeń RDP/SSH
- Ataki DDoS i aplikacji (partnerzy w zakresie zapory aplikacji sieci Web)
- Wykrywanie nieautoryzowanego dostępu (partnerzy w zakresie zapory NG)

**Instalacja i wykorzystanie**

- Znane sygnatury złośliwego oprogramowania (partnerzy we zakresie oprogramowania chroniącego przed złośliwym kodem)
- Złośliwe oprogramowanie rezydujące w pamięci i próby wykorzystania
- Wykonanie podejrzanego procesu
- Działania wymijające celem uniknięcia wykrycia
- Ruch boczny
- Rozpoznanie wewnętrzne
- Podejrzane działania programu PowerShell

**Po naruszeniu zabezpieczeń**  

- Komunikacja ze znanymi złośliwymi adresami IP (wykradanie danych lub sterowanie i kontrola)
- Korzystanie z zaatakowanych zasobów w celu przygotowania dodatkowych ataków (skanowanie portów wyjściowych metodą ataków siłowych RDP/SSH i spam)

Z każdym etapem są skojarzone inne typy ataków, a ich celem są różne podsystemy. Usługa Security Center ma trzy kategorie alertów związane z atakami na poszczególnych etapach:

- Analiza zachowania maszyny wirtualnej (VMBA)
- Analiza sieci
- Analiza zasobów

## Analiza zachowania maszyny wirtualnej

Usługa Azure Security Center może użyć analizy behawioralnej w celu identyfikacji zasobów, których bezpieczeństwo zostało naruszone, na podstawie analizy dzienników zdarzeń maszyny wirtualnej, na przykład zdarzeń tworzenia procesów, zdarzeń logowania itp. Ponadto istnieje korelacja z innymi sygnałami, które wykrywają dowody potwierdzające istnienie szeroko zakrojonej kampanii ataku.

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat sposobu działania funkcji wykrywania usługi Security Center, przeczytaj [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md).

### Analiza awarii

Analiza awaryjnego zrzutu pamięci jest metodą używaną do wykrywania zaawansowanego złośliwego oprogramowania, które jest w stanie ominąć tradycyjne rozwiązania w zakresie zabezpieczeń. Różne rodzaje złośliwego oprogramowania próbują zmniejszyć prawdopodobieństwo wykrycia przez programy antywirusowe, rezygnując z zapisu na dysku lub szyfrując składniki oprogramowania zapisywane na dysku. Dzięki temu złośliwe oprogramowanie jest trudne do wykrycia przy użyciu tradycyjnych metod ochrony. Może ono jednak zostać wykryte za pomocą analizy pamięci, ponieważ aby takie oprogramowanie mogło działać, musi zostawiać ślady w pamięci maszyny.

Jeśli wystąpi awaria oprogramowania, zrzut awaryjny przechwytuje część pamięci w chwili pojawienia się awarii. Awaria może być spowodowana przez złośliwe oprogramowanie, powszechnie stosowane aplikacje lub problemy z systemem. Analizując pamięć w zrzucie awaryjnym, usługa Security Center może wykryć metody, jakie zostały użyte w celu wykorzystania luk obecnych w oprogramowaniu, uzyskania dostępu do poufnych danych i dyskretnego pozostania na zainfekowanym komputerze. Jest to realizowane przy minimalnym wpływie na wydajność hostów, ponieważ analiza jest wykonywana przez zaplecze usługi Security Center.

Następujące pola są wspólne dla alertów analizy zrzutu awaryjnego:

- DUMPFILE: nazwa pliku zrzutu awaryjnego
- PROCESSNAME: nazwa procesu powodującego awarię
- PROCESSVERSION: wersja procesu powodującego awarię

### Wykryto kod powłoki

Kod powłoki to ładunek uruchamiany po wykorzystaniu przez złośliwe oprogramowanie luki w zabezpieczeniach oprogramowania. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła zachowanie kodu wykonywalnego typowe dla złośliwych ładunków. Wprawdzie niezłośliwe oprogramowanie może zachowywać się podobnie, jednak nie jest to typowe w przypadku zwykłych metod tworzenia oprogramowania.

Ten alert zawiera następujące pole dodatkowe:

- ADDRESS: lokalizacja kodu powłoki w pamięci

Oto przykład tego typu alertu:

![Alert kodu powłoki](./media/security-center-alerts-type/security-center-alerts-type-fig2.png) 

### Wykryto przejęcie modułu

System Windows używa bibliotek dołączanych dynamicznie (DLL, Dynamic Link Library), aby umożliwić oprogramowaniu korzystanie z typowych funkcji systemu Windows. Przejęcie biblioteki DLL ma miejsce, gdy złośliwe oprogramowanie zmienia kolejność ładowania bibliotek DLL, aby załadować złośliwy ładunek do pamięci, w której może zostać wykonany dowolny kod. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła ładowanie modułu o podobnej nazwie z dwóch różnych ścieżek, z których jedna pochodzi z typowej lokalizacji plików binarnych systemu Windows.

Wiarygodni programiści czasami zmieniają kolejność ładowania bibliotek DLL z niezłośliwych powodów, takich jak instrumentacja, rozbudowa systemu operacyjnego Windows lub aplikacji systemu Windows. Aby ułatwić rozróżnienie złośliwych i potencjalnie niegroźnych zmian kolejności ładowania bibliotek DLL, usługa Azure Security Center sprawdza, czy załadowany moduł jest zgodny z podejrzanym profilem. Wynik tej kontroli jest widoczny w polu alertu „SIGNATURE” i obejmuje ważność alertu, opis alertu oraz czynności zaradcze alertu. Analiza kopii dyskowej przejmującego modułu, na przykład sprawdzenie podpisu cyfrowego plików lub uruchomienie skanowania antywirusowego, może dostarczyć dodatkowych informacji na temat wiarygodności lub złośliwości tego modułu.

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola:

- SIGNATURE: pokazuje, czy przejmujący moduł jest zgodny z profilem podejrzanego zachowania
- HIJACKEDMODULE: nazwa przejętego modułu systemu Windows
- HIJACKEDMODULEPATH: ścieżka przejętego modułu systemu Windows
- HIJACKINGMODULEPATH: ścieżka przejmującego modułu

Oto przykład tego typu alertu:

![Alert o przejęciu modułu](./media/security-center-alerts-type/security-center-alerts-type-fig3.png) 

### Wykryto zamaskowany moduł systemu Windows

Złośliwe oprogramowanie może używać typowych nazw plików binarnych systemu Windows (np. SVCHOST.EXE) lub modułów (np. NTDLL.DLL) w celu zamaskowania charakteru złośliwego oprogramowania przed administratorami systemu. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła w pliku zrzutu awaryjnego moduły używające nazw modułów systemu Windows, które nie spełniają innych kryteriów typowych dla tego rodzaju modułów. Analiza kopii dyskowej zamaskowanego modułu może dostarczyć dodatkowych informacji na temat wiarygodności lub złośliwości tego modułu. Analiza może obejmować:

- Potwierdzenie, że dany plik jest dostarczany jako część wiarygodnego pakietu oprogramowania
- Sprawdzenie podpisu cyfrowego pliku
- Uruchomienie skanowania antywirusowego pliku

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola dodatkowe:

- DETAILS: opisuje, czy metadane modułów są prawidłowe i czy moduł został załadowany ze ścieżki systemowej.
- NAME: nazwa zamaskowanego modułu systemu Windows
- PATH: ścieżka zamaskowanego modułu systemu Windows

Ten alert wyodrębnia i wyświetla także niektóre pola z nagłówka PE modułu, takie jak „CHECKSUM” i „TIMESTAMP”. Te pola są wyświetlane tylko wtedy, gdy występują w module. Aby uzyskać szczegółowe informacje na temat tych pól, zobacz [specyfikację Microsoft PE i COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Oto przykład tego typu alertu:

![Alert o zamaskowanym elemencie systemu Windows](./media/security-center-alerts-type/security-center-alerts-type-fig4.png) 

### Wykryto zmodyfikowany plik binarny systemu

Złośliwe oprogramowanie może modyfikować podstawowe pliki binarne systemu, aby potajemnie uzyskać dostęp do danych lub niezauważenie przetrwać w zaatakowanym systemie. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła modyfikację podstawowych plików binarnych systemu operacyjnego Windows w pamięci lub na dysku.
Wiarygodni programiści czasami modyfikują moduły systemu w pamięci z niezłośliwych powodów, na przykład w celu obejścia lub uzyskania zgodności aplikacji. Aby ułatwić rozróżnienie złośliwych i potencjalnie wiarygodnych modułów, usługa Azure Security Center sprawdza, czy zmodyfikowany moduł jest zgodny z podejrzanym profilem. Wynik tej kontroli jest wskazywany przez ważność alertu, opis alertu oraz czynności zaradcze alertu.

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola dodatkowe:

- MODULENAME: nazwa zmodyfikowanego pliku binarnego systemu
- MODULEVERSION: wersja zmodyfikowanego pliku binarnego systemu

Oto przykład tego typu alertu:

![Alert o zmodyfikowanym pliku binarnym systemu](./media/security-center-alerts-type/security-center-alerts-type-fig5.png) 

### Wykonanie podejrzanego procesu

Usługa Security Center identyfikuje wykonywanie podejrzanego procesu na docelowej maszynie wirtualnej i wyzwala alert. Mechanizm wykrywania nie poszukuje określonej nazwy, ale działa z uwzględnieniem parametru, w związku z tym nawet jeśli osoba atakująca zmieni nazwę pliku wykonywalnego, usługa Security Center będzie w stanie wykryć zagrożenie.
 
Oto przykład tego typu alertu:

![Alert o wykonaniu podejrzanego procesu](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### Wiele zapytań do kont domeny

Usługa Security Center może wykryć wielokrotne próby zapytań do kont domeny, które są zazwyczaj wykonywane przez osoby atakujące podczas czynności rozpoznawczych sieci. Osoby atakujące mogą korzystać z tej techniki wysyłania zapytań do domeny w celu identyfikacji użytkowników, kont administratora domeny, określenia, które komputery są kontrolerami domeny, a także identyfikacji potencjalnych relacji zaufania z innymi domenami.

Oto przykład tego typu alertu:

![Alert o wielokrotnych zapytaniach do konta domeny](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

## Analiza sieci

Wykrywanie zagrożeń sieci za pomocą usługi Security Center polega na automatycznym zbieraniu informacji o zabezpieczeniach uzyskanych na podstawie ruchu protokołu IPFIX (Internet Protocol Flow Information Export) na platformie Azure. Analizuje ona te informacje, często zestawiając informacje z wielu źródeł, aby zidentyfikować zagrożenia.

### Wykryto podejrzany ruch wychodzący

Urządzenia sieciowe mogą być wykrywane i profilowane w podobny sposób jak innego rodzaju systemy. Osoby atakujące zazwyczaj zaczynają od skanowania portów. W poniższym przykładzie mamy podejrzany ruch SSH z maszyny wirtualnej, który może przeprowadzać siłowy atak SSH lub atak polegający na sprawdzaniu, czy dany port jest otwarty w zasobie zewnętrznym.

![Alert o podejrzanym ruchu wychodzącym](./media/security-center-alerts-type/security-center-alerts-type-fig8.png) 

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zasobu użytego do zainicjowania tego ataku, zagrożonej maszyny, czasu wykrycia, protokołu i użytego portu. Ten blok zawiera także listę środków zaradczych, które można zastosować, aby zminimalizować problem.

### Komunikacja sieciowa ze złośliwą maszyną

Wykorzystując źródła analizy zagrożeń firmy Microsoft, usługa Azure Security Center może wykryć zagrożone maszyny, które komunikują się ze złośliwym adresem IP — w wielu przypadkach centrum poleceń i kontroli. W tym przypadku usługa Azure Security Center wykryła, że komunikacja odbywała się przy użyciu złośliwego oprogramowania Pony Loader (znanego także jako [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![alert o komunikacji sieciowej](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zasobu użytego do zainicjowania tego ataku, zaatakowanego zasobu, adresu IP ofiary, adresu IP osoby atakującej oraz czasu wykrycia.

[AZURE.NOTE] W celu zachowania poufności informacji prawdziwe adresy IP zostały usunięte z tego zrzutu ekranu.


### Możliwy wychodzący atak typu „odmowa usługi”

Nienormalny ruch sieciowy pochodzący z jednej maszyny wirtualnej może spowodować zasygnalizowanie przez usługę Security Center możliwości ataku typu „odmowa usługi”.
 
Oto przykład tego typu alertu:

![Wychodzący atak typu „odmowa usługi”](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## Analiza zasobów

Analiza zasobów usługi Security Center koncentruje się na usługach PaaS, takich jak integracja z funkcją [wykrywania zagrożeń bazy danych SQL Azure](../sql-database/sql-database-threat-detection-get-started.md). Na podstawie analizy wyników z tych obszarów usługa Security Center generuje alert związany z zasobami.

### Potencjalna iniekcja SQL

Iniekcja SQL to atak polegający na wstawieniu złośliwego kodu do ciągów, które są następnie przekazywane do wystąpienia programu SQL Server w celu ich przeanalizowania i wykonania. Każda procedura tworząca instrukcje SQL powinna zostać przejrzana pod kątem zagrożenia iniekcją, ponieważ oprogramowanie SQL Server wykonuje wszystkie otrzymane zapytania, które mają poprawną składnię. Wykrywanie zagrożenia SQL wykorzystuje maszynowe uczenie się, analizę behawioralną i wykrywanie anomalii w celu określenia podejrzanych zdarzeń, które mogą mieć miejsce w bazach danych SQL Azure. Na przykład:

- Próba dostępu do bazy danych przez byłego pracownika 
- Ataki polegające na iniekcji SQL 
- Nieoczekiwany dostęp do produkcyjnej bazy danych przez użytkownika w domu

![Alert o potencjalnej iniekcji SQL](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zaatakowanego zasobu, czasu wykrycia i stanu ataku. Udostępnia także link do dalszych kroków analizowania problemu.

### Podatność na iniekcję SQL

Ten alert jest wyzwalany, gdy w bazie danych zostanie wykryty błąd aplikacji, który oznaczać możliwe luki w zabezpieczeniach umożliwiające ataki przez iniekcję kodu SQL.

![Alert o potencjalnej iniekcji SQL](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### Nietypowy dostęp z nieznanej lokalizacji

Ten alert jest wyzwalany, gdy na serwerze wykryto dostęp z nieznanego adresu IP, który nie występował w ostatnim okresie.

![Alert o nietypowym dostępie](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)


## Zobacz też

W tym artykule opisano różne typy alertów zabezpieczeń w usłudze Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Obsługa zdarzeń naruszenia zabezpieczeń w usłudze Azure Security Center](security-center-incident.md)
- [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md)
- [Przewodnik planowania i obsługi Centrum zabezpieczeń Azure](security-center-planning-and-operations-guide.md)
- [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.



<!--HONumber=Sep16_HO3-->


