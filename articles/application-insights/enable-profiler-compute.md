---
title: "Włącz Application Insights profilera dla rozwiązań usługi obliczenia Azure aplikacji | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania aplikacji profilera szczegółowych informacji w aplikacji uruchomionej w rozwiązań usługi obliczenia Azure."
services: application-insights
documentationcenter: 
author: ramach-msft
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach
ms.openlocfilehash: 66ea24cfe9dd03ed62c06daa76ee043886ad7bcc
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-cloud-services"></a>Włącz profilera Insights aplikacji dla maszyn wirtualnych platformy Azure, usługi Service Fabric i usług w chmurze

W tym artykule przedstawiono sposób włączania usługi Azure Application Insights profilera w aplikacji ASP.NET, która jest obsługiwana przez zasób obliczeń platformy Azure. 

W tym artykule przykładami pomocy technicznej dla maszyny wirtualnej platformy Azure, zestawy skalowania maszyny wirtualnej, sieć szkieletowa usług Azure i usługi w chmurze Azure. Przykłady polegać na szablony, które obsługują [usługi Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) modelu wdrażania.  


## <a name="overview"></a>Omówienie

Na poniższej ilustracji przedstawiono, jak działa profilera usługi Application Insights z zasobów platformy Azure. Jako przykład obrazu korzysta z maszyny wirtualnej platformy Azure.

  ![Omówienie](./media/enable-profiler-compute/overview.png)

Aby w pełni włączyć profilera, należy zmienić konfigurację w trzech miejscach:

* W okienku wystąpienia usługi Application Insights w portalu Azure.
* Kod źródłowy aplikacji (na przykład aplikacja sieci web ASP.NET).
* Środowisko wdrażania definicji kodu źródłowego (na przykład maszyna wirtualna wdrożenia JSON pliku szablonu).


## <a name="set-up-the-application-insights-instance"></a>Skonfiguruj wystąpienie usługi Application Insights

W portalu Azure Utwórz lub przejdź do wystąpienia usługi Application Insights, które chcesz użyć. Zanotuj klucz Instrumentacji wystąpienia. Używasz klucza Instrumentacji w inne czynności konfiguracyjne.

  ![Lokalizacja klucza Instrumentacji](./media/enable-profiler-compute/CopyAIKey.png)

To wystąpienie powinna być taka sama jak aplikacji. Jest on skonfigurowany do wysyłania danych telemetrycznych do na każdym żądaniu.
Wyniki profilera są również dostępne w tym wystąpieniu.  

W portalu Azure, wykonaj kroki opisane w [włączyć profilera](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler) do zakończenia konfigurowania wystąpienia usługi Application Insights profilera. Nie musisz połączyć aplikacji sieci web, na przykład w tym artykule. Po prostu upewnij się, że profiler jest włączone w portalu.


## <a name="set-up-the-application-source-code"></a>Konfigurowanie kodu źródłowego aplikacji

Konfigurowanie aplikacji do wysyłania danych telemetrycznych do wystąpienia usługi Application Insights w każdym `Request` operacji:  

1. Dodaj [zestaw SDK usługi Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview#get-started) projekt aplikacji. Upewnij się, że wersje pakietów NuGet są następujące:  
  - Dla aplikacji ASP.NET: [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 lub nowszym.
  - Dla aplikacji platformy ASP.NET Core: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 lub nowszym.
  - Dla innych aplikacji .NET i .NET Core (na przykład usługi bezstanowej sieci szkieletowej usług lub roli procesu roboczego usługi w chmurze): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) lub [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 lub nowszej.  

2. Jeśli aplikacja jest *nie* aplikacji ASP.NET lub ASP.NET Core (na przykład, jeśli jest to roli procesu roboczego usługi w chmurze lub bezstanowych interfejsów API usługi Service Fabric), wymagane jest następujące ustawienia dodatkowe Instrumentacji:  

  1. Dodaj następujący kod na początku istnienia aplikacji:  

    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    ...
    // Replace with your own Application Insights instrumentation key.
    TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
    ```
  Aby uzyskać więcej informacji na temat tej konfiguracji klucza Instrumentacji globalnych, zobacz [sieci szkieletowej usług użycia z usługą Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. Dla dowolnego elementu kodu, który ma zostać Instrumentacja, Dodaj `StartOperation<RequestTelemetry>` **USING** instrukcji wokół niego, jak w poniższym przykładzie:

    ```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;
    ...
    var client = new TelemetryClient();
    ...
    using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
    {
      // ... Code I want to profile.
    }
    ```

  Wywoływanie `StartOperation<RequestTelemetry>` w innym `StartOperation<RequestTelemetry>` zakres nie jest obsługiwany. Można użyć `StartOperation<DependencyTelemetry>` w zagnieżdżonych zamiast tego zakresu. Na przykład:  

    ```csharp
    using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
    {
      try
      {
        ProductDetail details = new ProductDetail() { Id = productId };
        getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();

        // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
        using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
        {
            double price = await _priceDataBase.GetAsync(productId);
            if (IsTooCheap(price))
            {
                throw new PriceTooLowException(productId);
            }
            details.Price = price;
        }

        // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
        using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
        {
            details.Reviews = await _reviewDataBase.GetAsync(productId);
        }

        getDetailsOperation.Telemetry.Success = true;
        return details;
      }
      catch(Exception ex)
      {
        getDetailsOperation.Telemetry.Success = false;

        // This exception gets linked to the 'GetProductDetails' request telemetry.
        client.TrackException(ex);
        throw;
      }
    }
    ```


## <a name="set-up-the-environment-deployment-definition"></a>Konfigurowanie środowiska definicji wdrożenia

Środowisko, w którym profilera i wykonywanie Twojej aplikacji może być maszynę wirtualną, zestaw skali maszyny wirtualnej, klaster sieci szkieletowej usług lub wystąpienia usługi w chmurze.  

### <a name="virtual-machines-virtual-machine-scale-sets-or-service-fabric"></a>Maszyny wirtualne, zestawy skalowania maszyny wirtualnej i sieci szkieletowej usług

Pełna przykłady:  
  * [Maszyny wirtualne](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Zestaw skali maszyny wirtualnej](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Klaster sieci szkieletowej usług](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

1. Aby upewnić się, że [.NET Framework 4.6.1](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) lub nowszym należy używana, jest wystarczające, aby potwierdzić, że jest wdrożony system operacyjny `Windows Server 2012 R2` lub nowszym.

2. Zlokalizuj [diagnostyki Azure](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) rozszerzenia w szablonie wdrożenia pliku, a następnie dodaj następujące `SinksConfig` sekcji jako element podrzędny elementu `WadCfg`. Zastąp `ApplicationInsightsProfiler` wartości właściwości z własnego klucza Instrumentacji usługi Application Insights:  
  ```json
  "SinksConfig": {
    "Sink": [
      {
        "name": "MyApplicationInsightsProfilerSink",
        "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
      }
    ]
  }
  ```

  Uzyskać informacji na temat dodawania rozszerzenia diagnostyki w szablonie wdrożenia, zobacz [Użyj monitorowania i diagnostyki z szablonów maszyny Wirtualnej systemu Windows i usługi Azure Resource Manager](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


### <a name="cloud-services"></a>Cloud Services

1. Aby upewnić się, że [.NET Framework 4.6.1](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) lub nowszej jest używana, jest wystarczające upewnić się, ten element ServiceConfiguration.\*. plików cscfg `osFamily` wartość **"5"** lub nowszym.

2. Zlokalizuj [diagnostyki Azure](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) diagnostics.wadcfgx plików dla roli użytkownika aplikacji:  
  ![Lokalizacja pliku konfiguracji diagnostyki](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  
  Jeśli nie można odnaleźć pliku, aby dowiedzieć się, jak włączyć rozszerzenie diagnostyki w projekcie usługi w chmurze, zobacz [skonfigurować diagnostykę dla usług Azure Cloud Services i maszyn wirtualnych](https://docs.microsoft.com/en-us/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

3. Dodaj następujące `SinksConfig` sekcji jako element podrzędny elementu `WadCfg`:  
  ```xml
  <WadCfg>
    <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
    <SinksConfig>
      <Sink name="MyApplicationInsightsProfiler">
        <!-- Replace with your own Application Insights instrumentation key. -->
        <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
      </Sink>
    </SinksConfig>
  </WadCfg>
  ```

> [!NOTE]  
> Jeśli `diagnostics.wadcfgx` plik zawiera także zbiornika innego typu `ApplicationInsights`, wszystkie trzy te klucze Instrumentacji musi odpowiadać:  
>  * Klucz Instrumentacji używanych przez aplikację.  
>  * Klucz Instrumentacji używany przez `ApplicationInsights` ujścia.  
>  * Klucz Instrumentacji używany przez `ApplicationInsightsProfiler` ujścia.  
>
> Znajduje się wartość klucza Instrumentacji rzeczywiste używane przez `ApplicationInsights` sink w element ServiceConfiguration.\*. pliki cscfg.  
> Po wydaniu programu Visual Studio SDK Azure 15,5 cala tylko klucze Instrumentacji używane przez aplikację i `ApplicationInsightsProfiler` potrzebę zbiornika pasują do siebie.


## <a name="environment-deployment-and-runtime-configurations"></a>Konfiguracje wdrożenia i środowiska wykonawczego środowiska

1. Wdrożenia zmodyfikowane środowiska definicji wdrożenia.  

  Aby zastosować zmiany, zwykle wdrożenia pełny szablon lub usługi w chmurze publikowania za pomocą poleceń cmdlet programu PowerShell lub uczestniczy Visual Studio.  

  Poniżej znajduje się alternatywne metody dla istniejących maszyn wirtualnych, która dotyka jego rozszerzenie diagnostyki Azure:  
  ```powershell
  $ConfigFilePath = [IO.Path]::GetTempFileName()
  # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
  (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
  # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
  # if your original diagnostics configuration had the storageAccountName property in the protectedSettings section
  # (which is not downloadable). Make sure to pass the same original value you had in this cmdlet call.
  Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
  ```

2. Jeśli aplikację zamierzone działa za pośrednictwem [IIS](https://www.microsoft.com/web/platform/server.aspx), Włącz `IIS Http Tracing` funkcji systemu Windows:  
  
  1. Ustanowić zdalnego dostępu do środowiska, a następnie użyj [dodać funkcji systemu Windows]( https://docs.microsoft.com/en-us/iis/configuration/system.webserver/tracing/) okna lub uruchom następujące polecenie w programie PowerShell (Administrator):  
    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
  2. Ustanawianie dostęp zdalny to problem, można użyć [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli) do uruchom następujące polecenie:  
    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-the-profiler-on-on-premises-servers"></a>Włącz profilera na serwerach lokalnych

Włączanie profilera na lokalnym serwerze jest nazywana uruchomionej aplikacji profilera wgląd w trybie autonomicznym (nie jest on związany na modyfikacje rozszerzenia diagnostyki Azure). 

Nie mamy żadnych planu świadczył oficjalną pomoc techniczną profilera dla serwerów lokalnych. Jeśli interesuje Cię eksperymentowanie z tego scenariusza, możesz [Pobierz kod obsługi](https://github.com/ramach-msft/AIProfiler-Standalone). Możemy *nie* odpowiedzialna za utrzymanie kodu lub reagowania na problemy i żądania funkcji związanych z kodem.

## <a name="next-steps"></a>Następne kroki

- Generowanie ruchu do aplikacji (na przykład uruchomić [test dostępności](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-monitor-web-app-availability)). Następnie zaczekaj 10 – 15 minut na śladów uruchomić do wysłania do wystąpienia usługi Application Insights.
- Zobacz [ślady profilera](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler) w portalu Azure.
- Uzyskaj pomoc dotyczącą rozwiązywania problemów profilera w [profilera Rozwiązywanie problemów z](app-insights-profiler.md#troubleshooting).
- Dowiedz się więcej o profilera w [Application Insights profilera](app-insights-profiler.md).
