---
title: Grupy maszyn wirtualnych z programu VMware vCenter znakowanie | Dokumentacja firmy Microsoft
description: "Opisuje sposób tworzenia grupy, przed uruchomieniem oceny w usłudze Azure migracji."
services: migration-planner
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: db33e31cdef143aa70809442457e447446a1681a
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2017
---
# <a name="group-vms-with-vcenter-tagging"></a>Maszyny wirtualne grupy z znakowanie vCenter

W tym artykule opisano sposób tworzenia grupy komputerów dla [migracji Azure](migrate-overview.md) oceny za pomocą znakowania w oprogramowaniu VMware vCenter. Należy określić kategorii tag, którego chcesz użyć podczas tworzenia grupy. 

## <a name="set-up-tagging"></a>Konfigurowanie znakowanie

Podczas migracji Azure wdrażania lokalnymi migracji maszyny Wirtualnej platformy Azure umożliwia odnajdywanie komputerów na hostach ESXi zarządzany przez serwer vCenter. Należy skonfigurować vCenter znakowanie przed procesu odnajdywania.

1. W VMware vSphere klienta sieci Web przejdź do serwera vCenter.
2. Kliknij przycisk **tagi**, aby przejrzeć wszystkie bieżące znaczniki.
3. Do znakowania Maszynę wirtualną, wybierz **powiązanych obiektów** > **maszyn wirtualnych**, a następnie wybierz maszynę Wirtualną, aby tag.
4. W **Podsumowanie** > **tagi**, kliknij przycisk **przypisać**. 
5. Kliknij przycisk **nowy znacznik**, a następnie określ nazwę tagu i opis.
6. Aby umieszczaniu go na palecie kategorii dla tagu, wybierz **nową kategorię** na liście rozwijanej.
7. Określ nazwę kategorii, opis i kardynalność. Następnie kliknij przycisk **OK**.

    ![Tagi maszyny Wirtualnej](./media/how-to-tag-v-center/vm-tag.png)

## <a name="use-tagging-to-create-groups"></a>Umożliwia oznakowanie tworzenie grup

1. Konfigurowanie odnajdywania lokalnych maszyn zgodnie z opisem w [samouczek oceny VMware](tutorial-assessment-vmware.md#run-the-collector-to-discover-vms).
2. W **Tag kategorii do grupowania**, wybierz kategorię tag vCenter, na którym bazuje grupy oceny. Azure migracji automatycznie tworzy grupy dla wybranej kategorii.

    

## <a name="next-steps"></a>Następne kroki

[Konfigurowanie maszyny Wirtualnej VMware oceny](tutorial-assessment-vmware.md).
