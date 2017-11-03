# Architektura

## Podstawy chmury

### [Projektowanie wytrzymałych aplikacji](guidance-resiliency-overview.md)
#### [Lista kontrolna odporności](guidance-resiliency-checklist.md)
#### [Analiza trybu błędu](guidance-resiliency-failure-mode-analysis.md)
#### [Odzyskiwanie po awarii](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Odzyskiwanie po awarii i wysoka dostępność](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Wysoka dostępność](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Lista kontrolna wysokiej dostępności](..\resiliency\resiliency-high-availability-checklist.md)
#### [Wskazówki dotyczące odporności specyficznej dla usługi platformy Azure](..\resiliency\resiliency-service-guidance-index.md)
#### [Odzyskiwanie po uszkodzeniu lub przypadkowym usunięciu danych](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Odzyskiwanie po błędach lokalnych](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Odzyskiwanie po obejmujących cały region zakłóceniach usługi](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Odzyskiwanie z zasobów lokalnych na platformę Azure](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Odporność platformy Azure — wskazówki techniczne](..\resiliency\resiliency-technical-guidance.md)


## Architektury referencyjne

### [Uruchamianie obciążeń maszyn wirtualnych na platformie Azure](guidance-ra-compute.md)
#### [Uruchamianie maszyny wirtualnej z systemem Linux na platformie Azure](guidance-compute-single-vm-linux.md)
#### [Uruchamianie maszyny wirtualnej z systemem Windows na platformie Azure](guidance-compute-single-vm.md)
#### [Uruchamianie wielu maszyn wirtualnych na platformie Azure w celu zapewnienia skalowalności i dostępności](guidance-compute-multi-vm.md)
#### [Uruchamianie maszyn wirtualnych z systemem Linux dla architektury wielowarstwowej](guidance-compute-n-tier-vm-linux.md)
#### [Uruchamianie maszyn wirtualnych z systemem Windows dla architektury wielowarstwowej](guidance-compute-n-tier-vm.md)
#### [Uruchamianie maszyn wirtualnych z systemem Linux w wielu regionach w celu zapewnienia wysokiej dostępności](guidance-compute-multiple-datacenters-linux.md)
#### [Uruchamianie maszyn wirtualnych z systemem Windows w wielu regionach w celu zapewnienia wysokiej dostępności](guidance-compute-multiple-datacenters.md)

### [Łączenie sieci lokalnej z platformą Azure](guidance-ra-hybrid-networking.md)
#### [Architektura sieci hybrydowej z usługą Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
#### [Architektura sieci hybrydowej z platformą Azure i lokalną siecią VPN](guidance-hybrid-network-vpn.md)
#### [Wysoko dostępna hybrydowa architektura sieci](guidance-hybrid-network-expressroute-vpn-failover.md)

### [Ochrona granic chmury na platformie Azure](guidance-ra-network-security.md)
#### [Bezpieczna hybrydowa architektura sieci na platformie Azure](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [Strefa DMZ między platformą Azure i Internetem](guidance-iaas-ra-secure-vnet-dmz.md)

### [Zarządzanie tożsamością na platformie Azure](guidance-ra-identity.md)
#### [Implementowanie usługi Azure Active Directory](guidance-identity-aad.md)
#### [Rozszerzanie usług ADDS na platformę Azure](guidance-identity-adds-extend-domain.md)
#### [Tworzenie lasu zasobów usług ADDS na platformie Azure](guidance-identity-adds-resource-forest.md)
#### [Implementowanie usług ADFS na platformie Azure](guidance-identity-adfs.md)

### [Architektury aplikacji sieci Web dla usługi Azure App Service](guidance-ra-app-service.md)
#### [Podstawowa aplikacja sieci Web](guidance-web-apps-basic.md)
#### [Aplikacja sieci Web o wysokiej dostępności](guidance-web-apps-multi-region.md)
#### [Ulepszanie skalowalności w aplikacji sieci Web](guidance-web-apps-scalability.md)


## Najlepsze rozwiązania dla aplikacji w chmurze

### [Wskazówki dotyczące projektowania interfejsów API](..\best-practices-api-design.md)
### [Wskazówki dotyczące implementowania interfejsów API](..\best-practices-api-implementation.md)
### [Wskazówki dotyczące skalowania automatycznego](..\best-practices-auto-scaling.md)
### [Lista kontrolna dotycząca dostępności](..\best-practices-availability-checklist.md)
### [Wskazówki dotyczące zadań w tle](..\best-practices-background-jobs.md)
### [Sparowane regiony platformy Azure](..\best-practices-availability-paired-regions.md)
### [Wskazówki dotyczące buforowania](..\best-practices-caching.md)
### [Wskazówki dotyczące usługi Content Delivery Network](..\best-practices-cdn.md)
### [Wskazówki dotyczące partycjonowania danych](..\best-practices-data-partitioning.md)
### [Wskazówki dotyczące monitorowania i diagnostyki](..\best-practices-monitoring.md)
### [Zabezpieczenia usług w chmurze i sieci firmy Microsoft](..\best-practices-network-security.md)
### [Wzorce projektowania szablonów usługi Azure Resource Manager](..\best-practices-resource-manager-design-templates.md)
### [Zalecane konwencje nazewnictwa dla zasobów platformy Azure](guidance-naming-conventions.md)
### [Udostępnianie stanu w szablonach usługi Azure Resource Manager](..\best-practices-resource-manager-state.md)
### [Ogólne wskazówki dotyczące ponawiania próby](..\best-practices-retry-general.md)
### [Wskazówki dotyczące ponawiania próby uruchomienia określonych usług](..\best-practices-retry-service-specific.md)
### [Lista kontrolna skalowalności](..\best-practices-scalability-checklist.md)


## Wytyczne scenariusza

### [Wskazówki dotyczące usługi Elasticsearch na platformie Azure](guidance-elasticsearch.md)
#### [Uruchamianie usługi Elasticsearch na platformie Azure](guidance-elasticsearch-running-on-azure.md)
#### [Dostosowywanie wydajności pozyskiwania danych](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [Dostosowywanie wydajności agregowania danych i zapytań](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Konfigurowanie odporności i odzyskiwania](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Tworzenie środowiska testowania wydajności](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [Implementowanie planu testów JMeter](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [Wdrażanie próbnika JUnit JMeter](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [Uruchamianie zautomatyzowanych testów odporności](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [Uruchamianie zautomatyzowanych testów wydajności](guidance-elasticsearch-running-automated-performance-tests.md)

### [Zarządzanie tożsamościami dla aplikacji wielodostępnych](guidance-multitenant-identity.md)
#### [Omówienie](guidance-multitenant-identity-intro.md)
#### [Aplikacja Tailspin Surveys](guidance-multitenant-identity-tailspin.md)
#### [Uwierzytelnianie za pomocą usługi Azure AD i OpenID Connect](guidance-multitenant-identity-authenticate.md)
#### [Tożsamości oparte na oświadczeniach](guidance-multitenant-identity-claims.md)
#### [Rejestrowanie i dołączanie dzierżawy](guidance-multitenant-identity-signup.md)
#### [Role aplikacji](guidance-multitenant-identity-app-roles.md)
#### [Uwierzytelnianie oparte na rolach i zasobach](guidance-multitenant-identity-authorize.md)
#### [Zabezpieczanie interfejsu API sieci Web zaplecza](guidance-multitenant-identity-web-api.md)
#### [Tokeny dostępu pamięci podręcznej](guidance-multitenant-identity-token-cache.md)
#### [Federacja z usługami AD FS klienta](guidance-multitenant-identity-adfs.md)
#### [Uzyskiwanie tokenów dostępu za pomocą potwierdzenia klienta](guidance-multitenant-identity-client-assertion.md)
#### [Korzystanie z usługi Azure Key Vault do ochrony wpisu tajnego aplikacji](guidance-multitenant-identity-keyvault.md)
#### [Wdrażanie urządzeń wirtualnych o wysokiej dostępności](guidance-nva-ha.md)
# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
