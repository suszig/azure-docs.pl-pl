---
title: "Monitorowania i diagnostyki dla usług platformy ASP.NET Core w sieci szkieletowej usług Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania monitorowania i diagnostyki dla aplikacji usługi Azure Service Fabric platformy ASP.NET Core."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/14/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: ce854a3dc41dec69c3f8de245a03d55a2354335f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="monitor-and-diagnose-an-aspnet-core-application-on-service-fabric"></a>Monitorowanie i diagnozowanie aplikacji platformy ASP.NET Core w sieci szkieletowej usług
W tym samouczku jest częścią czterech serii. Przechodzi ona przez proces konfigurowania monitorowania i diagnostyki dla aplikacji platformy ASP.NET Core w klastrze usługi sieć szkieletowa usług za pomocą usługi Application Insights. Firma Microsoft będzie zbierać dane telemetryczne z aplikacji, w pierwszej części samouczka [tworzenia aplikacji sieci szkieletowej usług .NET](service-fabric-tutorial-create-dotnet-app.md). 

W czterech części serii samouczka, dowiesz się, jak:
> [!div class="checklist"]
> * Konfiguruj usługę Application Insights dla aplikacji
> * Zbierać dane telemetryczne odpowiedzi do śledzenia oparte na protokole HTTP do komunikacji między usługami
> * Funkcja mapy aplikacji w usłudze Application Insights
> * Dodawanie zdarzeń niestandardowych za pomocą interfejsu API programu Application Insights

W tym samouczku dowiesz się, jak:
> [!div class="checklist"]
> * [Tworzenie aplikacji sieci szkieletowej usług .NET](service-fabric-tutorial-create-dotnet-app.md)
> * [Wdrażanie aplikacji do zdalnego klastra](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurowanie elementu konfiguracji/CD za pomocą programu Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * Konfigurowanie monitorowania i diagnostyki dla aplikacji

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Zainstaluj program Visual Studio 2017](https://www.visualstudio.com/) i zainstaluj **Azure programowanie** i **ASP.NET i sieć web development** obciążeń.
- [Zainstaluj zestaw SDK sieci szkieletowej usług](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Pobierz aplikację przykładową głosowania
Jeśli nie zbudować głosowania przykładowej aplikacji [część jednego z tego samouczka serii](service-fabric-tutorial-create-dotnet-app.md), można go pobrać. W oknie polecenia lub terminalu uruchom następujące polecenie sklonować repozytorium przykładowej aplikacji na komputerze lokalnym.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-an-application-insights-resource"></a>Ustawianie zasobu usługi Application Insights
Usługi Application Insights to platforma zarządzania wydajności aplikacji platformy Azure i usługi sieć szkieletowa zalecane platformy aplikacji monitorowania i diagnostyki. Aby utworzyć zasobu usługi Application Insights, przejdź do [portalu Azure](https://portal.azure.com). Kliknij przycisk **nowy** w menu nawigacji po lewej stronie Otwieranie portalu Azure Marketplace. Polecenie **monitorowania i zarządzania** , a następnie **usługi Application Insights**.

![Utwórz nowy zasób AI](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource.png)

Teraz musisz wypełnić wymaganych informacji dotyczących atrybutów zasobu do utworzenia. Wprowadź odpowiednie *nazwa*, *grupy zasobów*, i *subskrypcji*. Ustaw *lokalizacji* do której będzie wdrażania klastra usługi sieć szkieletowa w przyszłości. W tym samouczku wdrożymy aplikację z lokalnym klastrem więc *lokalizacji* pola nie ma znaczenia. *Typu aplikacji* powinny pozostać "Aplikacja sieci web ASP.NET." 

![Atrybuty zasobu AI](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource-attrib.png)

Po wypełnionego wymaganych informacji kliknij **Utwórz** do udostępniania zasobów — powinien zająć około minuty. 
<!-- When completed, navigate to the newly deployed resource, and find the "Instrumentation Key" (visible in the "Essentials" drop down section). Copy it to clipboard, since we will need it in the next step. -->

## <a name="add-application-insights-to-the-applications-services"></a>Dodawanie usługi Application Insights do aplikacji usług

Uruchom program Visual Studio 2017 z podwyższonym poziomem uprawnień. Aby to zrobić, klikając prawym przyciskiem myszy ikonę programu Visual Studio w Start Menu i wybierając polecenie **Uruchom jako administrator**. Kliknij przycisk **pliku** > **Otwórz** > **projektu/rozwiązania** i przejdź do aplikacji głosowania (albo utworzony w części samouczka lub git sklonowany). Otwórz *Voting.sln* i jeśli zostanie wyświetlony monit pod kątem przywracania pakietów NuGet aplikacji, kliknij przycisk **tak**.

Wykonaj następujące kroki, aby skonfigurować usługi Application Insights dla usługi zarówno VotingWeb i VotingData:
1. Kliknij prawym przyciskiem myszy nazwę usługi, a następnie kliknij przycisk **Konfiguruj usługę Application Insights...** .

    ![Skonfiguruj AI](./media/service-fabric-tutorial-monitoring-aspnet/configure-ai.png)

2. Kliknij przycisk **Start wolnego**.
3. Zaloguj się do konta (z którego można również skonfigurować Twojej subskrypcji platformy Azure), a następnie wybierz subskrypcję, w którym został utworzony zasób usługi Application Insights. Znajdowanie zasobów w obszarze *zasobu istniejącej usługi Application Insights* na liście rozwijanej "Zasobu". Kliknij przycisk **zarejestrować** można dodać usługi Application Insights z usługą.

    ![Zarejestruj AI](./media/service-fabric-tutorial-monitoring-aspnet/register-ai.png)

4. Kliknij przycisk **Zakończ** po okno dialogowe, które pojawia się ukończy akcji.
    
Upewnij się, że wykonaj powyższe kroki **zarówno** usług w aplikacji, aby ukończyć konfigurowanie usługi Application Insights dla aplikacji. Tego samego zasobu usługi Application Insights jest używany dla usług, aby zobaczyć przychodzących i wychodzących żądań i komunikacji między usługami.

## <a name="add-the-microsoftapplicationinsightsservicefabricnative-nuget-to-the-services"></a>Dodaj Microsoft.ApplicationInsights.ServiceFabric.Native NuGet do usług

Usługa Application Insights ma dwa NuGets określonej sieci szkieletowej usług, które mogą być używane w zależności od scenariusza. Używany jest jeden z usługi sieć szkieletowa usług natywnego, a druga z kontenerów i plików wykonywalnych gościa. W takim przypadku będziemy używać Microsoft.ApplicationInsights.ServiceFabric.Native NuGet wykorzystać zrozumienie kontekstu usługi, która stwarza. Aby uzyskać więcej informacji na temat zestawu SDK usługi Application Insights i NuGets określonej sieci szkieletowej usług, zobacz [Microsoft Application Insights dla usługi Service Fabric](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/develop/README.md). 

Poniżej przedstawiono kroki, aby skonfigurować NuGet:
1. Kliknij prawym przyciskiem myszy **rozwiązania "Głosowania"** u góry w Eksploratorze rozwiązań i kliknij przycisk **Zarządzaj pakietami NuGet rozwiązania...** .
2. Kliknij przycisk **Przeglądaj** w menu górnym menu nawigacyjnym okna "NuGet — rozwiązanie" i zaznacz pole wyboru **Uwzględnij wersję wstępną** pole wyboru obok pozycji na pasku wyszukiwania.
3. Wyszukaj `Microsoft.ApplicationInsights.ServiceFabric.Native` i kliknij odpowiedniego pakietu NuGet.
4. Po prawej stronie, kliknij dwa pola wyboru obok dwie usługi w aplikacji **VotingWeb** i **VotingData** i kliknij przycisk **zainstalować**.
    ![Zakończono rejestrację AI](./media/service-fabric-tutorial-monitoring-aspnet/aisdk-sf-nuget.png)
5. Kliknij przycisk **OK** na *Przejrzyj zmiany* okno dialogowe, którego pojawia się i zaakceptuj *akceptacji licencji*. Dodawanie do usług NuGet zostanie ukończona.
6. Teraz należy skonfigurować inicjator telemetrii w dwóch usług. Od tej reguły, otwarcie *VotingWeb.cs* i *VotingData.cs*. Dla obu z nich wykonaj następujące dwa kroki:
    1. Dodaj te dwie *przy użyciu* instrukcji u góry każdego  *\<ServiceName > .cs*:
    
    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.ServiceFabric;
    ```
    
    2. W zagnieżdżonych *zwracać* instrukcja *CreateServiceInstanceListeners()* lub *CreateServiceReplicaListeners()*w obszarze *ConfigureServices*  >  *usług*, między dwie usługi Singleton został zadeklarowany, Dodaj: `.AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))`.
    Spowoduje to dodanie *kontekstu usługi* do telemetrii, co umożliwia lepiej zrozumieć źródło telemetrii w usłudze Application Insights. Twoje zagnieżdżonych *zwracać* instrukcji w *VotingWeb.cs* powinien wyglądać następująco:
    
    ```csharp
    return new WebHostBuilder().UseWebListener()
        .ConfigureServices(
            services => services
                .AddSingleton<StatelessServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<HttpClient>())
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
        .UseUrls(url)
        .Build();
    ```

    Podobnie, w *VotingData.cs*, powinien mieć:

    ```csharp
    return new WebHostBuilder()
        .UseKestrel()
        .ConfigureServices(
            services => services
                .AddSingleton<StatefulServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<IReliableStateManager>(this.StateManager))
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
        .UseUrls(url)
        .Build();
    ```

W tym momencie można przystąpić do wdrażania aplikacji. Kliknij przycisk **Start** u góry (lub **F5**), i Visual Studio będzie kompilacji i pakiet aplikacji, konfigurowanie lokalnego klastra i wdrażanie aplikacji do niego. 

Po zakończeniu aplikacji wdrażanie, head za pośrednictwem do [localhost:8080](localhost:8080), gdzie powinien być wyświetlony aplikacji jednej strony próbki głosu. Głosowania dla kilku różnych elementów wybranych przez użytkownika do tworzenia niektóre przykładowe dane i dane telemetryczne — wykonano kroki dla desery!

![Głosy próbki AI](./media/service-fabric-tutorial-monitoring-aspnet/vote-sample.png)

Możesz także *Usuń* niektóre opcje głosowania, jak również w przypadku, gdy wszystko będzie gotowe Dodawanie kilka głosów.

## <a name="view-telemetry-and-the-app-map-in-application-insights"></a>Dane telemetryczne wyświetleń, a następnie mapować aplikacji w usłudze Application Insights 

HEAD za pośrednictwem do zasobu usługi Application Insights w portalu Azure, a na pasku nawigacyjnym po lewej stronie zasobu, wybierz polecenie **podglądy** w obszarze *Konfiguruj*. Włącz **na** *Mapa aplikacji usługi roli* na liście dostępnych podglądów.

![Włącz AI AppMap](./media/service-fabric-tutorial-monitoring-aspnet/ai-appmap-enable.png)

Kliknij przycisk **omówienie** aby powrócić do strony docelowej zasobu. Następnie kliknij przycisk **wyszukiwania** u góry, aby wyświetlić dane śledzenia przychodzących. Trwa kilka minut, aż śladów pojawią się w usłudze Application Insights. W przypadku, że nie widzą żadnych, poczekaj kilka minut i kliknij przycisk **Odśwież** na górze.
![Ślady Zobacz AI](./media/service-fabric-tutorial-monitoring-aspnet/ai-search.png)

Przewijania w dół na *wyszukiwania* okna wyświetli przychodzące telemetrii, możesz uzyskać poza pole z usługą Application Insights. Dla każdej akcji, która została wykonana w aplikacji głosowania powinny być żądania PUT wychodzącego z *VotingWeb* (PUT głosy/Put [nazwa]), przychodzącego żądania PUT z *VotingData* (PUT VoteData/Put [nazwa ]), a następnie parę żądania GET odświeżania danych będzie wyświetlany. Będzie również śledzenia zależności dla protokołu HTTP na hoście lokalnym, ponieważ są one żądania HTTP. Oto przykład zobaczysz dla jak jeden głos został dodany: ![ślad żądania próbki AI](./media/service-fabric-tutorial-monitoring-aspnet/sample-request.png)

Możesz kliknąć na jednym śledzenia, aby wyświetlić więcej szczegółów o nim. Jest przydatne informacje na temat żądania, które są udostępniane przez usługi Application Insights, w tym *czas odpowiedzi* i *adresu URL żądania*. Ponadto od dodania określonych NuGet usługi Service Fabric, również otrzymasz dane dotyczące Twojej aplikacji w kontekście klastra sieci szkieletowej usług w *danych niestandardowych* poniższej sekcji. W tym kontekście usługi pozwala zobaczyć *PartitionID* i *ReplicaId* źródła żądania oraz lepiej localize problemy podczas diagnozowania błędów w aplikacji.

![Szczegółowe informacje śledzenia AI](./media/service-fabric-tutorial-monitoring-aspnet/trace-details.png)

Ponadto, ponieważ firma Microsoft włączone mapy aplikacji *omówienie* strony, klikając **mapy aplikacji** ikony będą określać zarówno usługi połączone.

![Szczegółowe informacje śledzenia AI](./media/service-fabric-tutorial-monitoring-aspnet/app-map.png)

Mapa aplikacji mogą ułatwić zrozumienie topologii aplikacji lepsze, szczególnie, jak możesz rozpocząć dodawanie wielu różnych usług, które współpracują ze sobą. On również zapewnia podstawowe dane na odsetka pomyślnych żądań i mogą pomóc w diagnozowaniu nieudanych żądań, aby zrozumieć, w którym rzeczy może mieć niepowodzenia. Aby dowiedzieć się więcej o korzystaniu z mapy aplikacji, zobacz [mapowanie aplikacji w usłudze Application Insights](../application-insights/app-insights-app-map.md).

## <a name="add-custom-instrumentation-to-your-application"></a>Dodaj niestandardowy Instrumentacji do aplikacji

Chociaż usługa Application Insights udostępnia wiele telemetrii fabrycznej, można dodać więcej niestandardowych instrumentacji. Mogą to być oparte na potrzeby biznesowe lub zwiększenie diagnostykę w przypadku wystąpienia problemów w aplikacji. Interfejs API w celu pozyskiwania niestandardowe zdarzenia i metryki, który można uzyskać więcej informacji zawiera usługi Application Insights [tutaj](../application-insights/app-insights-api-custom-events-metrics.md).

Dodajmy niektórych zdarzeń niestandardowych w celu *VoteDataController.cs* (w obszarze *VotingData* > *kontrolerów*) do śledzenia głosy są dodawane i usuwane z bazowy *votesDictionary*. 
1. Dodaj `using Microsoft.ApplicationInsights;` na końcu innych za pomocą instrukcji.
2. Zadeklaruj nową *TelemetryClient* na początku tej klasy, w obszarze tworzenia *IReliableStateManager*: `private TelemetryClient telemetry = new TelemetryClient();`.
3. W *Put()* funkcji, należy dodać zdarzenie potwierdza głos został dodany. Dodaj `telemetry.TrackEvent($"Added a vote for {name}");` po zakończeniu transakcji, kliknij prawym przyciskiem myszy przed zwracany *OkResult* instrukcji.
4. W *Delete()*, Brak "if/else" na podstawie pod warunkiem że *votesDictionary* zawiera głosów dla danej opcji głosu. 
    1. Dodawanie zdarzenia potwierdzający usunięcie głosowanie w *Jeśli* instrukcji, po *await tx.CommitAsync()*:`telemetry.TrackEvent($"Deleted votes for {name}");`
    2. Dodawanie zdarzeń, aby pokazać, że usunięcia nie została wykonana *else* instrukcji przed instrukcji return:`telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");`

Oto przykład co Twoje *Put()* i *Delete()* funkcje może wyglądać po dodaniu zdarzenia:

```csharp
// PUT api/VoteData/name
[HttpPut("{name}")]
public async Task<IActionResult> Put(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
        await tx.CommitAsync();
    }

    telemetry.TrackEvent($"Added a vote for {name}");
    return new OkResult();
}

// DELETE api/VoteData/name
[HttpDelete("{name}")]
public async Task<IActionResult> Delete(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        if (await votesDictionary.ContainsKeyAsync(tx, name))
        {
            await votesDictionary.TryRemoveAsync(tx, name);
            await tx.CommitAsync();
            telemetry.TrackEvent($"Deleted votes for {name}");
            return new OkResult();
        }
        else
        {
            telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");
            return new NotFoundResult();
        }
    }
}
```

Po zakończeniu wprowadzania tych zmian **Start** aplikacji, tak aby tworzy i wdraża najnowszej wersji. Po zakończeniu aplikacji wdrażanie, head za pośrednictwem do [localhost:8080](localhost:8080), dodać i usunąć niektóre opcje głosowania. Następnie wróć do zasobu usługi Application Insights do Zobacz dane śledzenia dla przebiegu najnowszej (jak przedtem śladów może potrwać 1 2 min wyświetlani w usłudze Application Insights). Dla wszystkich głosy dodawane i usuwane powinien zostać wyświetlony "niestandardowe zdarzenie * oraz wszystkie dane telemetryczne odpowiedzi. 

![niestandardowe zdarzenia](./media/service-fabric-tutorial-monitoring-aspnet/custom-events.png)

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Konfiguruj usługę Application Insights dla aplikacji
> * Zbierać dane telemetryczne odpowiedzi do śledzenia oparte na protokole HTTP do komunikacji między usługami
> * Funkcja mapy aplikacji w usłudze Application Insights
> * Dodawanie zdarzeń niestandardowych za pomocą interfejsu API programu Application Insights

Teraz, gdy zostaną zakończone konfigurowania, monitorowania i diagnostyki dla aplikacji ASP.NET, należy spróbować wykonać następujące czynności:
- [Eksploruj monitorowania i diagnostyki w sieci szkieletowej usług](service-fabric-diagnostics-overview.md)
- [Analiza zdarzeń usługi sieć szkieletowa z usługą Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
- Aby dowiedzieć się więcej na temat usługi Application Insights, zobacz [Application Insights dokumentacji](https://docs.microsoft.com/azure/application-insights/)
