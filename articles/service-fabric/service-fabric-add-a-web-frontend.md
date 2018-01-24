---
title: "Tworzenie frontonu sieci web dla aplikacji sieci szkieletowej usług Azure przy użyciu platformy ASP.NET Core | Dokumentacja firmy Microsoft"
description: "Udostępnianie aplikacji sieci szkieletowej usług w sieci Web przy użyciu platformy ASP.NET Core projektu i komunikacja między usługami komunikacji za pośrednictwem usług zdalnych."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: d4f78c63117e5c54eb855178c75d6c294957f2a1
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Tworzenie frontonu sieci web usługi aplikacji przy użyciu platformy ASP.NET Core
Domyślnie usługi sieć szkieletowa usług Azure nie udostępniają interfejs publiczny w sieci Web. Udostępniać aplikacji funkcje do klientów protokołu HTTP, należy utworzyć projekt sieci web do działania jako punkt wejścia, a następnie komunikować się z niego do poszczególnych usług.

W tym samouczku będziemy uruchomienia instalacji gdzie możemy przerwał pracę w [tworzenie pierwszej aplikacji w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) samouczka i Dodaj usługę sieci web platformy ASP.NET Core przed usługi stanowej licznika. Jeśli jeszcze nie możesz wrócić do poprzedniej strony i najpierw kroków opisanych w tym samouczku.

## <a name="set-up-your-environment-for-aspnet-core"></a>Konfigurowanie środowiska dla platformy ASP.NET Core

Potrzebujesz programu Visual Studio 2017 r, aby skorzystać z tego samouczka. Dowolna wersja będzie wykonywać. 

 - [Zainstaluj program Visual Studio 2017 r.](https://www.visualstudio.com/)

Do tworzenia aplikacji platformy ASP.NET Core Service Fabric, musisz mieć następujące obciążenia zainstalowane:
 - **Programowanie Azure** (w obszarze *sieci Web & chmurze*)
 - **ASP.NET i sieć web development** (w obszarze *sieci Web & chmurze*)
 - **Programowanie wieloplatformowych .NET core** (w obszarze *inne procesami*)

>[!Note] 
>Narzędzia .NET Core dla programu Visual Studio 2015 są już aktualizowany, ale jeśli nadal używasz programu Visual Studio 2015, musisz mieć [.NET Core VS 2015 narzędzi Preview 2](https://www.microsoft.com/net/download/core) zainstalowane.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Dodaj usługi platformy ASP.NET Core do aplikacji
Platformy ASP.NET Core to platforma programistyczna lekka międzyplatformowa sieci web, który służy do tworzenia nowoczesnych witryn sieci web interfejsu użytkownika i interfejsów API sieci web. 

Dodajmy projektu interfejsu API sieci Web platformy ASP.NET z naszych istniejącą aplikacją.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **usług** aplikacji projekt i wybierz pozycję **Dodaj > Nowa usługa sieci szkieletowej usług**.
   
    ![Dodawanie nowej usługi do istniejącej aplikacji][vs-add-new-service]
2. Na **Tworzenie usługi** wybierz pozycję **platformy ASP.NET Core** i nadaj mu nazwę.
   
    ![Wybieranie usługi sieci web ASP.NET w oknie dialogowym Nowy usługi][vs-new-service-dialog]

3. Następna strona zawiera zestaw platformy ASP.NET Core szablonów projektu. Należy pamiętać, że są to te same możliwości, że można zobaczyć został utworzony projekt platformy ASP.NET Core poza aplikacją sieci szkieletowej usług z małej ilości dodatkowych kod, aby zarejestrować usługi o środowisku uruchomieniowym usługi sieć szkieletowa usług. W tym samouczku, wybierz **interfejsu API sieci Web**. Jednak można użyć tego samego pojęcia do tworzenia aplikacji sieci web pełna.
   
    ![Wybieranie typu projektu programu ASP.NET][vs-new-aspnet-project-dialog]
   
    Po utworzeniu projektu interfejsu API sieci Web powinien mieć dwie usługi aplikacji. Jak można kontynuować tworzenie aplikacji, można dodać więcej usług w taki sam sposób. Każdy może być niezależnie określonej wersji, jak i uaktualnionych.

## <a name="run-the-application"></a>Uruchamianie aplikacji
Aby uzyskać zorientować się, co możemy wykonane, umożliwia wdrażanie nowej aplikacji i spójrz na szablon interfejsu API platformy ASP.NET Core sieci Web udostępnia zachowanie domyślne.

1. Naciśnij klawisz F5 w programie Visual Studio do debugowania aplikacji.
2. Po zakończeniu wdrożenia programu Visual Studio spowoduje uruchomienie przeglądarki do katalogu głównego usługi interfejsu API sieci Web platformy ASP.NET. Szablon interfejsu API platformy ASP.NET Core sieci Web nie udostępniają domyślne zachowanie dla katalogu głównego, więc powinien zostać wyświetlony błąd 404 w przeglądarce.
3. Dodaj `/api/values` do lokalizacji w przeglądarce. Wywołuje to `Get` metoda ValuesController w szablonie interfejsu API sieci Web. Zwraca domyślny zapewnianej przez szablon — tablicy JSON, który zawiera dwa ciągi:
   
    ![Wartości domyślne zwrócony z interfejsu API platformy ASP.NET Core sieci Web szablonu][browser-aspnet-template-values]
   
    Na koniec samouczka tej strony wyświetli ostatniej wartość licznika z naszej usługi stanowej zamiast domyślne parametry.

## <a name="connect-the-services"></a>Połączenie usługi
Sieć szkieletowa usług oferuje pełną elastyczność w sposób komunikowania się z usługami reliable services. Wewnątrz aplikacji konieczne może być usług, które są dostępne za pośrednictwem protokołu TCP, innych usług, które są dostępne za pośrednictwem interfejsu API REST protokołu HTTP i nadal innych usług, które są dostępne za pośrednictwem gniazda sieci web. W tle na dostępne opcje oraz zaangażowanych skutków ubocznych, zobacz [komunikacji z usługami](service-fabric-connect-and-communicate-with-services.md). W tym samouczku używamy [Service Fabric Service Remoting](service-fabric-reliable-services-communication-remoting.md), podany w zestawie SDK.

W ujęciu Service Remoting (zaprojektowany na zdalnych wywołań procedur lub RPC) można zdefiniować interfejs do działania jako publicznego kontraktu usługi. Następnie należy użyć interfejsu do generowania klasy serwera proxy do interakcji z usługą.

### <a name="create-the-remoting-interface"></a>Tworzenie interfejsu usługi zdalne
Zacznijmy od utworzenia interfejsu, który ma działać jako kontraktu między usługi stanowej i innych usług, w tym przypadku platformy ASP.NET Core sieci web projektu. Ten interfejs musi być współużytkowane przez wszystkie usługi, które umożliwiają wywołań RPC, dlatego utworzymy własnego projektu biblioteki klas.

1. W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy i wybierz polecenie **Dodaj** > **nowy projekt**.

2. Wybierz **Visual C#** wpis w okienku nawigacji po lewej stronie, a następnie wybierz **biblioteki klas** szablonu. Upewnij się, że wersja programu .NET Framework jest ustawiona na **4.5.2**.
   
    ![Tworzenie projektu interfejsu usługi stanowej][vs-add-class-library-project]

3. Zainstaluj **Microsoft.ServiceFabric.Services.Remoting** pakietu NuGet. Wyszukaj **Microsoft.ServiceFabric.Services.Remoting** w polu pakiet manager, a następnie zainstaluj go dla wszystkich projektów w rozwiązaniu korzystających Service Remoting, w tym:
   - Projekt biblioteki klas, który zawiera interfejs usługi
   - Projekt usługi Stateful
   - Projekt usługi sieci web platformy ASP.NET Core
   
    ![Dodawanie pakietu NuGet usługi][vs-services-nuget-package]

4. W bibliotece klas programu utworzenie interfejsu z jedną metodę `GetCountAsync`, i rozszerzenie interfejsu z `Microsoft.ServiceFabric.Services.Remoting.IService`. Interfejsu usług zdalnych musi pochodzić od tego interfejsu, aby wskazać, że interfejs Service Remoting.
   
    ```csharp
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-the-interface-in-your-stateful-service"></a>Zaimplementuj interfejs w usłudze stanowe
Teraz, gdy zdefiniowaniu interfejsu, należy ją wdrożyć w usłudze stanowych.

1. W usłudze stanowe Dodaj odwołanie do projektu biblioteki klas, który zawiera interfejsu.
   
    ![Dodawanie odwołania do projektu biblioteki klas w usługi stanowej][vs-add-class-library-reference]
2. Zlokalizuj klasy, która dziedziczy `StatefulService`, takich jak `MyStatefulService`, i rozszerzyć ją do zaimplementowania `ICounter` interfejsu.
   
    ```csharp
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. Teraz wdrożyć pojedynczej metody, która jest zdefiniowana w `ICounter` interfejsu `GetCountAsync`.
   
    ```csharp
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Udostępnianie usługi stanowej przy użyciu odbiornika usługi zdalne usługi
Z `ICounter` interfejsu zaimplementowanego, ostatnim krokiem jest Otwórz kanał komunikacyjny Service Remoting. Usługi stanowej, Usługa Service Fabric realizuje z możliwością przesłonięcia metody o nazwie `CreateServiceReplicaListeners`. Przy użyciu tej metody można określić odbiorników komunikacji, na podstawie typu komunikacji, który chcesz włączyć usługi.

> [!NOTE]
> Wywołania metody równoważne otwierania kanał komunikacji usługi bezstanowej `CreateServiceInstanceListeners`.

W takim przypadku możemy zastąpić istniejącą `CreateServiceReplicaListeners` — metoda i zapewnić wystąpienie `ServiceRemotingListener`, co powoduje z punktem końcowym które jest wywoływane z klientów za pośrednictwem `ServiceProxy`.  

`CreateServiceRemotingListener` — Metoda rozszerzenia na `IService` interfejs umożliwia łatwe tworzenie `ServiceRemotingListener` przy użyciu wszystkich ustawień domyślnych. Aby użyć tej metody rozszerzenia, upewnij się, masz `Microsoft.ServiceFabric.Services.Remoting.Runtime` zaimportowane przestrzeni nazw. 

```csharp
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Klasa ServiceProxy służy do interakcji z usługą
Nasza usługa stanowa jest teraz gotowa do odbioru ruchu z innych usług za pośrednictwem wywołania RPC. Związku z czym jej pozostaje dodaje kod, aby komunikować się z nim z usługi sieci web ASP.NET.

1. W projekcie platformy ASP.NET, Dodaj odwołanie do biblioteki klas, który zawiera `ICounter` interfejsu.

2. Wcześniej dodane **Microsoft.ServiceFabric.Services.Remoting** pakiet NuGet do projektu programu ASP.NET. Ten pakiet zawiera `ServiceProxy` wywołań usługi stanowej klasy, która służy do przygotowywania usługi RPC. Upewnij się, że ten pakiet NuGet jest zainstalowany w projekcie usługi sieci web platformy ASP.NET Core.

4. W **kontrolerów** folder, otwórz `ValuesController` klasy. Należy pamiętać, że `Get` metoda obecnie tylko zwraca tablicę ustalony ciąg "wartość1" i "wartość2"--odpowiadającego widzieliśmy wcześniej w przeglądarce. Ta implementacja Zastąp następujący kod:
   
    ```csharp
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    Pierwszy wiersz kodu jest klucza. Aby utworzyć ICounter serwera proxy usługi stanowej, musisz podać informacje: identyfikator partycji i nazwę usługi.
   
    Możesz użyć przechodzenia do usługi stanowej skali przez podzielenie ich stan w różnych zasobników, oparte na kluczu zdefiniowana, takich jak identyfikator klienta lub kod pocztowy. W naszym prostej aplikacji usługi stanowej ma tylko jedną partycję, więc klucz nie ma znaczenia. Dowolny klawisz, który podasz doprowadzi do tej samej partycji. Aby dowiedzieć się więcej na temat partycjonowania usługi, zobacz [jak partycji usługi sieci szkieletowej usług niezawodnej](service-fabric-concepts-partitioning.md).
   
    Nazwa usługi jest identyfikatorem URI formularza sieci szkieletowej: /&lt;nazwa_aplikacji&gt;/&lt;service_name&gt;.
   
    Z tych dwóch rodzajów informacji usługi sieć szkieletowa może jednoznacznie zidentyfikować urządzenia, na którym powinny być wysyłane żądania. `ServiceProxy` Klasa obsługuje również bezproblemowo wtedy, gdy wystąpi awaria maszyny, który jest hostem partycji usługi stanowej i inny komputer musi zostać podniesiony do jego miejsce. Ta warstwa abstrakcji sprawia, że pisanie kodu klienta radzenia sobie z innymi usługami znacznie prostsze.
   
    Gdy mamy serwera proxy, możemy po prostu Wywołaj `GetCountAsync` metodę i zwraca jego wynik.

5. Naciśnij klawisz F5, aby ponownie uruchomić aplikację zmodyfikowane. Ponieważ przed, Visual Studio automatycznie uruchomi przeglądarkę, aby katalog główny projektu sieci web. Dodaj ścieżkę "interfejsu api/values", a powinien zostać wyświetlony zwrócił bieżącą wartość licznika.
   
    ![Wartość licznika stanowe wyświetlany w przeglądarce][browser-aspnet-counter-value]
   
    Odśwież przeglądarkę, okresowo, aby wyświetlić wartość licznika aktualizacji.

## <a name="connecting-to-a-reliable-actor-service"></a>Łączenie z usługą niezawodnego aktora
Ten samouczek koncentruje się na dodanie frontonu sieci web, które komunikowały się z usługi stanowej. Można jednak wykonać model bardzo podobne do komunikowania się złośliwych użytkowników. Podczas tworzenia projektu niezawodnego aktora Visual Studio automatycznie generuje projektu interfejsu. Ten interfejs umożliwia generowanie serwera proxy aktora w projekcie sieci web do komunikowania się z aktora. Kanał komunikacji znajduje się automatycznie. Dlatego nie trzeba wykonywać żadnych czynności jest odpowiednikiem ustanawianie `ServiceRemotingListener` jak dla usługi stanowej w tym samouczku.

## <a name="how-web-services-work-on-your-local-cluster"></a>Jak działają usługi sieci web w klastrze lokalnym
Ogólnie rzecz biorąc można wdrożyć dokładnie tej samej aplikacji sieci szkieletowej usług na klastrze z wieloma maszynami, która została wdrożona w klastrze lokalnym i można zdecydowanie pewność, że działa zgodnie z oczekiwaniami. Jest to spowodowane klaster lokalny jest po prostu konfigurację pięcioma węzłami, w której jest zwinięte na jednym komputerze.

Po przejściu do usługi sieci web, jednak istnieje jeden nuance klucza. Gdy klaster znajduje się za usługą równoważenia obciążenia, jak ma na platformie Azure, pamiętaj, że usługi sieci web są wdrażane na każdym komputerze od usługi równoważenia obciążenia po prostu okrężne ruchu na komputerach. Można to zrobić przez ustawienie `InstanceCount` usługi specjalna wartość "-1".

Z kolei po uruchomieniu usługi sieci web lokalnie, musisz upewnij się, że tylko jedno wystąpienie usługi jest uruchomiona. W przeciwnym razie wystąpiły konflikty z wielu procesów, które nasłuchują na tej samej ścieżce i portu. W związku z tym liczba wystąpień usługi sieci web powinna być równa "1" dla wdrożenia lokalnego.

Aby dowiedzieć się, jak skonfigurować różne wartości dla innego środowiska, zobacz [Zarządzanie parametry aplikacji dla wielu środowiskach](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Kolejne kroki
Teraz, gdy masz z przodu web przechodzili zestaw aplikacji z platformy ASP.NET Core, Dowiedz się więcej o [platformy ASP.NET Core w usługach niezawodnej usługi sieć szkieletowa](service-fabric-reliable-services-communication-aspnetcore.md) dla dobrej znajomości sposobu platformy ASP.NET Core współpracuje z sieci szkieletowej usług.

Następnie [Dowiedz się więcej o komunikacji z usługami](service-fabric-connect-and-communicate-with-services.md) ogólnie uzyskanie pełnego obrazu z jak usługi komunikacji działa w sieci szkieletowej usług.

Po utworzeniu dobrą znajomością działania komunikacji usług [utworzyć klaster na platformie Azure i wdrażanie aplikacji w chmurze](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
