---
title: "Przekaż plik VHD na usłudze Azure DevTest Labs przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Przekaż plik wirtualnego dysku twardego na konto magazynu w laboratorium przy użyciu programu PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: v-craic
ms.openlocfilehash: c940f5646bc7845db7b774d7ada40b8105534d69
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>Przekaż plik wirtualnego dysku twardego na konto magazynu w laboratorium przy użyciu programu PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

W usłudze Azure DevTest Labs pliki wirtualnego dysku twardego może służyć do tworzenia niestandardowych obrazów, które są używane do obsługi administracyjnej maszyn wirtualnych. W poniższych krokach objaśniono przy użyciu programu PowerShell, aby przekazać plik wirtualnego dysku twardego do konta magazynu laboratorium. Po przesłaniu pliku wirtualnego dysku twardego [następne kroki sekcji](#next-steps) wymieniono niektóre artykuły, które przedstawiają sposób tworzenia niestandardowego obrazu z przekazanego pliku wirtualnego dysku twardego. Aby uzyskać więcej informacji o dyskach i wirtualne dyski twarde na platformie Azure, zobacz [o dyskach i wirtualne dyski twarde dla maszyn wirtualnych](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Instrukcje krok po kroku

W poniższych krokach objaśniono za pośrednictwem przekazywania pliku wirtualnego dysku twardego Azure DevTest Labs przy użyciu programu PowerShell. 

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Wybierz **wszystkie usługi**, a następnie wybierz **DevTest Labs** z listy.

1. Z listy labs wybierz żądany laboratorium.  

1. W bloku laboratorium, wybierz **konfiguracji**. 

1. W środowisku laboratoryjnym **konfiguracji** bloku, wybierz opcję **niestandardowych obrazów (VHD)**.

1. Na **niestandardowych obrazów** bloku, wybierz **+ Dodaj**. 

1. Na **obraz niestandardowy** bloku, wybierz opcję **wirtualnego dysku twardego**.

1. Na **wirtualnego dysku twardego** bloku, wybierz opcję **przekazania dysku VHD za pomocą programu PowerShell**.

    ![Przekaż plik VHD za pomocą programu PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Na **przekazywanie obrazu za pomocą programu PowerShell** bloku, skopiuj wygenerowany skrypt programu PowerShell do edytora tekstu.

1. Modyfikowanie **LocalFilePath** parametr **Add-AzureVhd** polecenia cmdlet, aby wskazać lokalizację pliku wirtualnego dysku twardego, który chcesz przekazać.

1. W wierszu polecenia programu PowerShell, uruchom **Add-AzureVhd** polecenia cmdlet (z zmodyfikowanych **LocalFilePath** parametru).

> [!WARNING] 
> 
> Proces przekazywania pliku wirtualnego dysku twardego może być długi w zależności od rozmiaru pliku wirtualnego dysku twardego i szybkość połączenia.

## <a name="next-steps"></a>Kolejne kroki

- [Utworzyć obraz niestandardowy w usłudze Azure DevTest Labs z pliku VHD za pomocą portalu Azure](devtest-lab-create-template.md)
- [Utworzyć obraz niestandardowy w usłudze Azure DevTest Labs z pliku VHD za pomocą programu PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
