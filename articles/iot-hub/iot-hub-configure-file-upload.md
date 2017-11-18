---
title: "Użyj portalu Azure, aby skonfigurować przekazywanie pliku | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować Centrum IoT umożliwia przekazywania plików z połączonych urządzeń przy użyciu portalu Azure. Zawiera informacje o konfigurowaniu docelowym kontem magazynu platformy Azure."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: 2d875947297be5d47362369b96bc6ab0d90b3c93
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Konfigurowanie Centrum IoT przekazywania plików przy użyciu portalu Azure

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Przekazywanie pliku

Aby użyć [plików funkcji przekazywania w Centrum IoT][lnk-upload], należy najpierw powiązać konta usługi Azure Storage w Centrum. Wybierz **przekazywania pliku** do wyświetlenia listy właściwości przekazywania plików do Centrum IoT, która jest modyfikowana.

![Przekazywanie pliku Centrum IoT widoku ustawień w portalu][13]

**Kontener magazynu**: Użyj portalu Azure, aby wybrać kontenera obiektów blob na koncie magazynu Azure w ramach bieżącej subskrypcji platformy Azure do skojarzenia z Centrum IoT. Jeśli to konieczne, tworzenia konta usługi Azure Storage na **kont magazynu** kontenera bloku oraz obiektów blob na **kontenery** bloku. Centrum IoT automatycznie generuje identyfikator URI sygnatury dostępu Współdzielonego z uprawnieniami do zapisu do tego kontenera obiektów blob dla urządzeń do użycia podczas ich przekazywania plików.

![Wyświetl kontenery magazynu dla przekazywania pliku w portalu][14]

**Odbieranie powiadomień dla przekazanych plików**: Włącza lub wyłącza powiadomienia przekazywanie pliku przez przełącznik.

**Czas wygaśnięcia SAS**: to ustawienie jest time-to-live identyfikatorów SAS zwrócony do urządzenia przez Centrum IoT. Domyślnie ustawiany na godzinę, ale można dostosować, aby inne wartości za pomocą suwaka.

**Plik powiadomienia, ustawienia domyślne TTL**: czas wygaśnięcia pliku Przekaż powiadomienia przed jego wygaśnięciem. Domyślnie ustawiany na jeden dzień, ale można dostosować, aby inne wartości za pomocą suwaka.

**Powiadomienie dostarczania maksymalna liczba plików**: liczba prób Centrum IoT dostarczyć plik Przekaż powiadomień. Domyślnie ustawiony na 10, ale można dostosować, aby inne wartości za pomocą suwaka.

![Konfigurowanie przekazywania pliku Centrum IoT w portalu][15]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat możliwości przekazywania plików Centrum IoT, zobacz [przekazywania plików z urządzeniem] [ lnk-upload] w Podręczniku dewelopera Centrum IoT.

Skorzystaj z poniższych linków, aby dowiedzieć się więcej o zarządzaniu Centrum IoT Azure:

* [Zbiorcze zarządzania urządzeniami IoT][lnk-bulk]
* [Metryki Centrum IoT][lnk-metrics]
* [Operacje monitorowania][lnk-monitor]

Aby dokładniej analizować możliwości Centrum IoT, zobacz:

* [Przewodnik dewelopera Centrum IoT][lnk-devguide]
* [Wdrażanie urządzenia brzegowe AI krawędzi IoT Azure][lnk-iotedge]
* [Zabezpieczanie rozwiązania IoT od podstaw w górę][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
