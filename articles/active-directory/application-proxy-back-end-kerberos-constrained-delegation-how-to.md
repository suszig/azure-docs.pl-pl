---
title: "Jak skonfigurować serwer Proxy aplikacji aplikacji na używanie ograniczone delegowanie protokołu Kerberos | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować delegowanie ograniczone protokołu Kerberos dla aplikacji serwera Proxy aplikacji usługi Azure AD."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 3a768c30cb874d42d7b4fbd2eeaa6c0e23904e10
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-an-application-proxy-application-to-use-kerberos-constrained-delegation"></a>Jak skonfigurować serwer Proxy aplikacji aplikacji na używanie ograniczone delegowanie protokołu Kerberos

Dostępne metody do osiągnięcia logowania jednokrotnego do aplikacji opublikowanych nieco może różnić się od aplikacji i jedną z opcji, że serwer Proxy aplikacji Azure oferuje zaraz po jego zainstalowaniu, jest delegowanie ograniczone protokołu Kerberos (KCD). Jest to, których hostem łącznika jest skonfigurowany do uwierzytelniania kerberos ograniczonego do wewnętrznej bazy danych aplikacji, w imieniu użytkowników.

Procedura włączania KCD jest stosunkowo prosta i zwykle wymaga nie więcej niż ogólną wiedzą z różnymi składnikami i przebieg uwierzytelniania, która ułatwia obsługę logowania jednokrotnego. Znalezienie dobre źródła informacje ułatwiające rozwiązywanie problemów z scenariuszy, gdzie KCD logowania jednokrotnego nie działać zgodnie z oczekiwaniami, może być rozrzedzony.

Tak próbuje zapewnia jeden punkt odniesienia, które powinny pomóc w Rozwiązywanie problemów i własnym rozwiązania niektórych typowych problemów, które przedstawiono w tym artykule. W tym samym czasie oferują dodatkowe wskazówki dotyczące diagnozowania bardziej złożoną i plików implementacji.

należy pamiętać, że w tym artykule sprawia, że następujące założenia:

-   Serwera Proxy aplikacji Azure została wdrożona jako na naszych [dokumentacji](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) i dostępu do aplikacji z systemem innym niż KCD działa zgodnie z oczekiwaniami.

-   Aplikacja docelowa opublikowanych zależy od usług IIS i przez firmę Microsoft implementacja protokołu kerberos.

-   Host serwera i aplikacji znajdują się w jednej domenie usługi Active Directory. Szczegółowe informacje dotyczące wielu domen i lasów scenariusze można znaleźć w naszych [oficjalny dokument KCD](http://aka.ms/KCDPaper).

-   Aplikacja podmiotu jest publikowana na platformie Azure dzierżawcy z wstępne uwierzytelnianie jest włączone, a użytkownicy mogą uwierzytelniać na platformie Azure za pośrednictwem uwierzytelnianie oparte na formularzach. Scenariusze uwierzytelniania klienta sformatowanego nie są objęte w tym artykule, ale można dodać w pewnym momencie w przyszłości.

## <a name="prerequisites"></a>Wymagania wstępne

Serwer Proxy aplikacji Azure można wdrożyć do niemal dowolnego typu infrastruktury lub środowiska i architektur bez wątpienia różnią się od organizacji. Jedną z najbardziej typowych przyczyn KCD powiązanych problemy nie jest środowisk, ale zamiast prostego niewłaściwa konfiguracji lub ogólne nadzoru.

Z tego powodu informacjami jest zawsze najpierw upewnić się, czy zostały spełnione wszystkie wymagania wstępne określone w naszym głównym [przy użyciu logowania jednokrotnego KCD z artykułem serwera Proxy aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) przed rozpoczęciem rozwiązywania problemów.

Szczególnie sekcję na temat konfigurowania KCD na 2012 R2, jak to wykorzystuje podejście różni się w przypadku konfigurowania KCD w poprzednich wersjach systemu Windows, ale także będąc w trosce o kilka innych kwestii:

-   Nie jest często serwerem członkowskim domeny otworzyć okno dialogowe bezpiecznego kanału z określonego kontrolera domeny. Następnie przenieś innego okna dialogowego w dowolnym momencie, aby hosty łącznika zazwyczaj nie powinny być ograniczone do możliwość komunikacji z tylko określonej lokalnej lokacji kontrolerów domeny.

-   Podobnie jak powyżej punktu, obejmujące różne domeny, które scenariuszy zależą od odwołań, które bezpośrednie hostem łącznika do kontrolerów domeny, które mogą znajdować się poza lokalnej sieci obwodowej. W tym scenariuszu, który jest równie ważne upewnić się również umożliwi ruchu lub nowszej do kontrolerów domeny, które reprezentują innych domen lub innego delegowania zakończyć się niepowodzeniem.

-   Jeśli to możliwe, należy unikać wprowadzania żadnych aktywnych urządzeń adresy IP/Identyfikatory między hostami łącznika i kontrolery domeny, jak te są czasami za pośrednictwem niepożądanych i zakłócić ruch RPC core

Tyle chcielibyśmy szybkie rozwiązywanie problemów i ostatecznie może trwać, dlatego w miarę możliwości należy spróbuj i przetestować delegowania w najprostszym scenariuszy. Wprowadzenie więcej zmiennych, im bardziej może być konieczne będą konkurować o. Na przykład ograniczając testowanie do pojedynczego łącznika można zapisać cenny czas i dodatkowe łączników można dodać po rozwiązaniu problemów.

Pewne czynniki środowiska może również być przyczyną problemu, więc ponownie Jeśli to możliwe spróbuj i zminimalizować architektury do minimum systemu od zera do testowania. Na przykład nie są rzadko nieprawidłowej konfiguracji zapory wewnętrznej listy kontroli dostępu, więc jeśli to możliwe mieć cały ruch z łącznika może bezpośrednio za pomocą kontrolerów domeny i wewnętrznej bazy danych aplikacji. 

Faktycznie bezwzględną najlepszym miejscem, aby umieścić łączniki zbliża się zgodnie z ich celów jako może być. Wbudowany zapory nas o podczas testowania, po prostu dodaje niepotrzebne złożoność i właśnie przedłużyć czas badania sieci.

Tak co stanowi KCD problem mimo to? Dobrze istnieje kilka oznaczeń klasycznego KCD niepowodzeniem sesji rejestracji jednokrotnej i pierwszy objawy problemu zwykle pojawiają się w przeglądarce.

   ![Błąd konfiguracji KCD niepoprawne](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Autoryzacja nie powiodła się z powodu brakujących uprawnień](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

wszystkie które posiadają tej samej objaw nie może wykonać logowanie Jednokrotne i w związku z tym odmowie dostępu użytkownika do aplikacji.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

W jaki sposób można następnie Rozwiąż ten problem są zależne od problemu i obserwowanych objawów. Przed każdym przechodzi dalsze czy Sugerujemy następujące łącza, ponieważ zawierają one przydatne informacje, które można mogą nie mieć wszedł między:

-   [Rozwiązywanie problemów z serwera Proxy aplikacji i komunikaty o błędach](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot)

-   [Błędy protokołu Kerberos i objawy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#kerberos-errors)

-   [Praca z logowania jednokrotnego podczas lokalnej i w chmurze tożsamości nie są identyczne](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd#working-with-sso-when-on-premises-and-cloud-identities-are-not-identical)

Jeśli masz daleko, wówczas główny problem ostatecznie istnieje. Musisz szczegółowej analizy, więc uruchomienie oddzielając przepływu w trzech etapach distinct, które można rozwiązywać problemy.

**Wstępne uwierzytelnienie klienta** — użytkownika zewnętrznego, uwierzytelnianie na platformie Azure za pośrednictwem przeglądarki.

Możliwość wstępnego uwierzytelniania na platformie Azure jest potrzeby logowania jednokrotnego KCD funkcji. Powinno to przetestowane i skierowana najpierw, jeśli występują problemy. Należy pamiętać, etap uwierzytelniania wstępnego nie ma związku KCD lub opublikowanej aplikacji. Powinny być dość łatwe naprawić niezgodności przez związane z poprawnością Sprawdzanie konta podmiot istnieje na platformie Azure i że nie jest ono wyłączone/blokowane. Odpowiedzi na błąd w przeglądarce jest dość opisowa, że aby poznać przyczynę. Nasze dokumenty rozwiązywanie można zweryfikować, jeśli nie masz pewności, możesz również sprawdzić.

**Delegowanie usługi** — łącznika serwera Proxy Azure uzyskania biletu usługi z Centrum dystrybucji KLUCZY (Centrum dystrybucji protokołu Kerberos), protokołu kerberos w imieniu użytkowników.

Zewnętrzne komunikacji między klientem a Azure frontonu powinny nie ma wpływu na KCD, inne niż zapewnienia jego działanie. Jest to tak usługę serwera Proxy Azure można podać prawidłowy identyfikator które używane do uzyskania biletu kerberos. Bez tego KCD nie jest możliwe, a nie powiedzie się.

Jak wspomniano wcześniej, komunikaty o błędach przeglądarki zwykle zawierają niektóre dobrej wskazówek Dlaczego rzeczy kończą się niepowodzeniem. Upewnij się, że należy zanotować identyfikator działania i sygnaturę czasową w odpowiedzi jako pozwalają służące do skorelowania zachowania rzeczywiste zdarzenia w dzienniku zdarzeń serwera Proxy usługi Azure.

   ![Błąd konfiguracji KCD niepoprawne](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

I odpowiednie pozycje widoczne czy dziennik zdarzeń może być traktowany jako zdarzenia 13019 lub 12027. Można znaleźć w dziennikach zdarzeń łącznika **Dzienniki aplikacji i usług** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **Łącznik**&gt;**Admin**.

   ![Zdarzenie 13019 z dziennika zdarzeń serwera Proxy aplikacji](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Zdarzenie 12027 z dziennika zdarzeń serwera Proxy aplikacji](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

-   Na użytek rekord A w systemie DNS wewnętrzny adres aplikacji, a nie rekordu CName

-   Ponownie upewnij się, że host łącznika przyznano uprawnienia do delegowania wyznaczone konto docelowe głównej nazwy usługi, a **Użyj dowolnego protokołu uwierzytelniania** jest zaznaczone. Ten temat znajdują się w naszym głównym [artykułu konfiguracji logowania jednokrotnego](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd)

-   Sprawdź, czy jest tylko jedno wystąpienie SPN istniejących w AD przez wystawienie **setspn - x** z wiersza polecenia na żadnym hoście członek domeny

-   Sprawdź, czy zasady domeny jest wymuszany w celu ograniczenia [maksymalny rozmiar kerberos wystawionych tokenów](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/), jak to uniemożliwienia łącznik uzyskania tokenu Jeśli okaże się zbyt

Wymiany między hostem łącznika i domeny Centrum dystrybucji KLUCZY przechwytywania śledzenia sieci będzie wówczas następnego kroku najlepsze uzyskania bardziej niski poziom szczegółów dotyczących kwestii. Można je znaleźć w [nowości papieru rozwiązywanie](https://aka.ms/proxytshootpaper).

Jeśli biletów wygląda dobrze, powinny pojawić się zdarzenia w dzienniku informujący o tym uwierzytelnianie nie powiodło się z powodu aplikacji zwracanie 401. Zwykle wskazuje, że aplikacja docelowa odrzucenia biletu, więc kontynuować następujące kolejnego etapu.

**Aplikacji docelowej** -konsumenta biletu kerberos udostępniane przez łącznik programu

Na tym etapie łącznik powinien wysłali kerberos biletu usługi z zapleczem jako nagłówek w pierwszym żądaniu aplikacji.

-   Przy użyciu wewnętrzny adres URL aplikacji zdefiniowano w portalu, zweryfikuj, że aplikacja jest dostępny bezpośrednio z przeglądarki na hoście łącznika. Następnie możesz zalogować się pomyślnie. Szczegółowe informacje o tej czynności można znaleźć na stronie Rozwiązywanie problemów dotyczących łącznika.

-   Pracując nadal na hoście łącznika upewnij się, że uwierzytelniania między przeglądarką a aplikacja używa protokołu kerberos, wykonując jedną z następujących czynności:

1.  Uruchamianie narzędzi dla deweloperów (**F12**) w programie Internet Explorer lub użyj [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) z hosta łącznika. Przejdź do aplikacji przy użyciu wewnętrznego adresu URL i sprawdzić oferowany nagłówki autoryzacji WWW zwrócił w odpowiedzi z aplikacji, aby upewnić się, że negocjowania lub występuje protokołu kerberos. Obiektu blob kolejnych kerberos zwracany w odpowiedzi z przeglądarki do aplikacji zaczynają się zwykle **YII**, więc jest to dobry wskazanie protokołu kerberos w play. NTLM z drugiej strony zawsze rozpoczynają się **TlRMTVNTUAAB**, która odczytuje NTLMSSP podczas zdekodować z formatu Base64. Jeśli widzisz **TlRMTVNTUAAB** na początku obiektu blob, oznacza to, że protokół Kerberos jest **nie** dostępne. Jeśli widzisz, prawdopodobnie Kerberos dostępne.

  * Uwaga: Jeśli przy użyciu programu Fiddler, ta metoda wymaga czasowo wyłączyć ochrona rozszerzona na konfiguracji aplikacji w usługach IIS.

     ![Przeglądarce inspekcji sieci](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    *Rysunek:* ponieważ to nie rozpoczyna się od TIRMTVNTUAAB, jest to przykład, że protokół Kerberos jest dostępne. To jest przykład obiektu Blob protokołu Kerberos, który nie zaczyna się od YII.

2.  Z listy dostawców w lokacji i dostępu do aplikacji IIS bezpośrednio z programu Internet Explorer na hoście łącznika tymczasowe usunięcie NTLM. Z uwierzytelniania NTLM nie jest już na liście dostawców można uzyskać dostęp do aplikacji tylko przy użyciu protokołu Kerberos. Jeśli to się nie powiedzie, następnie które sugeruje, że występuje problem z konfiguracją aplikacji i uwierzytelnianie Kerberos nie działa.

Czy protokołu Kerberos nie jest dostępna, negocjowania ustawienia uwierzytelniania aplikacji w usługach IIS, aby upewnić się, czy są dostępne najwyżej z NTLM tuż poniżej. (Nie Negotiate: kerberos lub Negotiate: protokołu PKU2U). Nadal tylko, jeśli protokół Kerberos jest funkcjonalności.

   ![Dostawców uwierzytelniania systemu Windows](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
-   Z protokołu Kerberos i NTLM w miejscu umożliwia teraz tymczasowo wyłączyć wstępnego uwierzytelniania dla aplikacji w portalu. Zobacz, czy można do niego dostęp z Internetu przy użyciu zewnętrznego adresu URL. Powinien być monitowani o uwierzytelnienie i powinno być możliwe w tym celu z kontem używanym w poprzednim kroku. Jeśli nie, oznacza to problem z aplikacjami wewnętrznej bazy danych i nie KCD w ogóle.

-   Teraz ponownie włączyć wstępnego uwierzytelniania w portalu i uwierzytelniania za pośrednictwem platformy Azure przez próby połączenia aplikacji za pomocą jego zewnętrznego adresu URL. Logowanie Jednokrotne nie powiodło się, należy Zobacz niedozwolonych komunikat w przeglądarce, a także zdarzenie 13022 w dzienniku:

    *Łącznik serwera Proxy aplikacji usługi Microsoft AAD nie może uwierzytelnić użytkownika, ponieważ serwer wewnętrznej bazy danych odpowiada prób uwierzytelnienia Kerberos z powodu błędu HTTP 401.*

    ![Dostęp zabroniony błąd 401 HTTTP](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

-   Sprawdź, czy aplikacji usług IIS, aby upewnić się, pula aplikacji skonfigurowana jest skonfigurowany do używania tego samego konta, skonfigurowaną główną nazwę usługi przed w usłudze AD, przechodząc w usługach IIS, jak przedstawiono poniżej

    ![Okno konfiguracji aplikacji usług IIS](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

    Jeśli znasz już tożsamości, należy wydać następujące polecenie w wierszu polecenia, aby upewnić się, że to konto jest ostatecznie skonfigurowany z tej nazwy SPN. Na przykład **setspn – q http/spn.wacketywack.com**

    ![Okno polecenia SetSPN](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

-   Sprawdź, czy nazwa SPN zdefiniowane w odniesieniu do ustawień aplikacji w portalu jest tej samej nazwy SPN skonfigurowanego przed docelowe AD konto używane przez puli aplikacji

   ![Konfiguracja SPN w portalu Azure](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
-   Przejdź do usług IIS i wybierz **edytora konfiguracji** opcję dla aplikacji, a następnie przejdź do **system.webServer/security/authentication/windowsAuthentication** do upewnij się, że  **UseAppPoolCredentials** jest ustawiona na wartość true

   ![Opcja credential pul aplikacji konfiguracji usług IIS](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

Gdy są przydatne w zwiększanie wydajności operacji protokołu Kerberos, pozostawiając również włączono obsługę trybu jądra powoduje, że bilet żądanej usługi można odszyfrować za pomocą konta komputera. Jest to również system lokalny, więc po to ustawioną wartość true, podziału KCD w przypadku aplikacji znajduje się na wielu serwerach w farmie.

-   Jako dodatkowe sprawdzanie może również chcesz wyłączyć **rozszerzone** ochrony zbyt. Zostały napotkano scenariuszy, w którym to okazał się break KCD po włączeniu w bardzo określonej konfiguracji, gdy aplikacja jest publikowany jako podfolderem folderu domyślnej witryny sieci Web. Ten sam jest skonfigurowany do uwierzytelniania anonimowego, pozostawiając całego okna dialogowe nieaktywna co sugeruje, że obiekty podrzędne nie będzie dziedziczy wszystkie aktywne ustawienia. Jednak gdy możliwości zawsze zalecamy posiadanie to włączone, więc wszelkimi środkami testu, ale nie zapomnij Przywróć tutaj, aby włączyć.

Te dodatkowe kontrole powinny umieszczono należy na ścieżkę, aby rozpocząć korzystanie z opublikowanych aplikacji. Można teraz i aż dodatkowe łączniki, które także są skonfigurowane do delegowania, ale jeśli nie musisz kwestie następnie będzie Sugerujemy odczytu naszych bardziej szczegółowym opisem technicznym [kompletny przewodnik po dla serwera Proxy aplikacji usługi AD Rozwiązywanie problemów z platformy Azure](https://aka.ms/proxytshootpaper)

Jeśli nadal nie możesz się postęp problemu, pomocy technicznej będzie więcej niż przyjemnością pomóc i nadal w tym miejscu. Tworzenie biletu pomocy technicznej bezpośrednio z poziomu portalu i naszych inżynierów będzie dotrzeć do należy.

## <a name="other-scenarios"></a>Inne scenariusze

-   Serwer Proxy aplikacji Azure żąda biletu Kerberos przed wysłaniem żądania do aplikacji. Niektóre 3 aplikacje firm, takich jak Tableau serwera nie chcesz, aby ta metoda uwierzytelniania, a raczej oczekuje więcej z konwencjonalnej negocjacji została wykonana. Pierwsze żądanie jest anonimowe, co pozwala aplikacji na odpowiedź z typami uwierzytelniania obsługuje za pośrednictwem 401.

-   Podwójny przeskok uwierzytelnianie — często używane w scenariuszach, w którym warstwy aplikacji z wewnętrznej bazy danych i front end wymagające uwierzytelniania, takich jak SQL Reporting Services.

## <a name="next-steps"></a>Następne kroki
[Skonfiguruj ograniczone delegowanie protokołu kerberos (KCD) do domeny zarządzanej](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-enable-kcd)
