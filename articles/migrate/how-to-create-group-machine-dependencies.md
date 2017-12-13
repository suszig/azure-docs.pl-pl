---
title: "Grupowanie maszyn przy użyciu zależności maszyny w usłudze Azure migracji | Dokumentacja firmy Microsoft"
description: "Opisuje sposób tworzenia ocenę przy użyciu zależności komputera z usługą Azure migracji."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 769c05916de4e7ad5b14812c2c8dbcf69e91320c
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Grupy maszyn przy użyciu mapowania zależności maszyny

W tym artykule opisano sposób tworzenia grupy komputerów dla [migracji Azure](migrate-overview.md) ocena przy użyciu mapowania zależności maszyny. Jeśli chcesz ocenić grupy maszyn wirtualnych z wyższego poziomu zaufania krzyżowe sprawdzanie z zależnościami maszyny przed uruchomieniem ocenę zwykle Użyj tej metody.



## <a name="prepare-machines-for-dependency-mapping"></a>Przygotowanie maszyny do mapowania zależności
Aby uwzględnić maszyny w mapowaniu zależności, musisz pobrać i zainstalować agentów na każdej maszynie lokalnej, która ma zostać oceniona. Ponadto, jeśli masz maszyny bez łączności z Internetem, musisz pobrać i zainstalować [bramy OMS](../log-analytics/log-analytics-oms-gateway.md) na nich.

### <a name="download-and-install-the-vm-agents"></a>Pobierz i zainstaluj agentów maszyny Wirtualnej
1. W **omówienie**, kliknij przycisk **Zarządzaj** > **maszyny**i wybierz wymagany komputer.
2. W **zależności** kolumny, kliknij przycisk **zainstalowani agenci**. 
3. Na **zależności** strony, Pobierz i zainstaluj program Microsoft Monitoring Agent (MMA) i agenta zależności na każdej maszynie Wirtualnej, które chcesz ocenić.
4. Skopiuj identyfikator i klucz. Należy je po zainstalowaniu MMA na maszynie lokalnej.

### <a name="install-the-mma"></a>Zainstaluj MMA

Aby zainstalować agenta na komputerze z systemem Windows:

1. Kliknij dwukrotnie pobrany agenta.
2. Na **powitalnej** kliknij przycisk **dalej**. Na **postanowień licencyjnych** kliknij przycisk **zgadzam się** do akceptowania licencji.
3. W **Folder docelowy**, Zachowaj lub zmienić domyślny folder instalacji > **dalej**. 
4. W **opcje instalacji agenta**, wybierz pozycję **Analiza dzienników Azure (OMS)** > **dalej**. 
5. Kliknij przycisk **Dodaj** Aby dodać nowy obszar roboczy OMS. Wklej identyfikator i klucz, który został skopiowany z portalu. Kliknij przycisk **Dalej**.


Aby zainstalować agenta na komputerze z systemem Linux:

1. Transfer odpowiedniego pakietu (x86 lub x64) na komputerze systemu Linux przy użyciu scp/sftp.
2. Instalowanie pakietu przy użyciu argumentu--instalacji.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Zainstaluj agenta zależności
1. Aby zainstalować agenta zależności na komputerze z systemem Windows, kliknij dwukrotnie plik konfiguracji i wykonaj instrukcje kreatora.
2. Aby zainstalować agenta zależności na komputerze z systemem Linux, zainstalować jako główny przy użyciu następującego polecenia:

    ```sh InstallDependencyAgent-Linux64.bin```

[Dowiedz się więcej](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) o systemach operacyjnych obsługiwanych przez agenta zależności. 

## <a name="create-a-group"></a>Utwórz grupę

1. Po zainstalowaniu agentów, przejdź do portalu i kliknij przycisk **Zarządzaj** > **maszyny**.
2. **Zależności** kolumny powinny być teraz wyświetlane jako **zależności widoku**. Kliknij kolumnę, aby wyświetlić zależności.
3. Dla każdego komputera można sprawdzić:
    - Określa, czy MMA i agentem zależności są zainstalowane i czy został odnaleziony komputer.
    - System operacyjny gościa uruchomionych na komputerze.
    - Przychodzące i wychodzące połączenia IP i portów.
    - Procesy uruchomione na maszynach.
    - Zależności między komputerami.

4. Do zapewnienia jeszcze bardziej precyzyjnej zależności kliknij zakres czasu, aby go zmodyfikować. Domyślnie zakres jest godzinę. Możesz zmodyfikować zakres czasu lub określ rozpoczęcia oraz datę zakończenia oraz czas trwania.
5. Po wyłaniają zależnych maszyny, które będą grupowane, wybrać maszyny na mapie i kliknij przycisk **grupowanie maszyn**.
6. Określ nazwę grupy. Upewnij się, że komputer jest wykrywane przez Azure migracji. Jeśli nie jest ona ponownie uruchom odnajdywania procesu lokalną. Oceny można uruchomić natychmiast, jeśli chcesz.
7. Kliknij przycisk **OK** można zapisać grupy.

    ![Utwórz grupę z zależnościami maszyny](./media/how-to-create-group-machine-dependencies/create-group.png)

## <a name="next-steps"></a>Następne kroki

- [Dowiedz się, jak](how-to-create-group-dependencies.md) uściślić grupy sprawdzając grupy zależności
- [Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
