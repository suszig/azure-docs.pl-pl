<properties
   pageTitle="Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Informacje zawarte w tym dokumencie ułatwiają rozpoczęcie korzystania z funkcji monitorowania w Centrum zabezpieczeń Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="yurid"/>

#Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure
Informacje zawarte w tym dokumencie ułatwiają korzystanie z funkcji monitorowania w Centrum zabezpieczeń Azure w celu monitorowania zgodności z zasadami.

##Czym jest monitorowanie kondycji zabezpieczeń?
Monitorowanie jest często postrzegane jako obserwowanie i oczekiwanie na wystąpienie zdarzenia, aby można było zareagować na zaistniałą sytuację. Monitorowanie zabezpieczeń polega na wdrożeniu aktywnej strategii przeprowadzania inspekcji zasobów w celu zidentyfikowania systemów, które nie spełniają standardów organizacji lub są niezgodne z najlepszymi rozwiązaniami.

##Monitorowanie kondycji zabezpieczeń
Po włączeniu [zasad zabezpieczeń](security-center-policies.md) dla zasobów subskrypcji Centrum zabezpieczeń będzie analizować zabezpieczenia zasobów, aby zidentyfikować potencjalne luki.  Informacje o konfiguracji sieci są dostępne natychmiast. Informacje o konfiguracji maszyny wirtualnej — np. o stanie aktualizacji zabezpieczeń i konfiguracji systemu operacyjnego — mogą zostać udostępnione dopiero po godzinie lub jeszcze dłuższym czasie. Stan zabezpieczeń zasobów oraz informacje o problemach można wyświetlić w bloku **Kondycja zabezpieczeń zasobów**. Listę tych problemów można również wyświetlić w bloku **Zalecenia**.

Więcej informacji dotyczących stosowania zaleceń można znaleźć w temacie [Wdrażanie zaleceń dotyczących zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md).

Na kafelku **Kondycja zabezpieczeń zasobów** można monitorować stan zabezpieczeń zasobów. W przykładzie poniżej widać szereg problemów o wysokiej i średniej ważności, które wymagają uwagi. Włączone zasady zabezpieczeń wpływają na typy monitorowanych środków kontroli.

![Kondycja zasobów](./media/security-center-monitoring/security-center-monitoring-fig1-ga.png)

Jeśli w Centrum zabezpieczeń zostanie zidentyfikowana luka w zabezpieczeniach, którą trzeba usunąć, np. maszyna wirtualna z brakującymi aktualizacjami zabezpieczeń lub podsieć bez [sieciowej grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md), zostanie ona wyświetlona na tej liście.

###Monitorowanie maszyn wirtualnych
Po kliknięciu opcji **Maszyny wirtualne** na kafelku **Kondycja zabezpieczeń zasobów** zostanie otwarty blok **Maszyny wirtualne** zawierający więcej szczegółów dotyczących dołączania i środków zapobiegawczych oraz listę wszystkich maszyn wirtualnych, które są monitorowane przy użyciu Centrum zabezpieczeń, jak pokazano poniżej.

![Brak aktualizacji systemu na maszynie wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig2-ga.png)

- Kroki dołączania
- Zalecenia dotyczące maszyny wirtualnej
- Maszyny wirtualne

W każdej sekcji można wybrać poszczególne opcje, aby wyświetlić więcej szczegółowych informacji dotyczących zalecanych kroków rozwiązania problemu. Te zagadnienia zostały szczegółowo opisane poniżej.

#### Zalecenia dotyczące monitorowania
W tej sekcji przedstawiono całkowitą liczbę maszyn wirtualnych, dla których zainicjowano obsługę zbierania danych, oraz ich bieżący stan. Po zainicjowaniu obsługi zbierania danych na wszystkich maszynach wirtualnych maszyny będą gotowe do odbierania zasad zabezpieczeń Centrum zabezpieczeń. Kliknięcie tego wpisu spowoduje otwarcie bloku **Stan instalowania kolekcji danych**, w którym będą wyświetlone nazwy maszyn wirtualnych oraz bieżący stan zbierania danych w kolumnie **STAN INSTALACJI**, jak pokazano poniżej.

![Stan inicjowania](./media/security-center-monitoring/security-center-monitoring-fig3-ga.png)


####Zalecenia dotyczące maszyny wirtualnej
Ta sekcja zawiera zestaw zaleceń dotyczących każdej maszyny wirtualnej monitorowanej przez Centrum zabezpieczeń Azure. Pierwsza kolumna zawiera zalecenia, druga kolumna — całkowitą liczbę maszyn wirtualnych, których te zalecenia dotyczą, natomiast trzecia kolumna — wagę problemu, jak przedstawiono poniżej.

![Zalecenia dotyczące maszyny wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig4-ga.png)

> [AZURE.NOTE] W bloku Kondycja sieci na liście Topologie sieci wyświetlane są tylko maszyny wirtualne z co najmniej jednym publicznym punktem końcowym.

Każdy zalecenie obejmuje zestaw akcji możliwych do wykonania po jego kliknięciu. Na przykład kliknięcie pozycji **Brak aktualizacji systemu** spowoduje otwarcie bloku **Brak aktualizacji systemu**. Wyświetlona zostanie lista maszyn wirtualnych, na których brakuje poprawek, wraz z informacjami o ważności brakujących aktualizacji, jak pokazano poniżej.

![Brak aktualizacji systemu](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

W bloku **Brak aktualizacji systemu** zostanie wyświetlona tabela z następującymi informacjami:

- **MASZYNA WIRTUALNA**: nazwa maszyny wirtualnej, na której brakuje aktualizacji.
- **AKTUALIZACJE SYSTEMU**: liczba brakujących aktualizacji systemu.
- **CZAS OSTATNIEGO SKANOWANIA**: czas ostatniego skanowania maszyny wirtualnej pod kątem aktualizacji przez Centrum zabezpieczeń.
- **STAN**: bieżący stan zalecenia:
    - **Otwarte**: nie rozpoczęto jeszcze wykonywania zalecenia
    - **W toku**: zalecenie jest aktualnie stosowane do danych zasobów, żadna akcja użytkownika nie jest wymagana
    - **Rozwiązano**: zalecenie zostało już wykonane (po rozwiązaniu problemu pozycja jest wyszarzona).
- **WAŻNOŚĆ**: opisuje ważność określonego zalecenia:
    - **Wysoka**: istnieje luka w zabezpieczeniach, która dotyczy istotnego zasobu (aplikacji, maszyny wirtualnej, sieciowej grupy zabezpieczeń) i wymaga uwagi
    - **Średnia**: niekrytyczne lub dodatkowe kroki są wymagane w celu ukończenia procesu lub wyeliminowania luki w zabezpieczeniach
    - **Niska**: luka w zabezpieczeniach powinna zostać usunięta, ale nie wymaga natychmiastowej uwagi. (Domyślnie zalecenia o niskiej ważności nie są prezentowane, ale możesz ustawić filtr umożliwiający wyświetlanie zaleceń o niskiej ważności, jeśli chcesz je przeglądać).

Aby wyświetlić szczegóły zalecenia, kliknij nazwę maszyny wirtualnej. Zostanie otwarty nowy blok dla tej maszyny wirtualnej zawierający listę aktualizacji, jak przedstawiono poniżej.

![Brak aktualizacji systemu dla maszyny wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [AZURE.NOTE] Przedstawione tutaj zalecenia dotyczące zabezpieczeń są takie same, jak w bloku Zalecenia. Więcej informacji na temat stosowania zaleceń można znaleźć w temacie [Wdrażanie zaleceń dotyczących zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md). Dotyczy to nie tylko maszyn wirtualnych, ale także wszystkich zasobów, które są dostępne za pośrednictwem kafelka Kondycja zasobu.

####Sekcja Maszyny wirtualne
Sekcja Maszyny wirtualne zawiera przegląd wszystkich maszyn wirtualnych i zaleceń. Każda kolumna reprezentuje jeden zestaw zaleceń, jak pokazano poniżej:

![Maszyny wirtualne](./media/security-center-monitoring/security-center-monitoring-fig7-ga.png)

Ikona wyświetlana pod każdym zaleceniem pomaga szybko określić, które maszyny wirtualne wymagają uwagi oraz jakiego typu zalecenie ma do nich zastosowanie.

W przykładzie powyżej dla jednej maszyny wirtualnej widoczne jest krytyczne zalecenie dotyczące ochrony punktów końcowych. Aby uzyskać więcej informacji na temat maszyny wirtualnej, kliknij ją. Zostanie otwarty nowy blok, który reprezentuje tę maszynę wirtualną, jak pokazano poniżej.

![Szczegóły zabezpieczeń maszyny wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Ten blok zawiera szczegóły zabezpieczeń dotyczące maszyny wirtualnej. W dolnej części bloku widoczna jest zalecana akcja i ważność poszczególnych problemów.

#### Sekcja Cloud Services (wersja zapoznawcza)
Stan kondycji usług w chmurze jest dołączony do kafelka kondycji zabezpieczeń maszyn wirtualnych. Zalecenie jest tworzone, gdy wersja systemu operacyjnego jest nieaktualna, jak pokazano poniżej: 

![Cloud Services](./media/security-center-monitoring/security-center-monitoring-fig8-new2.png)

Należy wykonać kroki zawarte w zaleceniu, aby zaktualizować wersję systemu operacyjnego. Na przykład po kliknięciu czerwonego alertu w jednej z roli sieci Web lub roli procesu roboczego, w ramach której działa system Windows Server z aplikacją sieci Web automatycznie wdrożoną w usługach IIS, zostanie otwarty nowy blok zawierający więcej szczegółów na temat tego zalecenia, jak pokazano poniżej:

![Szczegóły dotyczące usługi w chmurze](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png) 

Aby zobaczyć więcej normatywnych wyjaśnień dotyczących tego zalecenia, kliknij pozycję **Aktualizuj wersję systemu operacyjnego** w kolumnie **OPIS**. Zostanie otwarty blok **Aktualizowanie wersji systemu operacyjnego (wersja zapoznawcza)** z bardziej szczegółowymi informacjami.

![Zalecenia dotyczące usług Cloud Services](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### Monitorowanie sieci wirtualnych
Po kliknięciu opcji **Sieć** na kafelku **Kondycja zabezpieczeń zasobów** zostanie otwarty blok **Sieć** zawierający więcej szczegółowych informacji, jak pokazano poniżej:

![Sieć](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

####Zalecenia dotyczące sieci

Podobnie jak w przypadku informacji o kondycji zasobów maszyn wirtualnych, w górnej części tego bloku widoczna jest skrócona lista problemów, a w dolnej — lista monitorowanych sieci.

Sekcja ze szczegółowymi danymi o stanie sieci zawiera listę potencjalnych problemów z zabezpieczeniami oraz zalecenia. Mogą wystąpić następujące problemy:

- Zapora nowej generacji (NGFW) nie jest zainstalowana
- Sieciowe grupy zabezpieczeń (NSG) w podsieciach nie są włączone
- Sieciowe grupy zabezpieczeń na maszynach wirtualnych nie są włączone
- Ograniczanie dostępu zewnętrznego za pośrednictwem publicznego zewnętrznego punktu końcowego
- Prawidłowe punkty końcowe umożliwiające dostęp do Internetu

Po kliknięciu jednego z tych zaleceń zostanie otwarty nowy blok zawierający więcej szczegółów dotyczących zalecenia, jak pokazano w poniższym przykładzie.

![Ograniczanie punktu końcowego](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

W tym przykładzie blok **Konfiguruj brakujące grupy zabezpieczeń sieci dla podsieci** zawiera listę podsieci i maszyn wirtualnych, dla których brakuje ochrony sieciowej grupy zabezpieczeń. Po kliknięciu podsieci, do której chcesz zastosować sieciową grupę zabezpieczeń, zostanie otwarty kolejny blok.

W bloku **Wybieranie grupy zabezpieczeń sieci** wybierz najbardziej odpowiednią sieciową grupę zabezpieczeń dla podsieci lub utwórz nową grupę zabezpieczeń sieci. 

####Sekcja Internet facing endpoints (Punkty końcowe umożliwiające dostęp do Internetu)

Sekcja **Internet facing endpoints** (Punkty końcowe umożliwiające dostęp do Internetu) zawiera maszyny wirtualne, które są aktualnie skonfigurowane z użyciem punktu końcowego umożliwiającego dostęp do Internetu oraz ich bieżący stan.

![Punkt końcowy umożliwiający dostęp do Internetu](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Ta tabela zawiera nazwę punktu końcowego, która reprezentuje maszynę wirtualną, adres IP umożliwiający dostęp do Internetu oraz bieżący stan ważności dla sieciowej grupy zabezpieczeń i zapory nowej generacji. Informacje w tabeli są posortowane według ważności, zgodnie z poniższym opisem:
- Kolor czerwony (u góry): wysoki priorytet, konieczne jest natychmiastowe rozwiązanie problemu 
- Kolor pomarańczowy: średni priorytet, konieczne jest możliwe szybkie rozwiązanie problemu
- Kolor zielony (ostatni): kondycja prawidłowa

####Sekcja Topologia sieci

W sekcji **Topologia sieci** zasoby są przedstawione według hierarchii, jak pokazano poniżej:

![Topologia sieci](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Informacje w tej tabeli (dotyczące maszyn wirtualnych i podsieci) zostały posortowane według ważności, zgodnie z poniższym opisem:
- Kolor czerwony (u góry): wysoki priorytet, konieczne jest natychmiastowe rozwiązanie problemu 
- Kolor pomarańczowy: średni priorytet, konieczne jest możliwe szybkie rozwiązanie problemu
- Kolor zielony (ostatni): kondycja prawidłowa

W tym widoku topologii pierwszy poziom obejmuje pozycje [Sieci wirtualne](../virtual-network/virtual-networks-overview.md), [Bramy sieci wirtualnej](../vpn-gateway/vpn-gateway-site-to-site-create.md) i [Sieć wirtualna (klasyczna)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Drugi poziom obejmuje podsieci, natomiast trzeci poziom przedstawia maszyny wirtualne, które należą do tych podsieci. Prawa kolumna przedstawia bieżący stan sieciowej grupy zabezpieczeń dla tych zasobów, jak pokazano w poniższym przykładzie:

![Drzewo sieci](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

W dolnej części tego bloku znajdują się zalecenia dotyczące tej maszyny wirtualnej, podobnie jak powyżej. Kliknij zalecenie, aby dowiedzieć się więcej lub zastosować odpowiednie środki kontroli/konfigurację zabezpieczeń.

###Monitorowanie zasobów SQL
Po kliknięciu opcji **SQL** na kafelku **Kondycja zabezpieczeń zasobów** zostanie otwarty blok SQL zawierający zalecenia dotyczące problemów, takich jak brak włączonych funkcji inspekcji lub przezroczystego szyfrowania danych. Przedstawia on także zalecenia dotyczące ogólnej kondycji bazy danych.

![Kondycja zasobów SQL](./media/security-center-monitoring/security-center-monitoring-fig13-ga.png)

Klikając jedno z tych zaleceń, można wyświetlić więcej szczegółów na temat dalszych działań w celu rozwiązania problemu. Poniższy przykład pokazuje rozszerzenie zalecenia **Inspekcja bazy danych nie jest włączona**.

![Kondycja zasobów SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga.png)

Blok **Włączanie inspekcji baz danych SQL** zawiera następujące informacje:

- Lista baz danych SQL
- Serwer, na którym znajdują się bazy danych SQL
- Informacje o tym, czy to ustawienie jest dziedziczone z serwera lub czy jest unikatowe w tej bazie danych
- Bieżący stan
- Ważność problemu

Po kliknięciu bazy danych w celu zastosowania tego zalecenia zostanie otwarty blok **Inspekcja i wykrywanie zagrożeń**, jak pokazano poniżej.

![Kondycja zasobów SQL](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Aby włączyć funkcję inspekcji, po prostu zaznacz pole wyboru **WŁĄCZONE** poniżej opcji **Inspekcja**.

### Monitorowanie aplikacji

Jeśli obciążenie platformy Azure obejmuje aplikacje znajdujące się na [maszynach wirtualnych Menedżera zasobów](../resource-manager-deployment-model.md) z uwidocznionymi portami sieci Web (porty TCP 80 i 443), Centrum zabezpieczeń może je monitorować w celu identyfikowania potencjalnych problemów z zabezpieczeniami oraz rekomendowania czynności naprawczych. Po kliknięciu kafelka **Aplikacje** zostanie otwarty blok **Aplikacje** zawierający serię zaleceń w sekcji dotyczącej czynności prewencyjnych. Widoczny będzie również podział aplikacji według hosta/wirtualnego adresu IP, jak pokazano poniżej.

![Kondycja zabezpieczeń aplikacji](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Podobnie jak w przypadku innych zaleceń możesz je kliknąć, aby wyświetlić więcej szczegółów o problemie i sposobie jego rozwiązania. Na przykładowym rysunku poniżej przedstawiono aplikację, która została zidentyfikowana jako niezabezpieczona aplikacja sieci Web. Po wybraniu aplikacji, która została uznana za niezabezpieczoną, zostanie otwarty kolejny blok, w którym dostępna będzie następująca opcja:

![Aplikacje](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Ten blok będzie zawierać listę wszystkich zaleceń dla tej aplikacji. Po kliknięciu rekomendacji **Dodaj zaporę aplikacji sieci Web** zostanie otwarty blok **Dodawanie zapory aplikacji sieci Web** z opcjami umożliwiającymi zainstalowanie zapory aplikacji sieci Web innej firmy, jak pokazano poniżej.

![Dodawanie zapory aplikacji sieci Web](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## Zobacz też
W tym dokumencie przedstawiono sposób korzystania z funkcji monitorowania w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w usłudze Azure Security Center](security-center-policies.md) — informacje na temat konfigurowania ustawień zabezpieczeń w usłudze Azure Security Center
- [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi
- [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
- [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure



<!--HONumber=sep16_HO1-->


