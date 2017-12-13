---
title: "Uściślij grupy oceny pomocą mapowania grup zależności w migracji Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób uściślić oceny w usłudze Azure migracji za pomocą mapowania grup zależności."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: c30d6546e7c2d471d4b262a8af1ce593b2c1c3fb
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
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
