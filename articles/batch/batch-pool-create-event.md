---
title: "Azure puli partii utworzyć zdarzenia | Dokumentacja firmy Microsoft"
description: "Dokumentacja dotycząca puli partii utworzyć zdarzenia."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 67edaa55d7ccd00d4aebb309f11bcf95486e87fa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="pool-create-event"></a>Tworzenie puli zdarzenia

 To zdarzenie jest emitowany po utworzeniu puli. Ogólne informacje o puli powoduje to udostępnienie zawartości dziennika. Należy pamiętać, że jeśli rozmiar docelowy puli jest większa niż 0 węzły obliczeniowe, start zdarzenie zmiany rozmiaru puli zastosują się natychmiast po tym zdarzeniu.

 W poniższym przykładzie przedstawiono treści puli utworzyć zdarzenia dla puli utworzone za pomocą właściwości CloudServiceConfiguration.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Element|Typ|Uwagi|
|-------------|----------|-----------|
|id|Ciąg|Identyfikator puli.|
|Nazwa wyświetlana|Ciąg|Nazwa wyświetlana puli.|
|vmSize|Ciąg|Rozmiar maszyn wirtualnych w puli. Wszystkie maszyny wirtualne w puli mają taki sam rozmiar. <br/><br/> Aby uzyskać informacje o dostępnych rozmiarów maszyn wirtualnych dla usług w chmurze pule (pule utworzone za pomocą cloudServiceConfiguration), zobacz [rozmiary dla usług w chmurze](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Wsadowe obsługuje wszystkich rozmiarów maszyn wirtualnych usługi w chmurze, z wyjątkiem `ExtraSmall`.<br/><br/> Informacje o dostępności VM rozmiary pule przy użyciu obrazów z witryny Marketplace maszyn wirtualnych (pule utworzone za pomocą virtualMachineConfiguration) dla [rozmiary maszyn wirtualnych](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) lub [rozmiary maszyn wirtualnych](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (system Windows). Usługa Batch obsługuje wszystkie rozmiary maszyn wirtualnych platformy Azure oprócz `STANDARD_A0` i maszyn z usługi Premium Storage (seria `STANDARD_GS`, `STANDARD_DS` i `STANDARD_DSV2`).|
|[cloudServiceConfiguration](#bk_csconf)|Typ złożony|Konfiguracja usługi chmury dla puli.|
|[virtualMachineConfiguration](#bk_vmconf)|Typ złożony|Konfiguracja maszyny wirtualnej w puli.|
|[Konfiguracja sieci](#bk_netconf)|Typ złożony|Konfigurację sieci dla tej puli.|
|resizeTimeout|Time|Limit czasu alokacji węzłów obliczeniowych do puli, określony dla ostatniej operacji zmiany rozmiaru w puli.  (Początkowej zmiany rozmiaru po utworzeniu puli traktowana jako zmiany rozmiaru.)|
|targetDedicated|Int32|Liczba węzłów obliczeniowych, które są wymagane dla puli.|
|enableAutoScale|wartość logiczna|Określa, czy rozmiar puli automatycznie dostosowuje się wraz z upływem czasu.|
|enableInterNodeCommunication|wartość logiczna|Określa, czy pula jest skonfigurowane do bezpośrednia komunikacja między węzłami.|
|isAutoPool|wartość logiczna|Speficies czy puli został utworzony za pomocą mechanizmu AutoPool zadania.|
|maxTasksPerNode|Int32|Maksymalna liczba zadań, które można uruchomić jednocześnie w jednym węźle obliczeń w puli.|
|vmFillType|Ciąg|Definiuje sposób rozdzielania zadań przez usługę Partia zadań między węzły obliczeń w puli. Prawidłowe wartości są rozkładane lub pakiecie.|

###  <a name="bk_csconf"></a>cloudServiceConfiguration

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|Rodzina systemów operacyjnych|Ciąg|Rodzina systemu operacyjnego gościa Azure, do zainstalowania na maszynach wirtualnych w puli.<br /><br /> Możliwe wartości:<br /><br /> **2** — OS rodziny 2, odpowiednikiem systemu Windows Server 2008 R2 z dodatkiem SP1.<br /><br /> **3** — 3 rodziny systemu operacyjnego, odpowiednikiem systemu Windows Server 2012.<br /><br /> **4** — 4 rodziny systemu operacyjnego, odpowiednikiem systemu Windows Server 2012 R2.<br /><br /> Aby uzyskać więcej informacji, zobacz [poszczególnych wersji systemu operacyjnego gościa Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Ciąg|Wersja systemu operacyjnego gościa Azure do zainstalowania na maszynach wirtualnych w puli.<br /><br /> Wartość domyślna to  **\***  określającej najnowszej wersji systemu operacyjnego dla określonej rodziny.<br /><br /> Dla innych dozwolonych wartości [poszczególnych wersji systemu operacyjnego gościa Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a>virtualMachineConfiguration

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|[elementu imageReference](#bk_imgref)|Typ złożony|Określa informacje dotyczące platformy lub obrazu do użycia z witryny Marketplace.|
|nodeAgentSKUId|Ciąg|Jednostka SKU agenta węzła partii udostępniane w węźle obliczeń.|
|[windowsConfiguration](#bk_winconf)|Typ złożony|Określa ustawienia systemu operacyjnego Windows na maszynie wirtualnej. Ta właściwość nie może być określony, jeśli elementu imageReference odwołuje się do obrazu systemu operacyjnego Linux.|

###  <a name="bk_imgref"></a>elementu imageReference

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|Wydawcy|Ciąg|Wydawca obrazu.|
|Oferta|Ciąg|Oferta obrazu.|
|Jednostka SKU|Ciąg|Jednostka SKU obrazu.|
|Wersja|Ciąg|Wersja obrazu.|

###  <a name="bk_winconf"></a>windowsConfiguration

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|enableAutomaticUpdates|Wartość logiczna|Wskazuje, czy maszyny wirtualnej jest włączona funkcja Aktualizacje automatyczne. Jeśli ta właściwość nie jest określony, wartość domyślna to true.|

###  <a name="bk_netconf"></a>Konfiguracja sieci

|Nazwa elementu|Typ|Uwagi|
|------------------|--------------|----------|
|subnetId|Ciąg|Określa identyfikator zasobu podsieci, w którym są tworzone węzłów obliczeniowych w puli.|
