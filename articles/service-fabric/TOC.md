# [Dokumentacja usługi Service Fabric](/azure/service-fabric)
# Omówienie
## [Co to jest Service Fabric?](service-fabric-overview.md)

# Przewodniki Szybki start
## [Tworzenie aplikacji .NET](service-fabric-quickstart-dotnet.md)
## [Wdrażanie aplikacji kontenera systemu Windows](service-fabric-quickstart-containers.md)
## [Wdrażanie aplikacji kontenera systemu Linux](service-fabric-quickstart-containers-linux.md)

# Samouczki
## Wdrażanie aplikacji .NET
### [1- Build a .NET application (1. Tworzenie aplikacji .NET)](service-fabric-tutorial-create-dotnet-app.md)
### [2- Deploy the application (2. Wdrażanie aplikacji)](service-fabric-tutorial-deploy-app-to-party-cluster.md)
### [3- Configure CI/CD (3. Konfigurowanie ciągłej integracji/ciągłego wdrażania)](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## Migracja aplikacji metodą „lift-and-shift”
### [1- Create a secure cluster on Azure (1. Tworzenie bezpiecznego klastra na platformie Azure)](service-fabric-tutorial-create-cluster-azure-ps.md)
### [2- Deploy a .NET app using Docker Compose (2. Wdrażanie aplikacji .NET przy użyciu narzędzia Docker Compose)](service-fabric-host-app-in-a-container.md)

# Przykłady
## [Przykłady kodu](https://azure.microsoft.com/en-us/resources/samples/?service=service-fabric)
## [PowerShell](service-fabric-powershell-samples.md)
## [Interfejs wiersza polecenia usługi Service Fabric](samples-cli.md)
# Pojęcia
## [Opis mikrousług](service-fabric-overview-microservices.md)
## [Duży obraz](service-fabric-content-roadmap.md)
## [Scenariusze aplikacji](service-fabric-application-scenarios.md)
## [Wzorce i scenariusze](service-fabric-patterns-and-scenarios.md)
## [Architektura](service-fabric-architecture.md)
## [Terminologia](service-fabric-technical-overview.md)

## Tworzenie aplikacji i usług
### Obsługiwane modele programowania
#### [Omówienie](service-fabric-choose-framework.md)
#### Kontenery
##### [Omówienie](service-fabric-containers-overview.md)
##### [Docker Compose (wersja zapoznawcza)](service-fabric-docker-compose.md)
##### [Resource governance (Zarządzanie zasobami)](service-fabric-resource-governance.md)
#### Reliable Services
##### [Omówienie](service-fabric-reliable-services-introduction.md)
##### [Cykl życia usług Reliable Services — C#](service-fabric-reliable-services-lifecycle.md)
##### [Cykl życia usług Reliable Services — Java](service-fabric-reliable-services-lifecycle-java.md)
##### [Elementy Reliable Collections](service-fabric-reliable-services-reliable-collections.md)
##### [Wytyczne i zalecenia dotyczące elementów Reliable Collections](service-fabric-reliable-services-reliable-collections-guidelines.md)
##### [Praca z elementami Reliable Collections](service-fabric-work-with-reliable-collections.md)
##### [Transakcje i blokady](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
##### [Niezawodna kolejka współbieżna](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Serializacja elementu Reliable Collection](service-fabric-reliable-services-reliable-collections-serialization.md)
##### [Elementy Reliable State Manager i Reliable Collection od środka](service-fabric-reliable-services-reliable-collections-internals.md)
##### [Użycie zaawansowane](service-fabric-reliable-services-advanced-usage.md)

#### Reliable Actors
##### [Omówienie](service-fabric-reliable-actors-introduction.md)
##### [Architektura](service-fabric-reliable-actors-platform.md)
##### [Cykl życia i odzyskiwanie pamięci](service-fabric-reliable-actors-lifecycle.md)
##### [Zarządzanie stanem](service-fabric-reliable-actors-state-management.md)
##### [Polimorfizm](service-fabric-reliable-actors-polymorphism.md)
##### [Ponowne wejścia](service-fabric-reliable-actors-reentrancy.md)
##### [Serializacja typów](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

### [Model aplikacji](service-fabric-application-model.md)
### [Model hostingu](service-fabric-hosting-model.md)

### Usługi
#### [Service resources (Zasoby usługi)](service-fabric-service-manifest-resources.md)
#### [Stan usługi](service-fabric-concepts-state.md)
#### [Partycjonowanie usługi](service-fabric-concepts-partitioning.md)
#### [Dostępność usług](service-fabric-availability-services.md)
#### Komunikacja usług
##### [Omówienie](service-fabric-connect-and-communicate-with-services.md)
##### [Usługa DNS](service-fabric-dnsservice.md)
##### [Odwrotny serwer proxy](service-fabric-reverseproxy.md)
##### [Konfigurowanie zwrotnego serwera proxy na potrzeby bezpiecznej komunikacji](service-fabric-reverseproxy-configure-secure-communication.md)
##### [Reverse proxy diagnostics (Diagnostyka zwrotnego serwera proxy)](service-fabric-reverse-proxy-diagnostics.md)
### [Skalowalność aplikacji](service-fabric-concepts-scalability.md)
### [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)

### [Plan application capacity (Planowanie pojemności aplikacji)](service-fabric-capacity-planning.md)

## Zarządzanie aplikacjami
### [Omówienie](service-fabric-application-lifecycle.md)
### [The ImageStoreConnectionString setting (Ustawienie ImageStoreConnectionString)](service-fabric-image-store-connection-string.md)
### Uaktualnienie aplikacji
#### [Omówienie](service-fabric-application-upgrade.md)
#### [Konfiguracja](service-fabric-visualstudio-configure-upgrade.md)
#### [Parametry uaktualniania aplikacji](service-fabric-application-upgrade-parameters.md)
#### [Serializacja danych podczas uaktualniania aplikacji](service-fabric-application-upgrade-data-serialization.md)
#### [Uaktualnianie aplikacji: tematy zaawansowane](service-fabric-application-upgrade-advanced.md)
### [Omówienie analizy błędów](service-fabric-testability-overview.md)

## Tworzenie klastrów i zarządzanie nimi
### [Omówienie](service-fabric-deploy-anywhere.md)
### [Service Fabric on Linux (Usługa Service Fabric w systemie Linux)](service-fabric-linux-overview.md)
### Planowanie i przygotowanie
#### [Planowanie wydajności](service-fabric-cluster-capacity.md)
#### [Odzyskiwanie po awarii](service-fabric-disaster-recovery.md)
### [Describing a cluster (Opisywanie klastra)](service-fabric-cluster-resource-manager-cluster-description.md)
### [Zabezpieczenia klastra](service-fabric-cluster-security.md)
### [Różnice między funkcjami systemów Linux i Windows](service-fabric-linux-windows-differences.md)
### Klastry na platformie Azure
#### [Typy węzłów i zestawy skalowania maszyn wirtualnych](service-fabric-cluster-nodetypes.md)
#### [Wzorce sieci klastra](service-fabric-patterns-networking.md)
### Menedżer zasobów klastra
#### [Omówienie](service-fabric-cluster-resource-manager-introduction.md)
#### [Architektura](service-fabric-cluster-resource-manager-architecture.md)
#### [Opisywanie klastra](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Omówienie grup aplikacji](service-fabric-cluster-resource-manager-application-groups.md)
#### [Konfigurowanie ustawień usługi Resource Manager dla klastra](service-fabric-cluster-resource-manager-configure-services.md)
#### [Metryki dotyczące użycia zasobów](service-fabric-cluster-resource-manager-metrics.md)
#### [Korzystanie z koligacji usług](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
#### [Zasady rozmieszczania usług](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
#### [Zarządzanie klastrem](service-fabric-cluster-resource-manager-management-integration.md)
#### [Defragmentacja klastra](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
#### [Równoważenie klastra](service-fabric-cluster-resource-manager-balancing.md)
#### [Ograniczanie przepływności](service-fabric-cluster-resource-manager-advanced-throttling.md)
#### [Przenoszenie usług](service-fabric-cluster-resource-manager-movement-cost.md)

## [Integrowanie z usługą API Management](service-fabric-api-management-overview.md)

## Monitorowanie i diagnozowanie
### [Omówienie](service-fabric-diagnostics-overview.md)
### [Health model (Model kondycji)](service-fabric-health-introduction.md)
### [Diagnostyka stanowych usług Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Diagnostyka w elementach Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Liczniki wydajności dla komunikacji zdalnej z usługami Reliable Services](service-fabric-reliable-serviceremoting-diagnostics.md)

# Przewodniki z instrukcjami
## Konfigurowanie środowiska projektowego
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)

## Tworzenie aplikacji
### [Tworzenie pierwszej aplikacji w języku C# w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
### Tworzenie usługi wykonywalnej gościa
#### [Host a Node.js application on Windows (Hostowanie aplikacji Node.js w systemie Windows)](quickstart-guest-app.md)
#### [Wdrażanie aplikacji wykonywalnej gościa](service-fabric-deploy-existing-app.md)
#### [Wdrażanie wielu aplikacji wykonywalnych gości](service-fabric-deploy-multiple-apps.md)
### Tworzenie usługi kontenera
#### [Create a Windows container application (Tworzenie aplikacji kontenera systemu Windows)](service-fabric-get-started-containers.md)
#### [Create a Linux container application (Tworzenie aplikacji kontenera systemu Linux)](service-fabric-get-started-containers-linux.md)
#### [Container security (Zabezpieczenia kontenerów)](service-fabric-securing-containers.md)
#### [Docker Compose (wersja zapoznawcza)](service-fabric-docker-compose.md)
#### [Nadzór nad zasobami dla kontenerów i usług](service-fabric-resource-governance.md)
#### [Sterowniki woluminów i rejestrowania](service-fabric-containers-volume-logging-drivers.md)
#### [Services inside containers (Usługi w kontenerach)](service-fabric-services-inside-containers.md)
#### [Container networking modes (Tryby sieci kontenera)](service-fabric-networking-modes.md)

### Tworzenie usługi Reliable Services
#### [Omówienie](service-fabric-reliable-services-introduction.md)
#### Pojęcia
##### [Cykl życia usług Reliable Services — C#](service-fabric-reliable-services-lifecycle.md)
##### [Cykl życia usług Reliable Services — Java](service-fabric-reliable-services-lifecycle-java.md)

#### Elementy Reliable Collections
##### [Elementy Reliable Collections](service-fabric-reliable-services-reliable-collections.md)
##### [Wytyczne i zalecenia dotyczące elementów Reliable Collections](service-fabric-reliable-services-reliable-collections-guidelines.md)
##### [Praca z elementami Reliable Collections](service-fabric-work-with-reliable-collections.md)
##### [Transakcje i blokady](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
##### [Niezawodna kolejka współbieżna](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Serializacja elementu Reliable Collection](service-fabric-reliable-services-reliable-collections-serialization.md)
##### [Elementy Reliable State Manager i Reliable Collection od środka](service-fabric-reliable-services-reliable-collections-internals.md)

#### Rozpoczęcie pracy
##### [C# w systemie Windows](service-fabric-reliable-services-quick-start.md)
##### [Java w systemie Linux](service-fabric-reliable-services-quick-start-java.md)
##### [Create C# application on Linux (Tworzenie aplikacji w języku C# w systemie Linux)](service-fabric-create-your-first-linux-application-with-csharp.md)
#### Komunikacja z usługami
##### [Komunikacja przy użyciu usług Reliable Services](service-fabric-reliable-services-communication.md)

##### [Zdalna komunikacja usług — C#](service-fabric-reliable-services-communication-remoting.md)
##### [Zdalna komunikacja usług — Java](service-fabric-reliable-services-communication-remoting-java.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Zabezpieczanie komunikacji — C#](service-fabric-reliable-services-secure-communication.md)
##### [Zabezpieczanie komunikacji — Java](service-fabric-reliable-services-secure-communication-java.md)

#### [Konfigurowanie](service-fabric-reliable-services-configuration.md)
#### [Wysyłanie powiadomień](service-fabric-reliable-services-notifications.md)
#### [Tworzenie kopii zapasowej i przywracanie](service-fabric-reliable-services-backup-restore.md)

### Tworzenie usługi Reliable Actors
#### Rozpoczęcie pracy
##### [C# w systemie Windows](service-fabric-reliable-actors-get-started.md)
##### [Java w systemie Linux](service-fabric-reliable-actors-get-started-java.md)
##### [Java Actor on Linux (Aktor Java w systemie Linux)](service-fabric-create-your-first-linux-application-with-java.md)
#### [Wysyłanie powiadomień](service-fabric-reliable-actors-events.md)
#### [Ustawianie czasomierzy i przypomnień](service-fabric-reliable-actors-timers-reminders.md)
#### [Konfigurowanie dostawcy KvsActorStateProvider](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Konfigurowanie ustawień komunikacji](service-fabric-reliable-actors-fabrictransportsettings.md)
#### [Konfigurowanie dostawcy ReliableDictionaryActorStateProvider](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

### [Migrate old Java Application to support Maven (Migrowanie starej aplikacji Java w celu obsługi narzędzia Maven)](service-fabric-migrate-old-javaapp-to-use-maven.md)

### [Konfigurowanie zwrotnego serwera proxy na potrzeby bezpiecznej komunikacji](service-fabric-reverseproxy-configure-secure-communication.md)

### [Build an ASP.NET Core service (Tworzenie usługi ASP.NET Core)](service-fabric-add-a-web-frontend.md)

### Konfigurowanie zabezpieczeń
#### [Zarządzanie wpisami tajnymi aplikacji](service-fabric-application-secret-management.md)  
#### [Konfigurowanie zasad zabezpieczeń aplikacji](service-fabric-application-runas-security.md)

## Praca w środowisku deweloperskim systemu Windows
### [Zarządzanie aplikacjami w programie Visual Studio](service-fabric-manage-application-in-visual-studio.md)
### [Konfigurowanie bezpiecznych połączeń w programie Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
### [Konfigurowanie aplikacji dla wielu środowisk](service-fabric-manage-multiple-environment-app-configuration.md)
### [Debug a .NET service in VS (Debugowanie usługi .NET w programie VS)](service-fabric-debugging-your-application.md)
### [Typowe błędy i wyjątki](service-fabric-errors-and-exceptions.md)
### [Lokalne monitorowanie i diagnozowanie](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

## Praca w środowisku deweloperskim systemu Linux
### [Pierwsze kroki z wtyczką Eclipse do programowania w języku Java](service-fabric-get-started-eclipse.md)
### [Debugowanie usługi Java w programie Eclipse](service-fabric-debugging-your-application-java.md)
### [Lokalne monitorowanie i diagnozowanie](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## [Wdrażanie usług API Management i Service Fabric na platformie Azure](service-fabric-api-management-quick-start.md)

## Migrowanie z usług Cloud Services
### [Porównanie usług Cloud Services i Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migracja do usługi Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
### [Zalecane praktyki](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Zarządzanie cyklem życia aplikacji
### [Tworzenie pakietu aplikacji](service-fabric-package-apps.md)

### Wdrażanie lub usuwanie aplikacji
#### [Wdrażanie aplikacji w klastrze lokalnym](service-fabric-get-started-with-a-local-cluster.md)
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Interfejs wiersza polecenia usługi Service Fabric](service-fabric-application-lifecycle-sfctl.md)
#### [Program Visual Studio](service-fabric-publish-app-remote-cluster.md)
#### [Interfejsy API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)

### Uaktualnianie aplikacji
#### [Uaktualnianie za pomocą programu PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Upgrade using Visual Studio (Uaktualnianie przy użyciu programu Visual Studio)](service-fabric-application-upgrade-tutorial.md)
#### [Rozwiązywanie problemów z uaktualnieniami aplikacji](service-fabric-application-upgrade-troubleshooting.md)

### Testowanie aplikacji i usług
#### [Testowanie komunikacji między usługami](service-fabric-testability-scenarios-service-communication.md)
#### Symulowanie błędów
##### [Przy użyciu kontrolowanego chaosu](service-fabric-controlled-chaos.md)
##### [Przy użyciu akcji testowych](service-fabric-testability-actions.md)
##### [Podczas obciążeń](service-fabric-testability-workload-tests.md)
##### [Przy użyciu scenariuszy testowania](service-fabric-testability-scenarios.md)
##### [Przy użyciu interfejsów API przechodzenia węzłów](service-fabric-node-transition-apis.md)
#### [Testowanie obciążenia aplikacji](service-fabric-vso-load-test.md)

### Konfiguracja integracji ciągłej
#### [Konfiguracja integracji ciągłej za pomocą usługi VSTS](service-fabric-set-up-continuous-integration.md)
#### [Deploy your Linux Java application using Jenkins (Wdrażanie aplikacji Java systemu Linux przy użyciu narzędzia Jenkins)](service-fabric-cicd-your-linux-java-application-with-jenkins.md)

## Tworzenie klastrów i zarządzanie nimi
### Klastry na platformie Azure
#### Przycisk Utwórz
##### [Tworzenie pierwszego klastra na platformie Azure](service-fabric-get-started-azure-cluster.md)
##### [Witryna Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### Skalowanie
##### [Ręczne](service-fabric-cluster-scale-up-down.md)
##### [Programistyczne](service-fabric-cluster-programmatic-scaling.md)
#### [Uaktualnienie](service-fabric-cluster-upgrade.md)
#### [Ustawianie kontroli dostępu](service-fabric-cluster-security-roles.md)
#### [Konfigurowanie](service-fabric-cluster-fabric-settings.md)
#### [Otwieranie portu w module równoważenia obciążenia](create-load-balancer-rule.md)
#### [Zarządzanie certyfikatami klastra](service-fabric-cluster-security-update-certs-azure.md)
#### [Usuwanie](service-fabric-cluster-delete.md)

### Klastry autonomiczne
#### [Planowanie i przygotowywanie wdrożenia](service-fabric-cluster-standalone-deployment-preparation.md)
#### Przycisk Utwórz
##### [Tworzenie pierwszego autonomicznego klastra](service-fabric-get-started-standalone-cluster.md)
##### [Tworzenie lokalnie](service-fabric-cluster-creation-for-windows-server.md)
##### [Zabezpieczenie przy użyciu certyfikatów](service-fabric-windows-cluster-x509-security.md)  
##### [Zabezpieczenie przy użyciu zabezpieczeń systemu Windows](service-fabric-windows-cluster-windows-security.md)
##### [Zawartość pakietu autonomicznego](service-fabric-cluster-standalone-package-contents.md)
#### [Skalowanie](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Ustawianie kontroli dostępu](service-fabric-cluster-security-roles.md)
#### [Konfigurowanie](service-fabric-cluster-manifest.md)
#### [Uaktualnienie](service-fabric-cluster-upgrade-windows-server.md)

### [Wizualizacja klastra](service-fabric-visualizing-your-cluster.md)
### [Nawiązywanie połączenia z zabezpieczonym klastrem](service-fabric-connect-to-secure-cluster.md)

### [Getting started with Service Fabric CLI (Wprowadzenie do interfejsu wiersza polecenia usługi Service Fabric)](service-fabric-cli.md)
### [Stosowanie poprawek do węzłów klastra](service-fabric-patch-orchestration-application.md)

### Organizowanie zasobów klastra i zarządzanie nimi
#### [Omówienie usługi Resource Manager dla klastra](service-fabric-cluster-resource-manager-introduction.md)
#### [Architektura usługi Resource Manager dla klastra](service-fabric-cluster-resource-manager-architecture.md)
#### [Opisywanie klastra](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Omówienie grup aplikacji](service-fabric-cluster-resource-manager-application-groups.md)
#### [Konfigurowanie ustawień usługi Resource Manager dla klastra](service-fabric-cluster-resource-manager-configure-services.md)
#### [Metryki dotyczące użycia zasobów](service-fabric-cluster-resource-manager-metrics.md)
#### [Korzystanie z koligacji usług](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
#### [Zasady rozmieszczania usług](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
#### [Zarządzanie klastrem](service-fabric-cluster-resource-manager-management-integration.md)
#### [Defragmentacja klastra](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
#### [Równoważenie klastra](service-fabric-cluster-resource-manager-balancing.md)
#### [Ograniczanie przepływności](service-fabric-cluster-resource-manager-advanced-throttling.md)
#### [Przenoszenie usług](service-fabric-cluster-resource-manager-movement-cost.md)

## Monitorowanie i diagnozowanie
### [Monitorowanie i diagnozowanie aplikacji](service-fabric-diagnostics-overview.md)
### Generowanie zdarzeń
#### [Generate platform level events (Generowanie zdarzeń na poziomie platformy)](service-fabric-diagnostics-event-generation-infra.md)
##### [Operational channel (Kanał operacyjny)](service-fabric-diagnostics-event-generation-operational.md)
##### [Zdarzenia interfejsu Reliable Services](service-fabric-reliable-services-diagnostics.md)
##### [Zdarzenia dotyczące elementów Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
##### [Performance metrics (Metryki wydajności)](service-fabric-diagnostics-event-generation-perf.md)
#### [Generowanie zdarzeń na poziomie aplikacji](service-fabric-diagnostics-event-generation-app.md)
### Inspekcja kondycji aplikacji i klastra
#### [Monitorowanie kondycji usługi Service Fabric](service-fabric-health-introduction.md)
#### [Tworzenie raportów i sprawdzanie kondycji usług](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
#### [Dodawanie niestandardowych raportów o kondycji](service-fabric-report-health.md)
#### [Rozwiązywanie problemów z raportami kondycji systemu](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
#### [Wyświetlanie raportów o kondycji](service-fabric-view-entities-aggregated-health.md)
#### [Monitorowanie kontenerów systemu Windows Server](service-fabric-diagnostics-containers-windowsserver.md)
### Agregowanie zdarzeń
#### [Agregowanie zdarzeń przy użyciu struktury EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)
#### Agregowanie zdarzeń przy użyciu Diagnostyki Azure
##### [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
##### [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
### Analiza zdarzeń
#### [Analizowanie zdarzeń za pomocą usługi Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
#### [Analizowanie zdarzeń za pomocą pakietu OMS](service-fabric-diagnostics-event-analysis-oms.md)
### [Rozwiązywanie problemów z klastrem lokalnym](service-fabric-troubleshoot-local-cluster-setup.md)

# Dokumentacja
## [PowerShell (Azure)](/powershell/module/azurerm.servicefabric/)
## [PowerShell](/powershell/module/servicefabric/?view=azureservicefabricps)
## [Interfejs wiersza polecenia platformy Azure](/cli/azure/sf)
## [Interfejs API języka Java](/java/api/overview/azure/servicefabric)
## [.NET](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet)
## [REST](/rest/api/servicefabric)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/)
## [Często zadawane pytania](service-fabric-common-questions.md)
## [Ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Forum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Cennik](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
## [Przykładowy kod](http://aka.ms/servicefabricsamples)
## [Opcje pomocy technicznej](service-fabric-support.md)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=service-fabric)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)
