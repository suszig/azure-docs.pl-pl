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
ms.topic: overview
ms.date: 01/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f8510bb6b412e9af8aad30ba32bc74206c22042f
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="web-apps-overview"></a>Przegląd usługi Web Apps

*Azure App Service Web Apps* (lub po prostu aplikacji sieci Web) to usługa do obsługi aplikacji sieci web, interfejsów API REST, i kończy się wstecz mobile. Możesz programowania w języku Ulubione, można go .NET, .NET Core, Java, Ruby, Node.js, PHP lub Python. Można uruchomić i skalowanie aplikacji z łatwością w systemie Windows lub maszyn wirtualnych systemu Linux (zobacz [usługi aplikacji w systemie Linux](containers/app-service-linux-intro.md)). 

Aplikacje sieci Web nie tylko dodaje do aplikacji, takich jak zabezpieczenia, równoważenia obciążenia, skalowanie automatyczne, możliwości programu Microsoft Azure i automatycznego zarządzania. Mogą również czerpać korzyści z jego możliwości opracowywania oprogramowania, takie jak ciągłego wdrażania od VSTS, GitHub, Centrum Docker i innych źródeł, zarządzania pakietami przemieszczania środowiskach, niestandardowe domeny i certyfikaty SSL. 

Z usługi aplikacji płacisz za zasoby obliczeniowe systemu Azure, którego używasz. Zasoby obliczeniowe, używany jest określany przez _planu usługi aplikacji_ uruchamiać aplikacje sieci Web. Aby uzyskać więcej informacji, zobacz [planów usługi App Service w aplikacjach sieci Web platformy Azure](azure-web-sites-web-hosting-plans-in-depth-overview.md).

Następujący 5-minutowy film wideo stanowi wprowadzenie do funkcji Azure App Service Web Apps.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Web-Apps-with-Yochay-Kiriaty/player]
>
>

## <a name="why-use-web-apps"></a>Dlaczego warto używać usługi Web Apps?
Poniżej przedstawiono niektóre główne funkcje aplikacji usługi sieci Web aplikacji:

* **Wiele języków i struktur** — aplikacji sieci Web ma najwyższej jakości pomoc techniczną dla platformy ASP.NET, platformy ASP.NET Core, Java, Ruby, Node.js, PHP lub Python. Można również uruchomić [PowerShell oraz inne skrypty lub pliki wykonywalne](web-sites-create-web-jobs.md) w tle jako usługi.
* **Optymalizacja metodyki DevOps** — Konfigurowanie [ciągłej integracji i wdrażania](app-service-continuous-deployment.md) z Visual Studio Team Services, GitHub, BitBucket, Centrum Docker lub usługi kontenera platformy Azure. Promowanie aktualizacji za pośrednictwem [środowisk testowych i przejściowych](web-sites-staged-publishing.md). Zarządzaj aplikacjami w aplikacji sieci Web przy użyciu [programu Azure PowerShell](/powershell/azureps-cmdlets-docs) lub [międzyplatformowego interfejsu wiersza polecenia (CLI)](/cli/azure/install-azure-cli).
* **Globalne skalowanie i wysoka dostępność** — ręczne lub automatyczne skalowanie [w pionie](web-sites-scale.md) lub [w poziomie](../monitoring-and-diagnostics/insights-how-to-scale.md). Hostuj aplikacje w dowolnym miejscu globalnej infrastruktury centrum danych firmy Microsoft i korzystaj z wysokiej dostępności usługi App Service zapewnianej w umowie [SLA](https://azure.microsoft.com/support/legal/sla/app-service/).
* **Połączenia danych platformy i lokalnymi SaaS** — wybierz jedną z więcej niż 50 [łączniki](../connectors/apis-list.md) obsługujących systemy dla przedsiębiorstw (takie jak SAP), usługi SaaS (np. Salesforce) oraz serwisy internetowe (takie jak Facebook). Dostęp do danych lokalnych przy użyciu [połączeń hybrydowych](../biztalk-services/integration-hybrid-connection-overview.md) i [sieci wirtualnych platformy Azure](web-sites-integrate-with-vnet.md).
* **Bezpieczeństwo i zgodność** — usługa App Service jest [zgodna ze standardami ISO, SOC i PCI](https://www.microsoft.com/TrustCenter/). Uwierzytelnianie użytkowników z [usługi Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) lub za pomocą logowania społecznościowych ([Google](app-service-mobile-how-to-configure-google-authentication.md), [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md), [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md), i [ Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)). Utwórz [ograniczenia adresu IP](app-service-ip-restrictions.md) i [Zarządzanie tożsamościami usługi](app-service-managed-service-identity.md).
* **Szablony aplikacji** — wybierz z obszernej listy szablonów aplikacji dostępnych w [portalu Azure Marketplace](https://azure.microsoft.com/marketplace/), takie jak WordPress, Joomla i Drupal.
* **Integracja z programem Visual Studio** —dedykowane narzędzia w programie Visual Studio usprawniają procesy tworzenia, wdrażania i debugowania.
* **Interfejs API i funkcje mobilne** — aplikacje sieci Web udostępnia gotowe obsługi mechanizmu CORS w scenariuszach interfejsu API RESTful i upraszcza aplikacji mobilnej scenariusze włączania uwierzytelniania, synchronizacji danych w trybie offline, powiadomień wypychanych i.
* **Kod niekorzystającą** — Uruchom fragment kodu lub skryptów na żądanie bez konieczności jawnego udostępniania lub zarządzania infrastrukturą, dzięki czemu płacisz tylko za czas obliczeń kodu faktycznie używa (zobacz [usługi Azure Functions](/azure/azure-functions/)).

Oprócz usługi Web Apps w usłudze App Service platforma Azure oferuje inne usługi, które mogą służyć do hostowania witryn i aplikacji sieci Web. W przypadku większości scenariuszy najlepszym wyborem jest usługa Web Apps.  W przypadku architektury mikrousługi należy wziąć pod uwagę [sieci szkieletowej usług](https://azure.microsoft.com/documentation/services/service-fabric). Jeśli potrzebujesz większej kontroli nad maszynami wirtualnymi, które działa kod, należy wziąć pod uwagę [maszyny wirtualne Azure](https://azure.microsoft.com/documentation/services/virtual-machines/). Aby uzyskać więcej informacji o tym, jak dokonać wyboru między usługami Azure, zobacz [Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison](choose-web-site-cloud-service-vm.md) (Porównanie usług Azure App Service, Virtual Machines, Service Fabric i Cloud Services).

## <a name="next-steps"></a>Następne kroki

Tworzenie pierwszej aplikacji sieci web.

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

