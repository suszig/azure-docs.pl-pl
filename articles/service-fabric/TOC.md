# Omówienie
## [Co to jest Service Fabric?](service-fabric-overview.md)
## [Opis mikrousług](service-fabric-overview-microservices.md)
## [Scenariusze aplikacji](service-fabric-application-scenarios.md)
## [Architektura](service-fabric-architecture.md)

# Wprowadzenie
## Konfigurowanie środowiska projektowego
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## Tworzenie pierwszej aplikacji
### [C# w systemie Windows](service-fabric-create-your-first-application-in-visual-studio.md)
### [Java w systemie Linux](service-fabric-create-your-first-linux-application-with-java.md)
### [C# w systemie Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Wdrażanie aplikacji w klastrze lokalnym](service-fabric-get-started-with-a-local-cluster.md)

# Instrukcje
## Tworzenie aplikacji
### Podstawy
#### [Model programowania](service-fabric-choose-framework.md)
#### [Model aplikacji](service-fabric-application-model.md)
#### [Komunikacja usług](service-fabric-connect-and-communicate-with-services.md)
#### [Narzędzia](service-fabric-manage-application-in-visual-studio.md)
#### [Debugowanie](service-fabric-debugging-your-application.md)
#### Monitorowanie i diagnozowanie
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Konfigurowanie zasad zabezpieczeń aplikacji](service-fabric-application-runas-security.md)
#### [Konfigurowanie aplikacji dla wielu środowisk](service-fabric-manage-multiple-environment-app-configuration.md)

### Aplikacja usługi Reliable Service
#### [Omówienie](service-fabric-reliable-services-introduction.md)
#### Rozpoczęcie pracy
##### [C# w systemie Windows](service-fabric-reliable-services-quick-start.md)
##### [Java w systemie Linux](service-fabric-reliable-services-quick-start-java.md)
#### [Architektura](service-fabric-reliable-services-platform-architecture.md)
#### [Niezawodne kolekcje](service-fabric-reliable-services-reliable-collections.md)
#### [Korzystanie z elementów Reliable Collections](service-fabric-work-with-reliable-collections.md)
#### [Konfigurowanie](service-fabric-reliable-services-configuration.md)
#### [Powiadomienia](service-fabric-reliable-services-notifications.md)
#### [Tworzenie kopii zapasowej i przywracanie](service-fabric-reliable-services-backup-restore.md)
#### [Komunikacja przy użyciu usług Reliable Services](service-fabric-reliable-services-communication.md)
##### [ASP.NET](service-fabric-reliable-services-communication-webapi.md)
##### [Zdalna komunikacja usług](service-fabric-reliable-services-communication-remoting.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Odwrotny serwer proxy](service-fabric-reverseproxy.md)
#### [Użycie zaawansowane](service-fabric-reliable-services-advanced-usage.md)

### Aplikacja oparta na elemencie Reliable Actor
#### [Omówienie](service-fabric-reliable-actors-introduction.md)
#### Wprowadzenie
##### [C# w systemie Windows](service-fabric-reliable-actors-get-started.md)
##### [Java w systemie Linux](service-fabric-reliable-actors-get-started-java.md)
#### [Architektura](service-fabric-reliable-actors-platform.md)
#### [Cykl życia i odzyskiwanie pamięci](service-fabric-reliable-actors-lifecycle.md)
#### [Polimorfizm](service-fabric-reliable-actors-polymorphism.md)
#### [Ponowne wejścia](service-fabric-reliable-actors-reentrancy.md)
#### [Czasomierze i przypomnienia](service-fabric-reliable-actors-timers-reminders.md)
#### [Zdarzenia](service-fabric-reliable-actors-events.md)
#### [Zarządzanie stanem](service-fabric-reliable-actors-state-management.md)
#### [Konfigurowanie dostawcy stanu](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Serializacja typów](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

### Aplikacja wykonywalna gościa
#### [Wdrażanie aplikacji wykonywalnej gościa](service-fabric-deploy-existing-app.md)
#### [Wdrażanie wielu aplikacji wykonywalnych gości](service-fabric-deploy-multiple-apps.md)

### Aplikacja kontenera
#### [Omówienie](service-fabric-containers-overview.md)
#### [Wdrażanie kontenera systemu Windows](service-fabric-deploy-container.md)
#### [Wdrażanie kontenera platformy Docker](service-fabric-deploy-container-linux.md)

## Migrowanie z usług Cloud Services
### [Porównanie usług Cloud Services i Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migracja do usługi Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

## Tworzenie klastrów i zarządzanie nimi

### Podstawy
#### [Omówienie](service-fabric-deploy-anywhere.md)
#### [Opisywanie klastra](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Planowanie wydajności](service-fabric-cluster-capacity.md)
#### [Wizualizacja klastra](service-fabric-visualizing-your-cluster.md)
#### [Nawiązywanie połączenia z zabezpieczonym klastrem](service-fabric-connect-to-secure-cluster.md)
#### [Bezpieczeństwo](service-fabric-cluster-security.md)
#### [Odzyskiwanie po awarii](service-fabric-disaster-recovery.md)

### Klastry na platformie Azure
#### Tworzenie klastra na platformie Azure
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### [Typy węzłów i zestawy skalowania maszyn wirtualnych](service-fabric-cluster-nodetypes.md)
#### [Skalowanie klastra](service-fabric-cluster-scale-up-down.md)
#### [Uaktualnianie klastra](service-fabric-cluster-upgrade.md)
#### [Usuwanie klastra](service-fabric-cluster-delete.md)
#### [Kontrola dostępu](service-fabric-cluster-security-roles.md)
#### [Konfigurowanie klastra](service-fabric-cluster-fabric-settings.md)
#### [Bezpłatne wypróbowywanie klastra testowego](http://aka.ms/tryservicefabric)

### Klastry autonomiczne
#### [Tworzenie klastra autonomicznego](service-fabric-cluster-creation-for-windows-server.md)
#### [Skalowanie klastra](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Uaktualnianie klastra](service-fabric-cluster-upgrade-windows-server.md)
#### [Zabezpieczanie klastra](service-fabric-windows-cluster-x509-security.md)
#### [Kontrola dostępu](service-fabric-cluster-security-roles.md)
#### [Konfigurowanie klastra](service-fabric-cluster-manifest.md)

## Organizowanie zasobów klastra i zarządzanie nimi
### [Omówienie usługi Resource Manager dla klastra](service-fabric-cluster-resource-manager-introduction.md)
### [Architektura usługi Resource Manager dla klastra](service-fabric-cluster-resource-manager-architecture.md)
### [Opisywanie klastra](service-fabric-cluster-resource-manager-cluster-description.md)
### [Omówienie grup aplikacji](service-fabric-cluster-resource-manager-application-groups.md)
### [Konfigurowanie ustawień usługi Resource Manager dla klastra](service-fabric-cluster-resource-manager-configure-services.md)
### [Metryki dotyczące użycia zasobów](service-fabric-cluster-resource-manager-metrics.md)
### [Korzystanie z koligacji usług](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
### [Zasady rozmieszczania usług](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
### [Zarządzanie klastrem](service-fabric-cluster-resource-manager-management-integration.md)
### [Defragmentacja klastra](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
### [Równoważenie klastra](service-fabric-cluster-resource-manager-balancing.md)
### [Ograniczanie przepływności](service-fabric-cluster-resource-manager-advanced-throttling.md)
### [Przenoszenie usług](service-fabric-cluster-resource-manager-movement-cost.md)

## Zarządzanie cyklem życia aplikacji
### [Omówienie](service-fabric-application-lifecycle.md)
### [Konfiguracja integracji ciągłej](service-fabric-set-up-continuous-integration.md)
### Wdrażanie lub usuwanie aplikacji
#### [Program PowerShell](service-fabric-deploy-remove-applications.md)
#### [Program Visual Studio](service-fabric-publish-app-remote-cluster.md)
### [Omówienie uaktualnienia aplikacji](service-fabric-application-upgrade.md)
### [Konfigurowanie uaktualnienia aplikacji](service-fabric-visualstudio-configure-upgrade.md)
### [Parametry uaktualniania aplikacji](service-fabric-application-upgrade-parameters.md)
### Uaktualnianie aplikacji
#### [Program PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Program Visual Studio](service-fabric-application-upgrade-tutorial.md)
### [Rozwiązywanie problemów z uaktualnieniami aplikacji](service-fabric-application-upgrade-troubleshooting.md)
### [Serializacja danych podczas uaktualniania aplikacji](service-fabric-application-upgrade-data-serialization.md)
### [Uaktualnianie aplikacji: tematy zaawansowane](service-fabric-application-upgrade-advanced.md)
### [Przykład cyklu życia aplikacji opartej na protokole REST](service-fabric-rest-based-application-lifecycle-sample.md)

## Inspekcja kondycji aplikacji i klastra
### [Monitorowanie kondycji usługi Service Fabric](service-fabric-health-introduction.md)
### [Tworzenie raportów i sprawdzanie kondycji usług](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Dodawanie niestandardowych raportów o kondycji](service-fabric-report-health.md)
### [Rozwiązywanie problemów z raportami kondycji systemu](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Wyświetlanie raportów o kondycji](service-fabric-view-entities-aggregated-health.md)

## Monitorowanie i diagnozowanie
### Lokalne monitorowanie i diagnozowanie usług
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Dzienniki diagnostyczne platformy Azure
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Śledzenie aplikacji usługi Service Fabric](service-fabric-diagnostic-how-to-use-elasticsearch.md)
### [Diagnostyka w elementach Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Diagnostyka stanowych usług Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Rozwiązywanie problemów z klastrem lokalnym](service-fabric-troubleshoot-local-cluster-setup.md)
### [Rozwiązywanie typowych problemów](service-fabric-diagnostics-troubleshoot-common-scenarios.md)

## Skalowanie aplikacji
### [Partycjonowanie usług Reliable Services](service-fabric-concepts-partitioning.md)
### [Dostępność usług](service-fabric-availability-services.md)
### [Skalowanie aplikacji](service-fabric-concepts-scalability.md)
### [Planowanie pojemności aplikacji](service-fabric-capacity-planning.md)

## Testowanie aplikacji i usług
### [Omówienie analizy błędów](service-fabric-testability-overview.md)
### [Testowanie komunikacji między usługami](service-fabric-testability-scenarios-service-communication.md)
### Symulowanie błędów
#### [Przy użyciu kontrolowanego chaosu](service-fabric-controlled-chaos.md)
#### [Przy użyciu akcji testowych](service-fabric-testability-actions.md)
#### [Podczas obciążeń](service-fabric-testability-workload-tests.md)
#### [Za pomocą wywołania utraty danych](service-fabric-use-data-loss-api.md)
#### [Przy użyciu scenariuszy testowania](service-fabric-testability-scenarios.md)
### [Testowanie obciążenia aplikacji](service-fabric-vso-load-test.md)

# Dokumentacja
## [Terminologia](service-fabric-technical-overview.md)
## [Reliable Actors](https://go.microsoft.com/fwlink/p/?linkid=833398)
## [Platforma WCF usługi Reliable Actors](https://go.microsoft.com/fwlink/p/?linkid=833401)
## [Usługi Reliable Services](https://go.microsoft.com/fwlink/p/?linkid=833402)
## [Platforma WCF usługi Reliable Services](https://go.microsoft.com/fwlink/p/?linkid=833403)
## [Dane](https://go.microsoft.com/fwlink/p/?linkid=833404)
## [Interfejsy danych](https://go.microsoft.com/fwlink/p/?linkid=833406)
## [System](https://go.microsoft.com/fwlink/p/?linkid=833407)
## [Przykładowy kod](http://aka.ms/servicefabricsamples)
## [Program PowerShell](/powershell/servicefabric/vlatest/servicefabric)
## [Interfejs API języka Java](https://go.microsoft.com/fwlink/p/?linkid=833410)
## [.NET](/dotnet/api)
## [REST](/rest/api/servicefabric/)

# Zasoby
## [Ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Cennik](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=service-fabric)
## [Forum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)


<!--HONumber=Nov16_HO4-->


