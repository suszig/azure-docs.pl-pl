---
title: "Rozwiązywanie problemów z usługi w chmurze przy użyciu usługi Application Insights | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak rozwiązywanie problemów dotyczących usługi w chmurze przy użyciu usługi Application Insights do przetwarzania danych z diagnostyki Azure."
services: cloud-services
documentationcenter: .net
author: sbtron
manager: timlt
editor: tysonn
ms.assetid: e93f387b-ef29-4731-ae41-0676722accb6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: saurabh
ms.openlocfilehash: 4001ca908ff00b1a40829d687589080e9b07b18a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-cloud-services-using-application-insights"></a>Rozwiązywanie problemów z usługi w chmurze przy użyciu usługi Application Insights
Z [Azure SDK 2.8](https://azure.microsoft.com/downloads/) i rozszerzenie diagnostycznych platformy Azure w wersji 1.5, możesz wysłać dane diagnostyczne platformy Azure dla usługi w chmurze bezpośrednio z usługą Application Insights. Dzienniki są zbierane przez diagnostyki Azure&mdash;w tym dzienniki aplikacji, dzienniki zdarzeń systemu Windows, dzienniki zdarzeń systemu Windows i liczniki wydajności&mdash;mogą być wysyłane do usługi Application Insights. Następnie można zwizualizować te informacje w portalu usługi Application Insights interfejsu użytkownika. Zestaw SDK usługi Application Insights umożliwia następnie Uzyskaj wgląd w dzienniki, które pochodzą z aplikacji, a także systemu i danych na poziomie infrastruktury, która pochodzi z diagnostyki Azure i metryki.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Skonfiguruj diagnostyki Azure w celu wysyłania danych do usługi Application Insights
Wykonaj następujące kroki, aby skonfigurować projekt usługi w chmurze na wysyłanie danych diagnostycznych Azure do usługi Application Insights.

1. W Eksploratorze rozwiązań programu Visual Studio, kliknij prawym przyciskiem myszy rolę i wybierz **właściwości** można otworzyć projektanta roli.

    ![Właściwości roli Eksploratora rozwiązań][1]

2. W **diagnostyki** sekcji projektanta roli, wybierz opcję **Wyślij dane diagnostyczne do usługi Application Insights** opcji.

    ![Projektant roli Wyślij dane diagnostyczne do usługi application insights][2]

3. W wyskakującym oknie dialogowym Wybierz zasób usługi Application Insights, które chcesz wysyłać dane diagnostyczne platformy Azure. Okno dialogowe umożliwia wybierz istniejący zasób usługi Application Insights z subskrypcji lub ręcznie określić klucz instrumentacji dla zasobu usługi Application Insights. Aby uzyskać więcej informacji na temat tworzenia zasobu usługi Application Insights, zobacz [utworzyć nowy zasób usługi Application Insights](../application-insights/app-insights-create-new-resource.md).

    ![Wybierz zasób usługi application insights][3]

    Po dodaniu zasobu usługi Application Insights klucza instrumentacji dla tego zasobu jest przechowywana jako ustawienia konfiguracji usługi o nazwie **APPINSIGHTS_INSTRUMENTATIONKEY**. Można zmienić tego ustawienia konfiguracji dla każdej konfiguracji usługi lub środowiska. Aby to zrobić, wybierz inną konfigurację z **konfiguracji usługi** listę i określenie nowego klucza Instrumentacji w takiej konfiguracji.

    ![Wybierz konfigurację usługi][4]

    **APPINSIGHTS_INSTRUMENTATIONKEY** ustawienie konfiguracji jest używany przez program Visual Studio do konfigurowania rozszerzenia diagnostyki odpowiednie informacje zasobu usługi Application Insights podczas publikowania. Ustawienie konfiguracji jest to wygodny sposób definiowania różnych Instrumentacji kluczy w przypadku konfiguracji z innej usługi. Programu Visual Studio będzie Przetłumacz ustawienie i wstawić je do diagnostyki publiczna Konfiguracja rozszerzenia podczas procesu publikowania. Aby uprościć proces konfigurowania rozszerzenia diagnostyki przy użyciu programu PowerShell, dane wyjściowe pakietu z programu Visual Studio zawiera publicznej konfiguracji XML przy użyciu odpowiedniego klucza Instrumentacji usługi Application Insights. Pliki konfiguracji publicznego są tworzone w folderze rozszerzenia i postępuj zgodnie ze wzorcem *PaaSDiagnostics.&lt; RoleName&gt;. PubConfig.xml*. Wszystkie wdrożenia oparte na środowisku PowerShell można używać tego wzorca do mapowania każdej konfiguracji do roli.

4) Aby skonfigurować diagnostycznych platformy Azure, aby wysłać wszystkie dzienniki poziom błędu zbieranych przez agenta diagnostyki Azure do usługi Application Insights i liczniki wydajności, należy włączyć **Wyślij dane diagnostyczne do usługi Application Insights** opcji. 

    Jeśli chcesz dodatkowo skonfigurować, jakie dane są wysyłane do usługi Application Insights, należy ręcznie zmienić *diagnostics.wadcfgx* plik dla każdej roli. Zobacz [skonfigurować diagnostyki Azure do przesyłania danych do usługi Application Insights](#configure-azure-diagnostics-to-send-data-to-application-insights) Aby dowiedzieć się więcej na temat ręcznego aktualizowania konfiguracji.

Gdy usługa w chmurze jest skonfigurowany do wysyłania danych diagnostycznych platformy Azure do usługi application insights, można wdrożyć go na platformie Azure zazwyczaj zagwarantowanie, że rozszerzenie Azure diagnostics jest włączone. Aby uzyskać więcej informacji, zobacz [publikowania usługi w chmurze przy użyciu programu Visual Studio](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Wyświetlanie danych diagnostycznych platformy Azure w usłudze Application Insights
Azure telemetrii diagnostycznych zostaną wyświetlone w zasobu usługi Application Insights skonfigurowany dla usługi w chmurze.

Mapa typy dziennika diagnostyki Azure do koncepcji usługi Application Insights w następujący sposób:

* Liczniki wydajności są wyświetlane jako metryki niestandardowe w usłudze Application Insights.
* Dzienniki zdarzeń systemu Windows są wyświetlane jako śladów i zdarzeń niestandardowych w usłudze Application Insights.
* Dzienniki aplikacji, dzienniki zdarzeń systemu Windows i żadnych dzienników diagnostycznych infrastruktury są wyświetlane jako dane śledzenia w usłudze Application Insights.

Aby wyświetlić dane diagnostyczne platformy Azure w usłudze Application Insights, wykonaj jedną z następujących czynności:

* Użyj [Eksploratora metryk](../application-insights/app-insights-metrics-explorer.md) do wizualizacji wszelkie niestandardowe liczniki wydajności lub liczby różnych rodzajów zdarzeń w dzienniku zdarzeń systemu Windows.

    ![Metryki niestandardowe w Eksploratorze metryk][5]

* Użyj [wyszukiwania](../application-insights/app-insights-diagnostic-search.md) można przeszukiwać dzienniki śledzenia wysyłane przez diagnostyki Azure. Na przykład, jeśli wystąpił nieobsługiwany wyjątek spowodował roli awarii i Odtwórz, informacje o wyjątku zostaną wyświetlone w *aplikacji* kanału *dziennika zdarzeń systemu Windows*. Wyszukiwania można używać, aby znaleźć w dzienniku zdarzeń systemu Windows i pobranie śladu stosu pełny wyjątek ustalić przyczynę problemu.

    ![Ślady wyszukiwania][6]

## <a name="next-steps"></a>Następne kroki
* [Dodaj zestaw SDK usługi Application Insights do usługi w chmurze](../application-insights/app-insights-cloudservices.md) Aby wysłać dane dotyczące żądań, wyjątków, zależności i wszelkie niestandardowe dane telemetryczne z aplikacji. W połączeniu z danymi diagnostyki Azure, te informacje można uzyskać pełny widok aplikacji i systemu, w tej samej zasobów szczegółowe informacje o aplikacji.  

<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
