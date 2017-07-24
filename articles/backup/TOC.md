
# Omówienie
## [Co to jest Azure Backup?](backup-introduction-to-azure-backup.md)

# Rozpoczęcie pracy
## [Tworzenie kopii zapasowych maszyn wirtualnych platformy Azure](backup-azure-vms-first-look-arm.md)
## [Tworzenie kopii zapasowych plików i folderów](backup-try-azure-backup-in-10-mins.md)
## [Back up VMware servers (Tworzenie kopii zapasowych serwerów VMware)](backup-azure-backup-server-vmware.md)

# Instrukcje
## Maszyny wirtualne platformy Azure
### Przygotowywanie maszyny wirtualnej
#### [Przygotowywanie maszyn wirtualnych wdrożonych przez usługę Resource Manager](backup-azure-arm-vms-prepare.md)
#### [Przygotowywanie maszyn wirtualnych platformy Azure](backup-azure-vms-prepare.md)
### Planowanie środowiska
#### [Planowanie infrastruktury kopii zapasowych maszyn wirtualnych](backup-azure-vms-introduction.md)
### Tworzenie kopii zapasowej maszyn wirtualnych
#### [Tworzenie kopii zapasowych maszyn wirtualnych w magazynie usługi Recovery Services](backup-azure-arm-vms.md)
#### [Tworzenie kopii zapasowych zaszyfrowanych maszyn wirtualnych](backup-azure-vms-encryption.md)
#### [Tworzenie kopii zapasowych maszyn wirtualnych platformy Azure w magazynie kopii zapasowych](backup-azure-vms.md)
### Monitorowanie maszyn wirtualnych i zarządzanie nimi
#### [Zarządzanie kopiami zapasowymi maszyny wirtualnej platformy Azure w witrynie Azure Portal](backup-azure-manage-vms.md)
#### [Monitorowanie alertów związanych z kopiami zapasowymi maszyny wirtualnej platformy Azure w witrynie Azure Portal](backup-azure-monitor-vms.md)
#### [Zarządzanie kopiami zapasowymi maszyny wirtualnej platformy Azure i monitorowanie ich w portalu klasycznym](backup-azure-manage-vms-classic.md)
### Przywracanie danych z maszyn wirtualnych
#### [Odzyskiwanie plików z kopii zapasowych maszyn wirtualnych platformy Azure](backup-azure-restore-files-from-vm.md)
#### [Przywracanie maszyn wirtualnych wdrożonych przez usługę Resource Manager w witrynie Azure Portal](backup-azure-arm-restore-vms.md)
#### [Przywracanie zaszyfrowanych maszyn wirtualnych](backup-azure-vms-encryption.md)
#### [Przywracanie maszyn wirtualnych na platformie Azure](backup-azure-restore-vms.md)
#### [Przywracanie klucza i wpisu tajnego usługi Key Vault dla szyfrowanych maszyn wirtualnych za pomocą usługi Azure Backup](backup-azure-restore-key-secret.md)


## Pliki i foldery systemu Windows
### [System Windows Server — z zastosowaniem modelu wdrażania przy użyciu usługi Resource Manager](backup-configure-vault.md)
### [System Windows Server — z zastosowaniem klasycznego modelu wdrażania](backup-configure-vault-classic.md)
### [Monitorowanie magazynów usługi Recovery Services i zarządzanie nimi](backup-azure-manage-windows-server.md)
### [Zarządzanie magazynami kopii zapasowych przy użyciu klasycznego modelu wdrażania](backup-azure-manage-windows-server-classic.md)
### [Odzyskiwanie plików w systemie Windows Server przy użyciu modelu wdrażania używającego usługi Resource Manager](backup-azure-restore-windows-server.md)
### [Odzyskiwanie plików w systemie Windows Server przy użyciu klasycznego modelu wdrażania](backup-azure-restore-windows-server-classic.md)

## Azure Backup Server
### [Macierz ochrony usługi Azure Backup Server](backup-mabs-protection-matrix.md)
### Instalowanie lub uaktualnianie
#### [Przygotowywanie obciążeń serwera Azure Backup Server w witrynie Azure Portal](backup-azure-microsoft-azure-backup.md)
#### [Przygotowywanie obciążeń serwera Azure Backup Server w portalu klasycznym](backup-azure-microsoft-azure-backup-classic.md)
#### [Dodawanie magazynu do usługi Azure Backup Server](backup-mabs-add-storage.md)
#### [Uaktualnianie usługi Azure Backup Server do wersji 2](backup-mabs-upgrade-to-v2.md)
#### [Instalacja nienadzorowana usługi Azure Backup Server](backup-mabs-unattended-install.md)
### Ochrona obciążeń
#### [Wykonywanie kopii zapasowej serwera VMware przy użyciu usługi Azure Backup Server](backup-azure-backup-server-vmware.md)
#### [Używanie usługi Azure Backup Server do tworzenia kopii zapasowej programu Exchange](backup-azure-exchange-mabs.md)
#### [Używanie usługi Azure Backup Server do tworzenia kopii zapasowej farmy programu SharePoint](backup-azure-backup-sharepoint-mabs.md)
#### [Używanie usługi Azure Backup Server do tworzenia kopii zapasowej bazy danych SQL](backup-azure-sql-mabs.md)
#### [Ochrona stanu systemu i odzyskiwanie systemu od zera](backup-mabs-system-state-and-bmr.md)

## Data Protection Manager
### [Przygotowywanie obciążeń programu DPM w witrynie Azure Portal](backup-azure-dpm-introduction.md)
### [Przygotowywanie obciążeń programu DPM w portalu klasycznym](backup-azure-dpm-introduction-classic.md)
### [Tworzenie kopii zapasowych serwera programu Exchange przy użyciu programu System Center DPM](backup-azure-backup-exchange-server.md)
### [Odzyskiwanie danych w magazynie kopii zapasowych na alternatywnym serwerze programu DPM](backup-azure-alternate-dpm-server.md)
### [Tworzenie kopii zapasowych obciążeń programu SQL Server przy użyciu programu DPM](backup-azure-backup-sql.md)
### [Tworzenie kopii zapasowych farmy programu SharePoint przy użyciu programu DPM](backup-azure-backup-sharepoint.md)

## Usługa Azure SQL Database
### [Konfigurowanie długoterminowego przechowywania kopii zapasowych](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Wyświetlanie kopii zapasowych w magazynie usługi Recovery Services](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Przywracanie po długoterminowym przechowywaniu kopii zapasowych](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Usuwanie długoterminowych kopii zapasowych usługi Azure SQL](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Korzystanie z programu PowerShell
### [Maszyny wirtualne platformy Azure w witrynie Azure Portal](backup-azure-vms-automation.md)
### [Maszyny wirtualne platformy Azure w portalu klasycznym](backup-azure-vms-classic-automation.md)
### [Program DPM w witrynie Azure Portal](backup-dpm-automation.md)
### [Program DPM w portalu klasycznym](backup-dpm-automation-classic.md)
### [System Windows Server w witrynie Azure Portal](backup-client-automation.md)
### [System Windows Server w portalu klasycznym](backup-client-automation-classic.md)

## Często zadawane pytania
### [FAQ on Recovery Services vault (Często zadawane pytania dotyczące magazynu usługi Recovery Services)](backup-azure-backup-faq.md)
### [FAQ on Azure VM backup (Często zadawane pytania dotyczące kopii zapasowych maszyn wirtualnych platformy Azure)](backup-azure-vm-backup-faq.md)
### [FAQ on file-folder backup using Azure Backup agent (Często zadawane pytania dotyczące tworzenia kopii zapasowej plików i folderów przy użyciu agenta usługi Azure Backup)](backup-azure-file-folder-backup-faq.md)

## Rozwiązywanie problemów
### [Problemy z tworzeniem kopii zapasowej maszyny wirtualnej platformy Azure w witrynie Azure Portal](backup-azure-vms-troubleshoot.md)
### [Problemy z tworzeniem kopii zapasowej maszyny wirtualnej platformy Azure w portalu klasycznym](backup-azure-vms-troubleshoot-classic.md)
### [Niepowodzenie tworzenia kopii zapasowej maszyny wirtualnej platformy Azure: Nie można nawiązać komunikacji z agentem maszyny wirtualnej w celu uzyskania stanu migawki — Upłynął limit czasu zadania podrzędnego migawki maszyny wirtualnej](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Powolne tworzenie kopii zapasowych plików i folderów w usłudze Azure Backup](backup-azure-troubleshoot-slow-backup-performance-issue.md)
### [Rozwiązywanie problemów ze składnikiem Azure Backup Server](backup-azure-mabs-troubleshoot.md)

# Pojęcia
## [Omówienie magazynów usługi Recovery Services](backup-azure-recovery-services-vault-overview.md)
## [Uaktualnienie magazynu usługi Backup do magazynu usługi Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md)
## [Usuwanie magazynu usługi Azure Backup](backup-azure-delete-vault.md)
## [Kontrola dostępu oparta na rolach](backup-rbac-rs-vault.md)
## [Zabezpieczenia hybrydowych kopii zapasowych](backup-azure-security-feature.md)
## [Konfigurowanie raportów usługi Azure Backup](backup-azure-configure-reports.md)
## [Model danych dla raportów usługi Azure Backup](backup-azure-reports-data-model.md)
## [Konfigurowanie tworzenia kopii zapasowej offline](backup-azure-backup-import-export.md)
## [Zastępowanie biblioteki taśm](backup-azure-backup-cloud-as-tape.md)
## [Kopie zapasowe maszyn wirtualnych z systemem Linux spójne z aplikacjami](backup-azure-linux-app-consistent.md)

# Dokumentacja
## [Program PowerShell](/powershell/module/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/)
## [Forum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Cennik](https://azure.microsoft.com/pricing/details/backup/)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=backup)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=backup)
