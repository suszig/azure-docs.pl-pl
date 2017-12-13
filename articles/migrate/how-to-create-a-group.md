---
title: "Grupy komputerów w celu oceny z migracji Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób grupowania maszyny przed uruchomieniem oceny w usłudze Azure migracji."
author: rayne-wiselman
ms.service: azure-migrate
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 429a9150d1fbf50c0e3fa2046eb64affc8db8e5d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
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
