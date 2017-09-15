---
title: "Usługa Azure App Service dla aplikacji sieci Web, aplikacji mobilnych i aplikacji interfejsu API | Microsoft Docs"
description: "Dowiedz się, jak usługa Azure App Service ułatwia opracowywanie i wdrażanie Aplikacji sieci Web i aplikacji mobilnych oraz zarządzanie nimi."
keywords: "app service, azure app service, koszt usługi app service, skala, skalowalny, wdrażanie aplikacji, wdrażanie aplikacji azure, paas, platforma jako usługa, witryna sieci Web, witryna internetowa, usługi mobilne azure"
services: app-service
documentationcenter: 
author: omarkmsft
manager: erikre
editor: cephalin
ms.assetid: 979cafa8-eeb6-4d3b-87cf-764a821c3e4f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/02/2016
ms.author: byvinyal
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 94869072a38be61819213fbf4f919764fe1849c0
ms.contentlocale: pl-pl
ms.lasthandoff: 09/07/2017

---
# <a name="what-is-azure-app-service"></a>Co to jest usługa Azure App Service?
*Usługa App Service* to oferta [platformy jako usługi](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) dostępna w ramach platformy Microsoft Azure. Umożliwia tworzenie aplikacji sieci Web i aplikacji mobilnych dla dowolnej platformy lub urządzenia. Pozwala integrować aplikacje z rozwiązaniami SaaS, nawiązywać połączenia z aplikacjami lokalnymi i automatyzować procesy biznesowe. Na platformie Azure aplikacje są uruchamiane na w pełni zarządzanych maszynach wirtualnych przy użyciu wybranych przez użytkownika udostępnianych zasobów maszyn wirtualnych lub dedykowanych maszyn wirtualnych.

Usługa App Service oferuje możliwości związane z aplikacjami sieci Web i aplikacjami mobilnymi, które były wcześniej dostępne oddzielnie jako usługi Azure Websites oraz Azure Mobile Services. Obejmuje ona także nowe funkcje automatyzacji procesów biznesowych i hostowania interfejsów API w chmurze. Usługa App Service jako pojedyncza zintegrowana usługa umożliwia korzystanie z różnych składników — witryn sieci Web, zaplecza aplikacji mobilnych, interfejsów API RESTful i procesów biznesowych — w ramach jednego rozwiązania.

Poniższy 4-minutowy film zawiera krótkie objaśnienie, w jaki sposób usługa App Service jest powiązana z wcześniejszymi ofertami platformy Azure i jakie nowe funkcje udostępnia.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/app-service-history-lesson/player]
> 
> 

## <a name="why-use-app-service"></a>Dlaczego warto korzystać z usługi App Service?
Poniżej przedstawiono niektóre najważniejszych funkcji i możliwości usługi App Service:

* **Wiele języków i struktur** — usługa App Service oferuje najwyższej jakości pomoc techniczną w zakresie rozwiązań ASP.NET, Node.js, Java, PHP i Python. Na maszynach wirtualnych usługi App Service można również uruchamiać [program Windows PowerShell oraz inne skrypty lub pliki wykonywalne](../app-service-web/web-sites-create-web-jobs.md).
* **Optymalizacja metodyki DevOps** — konfigurowanie [ciągłej integracji i wdrażania](../app-service-web/app-service-continuous-deployment.md) za pomocą usług Visual Studio Team Services, GitHub lub BitBucket. Promowanie aktualizacji za pośrednictwem [środowisk testowych i przejściowych](../app-service-web/web-sites-staged-publishing.md). Zarządzaj aplikacjami w usłudze App Service za pomocą programu [Azure PowerShell](/powershell/azureps-cmdlets-docs) lub [międzyplatformowego interfejsu wiersza polecenia](../cli-install-nodejs.md).
* **Globalne skalowanie i wysoka dostępność** — ręczne lub automatyczne skalowanie [w pionie](../app-service-web/web-sites-scale.md) lub [w poziomie](../monitoring-and-diagnostics/insights-how-to-scale.md). Hostuj aplikacje w dowolnym miejscu globalnej infrastruktury centrum danych firmy Microsoft i korzystaj z wysokiej dostępności usługi App Service zapewnianej w umowie [SLA](https://azure.microsoft.com/support/legal/sla/app-service/).
* **Połączenia z platformami SaaS i danymi lokalnymi** — możliwość wyboru spośród ponad 50 [łączników](../connectors/apis-list.md) obsługujących systemy dla przedsiębiorstw (takie jak SAP, Siebel i Oracle), usługi SaaS (takie jak Salesforce i Office 365) oraz serwisy internetowe (takie jak Facebook i Twitter). Dostęp do danych lokalnych przy użyciu [połączeń hybrydowych](../biztalk-services/integration-hybrid-connection-overview.md) i [sieci wirtualnych platformy Azure](../app-service-web/web-sites-integrate-with-vnet.md).
* **Bezpieczeństwo i zgodność** — usługa App Service jest [zgodna ze standardami ISO, SOC i PCI](https://www.microsoft.com/TrustCenter/).
* **Szablony aplikacji** — możliwość wyboru z obszernej listy szablonów dostępnych w portalu [Azure Marketplace](https://azure.microsoft.com/marketplace/) oraz instalowania popularnego oprogramowania typu open source, takiego jak WordPress, Joomla i Drupal, z użyciem kreatora.
* **Integracja z programem Visual Studio** —dedykowane narzędzia w programie Visual Studio usprawniają procesy tworzenia, wdrażania i debugowania.

## <a name="app-types-in-app-service"></a>Typy aplikacji w usłudze App Service
Usługa App Service oferuje kilka *typów aplikacji*, z których każdy jest przeznaczony do obsługi określonego obciążenia:

* [**Web Apps**](../app-service-web/app-service-web-overview.md) — do hostowania witryn sieci Web i aplikacji sieci Web.
* [**Mobile Apps**](../app-service-mobile/app-service-mobile-value-prop.md) — do hostowania zaplecza aplikacji mobilnych.
* [**API Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) — do hostowania interfejsów API RESTful.
* [**Logic Apps**](../logic-apps/logic-apps-what-are-logic-apps.md) — do automatyzacji procesów biznesowych oraz integrowania systemów i danych między chmurami bez pisania kodu.

Wyraz *aplikacja* odnosi się tutaj do zasobów hostowania przeznaczonych do obsługi obciążenia. Na przykład wyrażenie „aplikacja sieci Web” kojarzy się prawdopodobnie z zasobami obliczeniowymi i kodem aplikacji, które łącznie realizują funkcje używane w przeglądarce. W usłudze App Service *aplikacja sieci Web* oznacza zasoby obliczeniowe, które platforma Azure udostępnia na potrzeby hostowania kodu aplikacji. 

Aplikacja może się składać z wielu aplikacji usługi App Service różnego rodzaju. Jeśli na przykład aplikacja obejmuje fronton sieci Web i zaplecze interfejsu RESTful API, można:

- Wdrożyć oba rozwiązania (fronton i interfejs API) w jednej aplikacji sieci Web.  
- Wdrożyć kod frontonu w aplikacji sieci Web i kod zaplecza w aplikacji interfejsu API. 



## <a name="app-service-plans"></a>Plany usługi App Service
[Plany usługi App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) reprezentują kolekcję zasobów fizycznych służących do hostowania aplikacji.

Plany usługi App Service definiują następujące elementy:

- **Region** (Zachodnie stany USA, Wschodnie stany USA itp.)
- **Wartość licznika skali** (jedno, dwa, trzy wystąpienia itp.)
- **Rozmiar wystąpienia** (mały, średni, duży)
- **Jednostka magazynowa** (Bezpłatna, Współdzielona, Podstawowa, Standardowa, Premium)

Wszystkie aplikacje przypisane do **planu usługi App Service** współużytkują zasoby przez niego zdefiniowane, ograniczając koszt hostowania wielu aplikacji.

**Plan usługi App Service** może być jednostką magazynową typu **Bezpłatna**, **Współdzielona**, **Podstawowa**, **Standardowa** lub **Premium**, zapewniając dostęp do odpowiedniej liczby zasobów i funkcji. Kiedy dla planu usługi App Service zostanie wybrana opcja **Podstawowa** lub wyższa, można także wybrać **rozmiar** i wartość licznika skali maszyn wirtualnych.

Ustawienia **Jednostka magazynowa** i **Skala** planu usługi App Service określają koszt i liczbę hostowanych w nim aplikacji. 

Jeśli potrzebujesz większej skalowalności i izolacji sieci, możesz uruchamiać aplikacje w [środowisku usługi App Service](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Ceny
Informacje na temat cen usługi App Service można znaleźć w artykule [App Service — ceny](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Testowanie usługi App Service
[Utwórz przykładową aplikację sieci Web, aplikację mobilną lub aplikację logiki](https://azure.microsoft.com/try/app-service/) i testuj ją przez jedną godzinę bez konieczności podawania danych karty kredytowej, bez zobowiązań i bez problemu.

Możesz też założyć [bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/) i wypróbować jeden z naszych samouczków ułatwiających rozpoczynanie pracy:

* [Samouczek: tworzenie aplikacji sieci Web](../app-service-web/app-service-web-get-started.md)
* [Samouczek: tworzenie aplikacji mobilnej](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Samouczek: tworzenie aplikacji interfejsu API](../app-service-api/app-service-api-dotnet-get-started.md)
* [Samouczek: tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md)


