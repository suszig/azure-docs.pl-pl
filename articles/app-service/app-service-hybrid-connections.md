---
title: "Usługa aplikacji Azure połączeń hybrydowych | Dokumentacja firmy Microsoft"
description: "Jak utworzyć i dostęp do zasobów w różnych sieciach za pomocą połączeń hybrydowych"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Połączenia hybrydowe usługi aplikacji Azure #

## <a name="overview"></a>Omówienie ##

Połączenia hybrydowe jest zarówno usługa na platformie Azure, jak i funkcji w usłudze Azure App Service.  Jako usługa ma użycia i możliwości poza tymi, które można wykorzystać w usłudze Azure App Service.  Aby dowiedzieć się więcej na temat połączeń hybrydowych i ich użycia poza usługi Azure App Service można uruchomić w tym miejscu [połączeń hybrydowych przekaźnika usługi Azure][HCService]

W usłudze Azure App Service można uzyskać dostęp do zasobów aplikacji w innych sieciach połączeń hybrydowych było możliwe. Zapewnia dostęp z aplikacji do punktu końcowego w aplikacji.  Nie umożliwia alternatywnych możliwości uzyskania dostępu do Twojej aplikacji.  Używane w usłudze App Service, każde połączenie hybrydowe są powiązane z jednym kombinacji hosta i portu TCP.  Oznacza to, że punktu końcowego połączenia hybrydowe można w dowolnym systemie operacyjnym i dowolnej aplikacji, pod warunkiem, że są naciśnięcie port nasłuchiwania protokołu TCP. Połączeń hybrydowych wiedzieć, lub nie obsługę protokołu aplikacji jest lub co chcesz uzyskać dostęp.  Po prostu zapewnia dostęp do sieci.  


## <a name="how-it-works"></a>Jak to działa ##
Funkcja połączenia hybrydowe składa się z dwóch wywołań wychodzących do przekaźnik magistrali usług.  Brak połączenia z biblioteki na hoście, gdy aplikacja działa w usłudze app service i następnie istnieje połączenie hybrydowe Manager(HCM) połączenia przekaźnika usługi Service Bus.  HCM jest usługi przekazywania, który można wdrożyć w ramach hosting sieci 

Przez dwa połączenia przyłączone do Twojej aplikacji ma tunelu TCP kombinację stałych hosta: port. po drugiej stronie HCM.  Połączenie korzysta z protokołu TLS 1.2 dla zabezpieczeń i klucze sygnatury dostępu Współdzielonego dla uwierzytelniania/autoryzacji.    

![][1]

Gdy aplikacja wysyła żądanie DNS zgodny punktu końcowego połączenia hybrydowego Konfigurowanie, ruch wychodzący TCP zostanie przekierowany połączenie hybrydowe.  

> [!NOTE]
> Oznacza to, że należy zawsze używać nazwy DNS dla połączenia hybrydowego.  Niektóre oprogramowanie klienckie nie wyszukiwania DNS, jeśli zamiast tego punktu końcowego korzysta z adresu IP.
>
>

Istnieją dwa typy połączeń hybrydowych było możliwe, nowych połączeń hybrydowych, które są oferowane jako usługa przekaźnika usługi Azure i starszych połączeń hybrydowych BizTalk.  Starsze połączeń hybrydowych BizTalk są określane jako klasycznego połączeń hybrydowych w portalu.  Ma więcej informacji w dalszej części tego dokumentu o nich.

### <a name="app-service-hybrid-connection-benefits"></a>Korzyści połączenia hybrydowe usługi aplikacji ###

Istnieje wiele zalet tym możliwości połączenia hybrydowego

- Aplikacje mogą bezpiecznie uzyskiwać dostęp do systemów lokalnych i usług bezpiecznie
- funkcja nie wymaga dostępnym punkcie końcowym internet
- jest szybkie i łatwe konfigurowanie  
- Każde połączenie hybrydowe odpowiada kombinację pojedynczy host: port, który jest elementem znakomity zabezpieczeń
- zwykle nie wymaga luk zapory jako połączenia są wszystkie wychodzące przez porty standardowe sieci web
- ponieważ funkcja jest na poziomie sieci, który również oznacza, że jest niezależny od języka używanego przez aplikację i technologii używanych przez punkt końcowy
- może służyć do zapewnienia dostępu w wielu sieciach z jednej aplikacji 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Czynności, które nie można wykonywać z połączeń hybrydowych ###

Istnieje kilka rzeczy, które nie można wykonać za pomocą połączeń hybrydowych było możliwe, i zawierają:

- zainstalowanie dysku
- przy użyciu protokołu UDP
- Uzyskiwanie dostępu do TCP na podstawie usług, które używają portów dynamicznych, takie jak FTP w trybie pasywnym lub w trybie pasywnym rozszerzone
- Obsługa protokołu LDAP, ponieważ czasami wymaga protokołu UDP
- Obsługa Active Directory

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Dodawanie i tworzenie połączenia hybrydowego w aplikacji ##

Można tworzyć połączeń hybrydowych było możliwe za pośrednictwem portalu aplikacji lub za pomocą portalu usługi połączenia hybrydowego.  Zdecydowanie zaleca się, użyj portalu aplikacji do tworzenia połączeń hybrydowych mają być używane z aplikacji.  Aby utworzyć połączenie hybrydowe, przejdź do [portalu Azure] [ portal] i przejdź do interfejsu użytkownika dla aplikacji.  Wybierz **sieci > skonfiguruj punkty końcowe połączenia hybrydowego**.  W tym miejscu można wyświetlić połączeń hybrydowych, które są skonfigurowane przy użyciu aplikacji  

![][2]

Aby dodać nowe połączenie hybrydowe, kliknij przycisk Dodaj połączenie hybrydowe.  Interfejs użytkownika, który zostaje otwarty zawiera listę połączeń hybrydowych, które zostały już utworzone.  Aby dodać co najmniej jeden z nich do aplikacji, kliknij te mają i naciśnij **Dodaj wybrane połączenie hybrydowe**.  

![][3]

Jeśli chcesz utworzyć nowe połączenie hybrydowe, kliknij przycisk **Utwórz nowe połączenie hybrydowe**.  W tym miejscu można określić: 

- Nazwa punktu końcowego
- Nazwa hosta punktu końcowego
- Port punktu końcowego
- przestrzeń nazw magistrali usług, które chcesz użyć

![][4]

Przestrzeń nazw magistrali usług jest związany z każdego połączenia hybrydowe i każda przestrzeń nazw magistrali usług jest region platformy Azure.  Jest ważne, i spróbuj użyć w tym samym regionie co aplikację tak, aby uniknąć opóźnienia sieci powstaniu przestrzeń nazw magistrali usług.

Jeśli chcesz usunąć połączenie hybrydowe z aplikacji, kliknij prawym przyciskiem myszy na nim i wybierz **rozłączenia**.  

Po dodaniu połączenia hybrydowego do aplikacji sieci web, możesz wyświetlić szczegóły na nim, po prostu kliknij go.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Połączenia hybrydowe i planów usługi aplikacji ##

Połączenia hybrydowe tylko, które można teraz wykonać są nowych połączeń hybrydowych.  Tylko są dostępne w Basic, Standard i Premium izolowany cennik jednostki SKU.  Istnieją powiązane planu cenowego ograniczenia.  

| Cennik planu | Liczba połączeń hybrydowych, której można używać w planie |
|----|----|
| Podstawowa | 5 |
| Standardowa | 25 |
| Premium | 200 |
| Izolowane | 200 |

Ponieważ istnieją ograniczenia planu usługi App Service jest również interfejs użytkownika w planie aplikacji usługi, który pokazuje, jak wiele połączeń hybrydowych są używane i jakie aplikacje.  

![][6]

Podobnie jak z widokiem aplikacji można wyświetlić szczegółowe informacje na połączenie hybrydowe, klikając go.  We właściwościach pokazano tutaj wyświetlane są informacje, które miały w widoku aplikacji, ale można również sprawdzić, jak wiele aplikacji w tym samym planie usługi aplikacji korzystają z tego połączenia hybrydowego.

![][7]

Gdy istnieje limit liczby punkty końcowe połączenia hybrydowego, które mogą być używane w planie usługi aplikacji, każde połączenie hybrydowe używane można w dowolnej liczby aplikacji w tym planie usługi aplikacji.  To znaczy, gdyby 1 połączenia hybrydowego używany w 5 osobnych aplikacji w mojej planu usługi App Service, która jest nadal tylko 1 połączenia hybrydowego.

Brak dodatkowych kosztów połączeń hybrydowych poza trwa tylko w Basic, Standard, Premium lub izolowane jednostki SKU.  Dla informacji o cenach połączenia hybrydowe przejdź tutaj: [cennik usługi Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Menedżera połączeń hybrydowych ##

Aby połączeń hybrydowych było możliwe do pracy należy agenta przekazywania w sieci, który jest hostem punktu końcowego połączenia hybrydowego.  Agent przekazywania jest nazywany Menedżera połączeń hybrydowych (HCM).  To narzędzie można pobrać z **sieci > skonfiguruj punkty końcowe połączenia hybrydowego** interfejsu użytkownika dostępne z aplikacji w [portalu Azure][portal].  

To narzędzie jest uruchamiane w systemie Windows server 2008 R2 i nowszych wersjach systemu Windows.  Wcześniej zainstalowano HCM działa jako usługa.  Ta usługa łączy do skonfigurowanych punktów końcowych w oparciu przekaźnik magistrali usług Azure.  Połączenia z HCM są ruch wychodzący do portów 80 i 443.    

HCM ma interfejsu użytkownika, aby go skonfigurować.  Po zainstalowaniu HCM można uzupełnić interfejsu użytkownika, uruchamiając HybridConnectionManagerUi.exe, który znajduje się w katalogu instalacyjnym Menedżera połączeń hybrydowych.  Jest także łatwo osiągnął w systemie Windows 10, wpisując *interfejsu użytkownika Menedżera połączeń hybrydowych* w polu wyszukiwania.  

Po uruchomieniu interfejsu użytkownika HCM, zostanie wyświetlony w pierwszej kolejności jest tabelę, która zawiera listę wszystkich połączeń hybrydowych, które zostały skonfigurowane dla tego wystąpienia HCM.  Jeśli chcesz wprowadzić zmiany, które mają być do uwierzytelniania w usłudze Azure. 

Aby dodać jeden lub więcej połączeń hybrydowych było możliwe do Twojej HCM:

1. Uruchom HCM interfejsu użytkownika
1. Kliknij przycisk Konfiguruj innego połączenia hybrydowego![][8]

1. Zaloguj się przy użyciu konta platformy Azure
1. Wybieranie subskrypcji
1. Polecenie połączeń hybrydowych ma to HCM do przekazywania![][9]

1. Klikanie pozycji Zapisz.

W tym momencie zostanie wyświetlony połączeń hybrydowych, dodane.  Można również kliknij połączenie hybrydowe skonfigurowany i szczegółowe informacje o połączeniu.

![][10]

Dla Twojego HCM można było obsługiwać połączenia hybrydowe, który jest skonfigurowany z musi:

- TCP dostępu na platformie Azure przez porty 80 i 443
- TCP dostęp do punktu końcowego połączenia hybrydowego
- możliwości DNS wyszukiwań na hoście punktu końcowego i przestrzeń nazw magistrali usług azure

HCM obsługuje zarówno nowych połączeń hybrydowych, jak i starszych połączeń hybrydowych BizTalk.

### <a name="redundancy"></a>Nadmiarowość ###

Każdy HCM może obsługiwać wiele połączeń hybrydowych.  Ponadto każde połączenie hybrydowe danego mogą być obsługiwane przez wiele HCMs.  Domyślnym zachowaniem jest działanie okrężne ruchu między skonfigurowanego HCMs dla dowolnego danego punktu końcowego.  Jeśli chcesz wysokiej dostępności połączeń hybrydowych z sieci, po prostu utworzyć wystąpienia wielu HCMs na oddzielnych komputerach. 

### <a name="manually-adding-a-hybrid-connection"></a>Ręcznie dodać połączenie hybrydowe ###

Jeśli ktoś poza subskrypcją wystąpienia HCM połączenia hybrydowe danego hosta, możesz udostępniać je parametry połączenia bramy połączenia hybrydowego.  Zobacz ten we właściwościach połączenia hybrydowe w [portalu Azure][portal]. Aby użyć tego ciągu, kliknij przycisk **ręcznie skonfigurować** przycisk HCM i Wklej w parametrach połączenia bramy.


## <a name="troubleshooting"></a>Rozwiązywanie problemów ##

Gdy połączenie hybrydowe została skonfigurowana z uruchomioną aplikację i istnieje co najmniej jeden HCM, który ma skonfigurowane połączenie hybrydowe, a następnie dowiesz się, stan **połączony** w portalu.  Jeśli nie powiedzieć **połączony** oznacza to, że aplikacja nie działa lub Twoje HCM nie można nawiązać połączenia limit Azure na porty 80 i 443.  

Podstawowym powodem, że klienci nie mogą łączyć się ich punktu końcowego jest, ponieważ punkt końcowy został określony przy użyciu adresu IP zamiast nazwy DNS.  Jeśli aplikacja nie może połączyć się żądanego punktu końcowego i adres IP jest używany, przełącz się do przy użyciu nazwy DNS, która jest nieprawidłowa na hoście, którym jest uruchomiona HCM.  Inne czynności do wykonania są nazwy DNS rozpoznaje poprawnie na hoście, na którym jest uruchomiona HCM i że istnieje łączność z hosta, którym jest uruchomiona HCM do punktu końcowego połączenia hybrydowego.  

Brak narzędzia usługi aplikacji, który może być wywoływany z konsoli o nazwie tcpping.  To narzędzie można stwierdzić, jeśli mają dostęp do punktu końcowego TCP, ale właściwość nie określa, czy masz dostęp do punktu końcowego połączenia hybrydowego.  W przypadku używania w konsoli dla punktu końcowego połączenia hybrydowe, pomyślne polecenie ping tylko informuje czy masz połączenie hybrydowe skonfigurowane dla twojej aplikacji, który używa tej kombinacji hosta: port.  

## <a name="biztalk-hybrid-connections"></a>Połączenia hybrydowe BizTalk ##

Starsze możliwości połączeń hybrydowych BizTalk został zamknięty poza do dalszego Tworzenie połączenia hybrydowego BizTalk.  Nadal korzystać z istniejących połączeń hybrydowych BizTalk z aplikacjami, ale należy migrować do nowej usługi.  Korzyści w nową usługę BizTalk wersji należą:

- żadne dodatkowe konto usług BizTalk — wersja nie jest wymagane
- Protokół TLS to 1.2 zamiast 1.0 jak połączeń hybrydowych BizTalk
- Komunikacja odbywa się za pośrednictwem portów porty 80 i 443 do osiągnięcia Azure zamiast adresów IP i zakres dodatkowych innych przy użyciu nazwy DNS.  

Aby dodać połączenie hybrydowe BizTalk do aplikacji, przejdź do aplikacji w [portalu Azure] [ portal] i kliknij przycisk **sieci > skonfiguruj punkty końcowe połączenia hybrydowego**.  W tabeli połączeń hybrydowych klasycznym kliknij **Dodaj połączenie hybrydowe klasycznego**.  W tym miejscu zostanie wyświetlona lista połączeń hybrydowych BizTalk.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/

