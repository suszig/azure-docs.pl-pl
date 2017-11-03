---
title: "Rozwiązywanie problemów podczas usuwania konta magazynu platformy Azure, w pojemnikach lub wirtualne dyski twarde | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów podczas usuwania konta magazynu platformy Azure, w pojemnikach lub wirtualne dyski twarde"
services: storage
documentationcenter: 
author: passaree
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 174ab97ac14f4c05690306691df5ee4b6d33dd93
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-storage-delete-errors-in-resource-manager-deployment"></a>Rozwiązywanie problemów z magazynu Usuń błędy we wdrożeniu usługi Resource Manager
Ten artykuł zawiera wskazówki dotyczące rozwiązywania problemów, gdy jeden z następujących błędów podczas próby usunięcia konta magazynu platformy Azure, kontenera lub obiektu blob we wdrożeniu usługi Azure Resource Manager.

>**Nie można usunąć konta magazynu "StorageAccountName". Błąd: Nie można usunąć konta magazynu z powodu jego artefakty są nadal używane.**

>**Nie można usunąć # poza kontenerów #:<br>wirtualne dyski twarde: obecnie jest dzierżawa na kontenerze i identyfikator dzierżawy nie został określony w żądaniu.**

>**Nie można usunąć # poza # obiektów blob:<br>BlobName.vhd: jest obecnie dzierżawy w obiekcie blob i identyfikator dzierżawy nie został określony w żądaniu.**

Wirtualne dyski twarde używane w maszynach wirtualnych platformy Azure są pliki VHD przechowywany jako stronicowe obiekty BLOB na koncie magazynu standard lub premium na platformie Azure.  Więcej informacji o dyskach platformy Azure można znaleźć [tutaj](../../virtual-machines/windows/about-disks-and-vhds.md). Azure uniemożliwia usuwanie dysku, który jest dołączony do maszyny Wirtualnej, aby zapobiec uszkodzeniu. Uniemożliwia także usunięcie kontenery i kont magazynu, które mają stronicowych obiektów blob, który jest dołączony do maszyny Wirtualnej. 

## <a name="solution"></a>Rozwiązanie
Proces usuwania konta magazynu, kontenera lub obiektu blob podczas odbierania jeden z powyższych błędów jest: 
1. [Zidentyfikować obiekty BLOB dołączony do maszyny Wirtualnej](#step-1-identify-blobs-attached-to-a-vm)
2. [Dołączony maszyn wirtualnych DELETE o **dysku systemu operacyjnego**](#step-2-delete-vm-to-detach-os-disk)
3. [Odłącz wszystkich **dyski danych** z pozostałych wirtualne](#step-3-detach-data-disk-from-the-vm)

Po ukończeniu powyższych kroków ponów usuwania konta magazynu, kontenera lub obiektu blob.

### <a name="step-1-identify-blob-attached-to-a-vm"></a>: Krok 1 obiektów blob dołączony do maszyny Wirtualnej

#### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Scenariusz 1: Usuwanie obiektu blob — zidentyfikować podłączone maszyny Wirtualnej
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **wszystkie zasoby**. Przejdź do konta magazynu, w obszarze **usługa Blob** wybierz **kontenery** i przejdź do obiektu blob do usunięcia.
3. Jeśli obiekt blob **stanu dzierżawy** jest **Leased**, kliknij prawym przyciskiem myszy i wybierz **metadanych Edytuj** aby otworzyć okienko metadane obiektu Blob. 

    ![Zrzut ekranu przedstawiający portal, z magazynem konta obiekty BLOB i kliknij prawym przyciskiem myszy > hilighted "Edytuj metadanych"](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-edit-metadata-sm.png)

4. W okienku Metadane obiektu Blob, sprawdź i Zapisz tę wartość dla **MicrosoftAzureCompute_VMName**. Ta wartość jest nazwą dołączonego wirtualnego dysku twardego do maszyny wirtualnej. (Zobacz **ważne** Jeśli to pole nie istnieje)
5. W okienku Metadane obiektu Blob, sprawdź i rejestrować wartości **MicrosoftAzureCompute_DiskType**. Identyfikuje jeśli dołączone dysk jest dyskiem systemu operacyjnego lub danych (zobacz **ważne** Jeśli to pole nie istnieje). 

     ![Zrzut ekranu przedstawiający portal, otwórz okienko "Metadane obiektu Blob" magazynu](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-blob-metadata-sm.png)

6. Jeśli jest typu obiektu blob dysku **OSDisk** wykonaj [krok 2: usuwanie maszyny Wirtualnej, aby odłączyć dysk systemu operacyjnego](#step-2-delete-vm-to-detach-os-disk). W przeciwnym razie, jeśli jest typu obiektu blob dysku **DataDisk** postępuj zgodnie z instrukcjami [krok 3: odłączyć dysku danych z maszyny Wirtualnej](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Jeśli **MicrosoftAzureCompute_VMName** i **MicrosoftAzureCompute_DiskType** nie zostaną wyświetlone metadane obiektu blob wskazuje, czy obiekt blob jest jawnie dzierżawiony i nie jest dołączony do maszyny Wirtualnej. Nie można usunąć dzierżawionych obiekty BLOB bez przerywania pierwszy dzierżawy. Aby przerwać dzierżawy, kliknij prawym przyciskiem myszy obiekt blob i wybierz **dzierżawy podziału**. Dzierżawionych obiektów blob, które nie są dołączone do maszyn wirtualnych zapobiec usunięciu obiektu blob, ale nie uniemożliwiają usunięcie konta kontener lub magazynu.

#### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Scenariusz 2: Usuwanie kontenera - zidentyfikować wszystkie blob(s) w kontenerze, które są dołączone do maszyn wirtualnych
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **wszystkie zasoby**. Przejdź do konta magazynu, w obszarze **usługa Blob** wybierz **kontenery** i znaleźć kontenera do usunięcia.
3. Kliknij, aby otworzyć kontenera i zostanie wyświetlona lista obiektów blob w nim. Zidentyfikuj wszystkie obiekty BLOB z typem obiektu Blob = **stronicowych obiektów blob** i stanu dzierżawy = **Leased** z tej listy. Postępuj zgodnie z [scenariusz 1](#step-1-identify-blobs-attached-to-a-vm) do identyfikowania maszyny Wirtualnej skojarzone z każdą z tych obiektów blob.

    ![Zrzut ekranu przedstawiający portal, obiektów blob z konta magazynu i "Dzierżawy stanu" z "Leased" wyróżnione](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-disks-sm.png)

4. Postępuj zgodnie z [krok 2](#step-2-delete-vm-to-detach-os-disk) i [kroku 3](#step-3-detach-data-disk-from-the-vm) usunąć wirtualne z **OSDisk** i odłączania **DataDisk**. 

#### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Scenariusz 3: Usuwanie magazynu konta — zidentyfikować wszystkie blob(s) w ramach konta magazynu, które są dołączone do maszyn wirtualnych
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **wszystkie zasoby**. Przejdź do konta magazynu, w obszarze **usługa Blob** wybierz **kontenery**.

    ![Zrzut ekranu przedstawiający portal, kontenery konta magazynu i "Dzierżawy stanu" z "Leased" wyróżnione](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-containers-sm.png)

3. W **kontenery** bloku zidentyfikować wszystkich kontenerów gdzie **stanu dzierżawy** jest **Leased** i postępuj zgodnie z [2 scenariusza](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) dla każdego  **Wydzierżawionym** kontenera.
4. Postępuj zgodnie z [krok 2](#step-2-delete-vm-to-detach-os-disk) i [kroku 3](#step-3-detach-data-disk-from-the-vm) usunąć wirtualne z **OSDisk** i odłączania **DataDisk**. 

### <a name="step-2-delete-vm-to-detach-os-disk"></a>Krok 2: Usuwanie maszyny Wirtualnej można odłączyć dysku systemu operacyjnego
Jeśli wirtualny dysk twardy, dysk systemu operacyjnego, należy usunąć maszynę Wirtualną przed usunięciem dołączonego wirtualnego dysku twardego. Żadne dodatkowe działania jest wymagana w przypadku dyskach danych dołączonych do tej samej maszyny Wirtualnej, po wykonaniu tych kroków:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **maszyn wirtualnych**.
3. Wybierz wirtualny dysk twardy dołączony do maszyny Wirtualnej.
4. Upewnij się, że nic nie jest aktywnie przy użyciu maszyny wirtualnej i nie są już potrzebne maszyny wirtualnej.
5. W górnej części **szczegóły maszyny wirtualnej** bloku, wybierz opcję **usunąć**, a następnie kliknij przycisk **tak** o potwierdzenie.
6. Maszyna wirtualna powinien zostać usunięty, ale wirtualnego dysku twardego mogą być przechowywane. Jednak dysku VHD nie powinien być dołączony do maszyny Wirtualnej lub ma dzierżawę na nim. Może upłynąć kilka minut, aż dzierżawa do zwolnienia. Aby sprawdzić zwolnieniu dzierżawy, przejdź do lokalizacji obiektu blob i w **właściwości obiektu Blob** okienku **stanu dzierżawy** powinien być **dostępne**.

### <a name="step-3-detach-data-disk-from-the-vm"></a>Krok 3: Odłączyć dysku danych z maszyny Wirtualnej
Jeśli wirtualny dysk twardy jest dysk z danymi, odłącz wirtualny dysk twardy z maszyny Wirtualnej, aby usunąć dzierżawy:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu Centrum wybierz **maszyn wirtualnych**.
3. Wybierz wirtualny dysk twardy dołączony do maszyny Wirtualnej.
4. Wybierz **dysków** na **szczegóły maszyny wirtualnej** bloku.
5. Wybierz dysk danych do usunięcia wirtualny dysk twardy jest podłączony do. Można określić, które obiekt blob jest dołączony na dysku, sprawdzając adres URL dysku VHD.
6. Sprawdź lokalizacji obiektu blob, klikając na dysku, aby zaewidencjonować ścieżka **identyfikator URI dysku VHD** pola.
7. Wybierz **Edytuj** na wierzchu z **dysków** bloku.
8. Kliknij przycisk **odłączyć ikona** dysku danych do usunięcia.

     ![Zrzut ekranu przedstawiający portal, otwórz okienko "Metadane obiektu Blob" magazynu](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-vm-disks-edit.png)

9. Wybierz pozycję **Zapisz**. Dysk powinien teraz można odłączyć od maszyny Wirtualnej, i wirtualnego dysku twardego nie może mieć dzierżawę na nim. Może upłynąć kilka minut, aż dzierżawa do zwolnienia. Aby sprawdzić zwolnienie dzierżawy, przejdź do lokalizacji obiektu blob i w **właściwości obiektu Blob** okienku **stanu dzierżawy** wartość powinna być **odblokowany** lub **Dostępne**.

## <a name="next-steps"></a>Następne kroki
Ponów próbę usunięcia obiektu magazynu, który wcześniej nie powiodło się.

