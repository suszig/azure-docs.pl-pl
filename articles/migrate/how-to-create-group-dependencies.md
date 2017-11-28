---
title: "Uściślij grupy oceny pomocą mapowania grup zależności w migracji Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób uściślić oceny w usłudze Azure migracji za pomocą mapowania grup zależności."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: b4d6861f147fbb6e65a9d529f17f78b54075eb90
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2017
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Dostosuj grupę za pomocą mapowania grup zależności

W tym artykule opisano, jak skonfigurować mapowanie zależności grupy dla [migracji Azure](migrate-overview.md) oceny. Jeśli chcesz dostosować ustawienia dla istniejącej grupy, krzyżowe sprawdzanie zależnościami grupy przed uruchomieniem ocenę zwykle Użyj tej metody. Grupy, dla których chcesz uruchomić mapowanie zależności grupy nie powinna zawierać więcej niż 10 maszyn.  

## <a name="modify-a-group"></a>Modyfikowanie grupy

1. W systemie Azure migracji projektu, w obszarze **Zarządzaj**, kliknij przycisk **grup**i wybierz grupę.
2. Na stronie grupy kliknij **zależności widoku**, aby otworzyć mapę zależności grupy. 

     ![Widok grupy](./media/how-to-create-group-dependencies/create-group.png)

3. Aby wyświetlić bardziej szczegółowe zależności, kliknij zakres czasu, aby go zmodyfikować. Domyślnie zakres jest godzinę. Możesz zmodyfikować zakres czasu lub określ rozpoczęcia oraz datę zakończenia oraz czas trwania.
4. Użyj klawiszy Ctrl + kliknięcie, aby wybrać maszyny w planie. Dodawanie lub usuwanie maszyn z mapy.
    - Można dodawać tylko te komputery, które zostały odnalezione.
    - Dodawanie i usuwanie urządzenia z grupy unieważnia poza ocen dla niego.
    - Po zmodyfikowaniu grupy, można opcjonalnie utworzyć nowy oceny.
5. Kliknij przycisk **OK** można zapisać grupy.

    ![Dodawanie i usuwanie](./media/how-to-create-group-dependencies/add-remove.png)

Jeśli chcesz sprawdzić zależności określonym komputerze, na którym zostanie wyświetlona na mapie zależności grupy [skonfigurowanie mapowania zależności maszyny](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
