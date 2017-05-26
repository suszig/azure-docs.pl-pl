# Omówienie
## [Co to jest usługa Site Recovery?](site-recovery-overview.md)
## [W jaki sposób działa usługa Site Recovery?](site-recovery-components.md)
## [Jak działa replikacja funkcji Hyper-V na platformie Azure?](site-recovery-hyper-v-azure-architecture.md)
## [Jakie obciążenia można chronić?](site-recovery-workload.md)
## [Macierz obsługi usługi Site Recovery](site-recovery-support-matrix-to-azure.md)
## [Często zadawane pytania](site-recovery-faq.md)
## [Obejrzyj wprowadzenie](https://azure.microsoft.com/resources/videos/index/?services=site-recovery)

# Wprowadzenie
## [Replikacja maszyn wirtualnych VMware do platformy Azure](site-recovery-vmware-to-azure.md)
## [Replikacja serwerów fizycznych do platformy Azure](site-recovery-physical-servers-to-azure.md)
## [Replikacja maszyn wirtualnych funkcji Hyper-V do platformy Azure (z programem VMM)](site-recovery-vmm-to-azure.md)
## [Replikacja maszyn wirtualnych funkcji Hyper-V do platformy Azure](site-recovery-hyper-v-site-to-azure.md)
## [Replikacja maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej (z programem VMM)](site-recovery-vmm-to-vmm.md)
## [Replikacja maszyn wirtualnych VMware i serwerów fizycznych do lokacji dodatkowej](site-recovery-vmware-to-vmware.md)
## [Replikacja maszyn wirtualnych VMware do platformy Azure we wdrożeniu wielodostępnym (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)

# Instrukcje
## Planowanie
### [Wymagania wstępne dla replikacji platformy Azure](site-recovery-prereq.md)
### [Planowanie infrastruktury sieci](site-recovery-network-design.md)
### [Planowanie mapowania sieci](site-recovery-network-mapping.md)
### [Planowanie wydajności i skalowanie replikacji oprogramowania VMware do platformy Azure](site-recovery-plan-capacity-vmware.md)
### [Planista wdrożenia na potrzeby replikacji oprogramowania VMware na platformie Azure](site-recovery-deployment-planner.md)
### [Planista wydajności na potrzeby replikacji funkcji Hyper-V](site-recovery-capacity-planner.md)
### [Kontrolowanie replikowania maszyny wirtualnej przy użyciu dostępu opartego na rolach](site-recovery-role-based-linked-access-control.md)

## Konfigurowanie
### [Konfigurowanie środowiska źródłowego](site-recovery-set-up-vmware-to-azure.md)
### [Konfigurowanie środowiska docelowego](site-recovery-prepare-target-vmware-to-azure.md)
### [Konfigurowanie ustawień replikacji](site-recovery-setup-replication-settings-vmware.md)
### [Wdrażanie usługi mobilności na potrzeby replikacji oprogramowania VMware](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Wdrażanie usługi mobilności za pomocą programu System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
#### [Wdrażanie usługi mobilności przy użyciu usługi Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
### [Włączanie replikacji](site-recovery-replicate-vmware-to-azure.md)
## Przechodzenie do trybu failover i powrót po awarii
### [Przełączanie chronionych maszyn w tryb failover](site-recovery-failover.md)
### [Konfigurowanie planów odzyskiwania](site-recovery-create-recovery-plans.md)
#### [Dodawanie elementów Runbook platformy Azure do planów odzyskiwania](site-recovery-runbook-automation.md)
### [Wykonywanie testu przejścia w tryb failover](site-recovery-test-failover-to-azure.md)
### [Ponowne włączanie ochrony maszyn po zakończeniu pracy w trybie failover](site-recovery-how-to-reprotect.md)
### [Powrót po awarii z platformy Azure](site-recovery-failback-azure-to-vmware.md)

## Migracja
### [Migracja na platformę Azure](site-recovery-migrate-to-azure.md)
### [Migracja między regionami platformy Azure](site-recovery-migrate-azure-to-azure.md)
### [Migracja wystąpień usługi AWS dla systemu Windows na platformę Azure](site-recovery-migrate-aws-to-azure.md)
## Obciążenia
### [Usługi Active Directory i DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-sharepoint.md)
### [Dynamics AX](site-recovery-dynamicsax.md)
### [RDS](site-recovery-workload.md#protect-rds)
### [Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Aplikacje internetowe bazujące na usłudze IIS](site-recovery-iis.md)
### [Citrix XenApp i XenDesktop](site-recovery-citrix-xenapp-and-xendesktop.md)
### [Inne obciążenia](site-recovery-workload.md#workload-summary)
## Automatyzowanie replikacji
### [Automatyzowanie replikacji funkcji Hyper-V na platformie Azure (bez programu VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [Automatyzowanie replikacji funkcji Hyper-V na platformie Azure (z programem VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [Automatyzowanie replikacji funkcji Hyper-V w lokacji dodatkowej (z programem VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Zarządzanie
### [Edit replication settings](site-recovery-setup-replication-settings-vmware.md#edit-replication-policy.md) (Edytowanie ustawień replikacji)
### [Zarządzanie serwerami przetwarzania na platformie Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md)
### [Zarządzanie serwerem konfiguracji](site-recovery-vmware-to-azure-manage-configuration-server.md)
### [Zarządzanie serwerami przetwarzania skalowanymi w poziomie](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [Zarządzanie serwerami vCenter](site-recovery-vmware-to-azure-manage-vCenter.md)
### [Usuwanie serwerów i wyłączanie ochrony](site-recovery-manage-registration-and-protection.md)
## [Monitorowanie i rozwiązywanie problemów](site-recovery-monitoring-and-troubleshooting.md)

# Dokumentacja
## [Program PowerShell](/powershell/module/azurerm.siterecovery)
## [PowerShell — model klasyczny](/powershell/module/azure/?view=azuresmps-3.7.0)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# Powiązane
## [Azure Automation](/azure/automation/)

# Zasoby
## [Ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Cennik](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=site-recovery)
