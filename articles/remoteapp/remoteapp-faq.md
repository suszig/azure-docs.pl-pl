<properties 
    pageTitle="Usługa Azure RemoteApp — często zadawane pytania | Microsoft Azure" 
    description="Poznaj odpowiedzi na najczęściej zadawane pytania dotyczące usługi Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="04/08/2016" 
    ms.author="elizapo"/>

# Często zadawane pytania dotyczące usługi Azure RemoteApp
Otrzymujemy następujące pytania dotyczące usługi Azure RemoteApp. Masz więcej pytań? Odwiedź [fora usługi RemoteApp](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) i określ, jakich informacji potrzebujesz, lub dodaj komentarz poniżej.

## Co to jest usługa Azure RemoteApp? ##


- **Co to jest usługa Azure RemoteApp?** Azure RemoteApp jest usługą platformy Azure, która pomaga zapewnić bezpieczny, zdalny dostęp do aplikacji z wielu różnych urządzeń użytkowników. Dowiedz się więcej na temat usługi [Azure RemoteApp](remoteapp-whatis.md).
- **Jakie są opcje wdrażania?** Istnieją dwa rodzaje kolekcji usługi RemoteApp: w chmurze i hybrydowa. Wybór odpowiedniej opcji zależy od wielu czynników, takich jak potrzeba przyłączania do domeny. Wszystkie te aspekty zostały omówione [w tym miejscu](remoteapp-collections.md).

## Szybkie porady dotyczące korzystania z usługi Azure RemoteApp ##
- **Kiedy nastąpi odłączenie? Jak długo można pozostawać w stanie bezczynności przed utratą połączenia?** 4 godziny. W przypadku pozostawania w stanie bezczynności przez Ciebie lub jednego z Twoich użytkowników przez 4 godziny nastąpi automatyczne wylogowanie z usługi Azure RemoteApp. Zapoznaj się z innymi ustawieniami domyślnymi w temacie [Subskrypcja oraz limity, przydziały i ograniczenia usług platformy Azure](../azure-subscription-service-limits.md).
- **Czy można wypróbować tę usługę bezpłatnie?** Tak. Jest dostępna bezpłatna wersja próbna ważna przez 30 dni. Po zakończeniu okresu próbnego można przejść na konto płatne (które może być używane w środowisku produkcyjnym) lub zaprzestać korzystania z usługi. Uruchom bezpłatną wersję próbną, przechodząc do witryny [portal.azure.com](http://portal.azure.com) i utwórz nowe wystąpienie usługi RemoteApp. W przypadku bezpłatnej wersji próbnej możesz utworzyć 2 wystąpienia usługi RemoteApp z 10 użytkownikami dla każdego wystąpienia. Należy pamiętać, że ta wersja próbna działa tylko przez 30 dni.
## Szczegółowe informacje o subskrypcji usługi Azure RemoteApp ##

- **Jakie są ograniczenia usługi?** Więcej informacji na temat ustawień domyślnych i ograniczeń usługi Azure RemoteApp można znaleźć w temacie [Subskrypcja oraz limity, przydziały i ograniczenia usług platformy Azure](../azure-subscription-service-limits.md). Poinformuj nas, jeśli masz więcej pytań.
- **Ilu użytkowników muszę mieć?** Minimalna liczba użytkowników wynosi 20. Powtórzmy dla pełnej jasności — MINIMALNA liczba to 20. Opłaty będą naliczane za 20 użytkowników. 
- **Jaki jest koszt usługi RemoteApp?** Zapoznaj się z tematem [Szczegółowe informacje o cenach usługi Azure RemoteApp](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Czy kolekcje różnych typów różnią się ceną?** Tak, w zależności od wymagań kolekcji. Kolekcja hybrydowa wymaga połączenia usługi Azure RemoteApp z siecią lokalną. Jeśli używana jest sieć wirtualna lub usługa Express Route, nie ma żadnych dodatkowych kosztów. Jednak w przypadku korzystania z nowej sieci wirtualnej platformy Azure oraz bramy lub usługi Express Route zostanie naliczona opłata za usługę [VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) lub [Express Route](../../../pricing/details/expressroute/). Ten koszt (szczegóły dostępne w linkach) jest dodawany do miesięcznej opłaty za usługę Azure RemoteApp.

## Kolekcje — co jest obsługiwane, czego należy używać i inne informacje
- **Czy są obsługiwane niestandardowe aplikacje LOB (biznesowe)?** Tak. Aby użyć aplikacji niestandardowej w usłudze Azure RemoteApp, utwórz [obraz niestandardowego szablonu](remoteapp-create-custom-image.md) i przekaż go do kolekcji RemoteApp.
- **Czy moja niestandardowa aplikacja LOB będzie działać w usłudze Azure RemoteApp?** Najlepszym sposobem, aby to sprawdzić, jest przetestowanie. Zobacz [Centrum zgodności pulpitu zdalnego](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Która metoda wdrażania (w chmurze czy hybrydowa) jest najlepsza dla mojej organizacji?** Kolekcje hybrydowe zawierają najbardziej kompleksowe środowisko w przypadku pełnej integracji z logowaniem jednokrotnym (SSO) i bezpieczną lokalną łącznością sieciową. Kolekcje w chmurze umożliwiają elastyczne i łatwe izolowanie wdrożenia przy użyciu wielu metod uwierzytelniania. Dowiedz się więcej na temat [opcji wdrażania](remoteapp-whatis.md).
- **Korzystamy z bazy danych SQL lub innej bazy danych lokalnie lub na platformie Azure. Jaki typ wdrożenia powinniśmy zastosować?** To zależy od lokalizacji bazy danych SQL lub bazy danych zaplecza. Jeśli baza danych znajduje się w sieci prywatnej, należy użyć kolekcji hybrydowej. Jeśli baza danych ma połączenie z Internetem i umożliwia nawiązywanie połączeń przez klientów, można użyć kolekcji w chmurze.
- **Jak wygląda sytuacja w przypadku mapowania dysku, portów USB i szeregowych, udostępniania schowka i przekierowywania drukarki?** Wszystkie te funkcje są obsługiwane przez usługę Azure RemoteApp. Udostępnianie schowka i przekierowywanie drukarki jest domyślnie włączone. Dowiedz się więcej o przekierowywaniu [tutaj](remoteapp-redirection.md). 


## Obrazy szablonów
- **Czy można użyć istniejącej maszyny wirtualnej lub maszyny wirtualnej w chmurze jako szablonu mojej kolekcji usługi RemoteApp?** Tak! Można utworzyć obraz oparty na maszynie wirtualnej platformy Azure, użyć jednego z obrazów zawartych w ramach subskrypcji lub utworzyć obraz niestandardowy. Zobacz temat [Opcje obrazu usługi RemoteApp](remoteapp-imageoptions.md).


## Opcje sieciowe
- **Kolekcja hybrydowa wymaga sieci wirtualnej. Czy można użyć istniejącej sieci wirtualnej?** Można, pod warunkiem, że jest to istniejąca sieć wirtualna na platformie Azure. Aby uzyskać więcej informacji, zobacz temat „Krok 1: konfiguracja sieci wirtualnej” w [instrukcjach dotyczących kolekcji hybrydowej](remoteapp-create-hybrid-deployment.md).
- **Czy w przypadku kolekcji w chmurze można użyć sieci wirtualnej?** Tak, można. Aby uzyskać więcej informacji, zobacz temat [Tworzenie kolekcji w chmurze](remoteapp-create-cloud-deployment.md), w szczególności krok 1.

## Opcje uwierzytelniania



- **Jak rozwiązano kwestię uwierzytelniania? Które metody są obsługiwane?** Kolekcja w chmurze obsługuje konta Microsoft i konta usługi Azure Active Directory, które są również kontami usługi Office 365. Kolekcja hybrydowa obsługuje tylko konta usługi Azure Active Directory, które zostały zsynchronizowane (przy użyciu narzędzia, takiego jak [narzędzie do synchronizacji usługi Azure Active Directory](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) przy wdrażaniu usługi Active Directory systemu Windows Server z użyciem opcji synchronizacji haseł lub ze skonfigurowaną federacją usług Active Directory Federation Services (AD FS). Można również skonfigurować uwierzytelnianie [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]Użytkownicy usługi Azure Active Directory muszą pochodzić z dzierżawy, która jest skojarzona z subskrypcją. (Subskrypcję można wyświetlać i modyfikować na karcie **Ustawienia** w portalu. Aby uzyskać więcej informacji, zobacz temat [Zmiana dzierżawy usługi Azure Active Directory używanej przez usługę RemoteApp](remoteapp-changetenant.md)).

- **Dlaczego nie można przekazać dostępu do konta usługi Azure Active Directory?** Użytkownicy usługi Azure Active Directory muszą należeć do katalogu, który jest skojarzony z subskrypcją. Ten katalog można wyświetlać lub modyfikować na karcie ustawień w portalu. Aby uzyskać więcej informacji, zobacz temat [Zmiana dzierżawy usługi Azure Active Directory używanej przez usługę RemoteApp](remoteapp-changetenant.md).

## Klienci — jakiego urządzenia można używać do uzyskiwania dostępu do usługi Azure RemoteApp?
Informacje o klientach, w tym kroki instalacji różnych klientów, można znaleźć w temacie [Uzyskiwanie dostępu do aplikacji w usłudze Azure RemoteApp](remoteapp-clients.md).

- **Które urządzenia i systemy operacyjne są obsługiwane przez aplikacje klienckie?**
Po pierwsze, komputery i tablety: 
    - Windows 10 (wersja zapoznawcza klienta)
    - Windows 8.1 i Windows 8
    - Windows 7 z dodatkiem Service Pack 1
    - Mac OS X
    - Windows RT
    - Tablety z systemem Android
    - Urządzenia iPad i telefony:
    - iPhone
    - Android
    - Windows Phone
 
    [Pobierz](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) teraz klienta usługi RemoteApp.
- **Czy usługa Azure RemoteApp obsługuje zubożonych klientów?** Tak, obsługiwani są następujący zubożeni klienci z systemem Windows Embedded:
    - Windows Embedded Standard 7
    - Windows Embedded 8 Standard
    - Windows Embedded 8.1 Industry Pro
    - Windows 10 IoT Enterprise

- **Która wersja systemu Windows Server jest obsługiwana przez hosta sesji usług pulpitu zdalnego (RDSH)?** Windows Server 2012 R2.

##Pomoc techniczna i opinie


- **Jaki jest plan pomocy technicznej dla usługi RemoteApp?** Obsługa zarządzania rozliczeniami i subskrypcjami jest bezpłatna. Pomoc techniczna jest dostępna za pośrednictwem [planów usług platformy Azure](https://azure.microsoft.com/support/plans/). Można również uzyskać bezpłatne wsparcie społeczności za pośrednictwem [forum dyskusyjnego platformy Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Jak przesłać opinię?** Odwiedź [forum opinii](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Gdzie mogę dowiedzieć się więcej na temat usługi Azure RemoteApp?** Oprócz naszego [forum dyskusyjnego](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), które jest doskonałym miejscem do zadawania pytań, można dołączyć do cotygodniowych [seminariów internetowych z udziałem ekspertów](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), gdzie są omawiane wszystkie tematy związane z usługą RemoteApp.
- **Co z dokumentacją usługi RemoteApp?** Cieszymy się z tego pytania. Oprócz tematów pomocy w szufladzie pomocy portalu (wystarczy kliknąć ikonę **?** na dowolnej stronie w portalu) dostępne są następujące artykuły omawiające wszystkie aspekty usługi RemoteApp:
    - **Wprowadzenie:**
        - [Co to jest usługa RemoteApp?](remoteapp-whatis.md)
        - [Co zawierają obrazy szablonów usługi RemoteApp?](remoteapp-images.md)
        - [Jak działa licencjonowanie?](remoteapp-licensing.md)
        - [Jak programy pakietu Office współpracują z usługą RemoteApp?](remoteapp-o365.md)
        - [Jak działa przekierowanie w usłudze RemoteApp](remoteapp-redirection.md)?
    - **Wdrażanie:**
        - [Tworzenie niestandardowego obrazu szablonu](remoteapp-create-custom-image.md)
        - [Tworzenie kolekcji hybrydowej](remoteapp-create-hybrid-deployment.md)
        - [Tworzenie kolekcji w chmurze](remoteapp-create-cloud-deployment.md)
        - [Konfigurowanie usługi Azure Active Directory dla usługi RemoteApp](remoteapp-ad.md)
        - [Publikowanie aplikacji w usłudze RemoteApp](remoteapp-publish.md)
    - **Zarządzanie:**
        - [Dodawanie użytkowników](remoteapp-user.md)
        - [Najlepsze praktyki dotyczące konfigurowania i używania usługi RemoteApp](remoteapp-bestpractices.md)  

    Filmy. Mamy także szereg filmów dotyczących usługi RemoteApp. Niektóre zapewniają wprowadzenie ([Wprowadzenie do usługi Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), podczas gdy inne dotyczą wdrożenia ([Wdrożenie w chmurze](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) i [Wdrożenie hybrydowe](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Warto się z nimi zapoznać.

 
### Pomóż nam sobie pomóc 
Czy wiesz, że możesz nie tylko ocenić ten artykuł i dodać komentarze poniżej, ale także wprowadzać zmiany w samym artykule? Brakuje informacji? Coś jest nie tak? Treść artykułu jest niejasna? Przewiń w górę i kliknij pozycję **Edit on GitHub** (Edytuj w serwisie GitHub) w celu wprowadzenia zmian. Te modyfikacje zostaną przez nas sprawdzone, a po zatwierdzeniu Twoje zmiany i poprawki będą widoczne w tym miejscu.



<!--HONumber=Jun16_HO2-->


