---
title: "Grupy komputerów w celu oceny z migracji Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób grupowania maszyny przed uruchomieniem oceny w usłudze Azure migracji."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 966e0aebf56ab049e898d1787bfdfbb2ef23635e
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2017
---
# <a name="group-machines-for-assessment"></a>Grupy komputerów w celu oceny

W tym artykule opisano sposób tworzenia grupy komputerów w celu oceny przez [migracji Azure](migrate-overview.md). Azure migracji ocenia maszyn w grupie, aby sprawdzić, czy są odpowiednie dla migracji na platformie Azure i zapewnia kosztów i zmiany rozmiaru uzyskać szacunkowe wartości do uruchamiania komputera w systemie Azure.


## <a name="create-a-group"></a>Utwórz grupę

1. W **pulpitu nawigacyjnego** Azure migracji projektu, kliknij przycisk **grup** > **+ grupy**i określ nazwę grupy.
2. Dodaj co najmniej jednej maszyny do grupy, a następnie kliknij przycisk **Utwórz**. 
3. Opcjonalnie można wybrać do uruchomienia na podstawie oceny nowej grupy. 

    ![Utwórz grupę](./media/how-to-create-a-group/create-group.png)

Po utworzeniu grupy możesz ją zmodyfikować, wybierając grupę na **grup** stronę i dodawanie lub usuwanie maszyn.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak używać [mapowania zależności maszyny](how-to-create-group-machine-dependencies.md) do tworzenia bardziej szczegółowych grup.
- [Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
