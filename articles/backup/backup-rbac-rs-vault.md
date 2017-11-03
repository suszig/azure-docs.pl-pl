---
title: "Zarządzanie kopiami zapasowymi przy użyciu kontroli dostępu opartej na rolach na platformie Azure | Dokumentacja firmy Microsoft"
description: "Kontrola dostępu oparta na rolach umożliwia zarządzanie dostępem do operacji tworzenia kopii zapasowej zarządzania w magazynie usług odzyskiwania."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 3bd46b97-4b29-47a5-b5ac-ac174dd36760
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/22/2017
ms.author: trinadhk;markgal
ms.openlocfilehash: b6e4c6761e1bd5c17c9c3428491113042d3b1d31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Umożliwia zarządzanie punktami odzyskiwania kopia zapasowa Azure opartej na rolach kontrola dostępu
Kontrola dostępu oparta na rolach (Role-Based Access Control, RBAC) na platformie Azure umożliwia precyzyjne zarządzanie dostępem dla platformy Azure. Przy użyciu funkcji RBAC, można rozdzielenie obowiązków w obrębie organizacji i udzielić tylko takiego dostępu dla użytkowników, które są niezbędne do wykonywania swoich zadań.

> [!IMPORTANT]
> Role udostępniane przez usługi Kopia zapasowa Azure są ograniczone do działania, które mogą być wykonywane w portalu Azure lub poleceń cmdlet programu PowerShell magazyn usług odzyskiwania. Akcje wykonywane w Azure kopii zapasowej center interfejs użytkownika klienta usługi agenta lub systemu interfejs programu Data Protection Manager lub interfejsu użytkownika serwera kopii zapasowej Azure są poza kontrolą tych ról.

Kopia zapasowa Azure stanowi 3 wbudowane role do kontrolowania operacji zarządzania kopiami zapasowymi. Dowiedz się więcej na [wbudowane role Azure RBAC](../active-directory/role-based-access-built-in-roles.md)

* [Wykonaj kopię zapasową współautora](../active-directory/role-based-access-built-in-roles.md#backup-contributor) — ta rola ma wszystkie uprawnienia do tworzenia i zarządzanie kopiami zapasowymi, z wyjątkiem Tworzenie magazynu usług odzyskiwania i zapewnieniu dostępu do innych użytkowników. Wyobraź sobie tej roli administratora zarządzania kopiami zapasowymi, który można wykonać każdej operacji zarządzania kopiami zapasowymi.
* [Wykonaj kopię zapasową Operator](../active-directory/role-based-access-built-in-roles.md#backup-operator) — ta rola ma uprawnienia do wszystkich współautora z wyjątkiem usuwanie kopii zapasowej i zarządzanie nimi zasad tworzenia kopii zapasowych. Ta rola jest odpowiednikiem współautora, z wyjątkiem nie można wykonać operacji destrukcyjnego, takich jak tworzenie kopii zapasowej Zatrzymaj z usuwania danych lub usunąć rejestrację zasobów lokalnych.
* [Wykonaj kopię zapasową czytnika](../active-directory/role-based-access-built-in-roles.md#backup-reader) — ta rola ma uprawnienia do wyświetlania wszystkich operacji zarządzania kopiami zapasowymi. Wyobraź sobie tej roli, do monitorowania osoby.

Jeśli szukasz definiować własne role, aby uzyskać większą kontrolę, zobacz porady [Tworzenie niestandardowych ról](../active-directory/role-based-access-control-custom-roles.md) w Azure RBAC.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Mapowanie wbudowane role kopii zapasowej do akcji zarządzania kopiami zapasowymi
Poniższa tabela zawiera akcje zarządzania kopii zapasowej i odpowiedniego roli RBAC minimalnych wymaganych do wykonania tej operacji.

| Operacja zarządzania | Minimalna roli RBAC wymagane |
| --- | --- |
| Tworzenie magazynu usług odzyskiwania | Współautor grupy zasobów magazynu |
| Włączenia kopii zapasowej maszyn wirtualnych Azure | Operator kopii zapasowych w magazynie, współautora maszyny wirtualnej na maszynach wirtualnych |
| Na żądanie kopii zapasowej maszyny Wirtualnej | Operator kopii zapasowych |
| Przywracanie maszyny Wirtualnej | Operator kopii zapasowych, w którym maszyny Wirtualnej i sieci wirtualne mają być wdrożony Współautor grupy zasobów |
| Przywracanie dysków, pojedyncze pliki z kopii zapasowej maszyny Wirtualnej | Operator kopii zapasowych, współautora maszyny wirtualnej na maszynach wirtualnych |
| Tworzenie zasad tworzenia kopii zapasowej dla kopii zapasowej maszyny Wirtualnej Azure | Współautor kopii zapasowej |
| Modyfikowanie zasad tworzenia kopii zapasowej kopii zapasowej maszyny Wirtualnej Azure | Współautor kopii zapasowej |
| Usuń zasady tworzenia kopii zapasowej kopii zapasowej maszyny Wirtualnej Azure | Współautor kopii zapasowej |
| Zatrzymaj kopii zapasowej (z opcją zachowania danych lub usunięcie danych) w kopii zapasowej maszyny Wirtualnej | Współautor kopii zapasowej |
| Zarejestruj lokalnego systemu Windows serwera/klienta/SCDPM lub serwer kopii zapasowej systemu Azure | Operator kopii zapasowych |
| Usuń zarejestrowane lokalnie systemu Windows serwera/klienta/SCDPM lub serwer kopii zapasowej systemu Azure | Współautor kopii zapasowej |

## <a name="next-steps"></a>Następne kroki
* [Kontroli dostępu opartej na rolach](../active-directory/role-based-access-control-configure.md): rozpoczynanie pracy z RBAC w portalu Azure.
* Dowiedz się, jak Zarządzaj dostępem za pomocą:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Interfejs wiersza polecenia platformy Azure](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [Interfejs API REST](../active-directory/role-based-access-control-manage-access-rest.md)
* [Rozwiązywanie kontroli dostępu opartej na rolach](../active-directory/role-based-access-control-troubleshooting.md): Pobierz sugestie dotyczące rozwiązywania typowych problemów.
