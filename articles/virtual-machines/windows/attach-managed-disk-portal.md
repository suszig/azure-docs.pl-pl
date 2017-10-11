---
title: "Dołączenie dysku danych zarządzanych do maszyny Wirtualnej systemu Windows - Azure | Dokumentacja firmy Microsoft"
description: "Jak można dołączyć nowego dysku danych zarządzanych do maszyny Wirtualnej systemu Windows w portalu Azure przy użyciu modelu wdrażania Menedżera zasobów."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: f0cf88a06c5470ef173b22e7213419a6c8760723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Jak można dołączyć dysku danych zarządzanych do maszyny Wirtualnej systemu Windows, w portalu Azure

W tym artykule przedstawiono sposób dołączyć nowego dysku danych zarządzanych do maszyn wirtualnych z systemem Windows za pośrednictwem portalu Azure. Zanim to zrobisz, przejrzyj następujące wskazówki:

* Rozmiar maszyny wirtualnej Określa, ile można dołączać dysków z danymi. Aby uzyskać więcej informacji, zobacz [rozmiary maszyn wirtualnych](sizes.md).
* Dla nowego dysku nie trzeba go najpierw utworzyć ponieważ Azure tworzy go po dołączeniu go.

Możesz również [dołączenie dysku danych przy użyciu programu Powershell](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Dodaj dysk danych
1. W menu po lewej stronie kliknij **maszyn wirtualnych**.
2. Wybierz maszynę wirtualną z listy.
3. W bloku maszyny wirtualnej, kliknij polecenie **dysków**.
   4. Na **dysków** bloku, kliknij przycisk **+ Dodaj dysk danych**.
5. Z listy rozwijanej dla nowego dysku, wybierz **utworzyć pusty**.
6. W **dysków zarządzanych w Utwórz** bloku, wpisz nazwę dla dysku i Dostosuj pozostałe ustawienia odpowiednio do potrzeb. Gdy wszystko będzie gotowe, kliknij przycisk **Utwórz**.
7. W **dysków** bloku, kliknij przycisk Zapisz, aby zapisać nową konfigurację dysku dla maszyny Wirtualnej.
6. Po Azure utworzy dysk i dołącza go do maszyny wirtualnej, nowy dysk ma na liście ustawień dysku maszyny wirtualnej w obszarze **dysków z danymi**.


## <a name="initialize-a-new-data-disk"></a>Zainicjuj nowy dysk danych

1. Połączenie z maszyną Wirtualną.
1. Kliknij menu start wewnątrz maszyny Wirtualnej i typ **diskmgmt.msc** i trafień **Enter**. Spowoduje to uruchomienie przystawki Zarządzanie dyskami.
2. Zarządzanie dyskami rozpozna, że znajduje się dysk nowy, które nie zostały zainicjowane i będzie wyskakujące okno Inicjowanie dysku.
3. Upewnij się, że wybrano nowy dysk i kliknij przycisk **OK** można go zainicjować.
4. Nowy dysk będą teraz wyświetlane jako **nieprzydzielonego**. Kliknij prawym przyciskiem myszy w dowolnym miejscu na dysku i wybierz **nowy wolumin prosty**. **Kreatorze nowych woluminów prostych** zostanie uruchomiona.
5. Przejdź przez kreatora, przechowywanie ustawień domyślnych, po zakończeniu wybierz **Zakończ**.
6. Zamknij program Zarządzanie dyskami.
7. Zostanie wyświetlone okno podręczne, które należy sformatować nowy dysk przed jego użyciem. Kliknij przycisk **Format dysku**.
8. W **Format nowy dysk** okna dialogowego, sprawdź ustawienia, a następnie kliknij przycisk **Start**.
9. Zostanie wyświetlone ostrzeżenie, że formatowanie dysków spowoduje wymazanie wszystkich danych, kliknij przycisk **OK**.
10. Po zakończeniu format kliknij **OK**.

## <a name="use-trim-with-standard-storage"></a>PRZYCINANIE za pomocą magazynu w warstwie standardowa

Jeśli używasz standardowego magazynu (HDD), należy włączyć PRZYCINANIE. PRZYCINANIE odrzuca nieużywanych bloków na dysku dzięki rozliczenie jest przeprowadzane tylko dla magazynu, który faktycznie używasz. Można to zapisanie kosztów, jeśli Tworzenie dużych plików, a następnie usuń je. 

Można uruchomić tego polecenia, aby sprawdzić ustawienie PRZYCINANIA. Otwórz wiersz polecenia na Twojej maszyny Wirtualnej systemu Windows i wpisz:

```
fsutil behavior query DisableDeleteNotify
```

Jeśli polecenie zwróci wartość 0, PRZYCINANIE jest prawidłowo włączona. Jeśli zmienna zwraca 1, uruchom następujące polecenie, aby umożliwić PRZYCINANIE:
```
fsutil behavior set DisableDeleteNotify 0
```

Po usunięciu danych z dysku, można zapewnić PRZYCINANIA operacje opróżniania poprawnie, uruchamiając defragmentowania z operacją PRZYCINANIA:

```
defrag.exe <volume:> -l
```

Ponadto upewnij się, że cały wolumin jest ograniczona przez formatowania woluminu.

## <a name="next-steps"></a>Następne kroki
Jeśli użytkownik aplikacji musi używać D: dysków do przechowywania danych, możesz [zmienić literę dysku tymczasowym systemu Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
