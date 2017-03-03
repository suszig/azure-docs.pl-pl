# Omówienie
## [Co to jest usługa Site Recovery?](site-recovery-overview.md)
## [W jaki sposób działa usługa Site Recovery?](site-recovery-components.md)
## [Jakie obciążenia można chronić?](site-recovery-workload.md)
## [Macierz obsługi usługi Site Recovery](site-recovery-support-matrix-to-azure.md)
## [Często zadawane pytania](site-recovery-faq.md)
## [Obejrzyj wprowadzenie](https://www.youtube.com/watch?v=eOOwMQPBKfM)

# Wprowadzenie
## [Replikacja maszyn wirtualnych VMware do platformy Azure](site-recovery-vmware-to-azure.md)
## [Replikacja maszyn wirtualnych VMware do platformy Azure we wdrożeniu wielodostępnym (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)
## [Replikacja maszyn wirtualnych funkcji Hyper-V do platformy Azure (z programem VMM)](site-recovery-vmm-to-azure.md)
## [Replikacja maszyn wirtualnych funkcji Hyper-V do platformy Azure](site-recovery-hyper-v-site-to-azure.md)
## [Replikacja maszyn wirtualnych VMware i serwerów fizycznych do lokacji dodatkowej](site-recovery-vmware-to-vmware.md)
## [Replikacja maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej (z programem VMM)](site-recovery-vmm-to-vmm.md)

# Instrukcje
## Planowanie
### [Wymagania wstępne dotyczące wdrożenia](site-recovery-prereq.md)
### [Zagadnienia związane z infrastrukturą sieciową](site-recovery-network-design.md)
### [Korzystanie z planisty wydajności usługi Site Recovery](site-recovery-capacity-planner.md)
### [Planowanie wydajności i skalowanie replikacji oprogramowania VMware do platformy Azure](site-recovery-plan-capacity-vmware.md)
### [Uruchamianie planisty wdrożenia na potrzeby replikacji oprogramowania VMware na platformie Azure](site-recovery-deployment-planner.md)
## Konfigurowanie
### [Konfigurowanie środowiska źródłowego](site-recovery-set-up-vmware-to-azure.md)
### [Konfigurowanie środowiska docelowego](site-recovery-prepare-target-vmware-to-azure.md)
### [Konfigurowanie ustawień replikacji](site-recovery-setup-replication-settings-vmware.md)
### [Wdrażanie usługi mobilności na potrzeby replikacji oprogramowania VMware](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Wdrażanie usługi mobilności za pomocą programu System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
#### [Wdrażanie usługi mobilności przy użyciu usługi Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
## Praca w trybie failover i powrót po awarii
### [Praca w trybie failover w usłudze Site Recovery](site-recovery-failover.md)
### [Konfigurowanie planów odzyskiwania](site-recovery-create-recovery-plans.md)
#### [Dodawanie elementów Runbook platformy Azure do planów odzyskiwania](site-recovery-runbook-automation.md)
### [Run a test failover to Azure](site-recovery-test-failover-to-azure.md) (Wykonywanie próby przejścia w tryb failover na platformę Azure)
### [Przeprowadzanie testu przejścia w tryb failover między dwiema lokacjami programu VMM](site-recovery-test-failover-vmm-to-vmm.md)
### [Powrót po awarii serwerów fizycznych i maszyn wirtualnych VMware z platformy Azure](site-recovery-how-to-failback-azure-to-vmware.md)
### [Powrót po awarii maszyn wirtualnych funkcji Hyper-V z platformy Azure](site-recovery-failback-from-azure-to-hyper-v.md)

## Migrate (Migracja)
### [Migracja na platformę Azure](site-recovery-migrate-to-azure.md)
### [Migracja między regionami platformy Azure](site-recovery-migrate-azure-to-azure.md)
### [Migracja wystąpień usługi AWS dla systemu Windows na platformę Azure](site-recovery-migrate-aws-to-azure.md)
## Obciążenia
### [Usługi Active Directory i DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-workload.md#protect-sharepoint)
### [Dynamics AX](site-recovery-workload.md#protect-dynamics-ax)
### [RDS](site-recovery-workload.md#protect-rds)
### [Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Inne obciążenia](site-recovery-workload.md#workload-summary)
## Automatyzowanie replikacji
### [Automatyzowanie replikacji funkcji Hyper-V na platformie Azure (bez programu VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [Automatyzowanie replikacji funkcji Hyper-V na platformie Azure (z programem VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [Automatyzowanie replikacji funkcji Hyper-V w lokacji dodatkowej (z programem VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Zarządzanie
### [Usuwanie serwerów i wyłączanie ochrony](site-recovery-manage-registration-and-protection.md)
### [Skalowanie w poziomie serwerów przetwarzania](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [Monitorowanie i rozwiązywanie problemów](site-recovery-monitoring-and-troubleshooting.md)

# Dokumentacja
## [Program PowerShell](/powershell/resourcemanager/azurerm.siterecovery/v3.2.0/azurerm.siterecovery)
## [PowerShell — model klasyczny](/powershell/servicemanagement/azure.siterecovery/v3.1.0/azure.siterecovery)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# Powiązane
## [Azure Automation](/azure/automation/)

# Zasoby
## [Ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Cennik](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=site-recovery)
