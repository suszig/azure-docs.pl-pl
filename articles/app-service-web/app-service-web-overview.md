<properties
    pageTitle="Przegląd usługi Web Apps | Microsoft Azure"
    description="Dowiedz się, w jaki sposób usługa Azure App Service ułatwia tworzenie i hostowanie aplikacji sieci Web"
    services="app-service\web"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/25/2016"
    ms.author="tdykstra"/>

# Przegląd usługi Web Apps

*Aplikacje Web Apps w usłudze App Service* to w pełni zarządzana platforma obliczeniowa zoptymalizowana pod kątem hostowania witryn i aplikacji sieci Web. Oferta [platforma jako usługa](https://en.wikipedia.org/wiki/Platform_as_a_service) (Platform as a Service, PaaS) platformy Microsoft Azure umożliwia użytkownikowi skupienie się na logice biznesowej, podczas gdy platforma Azure jest odpowiedzialna za infrastrukturę służącą do uruchamiania i skalowania aplikacji.

Następujący 5-minutowy film wideo stanowi wprowadzenie do aplikacji Web Apps w usłudze Azure App Service.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

## Co to jest aplikacja sieci Web w usłudze App Service?

*Aplikacja sieci Web* w usłudze App Service to zasób obliczeniowy udostępniany przez platformę Azure na potrzeby hostowania witryny lub aplikacji sieci Web.  

Zasoby obliczeniowe mogą znajdować się na współdzielonych lub dedykowanych maszynach wirtualnych — jest to zależne od wybranej warstwy cenowej. Kod aplikacji jest uruchamiany na zarządzanej maszynie wirtualnej, która jest odizolowana od innych klientów.

Kod może być tworzony przy użyciu dowolnego języka lub struktury obsługiwanej przez [usługę Azure App Service](../app-service/app-service-value-prop-what-is.md), na przykład ASP.NET, Node.js, Java, PHP lub Python. W aplikacji sieci Web można również uruchamiać skrypty używające [programu PowerShell i innych języków skryptowych](web-sites-create-web-jobs.md#acceptablefiles).

Przykłady typowych scenariuszy aplikacji, których można użyć na potrzeby usługi Web Apps, można znaleźć w artykule [Scenariusze aplikacji sieci Web](https://azure.microsoft.com/documentation/scenarios/web-app/) i w sekcji **Scenarios and recommendations** (Scenariusze i zalecenia) w artykule [Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison](choose-web-site-cloud-service-vm.md#scenarios) (Porównanie usługi Azure App Service, usługi Virtual Machines, usługi Service Fabric i usług Cloud Services).

## Dlaczego warto używać usługi Web Apps?

Poniżej przedstawiono kilka najważniejszych funkcji usługi App Service, które dotyczą usługi Web Apps:

- **Wiele języków i struktur** — usługa App Service oferuje najwyższej jakości pomoc techniczną dotyczącą rozwiązań ASP.NET, Node.js, Java, PHP i Python. Na maszynach wirtualnych usługi App Service można również uruchamiać [program PowerShell oraz inne skrypty lub pliki wykonywalne](../app-service-web/web-sites-create-web-jobs.md).

- **Optymalizacja metodyki DevOps** — konfiguruj [ciągłą integrację i wdrażanie](../app-service-web/app-service-continous-deployment.md) za pomocą usługi Visual Studio Team Services, GitHub lub BitBucket. Podwyższaj poziom aktualizacji za pośrednictwem [środowisk testowych i przejściowych](../app-service-web/web-sites-staged-publishing.md). Wykonuj [testowanie A/B](../app-service-web/app-service-web-test-in-production-get-start.md). Zarządzaj aplikacjami w usłudze App Service za pomocą programu [Azure PowerShell](../powershell-install-configure.md) lub [międzyplatformowego interfejsu wiersza polecenia](../xplat-cli-install.md).
 
- **Skala globalna i wysoka dostępność** — skaluj [w górę](../app-service/app-service-scale.md) lub [w dół](../azure-portal/insights-how-to-scale.md) ręcznie bądź automatycznie. Hostuj aplikacje w dowolnym miejscu globalnej infrastruktury centrum danych firmy Microsoft i korzystaj z wysokiej dostępności usługi App Service zapewnianej w umowie [SLA](https://azure.microsoft.com/support/legal/sla/app-service/).

- **Połączenia z platformami SaaS i danymi lokalnymi** — wybieraj spośród ponad 50 [łączników](../connectors/apis-list.md) przeznaczonych dla systemów przedsiębiorstwa (takich jak SAP, Siebel i Oracle), usług SaaS (takich jak Salesforce i Office 365) oraz usług internetowych (takich jak Facebook i Twitter). Uzyskuj dostęp do danych lokalnych przy użyciu [połączeń hybrydowych](../biztalk-services/integration-hybrid-connection-overview.md) i [sieci wirtualnych platformy Azure](../app-service-web/web-sites-integrate-with-vnet.md).

- **Bezpieczeństwo i zgodność** — usługa App Service jest [zgodna ze standardami ISO, SOC i PCI](https://www.microsoft.com/TrustCenter/).

- **Szablony aplikacji** — wybieraj z obszernej listy szablonów aplikacji dostępnych w portalu [Azure Marketplace](https://azure.microsoft.com/marketplace/), która umożliwi Ci instalowanie popularnych programów typu open source, takich jak WordPress, Joomla i Drupal, za pomocą kreatora.

- **Integracja z programem Visual Studio** —dedykowane narzędzia w programie Visual Studio usprawniają procesy tworzenia, wdrażania i debugowania.

Ponadto aplikacja sieci Web może korzystać z funkcji oferowanych przez usługi [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (na przykład obsługa mechanizmu CORS) i [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (na przykład powiadomienia wypychane). Aby uzyskać więcej informacji na temat typów aplikacji w usłudze App Service, zobacz [omówienie usługi Azure App Service](../app-service/app-service-value-prop-what-is.md).

Oprócz usługi Web Apps w usłudze App Service platforma Azure oferuje inne usługi, które mogą służyć do hostowania witryn i aplikacji sieci Web. W przypadku większości scenariuszy najlepszym wyborem jest usługa Web Apps.  W przypadku architektury mikrousługi należy wziąć pod uwagę usługę [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric) i w razie konieczności sprawowania większej kontroli nad maszynami wirtualnymi, na których działa kod, należy wziąć pod uwagę usługę [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Aby uzyskać więcej informacji o tym, jak dokonać wyboru między usługami Azure, zobacz [Azure App Service, Virtual Machines, Service Fabric, and Cloud Services comparison](choose-web-site-cloud-service-vm.md) (Porównanie usługi Azure App Service, usługi Virtual Machines, usługi Service Fabric i usług Cloud Services).

## Wprowadzenie

Aby rozpocząć od wdrożenia przykładowego kodu w nowej aplikacji sieci Web w usłudze App Service, postępuj zgodnie z informacjami znajdującymi się w samouczku [Deploy your first web app to Azure in 5 minutes](app-service-web-get-started.md) (Wdrażanie pierwszej aplikacji sieci Web na platformie Azure w ciągu 5 minut). Konieczne będzie posiadanie bezpłatnego konta platformy Azure.

Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](http://go.microsoft.com/fwlink/?LinkId=523751), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.



<!--HONumber=Jun16_HO2-->


