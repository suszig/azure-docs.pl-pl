---
title: "Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center | Microsoft Docs"
description: "Informacje zawarte w tym artykule ułatwiają rozpoczęcie korzystania z funkcji monitorowania w usłudze Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: aac0354a8fbfb73bab3e7b5da7d9aefa69c9eb16
ms.openlocfilehash: 5251ab851dd2934050747cd3ad7b2c4f0b9e98bb


---
# <a name="security-health-monitoring-in-azure-security-center"></a>Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure
Informacje zawarte w tym artykule ułatwiają korzystanie z funkcji monitorowania w usłudze Azure Security Center w celu monitorowania zgodności z zasadami.

## <a name="what-is-security-health-monitoring"></a>Czym jest monitorowanie kondycji zabezpieczeń?
Monitorowanie jest często postrzegane jako obserwowanie i oczekiwanie na wystąpienie zdarzenia, aby można było zareagować na zaistniałą sytuację. Monitorowanie zabezpieczeń polega na wdrożeniu aktywnej strategii przeprowadzania inspekcji zasobów w celu zidentyfikowania systemów, które nie spełniają standardów organizacji lub są niezgodne z najlepszymi rozwiązaniami.

## <a name="monitoring-security-health"></a>Monitorowanie kondycji zabezpieczeń
Po włączeniu [zasad zabezpieczeń](security-center-policies.md) dla zasobów subskrypcji usługa Security Center analizuje zabezpieczenia zasobów, aby zidentyfikować potencjalne luki. Informacje o konfiguracji sieci są dostępne natychmiast. Informacje o konfiguracji maszyny wirtualnej — np. o stanie aktualizacji zabezpieczeń i konfiguracji systemu operacyjnego — mogą zostać udostępnione dopiero po godzinie lub jeszcze dłuższym czasie. Stan zabezpieczeń zasobów oraz informacje o problemach można wyświetlić w bloku **Kondycja zabezpieczeń zasobów**. Listę tych problemów można również wyświetlić w bloku **Zalecenia**.

Więcej informacji dotyczących stosowania zaleceń można znaleźć w temacie [Wdrażanie zaleceń dotyczących zabezpieczeń w usłudze Azure Security Center](security-center-recommendations.md).

Na kafelku **Kondycja zabezpieczeń zasobów** można monitorować stan zabezpieczeń zasobów. W poniższym przykładzie widać szereg problemów o wysokiej i średniej ważności, które wymagają uwagi. Włączone zasady zabezpieczeń wpływają na typy monitorowanych środków kontroli.

![Kafelek Kondycja zabezpieczeń zasobów](./media/security-center-monitoring/security-center-monitoring-fig1-new4.png)

Jeśli w usłudze Security Center zostanie zidentyfikowana luka w zabezpieczeniach, którą trzeba usunąć, np. maszyna wirtualna z brakującymi aktualizacjami zabezpieczeń lub podsieć bez [sieciowej grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md), zostanie ona wyświetlona na tej liście.

### <a name="monitor-virtual-machines"></a>Monitorowanie maszyn wirtualnych
Po kliknięciu opcji **Maszyny wirtualne** na kafelku **Kondycja zabezpieczeń zasobów** zostanie otwarty blok **Maszyny wirtualne** zawierający więcej szczegółów dotyczących dołączania i środków zapobiegawczych oraz listę wszystkich maszyn wirtualnych, które są monitorowane przy użyciu usługi Security Center, jak pokazano na poniższym zrzucie ekranu.

![Brak aktualizacji systemu na maszynie wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig2-ga.png)

* Kroki dołączania
* Zalecenia dotyczące maszyny wirtualnej
* Maszyny wirtualne

W każdej sekcji można wybrać poszczególne opcje, aby wyświetlić więcej szczegółowych informacji dotyczących zalecanych kroków rozwiązania problemu. Poniższe sekcje wyjaśnią te zagadnienia bardziej szczegółowo.

#### <a name="monitoring-recommendations"></a>Zalecenia dotyczące monitorowania
W tej sekcji przedstawiono całkowitą liczbę maszyn wirtualnych, dla których zainicjowano obsługę zbierania danych, oraz ich bieżący stan. Po zainicjowaniu obsługi zbierania danych na wszystkich maszynach wirtualnych maszyny będą gotowe do odbierania zasad zabezpieczeń usługi Security Center. Kliknięcie tego wpisu spowoduje otwarcie bloku **Stan instalowania kolekcji danych**, w którym są wyświetlone nazwy maszyn wirtualnych oraz bieżący stan zbierania danych w kolumnie **STAN INSTALACJI**, jak pokazano na poniższym zrzucie ekranu.

![Stan inicjalizacji maszyn wirtualnych](./media/security-center-monitoring/security-center-monitoring-fig3-ga.png)

#### <a name="virtual-machine-recommendations"></a>Zalecenia dotyczące maszyny wirtualnej
Ta sekcja zawiera zestaw [zaleceń dotyczących każdej maszyny wirtualnej](security-center-virtual-machine-recommendations.md) monitorowanej przez usługę Azure Security Center. W pierwszej kolumnie wyświetlane są zalecenia. Druga kolumna zawiera całkowitą liczbę maszyn wirtualnych, na które dane zalecenie ma wpływ. Trzecia kolumna pokazuje wagę problemu, co przedstawiono na następującym zrzucie ekranu.

![Zalecenia dotyczące maszyny wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig4-ga.png)

> [!NOTE]
> W bloku **Kondycja sieci** na liście **Topologie sieci** wyświetlane są tylko maszyny wirtualne z co najmniej jednym publicznym punktem końcowym.
> 
> 

Każdy zalecenie obejmuje zestaw akcji możliwych do wykonania po jego kliknięciu. Na przykład kliknięcie pozycji **Brak aktualizacji systemu** spowoduje otwarcie bloku **Brak aktualizacji systemu**. Wyświetlona zostanie lista maszyn wirtualnych, na których brakuje poprawek, wraz z informacjami o ważności brakujących aktualizacji, jak pokazano na poniższym zrzucie ekranu.

![Brak aktualizacji systemu na maszynach wirtualnych](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

W bloku **Brak aktualizacji systemu** zostanie wyświetlona tabela z następującymi informacjami:

* **MASZYNA WIRTUALNA**: nazwa maszyny wirtualnej, na której brakuje aktualizacji.
* **AKTUALIZACJE SYSTEMU**: liczba brakujących aktualizacji systemu.
* **CZAS OSTATNIEGO SKANOWANIA**: czas ostatniego skanowania maszyny wirtualnej pod kątem aktualizacji przez usługę Security Center.
* **STAN**: bieżący stan zalecenia:
  * **Otwarte**: nie rozpoczęto jeszcze wykonywania zalecenia.
  * **W toku**: zalecenie jest aktualnie stosowane do danych zasobów, żadna akcja użytkownika nie jest wymagana.
  * **Rozwiązane**: zalecenie zostało już zakończone. (Gdy problem został rozwiązany, wpis jest wyszarzony).
* **WAŻNOŚĆ**: opisuje ważność określonego zalecenia:
  * **Wysoka**: istnieje luka w zabezpieczeniach, która dotyczy istotnego zasobu (aplikacji, maszyny wirtualnej, sieciowej grupy zabezpieczeń) i wymaga uwagi.
  * **Średnia**: niekrytyczne lub dodatkowe kroki są wymagane w celu ukończenia procesu lub wyeliminowania luki w zabezpieczeniach.
  * **Niska**: luka w zabezpieczeniach powinna zostać usunięta, ale nie wymaga natychmiastowej uwagi. (Domyślnie zalecenia o niskiej ważności nie są prezentowane, ale możesz ustawić filtr umożliwiający wyświetlanie zaleceń o niskiej ważności, jeśli chcesz je przeglądać).

Aby wyświetlić szczegóły zalecenia, kliknij nazwę maszyny wirtualnej. Zostanie otwarty nowy blok dla tej maszyny wirtualnej zawierający listę aktualizacji, co pokazano na poniższym zrzucie ekranu.

![Brak aktualizacji systemu na określonej maszynie wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> Przedstawione tutaj zalecenia dotyczące zabezpieczeń są takie same jak w bloku **Zalecenia**. Więcej informacji dotyczących stosowania zaleceń można znaleźć w artykule [Wdrażanie zaleceń dotyczących zabezpieczeń w usłudze Azure Security Center](security-center-recommendations.md). Dotyczy to nie tylko maszyn wirtualnych, ale także wszystkich zasobów, które są dostępne za pośrednictwem kafelka **Kondycja zasobu**.
> 
> 

#### <a name="virtual-machines-section"></a>Sekcja Maszyny wirtualne
Sekcja Maszyny wirtualne zawiera przegląd wszystkich maszyn wirtualnych i zaleceń. Każda kolumna reprezentuje jeden zestaw zaleceń, jak pokazano na poniższym zrzucie ekranu:

![Przegląd wszystkich maszyn wirtualnych i zaleceń](./media/security-center-monitoring/security-center-monitoring-fig7-ga.png)

Ikona wyświetlana pod każdym zaleceniem pomaga szybko określić, które maszyny wirtualne wymagają uwagi oraz jakiego typu zalecenie ma do nich zastosowanie.

W powyższym przykładzie dla jednej maszyny wirtualnej widoczne jest krytyczne zalecenie dotyczące ochrony punktów końcowych. Aby uzyskać więcej informacji na temat maszyny wirtualnej, kliknij ją. Zostanie otwarty nowy blok, który reprezentuje daną maszynę wirtualną, jak pokazano na poniższym zrzucie ekranu.

![Szczegóły dotyczące zabezpieczeń maszyny wirtualnej](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Ten blok zawiera szczegóły zabezpieczeń dotyczące maszyny wirtualnej. W dolnej części bloku widoczna jest zalecana akcja i ważność poszczególnych problemów.

#### <a name="cloud-services-preview-section"></a>Sekcja Cloud Services (wersja zapoznawcza)
Stan kondycji usług w chmurze jest dołączony do kafelka **Kondycja zabezpieczeń** maszyny wirtualnej. Zalecenie jest tworzone, gdy wersja systemu operacyjnego jest nieaktualna, jak pokazano na poniższym zrzucie ekranu:

![Stan kondycji dla usług w chmurze](./media/security-center-monitoring/security-center-monitoring-fig8-new2.png)

Należy wykonać kroki zawarte w zaleceniu, aby zaktualizować wersję systemu operacyjnego. Na przykład po kliknięciu czerwonego alertu w roli sieci Web (WebRole1) lub roli procesu roboczego (WorkerRole1) — w ramach której działa system Windows Server z aplikacją sieci Web automatycznie wdrożoną w usługach IIS — zostanie otwarty nowy blok zawierający więcej szczegółów na temat tego zalecenia, co pokazano na poniższym zrzucie ekranu:

![Szczegóły dotyczące usługi w chmurze](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Aby zobaczyć więcej normatywnych wyjaśnień dotyczących tego zalecenia, kliknij pozycję **Aktualizuj wersję systemu operacyjnego** w kolumnie **OPIS**. Zostanie otwarty blok **Aktualizowanie wersji systemu operacyjnego (wersja zapoznawcza)** z bardziej szczegółowymi informacjami.

![Zalecenia dotyczące usług w chmurze](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Monitorowanie sieci wirtualnych
Po kliknięciu opcji **Sieć** na kafelku **Kondycja zabezpieczeń zasobów** zostanie otwarty blok **Sieć** zawierający więcej szczegółowych informacji, jak pokazano na poniższym zrzucie ekranu:

![Blok Sieć](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Zalecenia dotyczące sieci
Podobnie jak w przypadku informacji o kondycji zasobów maszyny wirtualnej, w górnej części tego bloku widoczna jest skrócona lista problemów, a w dolnej — lista monitorowanych sieci.

Sekcja ze szczegółowymi danymi o stanie sieci zawiera listę potencjalnych problemów z zabezpieczeniami oraz [zalecenia](security-center-network-recommendations.md). Mogą wystąpić następujące problemy:

* Zapora nowej generacji (NGFW) nie jest zainstalowana
* Sieciowe grupy zabezpieczeń w podsieciach nie są włączone
* Sieciowe grupy zabezpieczeń w maszynach wirtualnych nie są włączone
* Ograniczanie dostępu zewnętrznego za pośrednictwem publicznego zewnętrznego punktu końcowego
* Punkty końcowe mające połączenie z Internetem w dobrej kondycji

Po kliknięciu zalecenia zostanie otwarty nowy blok zawierający więcej szczegółów na temat zalecenia, jak pokazano w poniższym przykładzie.

![Szczegóły dotyczące zalecenia w bloku Sieć](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

W tym przykładzie blok **Konfiguruj brakujące grupy zabezpieczeń sieci dla podsieci** zawiera listę podsieci i maszyn wirtualnych, dla których brakuje ochrony sieciowej grupy zabezpieczeń. Po kliknięciu podsieci, do której chcesz zastosować grupę zabezpieczeń sieci, otworzy się kolejny blok.

W bloku **Wybieranie grupy zabezpieczeń sieci** wybierz najbardziej odpowiednią sieciową grupę zabezpieczeń dla podsieci lub utwórz nową grupę zabezpieczeń sieci.

#### <a name="internet-facing-endpoints-section"></a>Sekcja Internet facing endpoints (Punkty końcowe umożliwiające dostęp do Internetu)
Sekcja **Punkty końcowe mające połączenie z Internetem** zawiera maszyny wirtualne, które są aktualnie skonfigurowane z użyciem punktu końcowego połączonego z Internetem, oraz ich bieżący stan.

![Maszyny wirtualne skonfigurowane z użyciem punktu końcowego połączonego z Internetem oraz stan](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Ta tabela zawiera nazwę punktu końcowego, która reprezentuje maszynę wirtualną, adres IP umożliwiający dostęp do Internetu oraz bieżący stan ważności dla sieciowej grupy zabezpieczeń i zapory nowej generacji. Informacje w tabeli są posortowane według ważności:

* Kolor czerwony (u góry): wysoki priorytet, konieczne jest natychmiastowe rozwiązanie problemu
* Kolor pomarańczowy: średni priorytet, konieczne jest możliwie szybkie rozwiązanie problemu
* Kolor zielony (ostatni): stan prawidłowy

#### <a name="networking-topology-section"></a>Sekcja Topologia sieci
Sekcja **Topologia sieci** zawiera zasoby przedstawione według hierarchii, jak pokazano na poniższym zrzucie ekranu:

![Hierarchiczny widok zasobów w sekcji Topologia sieci](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Informacje w tej tabeli (dotyczące maszyn wirtualnych i podsieci) zostały posortowane według ważności:

* Kolor czerwony (u góry): wysoki priorytet, konieczne jest natychmiastowe rozwiązanie problemu
* Kolor pomarańczowy: średni priorytet, konieczne jest możliwie szybkie rozwiązanie problemu
* Kolor zielony (ostatni): stan prawidłowy

W tym widoku topologii pierwszy poziom obejmuje pozycje [sieci wirtualne](../virtual-network/virtual-networks-overview.md), [bramy sieci wirtualnej](../vpn-gateway/vpn-gateway-site-to-site-create.md) i [sieci wirtualne (klasyczne)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Drugi poziom obejmuje podsieci, natomiast trzeci poziom przedstawia maszyny wirtualne, które należą do tych podsieci. Prawa kolumna przedstawia bieżący stan sieciowej grupy zabezpieczeń dla tych zasobów, jak pokazano w poniższym przykładzie:

![Stan grupy zabezpieczeń sieci w sekcji Topologia sieci](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

W dolnej części tego bloku znajdują się zalecenia dotyczące tej maszyny wirtualnej, co jest podobne do powyższego opisu. Kliknij zalecenie, aby dowiedzieć się więcej i ewentualnie zastosować odpowiednie środki kontroli lub konfigurację zabezpieczeń.

### <a name="monitor-data"></a>Monitorowanie danych
Po kliknięciu opcji **Dane** na kafelku **Kondycja zabezpieczeń zasobów** zostanie otwarty blok **SQL** zawierający zalecenia dotyczące problemów, takich jak brak włączonych funkcji inspekcji lub przezroczystego szyfrowania danych. Przedstawia on także [zalecenia](security-center-sql-service-recommendations.md) dotyczące ogólnej kondycji bazy danych.

![Kondycja zasobów SQL](./media/security-center-monitoring/security-center-monitoring-fig13-ga.png)

Można kliknąć dowolne zalecenie i uzyskać więcej szczegółów na temat dalszych działań w celu rozwiązania problemu. Poniższy przykład pokazuje rozszerzenie zalecenia **Inspekcja bazy danych nie jest włączona**.

![Szczegółowe informacje o zaleceniach SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga.png)

Blok **Włączanie inspekcji baz danych SQL** zawiera następujące informacje:

* Lista baz danych SQL
* Serwer, na którym znajdują się bazy danych SQL
* Informacje o tym, czy to ustawienie jest dziedziczone z serwera lub czy jest unikatowe w tej bazie danych
* Bieżący stan
* Ważność problemu

Po kliknięciu bazy danych w celu zastosowania tego zalecenia zostanie otwarty blok **Inspekcja i wykrywanie zagrożeń**, jak pokazano na poniższym zrzucie ekranu.

![Blok Inspekcja i wykrywanie zagrożeń](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Aby włączyć funkcję inspekcji, zaznacz pole wyboru **WŁĄCZONE** poniżej opcji **Inspekcja**.

### <a name="monitor-applications"></a>Monitorowanie aplikacji
Jeśli obciążenie platformy Azure obejmuje aplikacje znajdujące się na [maszynach wirtualnych (utworzonych za pomocą usługi Azure Resource Manager)](../azure-resource-manager/resource-manager-deployment-model.md) z uwidocznionymi portami sieci Web (porty TCP 80 i 443), usługa Security Center może je monitorować w celu identyfikowania potencjalnych problemów z zabezpieczeniami oraz rekomendowania czynności naprawczych. Po kliknięciu kafelka **Aplikacje** zostanie otwarty blok **Aplikacje** zawierający serię zaleceń w sekcji **Zalecenia dotyczące aplikacji**. Widoczny będzie również podział aplikacji według hosta/wirtualnego adresu IP, jak pokazano na poniższym zrzucie ekranu.

![Kondycja zabezpieczeń aplikacji](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Podobnie jak w przypadku innych zaleceń możesz je kliknąć, aby wyświetlić więcej szczegółów o problemie i sposobie jego rozwiązania. Na poniższym rysunku przedstawiono przykładową aplikację, która została zidentyfikowana jako niezabezpieczona aplikacja sieci Web. Po wybraniu aplikacji, która została uznana za niezabezpieczoną, zostanie otwarty kolejny blok, w którym dostępna będzie następująca opcja:

![Szczegółowe informacje o niezabezpieczonej aplikacji](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Ten blok będzie zawierać listę wszystkich zaleceń dla tej aplikacji. Po kliknięciu zalecenia **Dodaj zaporę aplikacji sieci Web** zostanie otwarty blok **Dodawanie zapory aplikacji sieci Web** z opcjami umożliwiającymi zainstalowanie zapory aplikacji sieci Web innej firmy, jak pokazano na poniższym zrzucie ekranu.

![Okno dialogowe Dodaj zaporę aplikacji sieci Web](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Zobacz też
W tym artykule przedstawiono sposób korzystania z funkcji monitorowania w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Ustawianie zasad zabezpieczeń w usłudze Azure Security Center](security-center-policies.md) — informacje na temat konfigurowania ustawień zabezpieczeń w usłudze Azure Security Center.
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.




<!--HONumber=Dec16_HO1-->


