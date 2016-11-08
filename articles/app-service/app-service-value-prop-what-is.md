---
title: Usługa Azure App Service dla aplikacji sieci Web i aplikacji mobilnych | Microsoft Docs
description: Dowiedz się, jak usługa Azure App Service ułatwia opracowywanie i wdrażanie Aplikacji sieci Web i aplikacji mobilnych oraz zarządzanie nimi.
keywords: app service, azure app service, app service cost, scale, scalable, app deployment, azure app deployment, paas, platform-as-a-service
services: app-service
documentationcenter: ''
author: omarkmsft
manager: dwrede
editor: jimbe

ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2016
ms.author: omark

---
# Co to jest usługa Azure App Service?
*Usługa App Service* to oferta [platformy jako usługi](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) dostępna w ramach platformy Microsoft Azure. Umożliwia tworzenie aplikacji sieci Web i aplikacji mobilnych dla dowolnej platformy lub urządzenia. Pozwala integrować aplikacje z rozwiązaniami SaaS, nawiązywać połączenia z aplikacjami lokalnymi i automatyzować procesy biznesowe. Na platformie Azure aplikacje są uruchamiane na w pełni zarządzanych maszynach wirtualnych przy użyciu wybranych przez użytkownika udostępnianych zasobów maszyn wirtualnych lub dedykowanych maszyn wirtualnych. 

Usługa App Service oferuje możliwości związane z aplikacjami sieci Web i aplikacjami mobilnymi, które były wcześniej dostępne oddzielnie jako usługi Azure Websites oraz Azure Mobile Services.  Obejmuje ona także nowe funkcje automatyzacji procesów biznesowych i hostowania interfejsów API w chmurze. Usługa App Service jako pojedyncza zintegrowana usługa umożliwia korzystanie z różnych składników — witryn sieci Web, zaplecza aplikacji mobilnych, interfejsów API RESTful i procesów biznesowych — w ramach jednego rozwiązania.

Poniższy 4-minutowy film zawiera krótkie objaśnienie, w jaki sposób usługa App Service jest powiązana z wcześniejszymi ofertami platformy Azure i jakie nowe funkcje udostępnia.

+[AZURE.VIDEO app-service-history-lesson] 

## Dlaczego warto korzystać z usługi App Service?
Poniżej przedstawiono niektóre najważniejszych funkcji i możliwości usługi App Service: 

* **Wiele języków i struktur** — usługa App Service oferuje najwyższej jakości pomoc techniczną w zakresie rozwiązań ASP.NET, Node.js, Java, PHP i Python. Na maszynach wirtualnych usługi App Service można również uruchamiać [program Windows PowerShell oraz inne skrypty lub pliki wykonywalne](../app-service-web/web-sites-create-web-jobs.md).
* **Optymalizacja metodyki DevOps** — konfigurowanie [ciągłej integracji i wdrażania](../app-service-web/app-service-continuous-deployment.md) za pomocą usług Visual Studio Team Services, GitHub lub BitBucket. Promowanie aktualizacji za pośrednictwem [środowisk testowych i przejściowych](../app-service-web/web-sites-staged-publishing.md). Wykonywanie [testowania A/B](../app-service-web/app-service-web-test-in-production-get-start.md). Zarządzaj aplikacjami w usłudze App Service za pomocą programu [Azure PowerShell](../powershell-install-configure.md) lub [międzyplatformowego interfejsu wiersza polecenia](../xplat-cli-install.md).
* **Globalne skalowanie i wysoka dostępność** — ręczne lub automatyczne skalowanie [w pionie](../app-service-web/web-sites-scale.md) lub [w poziomie](../azure-portal/insights-how-to-scale.md). Hostuj aplikacje w dowolnym miejscu globalnej infrastruktury centrum danych firmy Microsoft i korzystaj z wysokiej dostępności usługi App Service zapewnianej w umowie [SLA](https://azure.microsoft.com/support/legal/sla/app-service/).
* **Połączenia z platformami SaaS i danymi lokalnymi** — możliwość wyboru spośród ponad 50 [łączników](../connectors/apis-list.md) obsługujących systemy dla przedsiębiorstw (takie jak SAP, Siebel i Oracle), usługi SaaS (takie jak Salesforce i Office 365) oraz serwisy internetowe (takie jak Facebook i Twitter). Dostęp do danych lokalnych przy użyciu [połączeń hybrydowych](../biztalk-services/integration-hybrid-connection-overview.md) i [sieci wirtualnych platformy Azure](../app-service-web/web-sites-integrate-with-vnet.md).
* **Bezpieczeństwo i zgodność** — usługa App Service jest [zgodna ze standardami ISO, SOC i PCI](https://www.microsoft.com/TrustCenter/).
* **Szablony aplikacji** — możliwość wyboru z obszernej listy szablonów aplikacji dostępnych w portalu [Azure Marketplace](https://azure.microsoft.com/marketplace/) oraz instalowania popularnego oprogramowania typu open source, takiego jak WordPress, Joomla i Drupal, z użyciem kreatora.
* **Integracja z programem Visual Studio** —dedykowane narzędzia w programie Visual Studio usprawniają procesy tworzenia, wdrażania i debugowania.

## Typy aplikacji w usłudze App Service
Usługa App Service oferuje kilka *typów aplikacji*, z których każdy jest przeznaczony do obsługi określonego rodzaju obciążenia:

* [**Web Apps**](../app-service-web/app-service-web-overview.md) — do hostowania witryn sieci Web i aplikacji sieci Web.
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md) — do hostowania zaplecza aplikacji mobilnych.
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) — do hostowania interfejsów API w chmurze. 
* [**Logic Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md) — do automatyzowania dostępu do danych i korzystania z nich w chmurze bez pisania kodu.

Wyraz *aplikacja* odnosi się tutaj do zasobów hostowania przeznaczonych do obsługi obciążenia. Na przykład wyrażenie „aplikacja sieci Web” kojarzy się prawdopodobnie z zasobami obliczeniowymi i kodem aplikacji, które łącznie realizują funkcje używane w przeglądarce. W usłudze App Service *aplikacja sieci Web* oznacza zasoby obliczeniowe, które platforma Azure udostępnia na potrzeby hostowania kodu aplikacji. Jeśli aplikacja składa się z frontonu sieci Web i zaplecza interfejsu API RESTful, można wdrożyć obydwa elementy w aplikacji sieci Web lub wdrożyć kod frontonu w aplikacji sieci Web, a kod zaplecza w aplikacji interfejsu API. Aplikacja może się składać z wielu aplikacji usługi App Service różnego rodzaju.

## Plany usługi App Service
[Plany usługi App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) określają rodzaj zasobów obliczeniowych obsługujących działanie aplikacji. Jeśli oczekujesz mniejszego ruchu, możesz korzystać z udostępnianych maszyn wirtualnych. W przypadku większych obciążeń możesz wybrać spośród kilku rozmiarów dedykowanych maszyn wirtualnych. Wiele aplikacji usługi App Service może współdzielić ten sam plan. Można je również skalować w górę i w dół zgodnie z założeniami planu.

Jeśli potrzebujesz większej skalowalności i izolacji sieci, możesz uruchamiać aplikacje w [środowisku usługi App Service](../app-service-web/app-service-app-service-environment-intro.md). 

## Ceny
Informacje na temat cen usługi App Service można znaleźć w artykule [App Service — ceny](https://azure.microsoft.com/pricing/details/app-service/). 

## Wprowadzenie do usługi App Service
[Utwórz tymczasową aplikację sieci Web, aplikację mobilną lub aplikację logiki](http://go.microsoft.com/fwlink/?LinkId=523751) od razu, za darmo, bez karty kredytowej, bez zobowiązań i bez problemu.

Możesz też założyć [bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/) i wypróbować jeden z naszych samouczków ułatwiających rozpoczynanie pracy:

* [Samouczek: tworzenie aplikacji sieci Web](../app-service-web/app-service-web-get-started.md)
* [Samouczek: tworzenie aplikacji mobilnej](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Samouczek: tworzenie aplikacji interfejsu API](../app-service-api/app-service-api-dotnet-get-started.md)
* [Samouczek: tworzenie aplikacji logiki](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!--HONumber=Sep16_HO3-->


