<properties
   pageTitle="Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument ułatwia zarządzanie alertami zabezpieczeń i reagowanie na nie przy użyciu funkcji Centrum zabezpieczeń Azure."
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
   ms.date="08/26/2016"
   ms.author="yurid"/>

# Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure
Ten dokument ułatwia zarządzanie alertami zabezpieczeń i reagowanie na nie przy użyciu usługi Azure Security Center.

> [AZURE.NOTE] Aby włączyć wykrywanie zaawansowane, przeprowadź uaktualnienie usługi Azure Security Center do wersji Standard. Dostępna jest bezpłatna 90-dniowa wersja próbna. W celu uaktualnienia wybierz warstwę cenową w [Zasadach zabezpieczeń](security-center-policies.md). Aby dowiedzieć się więcej, zobacz [stronę cen](https://azure.microsoft.com/pricing/details/security-center/).


## Czym są alerty zabezpieczeń?
Usługa Security Center automatycznie gromadzi, analizuje i integruje dane dzienników z zasobów platformy Azure, sieci oraz połączonych rozwiązań partnerskich, takich jak rozwiązania zapory i ochrony punktów końcowych, aby wykrywać prawdziwe zagrożenia i redukować liczbę fałszywych alarmów. W usłudze Security Center jest wyświetlana lista alertów zabezpieczeń uporządkowanych według priorytetu oraz informacje potrzebne do szybkiego analizowania problemu i zalecenia dotyczące postępowania w razie ataku. Usługa Azure Security Center agreguje również alerty, które przekształcają wzorce łańcuchowe w [zdarzenia](security-center-incident.md). 

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat sposobu działania funkcji wykrywania usługi Security Center, przeczytaj [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md).


## Zarządzanie alertami zabezpieczeń

Bieżące alerty można przeglądać przy użyciu kafelka **Alerty zabezpieczeń**. Otwórz witrynę Azure Portal i wykonaj poniższe kroki, aby wyświetlić więcej szczegółowych informacji dotyczących poszczególnych alertów:

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń widoczny jest kafelek **Alerty zabezpieczeń**.

    ![Kafelek Alerty zabezpieczeń w usłudze Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2.  Kliknij kafelek, aby otworzyć blok **Alerty zabezpieczeń** zawierający więcej szczegółowych informacji o alertach, jak pokazano poniżej.

    ![Blok kafelka Alerty zabezpieczeń w usłudze Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

W dolnej części tego bloku znajdują się szczegółowe informacje o każdym alercie. Aby posortować dane, kliknij kolumnę, według której chcesz wykonać sortowanie. Poniżej znajdują się definicje poszczególnych kolumn:

- **Alert**: krótki opis alertu.
- **Liczba**: lista wszystkich alertów określonego typu, które zostały wykryte w określonym dniu.
- **Wykryte przez**: usługa odpowiedzialna za wyzwolenie alertu.
- **Data**: dzień, w którym wystąpiło zdarzenie.
- **Stan**: bieżący stan alertu. Istnieją dwa typy stanów:
    - **Aktywny**: alert zabezpieczeń został wykryty.
    - **Odrzucony**: alert zabezpieczeń został odrzucony przez użytkownika. Ten stan jest zwykle używany w przypadku alertów, które zostały zbadane, ale ryzyko związane z nimi zostało zminimalizowane albo alert nie dotyczył rzeczywistego ataku.

- **Ważność**: poziom ważności (wysoki, średni lub niski).

### Filtrowanie alertów

Alerty można filtrować na podstawie daty, stanu i ważności. Filtrowanie alertów może być przydatne w przypadku scenariuszy, w których należy zawęzić zakres wyświetlanych alertów zabezpieczeń. Możesz na przykład sprawdzić alerty zabezpieczeń, które wystąpiły w ciągu ostatnich 24 godzin, ponieważ badasz potencjalne naruszenie zabezpieczeń systemu.

1. Kliknij pozycję **Filtr** w bloku **Alerty zabezpieczeń**. Zostanie otwarty blok **Filtr**. Wybierz wartości daty, stanu i ważności, które chcesz wyświetlić.

    ![Filtrowanie alertów w usłudze Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-ga.png)

2.  Po zbadaniu alertu zabezpieczeń może okazać się, że było to fałszywe ostrzeżenie związane ze środowiskiem lub że wskazuje on oczekiwane zachowanie określonego zasobu. Niezależnie od przyczyny wyświetlenia, jeśli uważasz, że alert zabezpieczeń nie ma zastosowania, możesz go odrzucić i odfiltrować z widoku. Istnieją dwa sposoby odrzucania alertu zabezpieczeń. Kliknij alert prawym przyciskiem myszy i wybierz pozycję **Odrzuć** lub umieść wskaźnik myszy nad elementem, kliknij przycisk z wielokropkiem po prawej stronie i wybierz pozycję **Odrzuć**. Odrzucone alerty zabezpieczeń możesz wyświetlić, klikając pozycję **Filtr** i wybierając opcję **Odrzucone**.

    ![Odrzucanie alertów w usłudze Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig4-ga.png)

### Odpowiadanie na alerty zabezpieczeń

Wybierz alert zabezpieczeń, aby dowiedzieć się więcej o zdarzeniach, które go wywołały, oraz o czynnościach, które należy wykonać w celu wyeliminowania skutków ataku (jeśli ma to zastosowanie). Alerty zabezpieczeń są grupowane według typu i daty. Kliknięcie alertu zabezpieczeń spowoduje otwarcie bloku zawierającego listę pogrupowanych alertów.

![Odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

W tym przypadku wyzwolone alerty dotyczą podejrzanego działania protokołu RDP (Remote Desktop Protocol). Pierwsza kolumna zawiera zaatakowane zasoby, druga przedstawia liczbę ataków na zasób, trzecia — czas ataku, czwarta — stan alertu, a piąta — ważność ataku. Po przejrzeniu tych informacji kliknij zaatakowany zasób. Zostanie otwarty nowy blok.

![Sugestie dotyczące zalecanych działań związanych z alertami zabezpieczeń w Centrum zabezpieczeń Azure](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

W polu **Opis** tego bloku znajdują się dalsze szczegółowe informacje na temat danego zdarzenia. Te dodatkowe szczegóły dotyczą przyczyn wyzwolenia alertu zabezpieczeń, zasobu docelowego, źródłowego adresu IP (jeśli ma to zastosowanie) i zaleceń dotyczących sposobu wyeliminowania skutków ataku.  W niektórych przypadkach źródłowy adres IP będzie pusty (niedostępny), ponieważ nie wszystkie dzienniki zdarzeń zabezpieczeń systemu Windows obejmują adres IP.

> [AZURE.NOTE] Czynności naprawcze sugerowane w Centrum zabezpieczeń będą różnić się w zależności od alertu zabezpieczeń. W niektórych przypadkach do wykonania zalecanych czynności naprawczych trzeba będzie użyć innych funkcji platformy Azure. Na przykład aby wyeliminować skutki takiego ataku, należy utworzyć listę niedozwolonych adresów IP generujących atak przy użyciu [sieciowej listy ACL](../virtual-network/virtual-networks-acl.md) lub reguły [sieciowej grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md).

## Alerty zabezpieczeń według typu
Te same czynności, które były używane w celu uzyskania dostępu do alertu o podejrzanym działaniu protokołu RDP, można wykonać, aby uzyskać dostęp do innych typów alertów. Oto kilka innych przykładów alertów, które mogą być widoczne wśród alertów usługi Security Center:

### Potencjalna iniekcja SQL
Iniekcja SQL to atak polegający na wstawieniu złośliwego kodu do ciągów, które są następnie przekazywane do wystąpienia programu SQL Server w celu ich przeanalizowania i wykonania. Każda procedura tworząca instrukcje SQL powinna zostać przejrzana pod kątem zagrożenia iniekcją, ponieważ oprogramowanie SQL Server wykonuje wszystkie otrzymane zapytania, które mają poprawną składnię. 

![Alert o iniekcji SQL](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9.png) 

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zaatakowanego zasobu, czasu wykrycia i stanu ataku. Udostępnia także link do dalszych kroków analizowania problemu.

### Wykryto podejrzany ruch wychodzący

Urządzenia sieciowe mogą być wykrywane i profilowane w podobny sposób jak innego rodzaju systemy. Osoby atakujące zazwyczaj zaczynają od skanowania portów. W poniższym przykładzie mamy podejrzany ruch SSH z maszyny wirtualnej, który może przeprowadzać siłowy atak SSH lub atak polegający na sprawdzaniu, czy dany port jest otwarty w zasobie zewnętrznym. 

![Alert o podejrzanym ruchu wychodzącym](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig-10.png)

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zasobu użytego do zainicjowania tego ataku, zagrożonej maszyny, czasu wykrycia, protokołu i użytego portu. Ten blok zawiera także listę środków zaradczych, które można zastosować, aby zminimalizować problem.

### Komunikacja sieciowa ze złośliwą maszyną
 
Wykorzystując źródła analizy zagrożeń firmy Microsoft, usługa Azure Security Center może wykryć zagrożone maszyny, które komunikują się ze złośliwym adresem IP — w wielu przypadkach centrum poleceń i kontroli. W tym przypadku usługa Azure Security Center wykryła, że komunikacja odbywała się przy użyciu złośliwego oprogramowania Pony Loader (znanego także jako [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![Alert o komunikacji sieciowej ze złośliwą maszyną](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9-ga.png)

Ten alert zawiera informacje, które umożliwiają zidentyfikowanie zasobu użytego do zainicjowania tego ataku, zaatakowanego zasobu, adresu IP ofiary, adresu IP osoby atakującej oraz czasu wykrycia.

> [AZURE.NOTE] W celu zachowania poufności informacji prawdziwe adresy IP zostały usunięte z tego zrzutu ekranu.

### Wykryto kod powłoki 

Kod powłoki to ładunek uruchamiany po wykorzystaniu przez złośliwe oprogramowanie luki w zabezpieczeniach oprogramowania.  Ten alert oznacza, że analiza zrzutu awaryjnego wykryła zachowanie kodu wykonywalnego typowe dla złośliwych ładunków.  Wprawdzie niezłośliwe oprogramowanie może zachowywać się podobnie, jednak nie jest to typowe w przypadku zwykłych metod tworzenia oprogramowania. 

Następujące pola są wspólne dla wszystkich alertów o zrzucie awaryjnym:

- DUMPFILE: nazwa pliku zrzutu awaryjnego 
- PROCESSNAME: nazwa procesu powodującego awarię 
- PROCESSVERSION: wersja procesu powodującego awarię 

Ten alert zawiera następujące pole dodatkowe:

- ADDRESS: lokalizacja kodu powłoki w pamięci

Oto przykład tego typu alertu:

![Alert kodu powłoki](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig10-ga.png)

### Wykryto iniekcję kodu

Iniekcja kodu to wstawianie modułów wykonywalnych do uruchomionych procesów lub wątków.  Ta metoda jest używana przez złośliwe oprogramowanie do uzyskiwania dostępu do danych, ukrywania swojej obecności lub zapobiegania usunięciu (np. trwałość).  Ten alert oznacza, że analiza zrzutu awaryjnego wykryła przeprowadzenie iniekcji modułu za pomocą zrzutu awaryjnego.
 
Wiarygodni programiści czasami wykonują iniekcję kodu z niezłośliwych powodów, takich jak modyfikowanie lub rozbudowywanie istniejącej aplikacji albo składnika systemu operacyjnego.  Aby ułatwić odróżnienie niezłośliwych modułów od złośliwych, usługa Azure Security Center sprawdza, czy wprowadzony moduł odpowiada profilowi podejrzanego zachowania. Wynik tej kontroli jest widoczny w polu alertu „SIGNATURE” i obejmuje ważność alertu, opis alertu oraz czynności zaradcze alertu.  

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola dodatkowe:

- ADDRESS: lokalizacja wprowadzonego modułu w pamięci
- IMAGENAME: nazwa wprowadzonego modułu. Zwróć uwagę, że to pole może być puste, jeśli obraz nie zawiera nazwy obrazu.
- SIGNATURE: pokazuje, czy wprowadzony moduł jest zgodny z profilem podejrzanego zachowania. W poniższej tabeli przedstawiono przykłady wyników i ich opisy:

| **Wartość podpisu**                  | **Opis**                                                                                                   |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Podejrzane wykorzystanie odpowiedniego modułu ładującego | To podejrzane zachowanie często jest skorelowane z ładowaniem wprowadzonego kodu niezależnie od modułu ładującego systemu operacyjnego |
| Podejrzane wykorzystanie wprowadzonego kodu          | Oznacza złośliwe działanie, które często jest skorelowane z wprowadzeniem kodu do pamięci                                       |
| Podejrzane wykorzystanie iniekcji         | Oznacza złośliwe działanie, które często jest skorelowane z użyciem wprowadzonego kodu w pamięci                                   |
| Podejrzane wykorzystanie wprowadzonego debugera | Oznacza złośliwe działanie, które często jest skorelowane z wykryciem lub obejściem debugera                         |
| Podejrzane zdalne wykorzystanie wprowadzonego kodu   | Oznacza złośliwe działanie, które często jest skorelowane ze scenariuszami kontrolki polecenia n (C2)                                 |

Oto przykład tego typu alertu:

![Wykryto iniekcję kodu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig11-ga.png)

### Wykryto przejęcie modułu

System Windows używa bibliotek dołączanych dynamicznie (DLL, Dynamic Link Library), aby umożliwić oprogramowaniu korzystanie z typowych funkcji systemu Windows.  Przejęcie biblioteki DLL ma miejsce, gdy złośliwe oprogramowanie zmienia kolejność ładowania bibliotek DLL, aby załadować złośliwy ładunek do pamięci, w której może zostać wykonany dowolny kod. Ten alert oznacza, że analiza zrzutu awaryjnego wykryła ładowanie modułu o podobnej nazwie z dwóch różnych ścieżek, z których jedna pochodzi z typowej lokalizacji plików binarnych systemu Windows.

Wiarygodni programiści czasami zmieniają kolejność ładowania bibliotek DLL z niezłośliwych powodów, takich jak instrumentacja, rozbudowa systemu operacyjnego Windows lub aplikacji systemu Windows.  Aby ułatwić rozróżnienie złośliwych i potencjalnie niegroźnych zmian kolejności ładowania bibliotek DLL, usługa Azure Security Center sprawdza, czy załadowany moduł jest zgodny z podejrzanym profilem.   Wynik tej kontroli jest widoczny w polu alertu „SIGNATURE” i obejmuje ważność alertu, opis alertu oraz czynności zaradcze alertu.  Analiza kopii dyskowej przejmującego modułu, na przykład sprawdzenie podpisu cyfrowego plików lub uruchomienie skanowania antywirusowego, może dostarczyć dodatkowych informacji na temat wiarygodności lub złośliwości tego modułu.

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola:

- SIGNATURE: pokazuje, czy przejmujący moduł jest zgodny z profilem podejrzanego zachowania
- HIJACKEDMODULE: nazwa przejętego modułu systemu Windows
- HIJACKEDMODULEPATH: ścieżka przejętego modułu systemu Windows
- HIJACKINGMODULEPATH: ścieżka przejmującego modułu 

Oto przykład tego typu alertu:

![Alert przejęcia biblioteki DLL](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig12-ga.png)

### Wykryto zamaskowany moduł systemu Windows

Złośliwe oprogramowanie może używać typowych nazw plików binarnych systemu Windows (np. SVCHOST.EXE) lub modułów (np. NTDLL.DLL) w celu zamaskowania charakteru złośliwego oprogramowania przed administratorami systemu.  Ten alert oznacza, że analiza zrzutu awaryjnego wykryła w pliku zrzutu awaryjnego moduły używające nazw modułów systemu Windows, które nie spełniają innych kryteriów typowych dla tego rodzaju modułów. Analiza kopii dyskowej zamaskowanego modułu może dostarczyć dodatkowych informacji na temat wiarygodności lub złośliwości tego modułu. Analiza może obejmować:

- Potwierdzenie, że dany plik jest dostarczany jako część wiarygodnego pakietu oprogramowania
- Sprawdzenie podpisu cyfrowego pliku 
- Uruchomienie skanowania antywirusowego pliku

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola dodatkowe:

- DETAILS: opisuje, czy metadane modułów są prawidłowe i czy moduł został załadowany ze ścieżki systemowej.
- NAME: nazwa zamaskowanego modułu systemu Windows
- PATH: ścieżka zamaskowanego modułu systemu Windows

Ten alert wyodrębnia i wyświetla także niektóre pola z nagłówka PE modułu, takie jak „CHECKSUM” i „TIMESTAMP”.  Te pola są wyświetlane tylko wtedy, gdy występują w module. Aby uzyskać szczegółowe informacje na temat tych pól, zobacz [specyfikację Microsoft PE i COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Oto przykład tego typu alertu:

![Zamaskowany alert](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig13-ga.png)

### Wykryto zmodyfikowany plik binarny systemu 

Złośliwe oprogramowanie może modyfikować podstawowe pliki binarne systemu, aby potajemnie uzyskać dostęp do danych lub niezauważenie przetrwać w zaatakowanym systemie.  Ten alert oznacza, że analiza zrzutu awaryjnego wykryła modyfikację podstawowych plików binarnych systemu operacyjnego Windows w pamięci lub na dysku. 

Wiarygodni programiści czasami modyfikują moduły systemu w pamięci z niezłośliwych powodów, na przykład w celu obejścia lub uzyskania zgodności aplikacji. Aby ułatwić rozróżnienie złośliwych i potencjalnie wiarygodnych modułów, usługa Azure Security Center sprawdza, czy zmodyfikowany moduł jest zgodny z podejrzanym profilem.   Wynik tej kontroli jest wskazywany przez ważność alertu, opis alertu oraz czynności zaradcze alertu. 

Oprócz typowych pól opisanych powyżej w sekcji „Wykryto kod powłoki” ten alert zawiera następujące pola dodatkowe:

- MODULENAME: nazwa zmodyfikowanego pliku binarnego systemu 
- MODULEVERSION: wersja zmodyfikowanego pliku binarnego systemu

Oto przykład tego typu alertu:

![Alert zmodyfikowanego pliku binarnego](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig14-ga.png)


## Zobacz też

W tym dokumencie przedstawiono sposób konfigurowania zasad zabezpieczeń w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Obsługa zdarzeń naruszenia zabezpieczeń w usłudze Azure Security Center](security-center-incident.md)
- [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md)
- [Przewodnik planowania i obsługi Centrum zabezpieczeń Azure](security-center-planning-and-operations-guide.md)
- [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.



<!--HONumber=sep16_HO1-->


