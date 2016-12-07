---
title: "Często zadawane pytania dotyczące magazynu usług Recovery Services | Microsoft Docs"
description: "Ta wersja często zadawanych pytań dotyczy publicznej wersji zapoznawczej usługi Azure Backup. Odpowiedzi na często zadawane pytania dotyczące agenta kopii zapasowej, kopii zapasowej i przechowywania, odzyskiwania, zabezpieczeń i inne typowe pytania dotyczące rozwiązania Azure Backup."
services: backup
documentationcenter: 
author: markgalioto
manager: jwhit
editor: 
keywords: "rozwiązanie kopii zapasowych; usługa kopii zapasowych"
ms.assetid: 5f55b500-1ee9-4f64-9306-02d6f7a8eded
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: trinadhk; markgal; jimpark;
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fdc596def57968381ea48dfc5df2269a682c9de7


---
# <a name="recovery-services-vault-faq"></a>Magazyn usług Recovery Services — często zadawane pytania
Ten artykuł zawiera informacje specyficzne dla magazynu usług Recovery Services oraz uzupełnia artykuł [Azure Backup FAQ](backup-azure-backup-faq.md) (Azure Backup — często zadawane pytania). Często zadawane pytania usługi Azure Backup zawierają pełny zestaw pytań i odpowiedzi dotyczących usługi Azure Backup.  

Pytania dotyczące usługi Azure Backup można zadawać w sekcji Disqus tego artykułu lub powiązanego artykułu. Pytania dotyczące usługi Azure Backup można również zadawać na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Magazyny usług Recovery Services są oparte na usłudze Resource Manager. Czy magazyny usługi Backup (tryb klasyczny) są nadal obsługiwane? <br/>
Tak. Magazyny usługi Backup nadal są obsługiwane. Magazyny usługi Backup są tworzone w [portalu klasycznym](https://manage.windowsazure.com). Magazyny usługi Recovery Services są tworzone w witrynie [Azure Portal](https://portal.azure.com). Jednak zdecydowanie zaleca się utworzenie magazynu usługi Recovery Services, ponieważ wszystkie przyszłe rozszerzenia będą dostępne tylko w magazynie tej usługi.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Czy mogę przeprowadzić migrację magazynu usługi Backup do magazynu usługi Recovery Services? <br/>
Niestety nie. W tej chwili nie ma możliwości przeprowadzania migracji zawartości magazynu usługi Backup do magazynu usługi Recovery Services. Pracujemy nad dodaniem tej funkcji, ale nie jest ona dostępna w ramach publicznej wersji zapoznawczej.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Czy magazyny usługi Recovery Services obsługują klasyczne maszyny wirtualne, czy maszyny wirtualne oparte na usłudze Resource Manager? <br/>
Magazyny usługi Recovery Services obsługują oba modele.  Możliwe jest utworzenie kopii zapasowej maszyny wirtualnej utworzonej w portalu klasycznym (czyli maszyny wirtualnej w wersji klasycznej) lub maszyny wirtualnej utworzonej w witrynie Azure Portal (czyli opartej na usłudze Resource Manager) w magazynie usługi Recovery Services.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault"></a>Mam utworzone kopie zapasowe moich klasycznych maszyn wirtualnych w magazynie usługi Backup. Teraz chcę zmigrować moje maszyny wirtualne z trybu klasycznego do trybu usługi Resource Manager.  Jak mogę utworzyć ich kopię zapasową w magazynie usługi Recovery Services?
Kopie zapasowe klasycznych maszyn wirtualnych w magazynie usługi Backup nie są migrowane automatycznie do magazynu usługi Recovery Services podczas migracji maszyn wirtualnych z trybu klasycznego do trybu usługi Resource Manager. Wykonaj poniższe działania, aby przeprowadzić migrację kopii zapasowych maszyn wirtualnych:

1. W magazynie usługi Backup przejdź do karty **Elementy chronione** i wybierz maszynę wirtualną. Kliknij pozycję [Zatrzymaj ochronę](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Pozostaw opcję *Usuń powiązane dane kopii zapasowych* **niezaznaczoną**.
2. Przeprowadź migrację maszyny wirtualnej z trybu klasycznego do trybu usługi Resource Manager. Upewnij się, że magazyn i sieć odpowiadające maszynie wirtualnej również zostały zmigrowane do trybu usługi Resource Manager.
3. Utwórz magazyn usługi Recovery Services i skonfiguruj kopię zapasową w zmigrowanej maszynie wirtualnej przy użyciu akcji **Kopia zapasowa** w górnej części pulpitu nawigacyjnego magazynu. Dowiedz się więcej na temat [włączania kopii zapasowych w magazynie usługi Recovery Services](backup-azure-vms-first-look-arm.md)




<!--HONumber=Nov16_HO2-->


