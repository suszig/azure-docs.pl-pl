<properties
   pageTitle="Publiczna wersja zapoznawcza usługi Azure Backup — często zadawane pytania | Microsoft Azure"
   description="Ta wersja często zadawanych pytań dotyczy publicznej wersji zapoznawczej usługi Azure Backup. Odpowiedzi na często zadawane pytania dotyczące agenta kopii zapasowej, kopii zapasowej i przechowywania, odzyskiwania, zabezpieczeń i inne typowe pytania dotyczące rozwiązania Azure Backup."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="backup solution; backup service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="03/30/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# Publiczna wersja zapoznawcza usługi Azure Backup — często zadawane pytania

> [AZURE.SELECTOR]
- [Często zadawane pytania dla trybu klasycznego](backup-azure-backup-faq.md)
- [Często zadawane pytania dla trybu ARM](backup-azure-backup-ibiza-faq.md)

Ten artykuł zawiera informacje specyficzne dla publicznej wersji zapoznawczej usługi Azure Backup. Ten artykuł będzie aktualizowany, gdy otrzymane zostaną nowe często zadawane pytania. Uzupełnia on również informacje zawarte w artykule [Azure Backup — często zadawane pytania](backup-azure-backup-faq). Często zadawane pytania usługi Azure Backup zawierają pełny zestaw pytań i odpowiedzi dotyczących usługi Azure Backup.  

Pytania dotyczące usługi Azure Backup można zadawać w sekcji Disqus tego artykułu lub powiązanego artykułu. Pytania dotyczące usługi Azure Backup można również zadawać na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## Co znajduje się w publicznej wersji zapoznawczej?
W publicznej wersji zapoznawczej wprowadzony został magazyn Usług odzyskiwania i obsługa usługi ARM w przypadku ochrony maszyn wirtualnych platformy Azure. Magazyn Usług odzyskiwania to nowa generacja magazynów. Jest to magazyn używany przez usługi Azure Backup i Azure Site Recovery (ASR). Należy traktować go jako magazyn w wersji 2.

## Usługi odzyskiwania i magazyny kopii zapasowych

**Pytanie 1. Magazyny usług odzyskiwania są w wersji 2. Czy magazyny kopii zapasowych (wersja 1) nadal są obsługiwane?** <br/>
Odpowiedź 1. Tak. Magazyny kopii zapasowych nadal są obsługiwane. Magazyny kopii zapasowych są tworzone w klasycznym portalu. Magazyny Usług odzyskiwania są tworzone w Portalu Azure.

**Pytanie 2. Czy mogę przeprowadzić migrację magazynu kopii zapasowych do magazynu Usług odzyskiwania?** <br/>
Odpowiedź 2. Niestety nie. W tej chwili nie ma możliwości przeprowadzania migracji zawartości magazynu kopii zapasowych do magazynu Usług odzyskiwania. Pracujemy nad dodaniem tej funkcji, ale nie jest ona dostępna w ramach publicznej wersji zapoznawczej.

**Pytanie 3. Czy magazyny Usług odzyskiwania obsługują maszyny wirtualne w wersji 1 czy w wersji 2?** <br/>
 Odpowiedź 3. Magazyny Usług odzyskiwania obsługują maszyny wirtualne w wersji 1 i w wersji 2. Możliwe jest utworzenie kopii zapasowej maszyny wirtualnej utworzonej w portalu klasycznym (czyli w wersji 1) lub maszyny wirtualnej utworzonej w Portalu Azure (czyli w wersji 2) w magazynie Usług odzyskiwania.


## Obsługa usługi ARM dla maszyn wirtualnych platformy Azure

**Pytanie 1. Czy istnieją jakiekolwiek ograniczenia obsługi usługi ARM dla maszyn wirtualnych platformy Azure?** <br/>
Odpowiedź 1. Polecenia cmdlet programu PowerShell dla usługi ARM nie są obecnie dostępne. Aby dodać zasoby do grupy zasobów, należy użyć interfejsu użytkownika Portalu Azure.



<!--HONumber=Jun16_HO2-->


