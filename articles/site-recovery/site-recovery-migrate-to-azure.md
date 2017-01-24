---
title: "Migrowanie na platformę Azure za pomocą usługi Site Recovery | Microsoft Docs"
description: "Ten artykuł zawiera omówienie migrowania maszyn wirtualnych i serwerów fizycznych na platformę Azure za pomocą usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 20aecf6df4af0b89fb6cafbd2047facdb345bae5
ms.openlocfilehash: 8f658cb80a43e0eb21585ea851251b1339ab546b


---
# <a name="migrate-to-azure-with-site-recovery"></a>Migrujesz na platformę Azure za pomocą usługi Site Recovery?

Przeczytaj ten artykuł, aby zapoznać się z usługą Azure Site Recovery pod kątem migrowania maszyn wirtualnych i serwerów fizycznych.

Organizacje wymagają strategii BCDR, która określa, w jaki sposób aplikacje, obciążenia i dane pozostają uruchomione i dostępne podczas planowanych lub nieplanowanych przerw w pracy oraz są przywracane do normalnych warunków roboczych z możliwie dużą prędkością. Strategia BCDR powinna utrzymywać dane firmowe z zachowaniem bezpieczeństwa i umożliwiać ich odzyskiwanie, a także zapewniać, że obciążenia pozostają stale dostępne w przypadku awarii.

Usługa Site Recovery jest usługą platformy Azure, która wspiera strategię BCDR przez organizowanie replikacji lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (Azure) lub dodatkowego centrum danych. W przypadku wystąpienia awarii w lokalizacji głównej następuje przełączenie w trybie failover do lokalizacji dodatkowej, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót po awarii może mieć miejsce do lokalizacji głównej, gdy powróci ona do normalnego działania. Dowiedz się więcej w temacie [Co to jest usługa Site Recovery?](site-recovery-overview.md)

W tym artykule opisano wdrażanie w witrynie [Azure Portal](https://portal.azure.com). [Klasycznej witryny Azure Portal](https://manage.windowsazure.com/) można używać do obsługi istniejących magazynów usługi Site Recovery, ale nie można tworzyć nowych magazynów za jej pomocą.

Komentarze możesz zamieścić na dole tego artykułu. Zadawaj pytania techniczne na [Forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>Co mamy na myśli przez migrację?

Usługę Site Recovery można wdrożyć na potrzeby pełnej replikacji lokalnych maszyn wirtualnych i serwerów fizycznych na platformę Azure lub do lokacji dodatkowej. Maszyny są duplikowane, przechodzą do trybu failover z lokacji głównej gdy wystąpi awaria i z powrotem do lokacji głównej, gdy zostanie wykonane odzyskiwanie. Oprócz pełnej replikacji usługa Site Recovery umożliwia migrowanie maszyn wirtualnych i serwerów fizycznych na platformę Azure, dzięki czemu użytkownicy mają dostęp do obciążenia maszyny z maszyn wirtualnych na platformie Azure. Migracja pociąga za sobą replikację i przejście do trybu failover z lokacji głównej na platformę Azure. Jednak w przeciwieństwie do pełnej replikacji nie obejmuje mechanizmu powrotu po awarii.

## <a name="what-can-site-recovery-migrate"></a>Co może migrować usługa Site Recovery?

Możesz:

- Przeprowadzić migrację obciążeń działających na lokalnych maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych programu VMware i serwerach fizycznych, aby działały na maszynach wirtualnych platformy Azure. W ramach tego scenariusza możesz również wykonać pełną replikację i powrót po awarii.
- Przeprowadzić migrację [maszyn wirtualnych IaaS platformy Azure](site-recovery-migrate-azure-to-azure.md) między regionami platformy Azure. W tym scenariuszu aktualnie jest obsługiwana tylko migracja, co oznacza, że powrót po awarii nie jest obsługiwany.
- Przeprowadzić migrację [wystąpień usługi AWS dla systemu Windows](site-recovery-migrate-aws-to-azure.md) do maszyn wirtualnych IaaS platformy Azure. W tym scenariuszu aktualnie jest obsługiwana tylko migracja, co oznacza, że powrót po awarii nie jest obsługiwany.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Migracja lokalnych maszyn wirtualnych i serwerów fizycznych

Aby zmigrować lokalne maszyny wirtualne funkcji Hyper-V, maszyny wirtualne programu VMware i serwery fizyczne, należy wykonać prawie te same kroki co w przypadku zwykłej replikacji. Należy skonfigurować magazyn usługi Recovery Services, skonfigurować wymagane serwery zarządzania (w zależności od tego, co jest migrowane), dodać je do magazynu, a następnie określić ustawienia replikacji. Następnie należy włączyć replikację dla maszyn, które mają zostać zmigrowane, i uruchomić szybkie testowe przejście do trybu failover, aby upewnić się, że wszystko pracuje poprawnie.

Po zweryfikowaniu, że środowisko replikacji działa, należy użyć planowanego lub nieplanowanego przejścia do trybu failover w zależności od tego, [co obsługuje](site-recovery-failover.md#failover-and-failback) scenariusz. W przypadku migracji nie jest koniecznie zatwierdzanie trybu failover ani usuwanie czegokolwiek. Zamiast tego należy wybrać opcję **Zakończ migrację** dla każdej maszyny, którą chcesz zmigrować. Akcja **Zakończ migrację** kończy proces migracji, usuwa replikację dla maszyny i zatrzymuje naliczanie opłat za usługę Site Recovery dla maszyny.

## <a name="migrate-between-azure-regions"></a>Migracja między regionami platformy Azure

Maszyny wirtualne platformy Azure można migrować między regionami przy użyciu usługi Site Recovery. W tym scenariuszu jest obsługiwana tylko migracja. Inaczej mówiąc, możesz zreplikować maszyny wirtualne platformy Azure i przełączyć je w tryb failover do innego regionu, ale nie możesz wykonać powrotu po awarii. W ramach tego scenariusza należy skonfigurować magazyn usługi Recovery Services, wdrożyć lokalny serwer konfiguracji na potrzeby zarządzania replikacją, dodać go do magazynu i określić ustawienia replikacji. Następnie należy włączyć replikację dla maszyn, które mają zostać zmigrowane, i uruchomić szybkie testowe przejście do trybu failover. Potem należy uruchomić nieplanowane przejście do trybu failover z opcją **Zakończ migrację**.

## <a name="migrate-aws-to-azure"></a>Migracja usługi AWS na platformę Azure

Możesz zmigrować wystąpienia usługi AWS na maszyny wirtualne platformy Azure. W tym scenariuszu jest obsługiwana tylko migracja. Inaczej mówiąc, możesz zreplikować wystąpienia usługi AWS i przełączyć je w tryb failover na platformę Azure, ale nie możesz wykonać powrotu po awarii. Na potrzeby migracji wystąpienia usługi AWS są obsługiwane w taki sam sposób jak serwery fizyczne. Należy skonfigurować magazyn usługi Recovery Services, wdrożyć lokalny serwer konfiguracji na potrzeby zarządzania replikacją, dodać go do magazynu i określić ustawienia replikacji. Następnie należy włączyć replikację dla maszyn, które mają zostać zmigrowane, i uruchomić szybkie testowe przejście do trybu failover. Potem należy uruchomić nieplanowane przejście do trybu failover z opcją **Zakończ migrację**.




## <a name="next-steps"></a>Następne kroki

- [Migrowanie maszyn wirtualnych programu VMware na platformę Azure](site-recovery-vmware-to-azure.md)
- [Migrowanie serwerów fizycznych na platformę Azure](site-recovery-vmware-to-azure.md)
- [Migrowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM na platformę Azure](site-recovery-vmm-to-azure.md)
- [Migrowanie maszyn wirtualnych funkcji Hyper-V (bez programu VMM) na platformę Azure](site-recovery-hyper-v-site-to-azure.md)
- [Migrowanie maszyn wirtualnych platformy Azure między regionami platformy Azure](site-recovery-migrate-azure-to-azure.md)
- [Migrowanie wystąpień usługi AWS na platformę Azure](site-recovery-migrate-aws-to-azure.md)



<!--HONumber=Jan17_HO1-->


