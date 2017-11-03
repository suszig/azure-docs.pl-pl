---
title: "Azure Notification Hubs — wytyczne diagnostyki"
description: "Wytyczne dotyczące zdiagnozować typowe problemy z usługą Azure Notification Hubs."
services: notification-hubs
documentationcenter: Mobile
author: ysxu
manager: erikre
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 32e3a2e6f840afd865375a622cfae0d33ba65090
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-notification-hubs---diagnosis-guidelines"></a>Centra powiadomień Azure - wytyczne diagnostyki
## <a name="overview"></a>Omówienie
Jedną z najbardziej typowe pytania, na które Otrzymaliśmy od klientów usługi Azure Notification Hubs jest sposób, aby dowiedzieć się, dlaczego nie widzą powiadomień wysyłanych z ich zaplecza aplikacji są wyświetlane na urządzeniu klienckim — gdy i dlaczego powiadomienia zostały usunięte i jak rozwiązać ten problem. W tym artykule będzie rozszerzana za pomocą różnych powodów, dlaczego powiadomienia mogą pobrać usunięte lub nie kończą na urządzeniach. Firma Microsoft będzie wyglądać za pośrednictwem metod można analizować i ustalić główną przyczynę. 

Po pierwsze, bardzo ważne jest zrozumienie, jak usługi Azure Notification Hubs wypychanie powiadomień do urządzeń.
![][0]

Do typowych wysyłania powiadomień przepływu, komunikat jest wysyłany z **zaplecza aplikacji** do **Centrum powiadomień Azure (NH)** która z kolei działa przetwarza wszystkie rejestracji, biorąc pod uwagę skonfigurowanych znaczników & wyrażeń tagów w celu ustalenia "cele" tzn. wszystkie rejestracje wymagające do odbierania powiadomień wypychanych. Te mogą znajdować się w całości lub części naszych obsługiwanych platform - iOS, Google, Windows, Windows Phone urządzeń Kindle i Baidu dla Chin systemu Android. Po są elementy docelowe NH, a następnie umieszcza powiadomień, podzielić na wiele partii rejestracji dla określonej platformy urządzenia **Push Notification Service (PNS)** -np. usługi APNS do firmy Apple, usługi GCM dla Google itp. NH jest uwierzytelniany w usłudze odpowiednich systemu powiadomień platformy dla poświadczeń ustawiona w klasycznym portalu Azure na stronie Konfigurowanie Centrum powiadomień. System powiadomień platformy przekazuje następnie powiadomienia do odpowiednich **urządzeń klienckich**. Jest to platforma zalecany sposób dostarczać powiadomienia wypychane i należy pamiętać, że do końcowej gałęzi dostarczania powiadomień o zapytaniach odbywa się między Platforma systemu powiadomień platformy i urządzenia. W związku z tym mamy czterech głównych elementów - *klienta*, *zaplecza aplikacji*, *centra powiadomień Azure (NH)* i *Push Notification Services (PNS)* i któregoś z powyższych może spowodować powiadomienia porzucane. Więcej informacji na temat tej architektury jest dostępna w [omówienie centra powiadomień].

Nie można dostarczyć powiadomień może się tak zdarzyć podczas początkowej testu/przemieszczania fazy, który może wskazywać na problem z konfiguracją lub może się zdarzyć, w którym wszystkie lub niektóre powiadomień może występować porzuconych wskazujący niektórych bardziej aplikacji lub problem wzorzec do obsługi komunikatów w środowisku produkcyjnym. W sekcji poniżej przedstawiono różne scenariusze porzuconych powiadomień od typowych do rzadkich rodzaj, niektóre z nich może się okazać oczywiste i inne nie wiele. 

## <a name="azure-notifications-hub-mis-configuration"></a>Nieprawidłowa konfiguracja usługi Azure Centrum powiadomień
Centra powiadomień Azure musi zostać uwierzytelniony w kontekście dewelopera aplikacji, aby można było pomyślnie wysyłać powiadomienia do odpowiednich systemu powiadomień platformy. Jest to możliwe przez deweloperów, tworzenie konta dewelopera z odpowiednich platform (Google, Apple Windows itp), a następnie rejestrując ich aplikacji, w którym znaleźć poświadczenia, które należy skonfigurować w portalu w sekcji konfiguracji usługi Notification Hubs. Jeśli żadne powiadomienia są za pośrednictwem, pierwszym krokiem powinno być upewnij się, że poprawne poświadczenia są konfigurowane w Centrum powiadomień dopasowywanie ich do aplikacji utworzone na podstawie ich konta dewelopera określonych platform. Znajdziesz nasze [pobierania samouczków dotyczących] warto go przez ten proces w sposób krok po kroku. Poniżej przedstawiono niektóre typowe konfiguracje niewłaściwa:

1. **Ogólne**
   
    ) upewnij się, że nazwą Centrum powiadomień (bez błędów pisowni) jest taka sama:
   
   * Gdy w przypadku rejestracji od klienta, 
   * Gdzie są wysyłanie powiadomień z zaplecza,  
   * Jeżeli skonfigurowano poświadczeń systemu powiadomień platformy i 
   * Poświadczenia SAS, których skonfigurowano na kliencie i wewnętrznej bazy danych. 
     
     b) upewnij się, że na kliencie i wewnętrznej bazy danych aplikacji są używane prawidłowe parametry konfiguracji sygnatury dostępu Współdzielonego. Jako zasadą, należy używać **DefaultListenSharedAccessSignature** na kliencie i **DefaultFullSharedAccessSignature** wewnętrznej bazy danych aplikacji, (który daje uprawnienia, aby można było wysłać powiadomienia o NH)
2. **Konfiguracja Notification Service (APNS, Apple Push)**
   
    Trzeba zachować dwóch różnych koncentratory — jedną w środowisku produkcyjnym i drugiego do testowania celu. Oznacza to, przekazywanie certyfikatu, którego zamierzasz używać w środowisku piaskownicy do osobnego koncentratora i certyfikatów, które mają być używane w środowisku produkcyjnym do osobnego koncentratora. Nie należy próbować przekazać różne typy certyfikatów do tego samego Centrum, ponieważ może to spowodować błędy powiadomień wiersz w dół. Jeśli okaże się, w miejscu, gdzie przypadkowo zostały przekazane różnych typów certyfikatów do tego samego Centrum, zaleca się usunąć koncentratora i rozpocząć od nowa pracę. Jeśli z jakiegoś powodu nie jest możliwe usunąć Centrum, następnie co najmniej, należy usunąć wszystkie istniejące rejestracje z koncentratora. 
3. **Konfiguracja usługi Google Cloud Messaging (GCM)** 
   
    ) upewnij się, że jest włączane "Google Cloud Messaging dla systemu Android" w obszarze projektu w chmurze. 
   
    ![][2]
   
    b) upewnij się, że utworzona klucz"serwera" podczas uzyskiwania poświadczenia, które NH będzie używany do uwierzytelniania w usłudze GCM. 
   
    ![][3]
   
    c) upewnij się, że na komputerze klienckim, który jest całkowicie numeryczny jednostki, który można uzyskać z poziomu pulpitu nawigacyjnego mają skonfigurowane "Identyfikator projektu":
   
    ![][1]

## <a name="application-issues"></a>Problemy z aplikacji
1) **Znaczniki / tagu wyrażenia**

Jeśli używasz znaczników lub wyrażeń tagów do segmentowania odbiorców zawsze jest możliwe, że podczas wysyłania powiadomienia, Brak elementu docelowego znaleziono oparte na wyrażeniach tagi/tagów, które określisz wywołania wysyłania. Warto przejrzeć rejestracji w taki sposób, aby upewnić się, że istnieją znaczniki zgodnych podczas wysyłania powiadomień, a następnie sprawdź odbieranie powiadomień tylko od klientów z tych rejestracji. Na przykład Jeśli powiedzieć wszystkich rejestracji z NH zostały odbywa się za pomocą znacznika "Polityka" i wysyłania powiadomień z tagiem "Sport", nie będą wysyłane do każdego urządzenia. Złożonych przypadkach może obejmować wyrażeń tagów, gdzie można zarejestrować tylko z "Tag A" lub "Tag B", ale podczas wysyłania powiadomienia, przeznaczonych "Tag A & & Tag B". W poniższej sekcji etykietki zdiagnozowania własnym istnieją sposoby, w którym można przejrzeć rejestracji wraz z tagami, które mają. 

2) **Problemy dotyczące szablonu**

Jeśli są używane szablony, upewnij się, że znajdują się wytyczne w sposób opisany w [wskazówki szablonu]. 

3) **Nieprawidłowy rejestracji**

Zakładając, że Centrum powiadomień został skonfigurowany prawidłowo i żadnych wyrażeń tagów/tag użyto poprawnie powodujące Znajdź prawidłowe elementy docelowe, do których należy wysłać powiadomienia, NH generowane poza partie przetwarzanie równoległe - każdej partii wysyłanie komunikatów do zestawu rejestracji. 

> [!NOTE]
> Ponieważ jak przetwarzanie równoległe, nie gwarantujemy, kolejność, w którym będzie można dostarczyć powiadomień. 
> 
> 

Centrum powiadomień Azure jest zoptymalizowane pod kątem model dostarczania komunikatu "raz w większości". Oznacza to, możemy podejmować do duplikacji, dzięki czemu powiadomienia nie są dostępne w ramach więcej niż raz na urządzeniu. Aby to zapewnić możemy Przejrzyj rejestracje i upewnij się, że ten tylko jeden komunikat jest wysyłany na identyfikator urządzenia przed faktycznego wysyłania komunikatu do systemu powiadomień platformy. W każdej partii są wysyłane do systemu powiadomień platformy, które z kolei akceptuje i sprawdzanie poprawności rejestracje, jest to możliwe, że systemu powiadomień platformy wykrywa błąd co najmniej jednym rejestracji w partii, zwraca błąd Azure NH i zatrzymuje przetwarzanie co całkowicie porzucenie tej partii. Jest to szczególnie ważne przy użyciu usługi APNS, który używa protokołu strumienia TCP. Mimo że firma Microsoft są optymalizowane w większości po dostarczania, w tym przypadku firma Microsoft usunąć rejestrację źle działający z naszej bazie danych, a następnie spróbuj ponownie dostarczania powiadomień o zapytaniach w pozostałej części urządzeń w danej partii.

Można uzyskać informacji o błędzie dla danej próby dostarczenia nie powiodło się dla rejestracji przy użyciu interfejsów API REST Azure Notification Hubs: [na Telemetrii wiadomości: pobrać Telemetrii komunikat powiadomienia](https://msdn.microsoft.com/library/azure/mt608135.aspx) i [opinii systemu powiadomień platformy](https://msdn.microsoft.com/library/azure/mt705560.aspx). Zobacz [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) na przykład kodu.

## <a name="pns-issues"></a>Problemy z systemem powiadomień platformy
Po otrzymaniu komunikatu powiadomienia przez odpowiedniego systemu powiadomień platformy jest zobowiązana do dostarczenia powiadomienia do urządzenia. Centra powiadomień Azure jest poza obrazów i nie ma kontroli przypadku lub jeśli powiadomienie ma być dostarczane na urządzeniu. Ponieważ usług powiadomień platformy są bardzo rozbudowane, powiadomienia zwykle osiągnąć urządzenia w ciągu kilku sekund od z systemem powiadomień platformy. Jeżeli system powiadomień platformy jednak jest ograniczanie usługi Azure Notification Hubs zastosowanie wykładniczego wycofywania strategii i jeśli systemu powiadomień platformy pozostaje nieosiągalny na 30 minut następnie mamy zasady w celu wygaśnie i trwale usunąć te wiadomości. 

Jeśli system powiadomień platformy prób dostarczenia powiadomienia, ale urządzenie jest w trybie offline, powiadomienie jest przechowywane przez system powiadomień platformy przez pewien czas i dostarczony do urządzenia, po ich udostępnieniu. Znajduje się tylko jedno powiadomienie ostatnie dla danej aplikacji. Jeśli wiele powiadomienia są wysyłane, gdy urządzenie jest w trybie offline, każde nowe powiadomienie powoduje, że wcześniejsze powiadomienie do usunięcia. To zachowanie zachowania najnowsze powiadomienia nazywa się łączenie powiadomienia w APNS i zwijanie w GCM (który używa klucza się). Jeśli urządzenie pozostaje w trybie offline przez dłuższy czas, są przechowywane dla tego powiadomienia są odrzucane. Source — [wskazówki APNS] & [wskazówki dotyczące usługi GCM]

Przy użyciu usługi Azure Notification Hubs — można przekazać klucza łączącego za pośrednictwem nagłówka HTTP przy użyciu ogólnych `SendNotification` interfejsu API (np. dla zestawu .NET SDK — `SendNotificationAsync`) używającą nagłówków HTTP, które są przekazywane jako ma odpowiednie systemu powiadomień platformy. 

## <a name="self-diagnose-tips"></a>Etykietki zdiagnozowania własnym
W tym miejscu zostaną omówione różnych sposobów diagnozowanie i główny spowodować problemy Centrum powiadomień:

### <a name="verify-credentials"></a>Weryfikowanie poświadczeń
1. **Portalu dla deweloperów systemu powiadomień platformy**
   
    Sprawdź je na odpowiednich systemu powiadomień platformy developer portal (APN, GCM, itp. WNS) przy użyciu naszych [pobierania samouczków dotyczących].
2. **Klasyczny portal Azure**
   
    Przejdź do karty Konfiguruj, aby przejrzeć i odpowiadać poświadczeniom z identyfikatorami uzyskanymi z portalu dla deweloperów systemu powiadomień platformy. 
   
    ![][4]

### <a name="verify-registrations"></a>Weryfikowanie rejestracji
1. **Program Visual Studio**
   
    Jeśli używasz programu Visual Studio do tworzenia aplikacji można połączyć Microsoft Azure i widoku i zarządzać grupy usług Azure, w tym Centrum powiadomień "Eksploratora serwera". Jest to szczególnie przydatne w przypadku środowiska i testowania. 
   
    ![][9]
   
    Można wyświetlić i zarządzać wszystkich rejestracji w Centrum, które dobrze są podzielone dla platformy native lub szablon rejestracji, wszystkie tagi, systemu powiadomień platformy identyfikator, identyfikator rejestracji i datę wygaśnięcia. Można również edytować rejestracji na bieżąco — co przydaje się, że, jeśli chcesz edytować dowolne tagi. 
   
    ![][8]
   
   > [!NOTE]
   > Visual Studio umożliwiające edytowanie rejestracji można używać tylko podczas tworzenia/testowania, z ograniczoną liczbę rejestracji. W razie trzeba naprawić Twojej rejestracji w trybie zbiorczym, należy rozważyć użycie eksportu/importu funkcji rejestracji opisanych tutaj — [rejestracje eksportu/importu](https://msdn.microsoft.com/library/dn790624.aspx)
   > 
   > 
2. **Eksplorator usługi Service Bus**
   
    Wielu klientów używa magistrali usług explorer opisane tutaj — [Explorer magistrali usług] umożliwiające wyświetlanie i zarządzanie ich Centrum powiadomień. Jest dostępna z code.microsoft.com - projekt typu open source [kodu Explorer magistrali usług]

### <a name="verify-message-notifications"></a>Sprawdź komunikat powiadomienia
1. **Klasyczny portal Azure**
   
    Można przejść do karty "Debugowanie" do wysyłania powiadomień testu do klientów bez konieczności dowolnego zaplecza usługi zapasowej i uruchamiania. 
   
    ![][7]
2. **Program Visual Studio**
   
    Można również wysłać powiadomienia testowe z comforts programu Visual Studio:
   
    ![][10]
   
    Więcej na temat tutaj — funkcji explorer Azure Centrum powiadomień programu Visual Studio 
   
   * [Omówienie Eksploratora serwera VS]
   * [Post na blogu Eksploratora serwera VS - 1]
   * [Post na blogu Eksploratora serwera VS - 2]

### <a name="debug-failed-notifications-review-notification-outcome"></a>Debugowanie nie powiodło się powiadomienia / Przejrzyj wyniku powiadomienia
**Właściwość EnableTestSend**

W przypadku wysyłania powiadomień przy użyciu usługi Notification Hubs początkowo go tylko pobiera w kolejce dla NH do przetwarzania, aby dowiedzieć się, wszystkie jego elementy docelowe, a następnie ostatecznie NH wysyła go do systemu powiadomień platformy. Oznacza to, że używając interfejsu API REST lub dowolny z zestawu SDK klienta, zwracany pomyślnego połączenia wysyłania tylko oznacza, że komunikat ma zostały pomyślnie umieszczone w kolejce z Centrum powiadomień. Go nie zapewnia wgląd co się stało, gdy NH ostatecznie otrzymano do wysłania tej wiadomości do systemu powiadomień platformy. Jeśli urządzenie klienckie nie jest otrzymywanych powiadomienia, istnieje możliwość, gdy NH próbował dostarczenie wiadomości do systemu powiadomień platformy, wystąpił błąd, np. rozmiar ładunku przekracza maksymalną dozwoloną przez system powiadomień platformy lub skonfigurowany w NH poświadczenia są nieprawidłowe itp. Aby uzyskać wgląd w błędy systemu powiadomień platformy, wprowadzono mają właściwość o nazwie [EnableTestSend funkcji]. Ta właściwość jest automatycznie włączone podczas wysyłania wiadomości testowych z portalu lub klienta programu Visual Studio i w związku z tym umożliwia wyświetlenie szczegółowych informacji debugowania. Użyć tej funkcji za pośrednictwem interfejsów API w powyższym przykładzie, gdy są one dostępne teraz zestawu SDK .NET i zostanie dodany do wszystkich zestawów SDK klienta po pewnym czasie. Aby użyć tego wywołania REST, po prostu Dołącz parametr querystring o nazwie "test" na końcu wywołania wysyłania np. 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Przykład (.NET SDK)*

Załóżmy, że używasz zestawu .NET SDK do wysyłania natywnych wyskakujących powiadomień:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);

`result.State`będzie po prostu stanu `Enqueued` po zakończeniu wykonywania bez analizę co się stało z wypychania. Teraz można używać `EnableTestSend` właściwości typu boolean podczas inicjowania `NotificationHubClient` i uzyskać szczegółowe informacje o błędach systemu powiadomień platformy podczas wysyłania powiadomienia. W tym miejscu wywołania wysyłania zajmuje dodatkowy czas zwrócić, ponieważ jest to zwracanie tylko po NH został dostarczony powiadomienia do systemu powiadomień platformy, aby określić wyniki. 

    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Przykładowe dane wyjściowe*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong

Ten komunikat oznacza albo nieprawidłowe poświadczenia są konfigurowane w Centrum powiadomień lub problem z rejestracji koncentratora i zalecanym kierunkiem będzie usunąć tej rejestracji i umożliwić klientowi, utwórz go ponownie przed wysłaniem wiadomości. 

> [!NOTE]
> Warto zauważyć, że użycie tej właściwości jest silnie ograniczany tak tylko w należy użyć tego środowiska i testowania ograniczonym zestawie rejestracji. Wyślemy powiadomienia debugowania tylko do 10 urządzeń. Istnieje również limit przetwarzania debugowania wysyła się 10 na minutę. 
> 
> 

### <a name="review-telemetry"></a>Przejrzyj telemetrii
1. **Użyj klasycznego portalu Azure**
   
    Portalu można uzyskać szybki przegląd wszystkich działań w Centrum powiadomień. 
   
    ) na karcie "pulpitu nawigacyjnego" można wyświetlić zagregowany widok rejestracji, powiadomienia, jak również błędy dla każdej platformy. 
   
    ![][5]
   
    (b) można również dodać wiele innych platform określonych metryk na karcie "Monitora", aby przyjrzeć głębiej szczególnie systemu powiadomień platformy konkretne błędy zwracane, gdy NH próbuje wysłać powiadomienie do systemu powiadomień platformy. 
   
    ![][6]
   
    c) należy rozpocząć od sprawdzenia **wiadomości przychodzących**, **operacji rejestracji**, **powiadomienia o pomyślnym** , a następnie przejdź do na znak tabulacji platformy, aby przejrzeć błędy określonego systemu powiadomień platformy. 
   
    d) Jeśli Centrum powiadomień, niepoprawnie skonfigurowany z ustawieniami uwierzytelniania, a następnie zostanie wyświetlony błąd uwierzytelniania systemu powiadomień platformy. To jest wskazuje, aby sprawdzić poświadczenia systemu powiadomień platformy. 

2) **Programowy dostęp**

W tym miejscu — więcej informacji 

* [Dostęp programistyczny Telemetrii]
* [Dane telemetryczne dostęp za pośrednictwem interfejsów API próbki] 

> [!NOTE]
> Kilka związane z funkcje, takie jak **rejestracje eksportu/importu**, **Telemetrii dostęp za pośrednictwem interfejsów API** itp są dostępne tylko w warstwie standardowa. Jeśli próba użycia tych funkcji, jeśli jest bezpłatna lub warstwa podstawowa następnie otrzymasz komunikat o wyjątku w tym celu podczas przy użyciu zestawu SDK i HTTP 403 (Dostęp zabroniony), korzystając z nimi bezpośrednio z interfejsów API REST. Upewnij się, że zostały przeniesione do standardowego warstwy przy użyciu klasycznego portalu Azure.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[omówienie centra powiadomień]: notification-hubs-push-notification-overview.md (Notification Hubs — omówienie)
[pobierania samouczków dotyczących]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[wskazówki szablonu]: https://msdn.microsoft.com/library/dn530748.aspx 
[wskazówki APNS]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[wskazówki dotyczące usługi GCM]: http://developer.android.com/google/gcm/adv.html
[Export/Import Registrations]: http://msdn.microsoft.com/library/dn790624.aspx
[Explorer magistrali usług]: http://msdn.microsoft.com/library/dn530751.aspx
[kodu Explorer magistrali usług]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Omówienie Eksploratora serwera VS]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Post na blogu Eksploratora serwera VS - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Post na blogu Eksploratora serwera VS - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend funkcji]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Dostęp programistyczny Telemetrii]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Dane telemetryczne dostęp za pośrednictwem interfejsów API próbki]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

