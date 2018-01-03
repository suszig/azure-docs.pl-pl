---
title: "Grupy komputerów w celu oceny z migracji Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób grupowania maszyny przed uruchomieniem oceny w usłudze Azure migracji."
author: rayne-wiselman
ms.service: azure-migrate
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: raynew
ms.openlocfilehash: f42b184cddb3274d7ee0163c10cac002ccfbef62
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="group-machines-for-assessment"></a>Grupy komputerów w celu oceny

W tym artykule opisano sposób tworzenia grupy komputerów w celu oceny przez [migracji Azure](migrate-overview.md). Azure migracji ocenia maszyn w grupie, aby sprawdzić, czy są odpowiednie dla migracji na platformie Azure i zapewnia kosztów i zmiany rozmiaru uzyskać szacunkowe wartości do uruchamiania komputera w systemie Azure.


## <a name="create-a-group"></a>Utwórz grupę

1. W **omówienie** Azure migracji projektu, w obszarze Zarządzanie, kliknij przycisk **grup** > **+ grupy**i określ nazwę grupy.
2. Dodaj co najmniej jednej maszyny do grupy, a następnie kliknij przycisk **Utwórz**. 
3. Opcjonalnie można wybrać do uruchomienia na podstawie oceny nowej grupy. 

    ![Utwórz grupę](./media/how-to-create-a-group/create-group.png)

Po utworzeniu grupy możesz ją zmodyfikować, wybierając grupę na **grup** stronę i dodawanie lub usuwanie maszyn.

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się, jak używać [mapowania zależności maszyny](how-to-create-group-machine-dependencies.md) do tworzenia grup wysokiego zaufania.
- [Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
