
# Omówienie
## [Co to jest Service Fabric?](service-fabric-overview.md)
## [Opis mikrousług](service-fabric-overview-microservices.md)
## [Scenariusze aplikacji](service-fabric-application-scenarios.md)
## [Architektura](service-fabric-architecture.md)
## [Terminologia](service-fabric-technical-overview.md)
## [Przewodnik po zawartości](service-fabric-content-roadmap.md)

# Rozpoczęcie pracy
## Konfigurowanie środowiska projektowego
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## Tworzenie pierwszej aplikacji
### [C# w systemie Windows](service-fabric-create-your-first-application-in-visual-studio.md)
### [Java w systemie Linux](service-fabric-create-your-first-linux-application-with-java.md)
### [C# w systemie Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Wdrażanie aplikacji w klastrze lokalnym](service-fabric-get-started-with-a-local-cluster.md)
## [Tworzenie klastra autonomicznego](service-fabric-get-started-standalone-cluster.md)

# Instrukcje
## Tworzenie aplikacji
### [Wzorce i scenariusze](service-fabric-patterns-and-scenarios.md)
### Podstawy
#### [Model aplikacji](service-fabric-application-model.md)
#### [Tworzenie pakietu aplikacji](service-fabric-package-apps.md)
#### [Obsługiwany model programowania](service-fabric-choose-framework.md)
#### [Stan usługi](service-fabric-concepts-state.md)
#### [Komunikacja usług](service-fabric-connect-and-communicate-with-services.md)
#### [Dodawanie frontonu sieci Web](service-fabric-add-a-web-frontend.md)
#### [Zasoby manifestu usługi](service-fabric-service-manifest-resources.md)
#### [Pierwsze kroki z wtyczką Eclipse do programowania w języku Java](service-fabric-get-started-eclipse.md)
#### [Zarządzanie aplikacjami w programie Visual Studio](service-fabric-manage-application-in-visual-studio.md)
#### [Konfigurowanie bezpiecznych połączeń w programie Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
#### Debugowanie
##### [Debugowanie usługi C# w programie VS](service-fabric-debugging-your-application.md)
##### [Debugowanie usługi Java w programie Eclipse](service-fabric-debugging-your-application-java.md)
#### Monitorowanie i diagnozowanie
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Zarządzanie wpisami tajnymi aplikacji](service-fabric-application-secret-management.md)  
#### [Konfigurowanie zasad zabezpieczeń aplikacji](service-fabric-application-runas-security.md)  
#### [Konfigurowanie aplikacji dla wielu środowisk](service-fabric-manage-multiple-environment-app-configuration.md)  
#### [Typowe błędy i wyjątki](service-fabric-errors-and-exceptions.md) 

### Aplikacja wykonywalna gościa
#### [Wdrażanie aplikacji wykonywalnej gościa](service-fabric-deploy-existing-app.md)
#### [Wdrażanie wielu aplikacji wykonywalnych gości](service-fabric-deploy-multiple-apps.md)

### Aplikacja kontenera
#### [Omówienie](service-fabric-containers-overview.md)
#### [Wdrażanie kontenera systemu Windows](service-fabric-deploy-container.md)
#### [Wdrażanie kontenera platformy Docker](service-fabric-deploy-container-linux.md)

### Aplikacja usługi Reliable Service
#### [Omówienie](service-fabric-reliable-services-introduction.md)
#### Rozpoczęcie pracy
##### [C# w systemie Windows](service-fabric-reliable-services-quick-start.md)
##### [Java w systemie Linux](service-fabric-reliable-services-quick-start-java.md)
#### [Cykl życia usług Reliable Services — C#](service-fabric-reliable-services-lifecycle.md)
#### [Cykl życia usług Reliable Services — Java](service-fabric-reliable-services-lifecycle-java.md)
#### [Niezawodne kolekcje](service-fabric-reliable-services-reliable-collections.md)
#### [Korzystanie z elementów Reliable Collections](service-fabric-work-with-reliable-collections.md)
#### [Konfigurowanie](service-fabric-reliable-services-configuration.md)
#### [Powiadomienia](service-fabric-reliable-services-notifications.md)
#### [Tworzenie kopii zapasowej i przywracanie](service-fabric-reliable-services-backup-restore.md)
#### [Komunikacja przy użyciu usług Reliable Services](service-fabric-reliable-services-communication.md)
#### [Bezpieczna komunikacja przy użyciu usług Reliable Services — C#](service-fabric-reliable-services-secure-communication.md)
#### [Bezpieczna komunikacja przy użyciu usług Reliable Services — Java](service-fabric-reliable-services-secure-communication-java.md)
##### [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)
##### [Zdalna komunikacja usług — C#](service-fabric-reliable-services-communication-remoting.md)
##### [Zdalna komunikacja usług — Java](service-fabric-reliable-services-communication-remoting-java.md)
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
#### [Zdarzenia](service-fabric-reliable-actors-events.md) 
#### [Czasomierze i przypomnienia](service-fabric-reliable-actors-timers-reminders.md)
#### [Zarządzanie stanem](service-fabric-reliable-actors-state-management.md)
#### [Konfigurowanie dostawcy KvsActorStateProvider](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Serializacja typów](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)
#### [Konfigurowanie ustawień komunikacji](service-fabric-reliable-actors-fabrictransportsettings.md) 
#### [Konfigurowanie dostawcy ReliableDictionaryActorStateProvider](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

## Migrowanie z usług Cloud Services
### [Porównanie usług Cloud Services i Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migracja do usługi Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
### [Zalecane rozwiązania](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Tworzenie klastrów i zarządzanie nimi

### Podstawy
#### [Omówienie](service-fabric-deploy-anywhere.md)
#### [Opisywanie klastra](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Planowanie wydajności](service-fabric-cluster-capacity.md)
#### [Wizualizacja klastra](service-fabric-visualizing-your-cluster.md)
#### [Nawiązywanie połączenia z zabezpieczonym klastrem](service-fabric-connect-to-secure-cluster.md)
#### [Zarządzanie klastrem przy użyciu interfejsu wiersza polecenia platformy Azure](service-fabric-azure-cli.md) 
#### [Zabezpieczanie klastra](service-fabric-cluster-security.md)
#### [Odzyskiwanie po awarii](service-fabric-disaster-recovery.md)
#### [Różnice między funkcjami systemów Linux i Windows](service-fabric-linux-windows-differences.md)

### Klastry na platformie Azure
#### Tworzenie klastra na platformie Azure
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
##### [Program Visual Studio i usługa Azure Resource Manager](service-fabric-cluster-creation-via-visual-studio.md)
#### [Wzorce sieci klastra](service-fabric-patterns-networking.md)
#### [Typy węzłów i zestawy skalowania maszyn wirtualnych](service-fabric-cluster-nodetypes.md)
#### [Skalowanie klastra](service-fabric-cluster-scale-up-down.md)
#### [Scale a cluster programmatically](service-fabric-cluster-programmatic-scaling.md) (Programowe skalowania klastra)
#### [Uaktualnianie klastra](service-fabric-cluster-upgrade.md)
#### [Usuwanie klastra](service-fabric-cluster-delete.md)
#### [Kontrola dostępu](service-fabric-cluster-security-roles.md)
#### [Konfigurowanie klastra](service-fabric-cluster-fabric-settings.md)
#### [Dodawanie lub przerzucanie certyfikatów klastra](service-fabric-cluster-security-update-certs-azure.md) 
#### [Bezpłatne wypróbowywanie klastra testowego](http://aka.ms/tryservicefabric)

### Klastry autonomiczne
#### [Planowanie i przygotowywanie wdrożenia](service-fabric-cluster-standalone-deployment-preparation.md)
#### [Zawartość pakietu autonomicznego usługi Service Fabric](service-fabric-cluster-standalone-package-contents.md)
#### [Tworzenie klastra autonomicznego](service-fabric-cluster-creation-for-windows-server.md)
#### [Tworzenie klastra autonomicznego przy użyciu usługi Azure Virtual Machines](service-fabric-cluster-creation-with-windows-azure-vms.md)
#### [Skalowanie klastra](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Uaktualnianie klastra](service-fabric-cluster-upgrade-windows-server.md)
#### [Kontrola dostępu](service-fabric-cluster-security-roles.md)
#### [Konfigurowanie klastra](service-fabric-cluster-manifest.md)
#### [Zabezpieczenia klastra przy użyciu certyfikatów](service-fabric-windows-cluster-x509-security.md)  
#### [Zabezpieczenia klastra przy użyciu zabezpieczeń systemu Windows](service-fabric-windows-cluster-windows-security.md) 

## Zarządzanie cyklem życia aplikacji
### [Omówienie](service-fabric-application-lifecycle.md)
### [Set up continuous integration with VSTS](service-fabric-set-up-continuous-integration.md) (Konfiguracja integracji ciągłej za pomocą usługi VSTS)
### [Deploy your Linux Java app using Jenkins](service-fabric-cicd-your-linux-java-application-with-jenkins.md) (Wdrażanie aplikacji Java w systemie Linux przy użyciu narzędzia Jenkins)
### [Omówienie ustawienia ImageStoreConnectionString](service-fabric-image-store-connection-string.md)
### Wdrażanie lub usuwanie aplikacji
#### [Program PowerShell](service-fabric-deploy-remove-applications.md)
#### [Program Visual Studio](service-fabric-publish-app-remote-cluster.md)
#### [Interfejsy API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)
### [Omówienie uaktualnienia aplikacji](service-fabric-application-upgrade.md)
### [Konfigurowanie uaktualnienia aplikacji](service-fabric-visualstudio-configure-upgrade.md)
### [Parametry uaktualniania aplikacji](service-fabric-application-upgrade-parameters.md)
### Uaktualnianie aplikacji
#### [Program PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Program Visual Studio](service-fabric-application-upgrade-tutorial.md)
### [Rozwiązywanie problemów z uaktualnieniami aplikacji](service-fabric-application-upgrade-troubleshooting.md)
### [Serializacja danych podczas uaktualniania aplikacji](service-fabric-application-upgrade-data-serialization.md)
### [Uaktualnianie aplikacji: tematy zaawansowane](service-fabric-application-upgrade-advanced.md)

## Inspekcja kondycji aplikacji i klastra
### [Monitorowanie kondycji usługi Service Fabric](service-fabric-health-introduction.md)
### [Tworzenie raportów i sprawdzanie kondycji usług](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Dodawanie niestandardowych raportów o kondycji](service-fabric-report-health.md)
### [Rozwiązywanie problemów z raportami kondycji systemu](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Wyświetlanie raportów o kondycji](service-fabric-view-entities-aggregated-health.md)

## Monitorowanie i diagnozowanie
### [Monitorowanie i diagnozowanie aplikacji](service-fabric-diagnostics-overview.md)
### Lokalne monitorowanie i diagnozowanie usług
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Dzienniki diagnostyczne platformy Azure
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Zbieranie dzienników z procesu usługi](service-fabric-diagnostic-collect-logs-without-an-agent.md)
### [Diagnostyka stanowych usług Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Diagnostyka w elementach Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Rozwiązywanie problemów z klastrem lokalnym](service-fabric-troubleshoot-local-cluster-setup.md)

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
#### [Przy użyciu scenariuszy testowania](service-fabric-testability-scenarios.md)
#### [Przy użyciu interfejsów API przechodzenia węzłów](service-fabric-node-transition-apis.md)
### [Testowanie obciążenia aplikacji](service-fabric-vso-load-test.md)

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

# Dokumentacja
## [PowerShell](/powershell/module/ServiceFabric/)
## [Interfejs API języka Java](/java/api/)
## [.NET](/dotnet/api/)
## [REST](/rest/api/servicefabric)

# Zasoby
## [Często zadawane pytania dotyczące usługi Service Fabric](service-fabric-common-questions.md)
## [Opcje pomocy technicznej usługi Service Fabric](service-fabric-support.md)
## [Przykładowy kod](http://aka.ms/servicefabricsamples)
## [Ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Cennik](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=service-fabric)
## [Forum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)
