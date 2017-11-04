---
title: Informacje o obrazach dla maszyn wirtualnych z systemem Windows | Dokumentacja firmy Microsoft
description: "Więcej informacji na temat używania obrazów maszyn wirtualnych systemu Windows na platformie Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: d421cee0becabdf81d865036d0c98b12b077152b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="about-images-for-windows-virtual-machines"></a>Informacje o obrazach dla maszyn wirtualnych systemu Windows
> [!IMPORTANT]
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Informacje o wyszukiwanie i używanie obrazów w modelu usługi Resource Manager, zobacz [tutaj](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Praca z obrazami

Moduł Azure PowerShell i portalu Azure można użyć do zarządzania dostępnych obrazów do subskrypcji platformy Azure. Moduł Azure PowerShell zawiera więcej opcji polecenia, więc będzie można wyznaczyć dokładnie co chcesz wyświetlić, lub czy. Azure portal udostępnia graficznego interfejsu użytkownika dla wielu codziennych zadań administracyjnych.

Oto kilka przykładów, korzystających z modułu Azure PowerShell.

* **Pobierz wszystkie obrazy**:`Get-AzureVMImage`zwraca listę wszystkich dostępnych w ramach Twojej bieżącej subskrypcji obrazów: obrazów i udostępnianych przez zespół Azure lub partnerów. Wynikowa lista może być duży. Następny przykłady przedstawiają metody w celu uzyskania listy krótszy.
* **Pobierz obraz rodzin**:`Get-AzureVMImage | select ImageFamily` pobiera listę rodzin obrazu pokazując ciągów **ImageFamily** właściwości.
* **Pobierz wszystkie obrazy w określonej rodzinie**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **Znajdź obrazów maszyn wirtualnych**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` działa to polecenie cmdlet filtrując właściwość DataDiskConfiguration, która ma zastosowanie tylko do obrazów maszyn wirtualnych. W tym przykładzie filtruje także dane wyjściowe do tylko nazwę etykiety i obrazów.
* **Zapisz uogólniony obraz**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Zapisz obraz specjalne**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`

  > [!TIP]
  > Parametr OSState jest wymagana do utworzenia obrazu maszyny Wirtualnej, która zawiera dysku systemu operacyjnego i dołączone dyski danych. Jeśli parametr nie jest używany, polecenie cmdlet tworzy obrazu systemu operacyjnego. Wartość parametru wskazuje, czy uogólniony obraz lub specjalizowany, w zależności od tego, czy dysk systemu operacyjnego został przygotowany do ponownego użycia.

* **Usuń obraz**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Następne kroki
Możesz również [Utwórz maszynę z systemem Windows przy użyciu portalu Azure](tutorial.md).
