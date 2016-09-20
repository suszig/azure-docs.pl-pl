<properties
   pageTitle="Często zadawane pytania dotyczące magazynu usług Recovery Services | Microsoft Azure"
   description="Ta wersja często zadawanych pytań dotyczy publicznej wersji zapoznawczej usługi Azure Backup. Odpowiedzi na często zadawane pytania dotyczące agenta kopii zapasowej, kopii zapasowej i przechowywania, odzyskiwania, zabezpieczeń i inne typowe pytania dotyczące rozwiązania Azure Backup."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="rozwiązanie kopii zapasowych; usługa kopii zapasowych"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="08/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# Magazyn usług Recovery Services — często zadawane pytania

> [AZURE.SELECTOR]
- [Często zadawane pytania dotyczące kopii zapasowych dla trybu klasycznego](backup-azure-backup-faq.md)
- [Często zadawane pytania dotyczące kopii zapasowych dla trybu usługi Resource Manager](backup-azure-backup-ibiza-faq.md)

Ten artykuł zawiera informacje specyficzne dla magazynu usług Recovery Services oraz uzupełnia artykuł [Azure Backup FAQ](backup-azure-backup-faq) (Azure Backup — często zadawane pytania). Często zadawane pytania usługi Azure Backup zawierają pełny zestaw pytań i odpowiedzi dotyczących usługi Azure Backup.  

Pytania dotyczące usługi Azure Backup można zadawać w sekcji Disqus tego artykułu lub powiązanego artykułu. Pytania dotyczące usługi Azure Backup można również zadawać na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## Magazyny usługi Recovery Services opierają się na usłudze Resource Manager. Czy magazyny usługi Backup (tryb klasyczny) są nadal obsługiwane? <br/>
Tak. Magazyny usługi Backup nadal są obsługiwane. Magazyny usługi Backup są tworzone w [portalu klasycznym](https://manage.windowsazure.com). Magazyny usługi Recovery Services są tworzone w witrynie [Azure Portal](https://portal.azure.com). Jednak zdecydowanie zaleca się utworzenie magazynu usługi Recovery Services, ponieważ wszystkie przyszłe rozszerzenia będą dostępne tylko w magazynie tej usługi. 

## Czy mogę przeprowadzić migrację magazynu usługi Backup do magazynu usługi Recovery Services? <br/>
Niestety nie. W tej chwili nie ma możliwości przeprowadzania migracji zawartości magazynu usługi Backup do magazynu usługi Recovery Services. Pracujemy nad dodaniem tej funkcji, ale nie jest ona dostępna w ramach publicznej wersji zapoznawczej.

## Czy magazyny usługi Recovery Services obsługują klasyczne maszyny wirtualne, czy maszyny wirtualne oparte na usłudze Resource Manager? <br/>
Magazyny usługi Recovery Services obsługują oba modele.  Możliwe jest utworzenie kopii zapasowej maszyny wirtualnej utworzonej w portalu klasycznym (czyli maszyny wirtualnej w wersji klasycznej) lub maszyny wirtualnej utworzonej w witrynie Azure Portal (czyli opartej na usłudze Resource Manager) w magazynie usługi Recovery Services.

## Mam utworzone kopie zapasowe moich klasycznych maszyn wirtualnych w magazynie usługi Backup. Teraz chcę zmigrować moje maszyny wirtualne z trybu klasycznego do trybu usługi Resource Manager.  Jak mogę utworzyć ich kopię zapasową w magazynie usługi Recovery Services?
Kopie zapasowe klasycznych maszyn wirtualnych w magazynie usługi Backup nie są migrowane automatycznie do magazynu usługi Recovery Services podczas migracji maszyn wirtualnych z trybu klasycznego do trybu usługi Resource Manager. Wykonaj poniższe działania, aby przeprowadzić migrację kopii zapasowych maszyn wirtualnych:

1. W magazynie usługi Backup przejdź do karty **Elementy chronione** i wybierz maszynę wirtualną. Kliknij pozycję [Zatrzymaj ochronę](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Pozostaw opcję *Usuń powiązane dane kopii zapasowych* **niezaznaczoną**. 
2. Przeprowadź migrację maszyny wirtualnej z trybu klasycznego do trybu usługi Resource Manager. Upewnij się, że magazyn i sieć odpowiadające maszynie wirtualnej również zostały zmigrowane do trybu usługi Resource Manager. 
3. Utwórz magazyn usługi Recovery Services i skonfiguruj kopię zapasową w zmigrowanej maszynie wirtualnej przy użyciu akcji **Kopia zapasowa** w górnej części pulpitu nawigacyjnego magazynu. Dowiedz się więcej na temat [włączania kopii zapasowych w magazynie usługi Recovery Services](backup-azure-vms-first-look-arm.md)



<!--HONumber=sep16_HO1-->


