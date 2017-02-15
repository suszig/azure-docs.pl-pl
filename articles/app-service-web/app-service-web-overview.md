---
title: "Przegląd usługi Web Apps | Microsoft Docs"
description: "Dowiedz się, w jaki sposób usługa Azure App Service ułatwia tworzenie i hostowanie aplikacji sieci Web"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 18959934c53e2e1c719cc627ffa286acbdcaa967


---
# <a name="web-apps-overview"></a>Przegląd usługi Web Apps
*Azure App Service Web Apps* to w pełni zarządzana platforma obliczeniowa zoptymalizowana pod kątem hostowania witryn i aplikacji sieci Web. Oferta [platforma jako usługa](https://en.wikipedia.org/wiki/Platform_as_a_service) (Platform as a Service, PaaS) platformy Microsoft Azure umożliwia użytkownikowi skupienie się na logice biznesowej, podczas gdy platforma Azure jest odpowiedzialna za infrastrukturę służącą do uruchamiania i skalowania aplikacji.

Następujący 5-minutowy film wideo stanowi wprowadzenie do funkcji Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

## <a name="what-is-a-web-app-in-app-service"></a>Co to jest aplikacja sieci Web w usłudze App Service?
*Aplikacja sieci Web* w usłudze App Service to zasób obliczeniowy udostępniany przez platformę Azure na potrzeby hostowania witryny lub aplikacji sieci Web.  

Zasoby obliczeniowe mogą znajdować się na współdzielonych lub dedykowanych maszynach wirtualnych — jest to zależne od wybranej warstwy cenowej. Kod aplikacji jest uruchamiany na zarządzanej maszynie wirtualnej, która jest odizolowana od innych klientów.

Kod może być tworzony przy użyciu dowolnego języka lub struktury obsługiwanej przez [usługę Azure App Service](../app-service/app-service-value-prop-what-is.md), na przykład ASP.NET, Node.js, Java, PHP lub Python. W aplikacji sieci Web można również uruchamiać skrypty używające [programu PowerShell i innych języków skryptowych](web-sites-create-web-jobs.md#acceptablefiles).

Przykłady typowych scenariuszy aplikacji, których można użyć na potrzeby funkcji Web Apps, można znaleźć w artykule [Scenariusze aplikacji sieci Web](https://azure.microsoft.com/documentation/scenarios/web-app/) i w sekcji **Scenarios and recommendations** (Scenariusze i zalecenia) w artykule [Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison](choose-web-site-cloud-service-vm.md#scenarios) (Porównanie usług Azure App Service, Virtual Machines, Service Fabric i Cloud Services).

## <a name="why-use-web-apps"></a>Dlaczego warto używać usługi Web Apps?
Poniżej przedstawiono kilka najważniejszych funkcji usługi App Service, które dotyczą usługi Web Apps:

* **Wiele języków i struktur** — usługa App Service oferuje najwyższej jakości pomoc techniczną dotyczącą rozwiązań ASP.NET, Node.js, Java, PHP i Python. Na maszynach wirtualnych usługi App Service można również uruchamiać [program PowerShell oraz inne skrypty lub pliki wykonywalne](web-sites-create-web-jobs.md).
* **Optymalizacja metodyki DevOps** — konfigurowanie [ciągłej integracji i wdrażania](app-service-continuous-deployment.md) za pomocą usług Visual Studio Team Services, GitHub lub BitBucket. Promowanie aktualizacji za pośrednictwem [środowisk testowych i przejściowych](web-sites-staged-publishing.md). Wykonywanie [testowania A/B](app-service-web-test-in-production-get-start.md). Zarządzaj aplikacjami w usłudze App Service za pomocą programu [Azure PowerShell](/powershell/azureps-cmdlets-docs) lub [międzyplatformowego interfejsu wiersza polecenia](../xplat-cli-install.md).
* **Globalne skalowanie i wysoka dostępność** — ręczne lub automatyczne skalowanie [w pionie](web-sites-scale.md) lub [w poziomie](../monitoring-and-diagnostics/insights-how-to-scale.md). Hostuj aplikacje w dowolnym miejscu globalnej infrastruktury centrum danych firmy Microsoft i korzystaj z wysokiej dostępności usługi App Service zapewnianej w umowie [SLA](https://azure.microsoft.com/support/legal/sla/app-service/).
* **Połączenia z platformami SaaS i danymi lokalnymi** — możliwość wyboru spośród ponad 50 [łączników](../connectors/apis-list.md) obsługujących systemy dla przedsiębiorstw (takie jak SAP, Siebel i Oracle), usługi SaaS (takie jak Salesforce i Office 365) oraz serwisy internetowe (takie jak Facebook i Twitter). Dostęp do danych lokalnych przy użyciu [połączeń hybrydowych](../biztalk-services/integration-hybrid-connection-overview.md) i [sieci wirtualnych platformy Azure](web-sites-integrate-with-vnet.md).
* **Bezpieczeństwo i zgodność** — usługa App Service jest [zgodna ze standardami ISO, SOC i PCI](https://www.microsoft.com/TrustCenter/).
* **Szablony aplikacji** — możliwość wyboru z obszernej listy szablonów aplikacji dostępnych w portalu [Azure Marketplace](https://azure.microsoft.com/marketplace/) oraz instalowania popularnego oprogramowania typu open source, takiego jak WordPress, Joomla i Drupal, z użyciem kreatora.
* **Integracja z programem Visual Studio** —dedykowane narzędzia w programie Visual Studio usprawniają procesy tworzenia, wdrażania i debugowania.

Ponadto aplikacja sieci Web może korzystać z funkcji oferowanych przez usługi [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (na przykład obsługa mechanizmu CORS) i [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (na przykład powiadomienia wypychane). Aby uzyskać więcej informacji na temat typów aplikacji w usłudze App Service, zobacz [omówienie usługi Azure App Service](../app-service/app-service-value-prop-what-is.md).

Oprócz usługi Web Apps w usłudze App Service platforma Azure oferuje inne usługi, które mogą służyć do hostowania witryn i aplikacji sieci Web. W przypadku większości scenariuszy najlepszym wyborem jest usługa Web Apps.  W przypadku architektury mikrousługi należy wziąć pod uwagę usługę [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric) i w razie konieczności sprawowania większej kontroli nad maszynami wirtualnymi, na których działa kod, należy wziąć pod uwagę usługę [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Aby uzyskać więcej informacji o tym, jak dokonać wyboru między usługami Azure, zobacz [Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison](choose-web-site-cloud-service-vm.md) (Porównanie usług Azure App Service, Virtual Machines, Service Fabric i Cloud Services).

## <a name="getting-started"></a>Wprowadzenie
Aby rozpocząć od wdrożenia przykładowego kodu w nowej aplikacji sieci Web w usłudze App Service, postępuj zgodnie z informacjami znajdującymi się w jednym z samouczków w poniższym polu listy rozwijanej. Konieczne będzie posiadanie bezpłatnego konta platformy Azure.

> [!div class="op_single_selector"]
> * [Wdrażanie pierwszej witryny HTML na platformie Azure w ciągu 5 minut](app-service-web-get-started-html-cli-nodejs.md)
> * [Wdrażanie pierwszej aplikacji sieci Web platformy ASP.NET na platformie Azure w ciągu 5 minut](app-service-web-get-started-dotnet-cli-nodejs.md)
> * [Wdrażanie pierwszej aplikacji sieci Web w języku PHP na platformie Azure w ciągu 5 minut](app-service-web-get-started-php-cli-nodejs.md)
> * [Wdrażanie pierwszej aplikacji sieci Web w języku Node.js na platformie Azure w ciągu 5 minut](app-service-web-get-started-nodejs-cli-nodejs.md)
> * [Wdrażanie pierwszej aplikacji sieci Web w języku Python na platformie Azure w ciągu 5 minut](app-service-web-get-started-python-cli-nodejs.md)
> * [Wdrażanie pierwszej aplikacji sieci Web w języku Java na platformie Azure w ciągu 5 minut](app-service-web-get-started-java.md)
> 
> 

> [!NOTE]
> Usługę [App Service](https://azure.microsoft.com/try/app-service/) możesz wypróbować, nie mając konta platformy Azure. Utwórz aplikację startową i testuj ją nawet przez godzinę — bez kart kredytowych i bez zobowiązań.
> 
> 



<!--HONumber=Feb17_HO2-->


