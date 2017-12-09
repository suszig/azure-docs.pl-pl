---
title: "Utwórz i Przekaż obraz maszyny Wirtualnej przy użyciu programu Powershell | Dokumentacja firmy Microsoft"
description: "Dowiedz się utworzyć i przekazać uogólniony obraz systemu Windows Server (VHD) przy użyciu klasycznego modelu wdrażania i programu Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: fa672fd7811e68368c311ef6a3f57eac4b240a4a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Tworzenie wirtualnego dysku twardego systemu Windows Server i przekazywanie go na platformę Azure
W tym artykule przedstawiono sposób przekazywania własnego uogólniony obraz maszyny Wirtualnej jako wirtualny dysk twardy (VHD), aby można było ich użyć do utworzenia maszyny wirtualnej. Aby uzyskać więcej informacji o dyskach i wirtualne dyski twarde w systemie Microsoft Azure, zobacz [o dyski i wirtualne dyski twarde dla maszyn wirtualnych](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Możesz również [przekazać](../upload-generalized-managed.md) maszynę wirtualną przy użyciu modelu Resource Manager.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule przyjęto założenie, że masz:

* **Subskrypcja platformy Azure** — Jeśli nie masz, możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[Microsoft Azure PowerShell](/powershell/azure/overview)**  -mają modułu Microsoft Azure PowerShell zainstalowane i skonfigurowane do korzystania z subskrypcji.
* **A. Plik VHD** — obsługiwane systemu operacyjnego przechowywanego w pliku VHD i podłączony do maszyny wirtualnej Windows. Sprawdź, czy ról serwera uruchomionych na dysku VHD są obsługiwane przez program Sysprep. Aby uzyskać więcej informacji, zobacz [Obsługa programu Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

    > [!IMPORTANT]
    > VHDX format nie jest obsługiwany w systemie Microsoft Azure. Dysk można przekonwertować na format wirtualnego dysku twardego za pomocą Menedżera funkcji Hyper-V lub [polecenia cmdlet Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx). Aby uzyskać więcej informacji, zobacz [wpis w blogu](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Krok 1: Przygotowywanie wirtualnego dysku twardego
Przed przekazaniem dysku VHD na platformę Azure, musi być uogólniony przy użyciu narzędzia Sysprep. — Przygotowanie do użycia jako obraz dysku VHD. Aby uzyskać więcej informacji o narzędziu Sysprep, zobacz [sposobu użycia programu Sysprep: wprowadzenie](http://technet.microsoft.com/library/bb457073.aspx). Utwórz kopię zapasową maszyny Wirtualnej przed uruchomieniem programu Sysprep.

Z poziomu maszyny wirtualnej, który został zainstalowany system operacyjny, aby wykonaj następującą procedurę:

1. Zaloguj się do systemu operacyjnego.
2. Otwórz okno wiersza polecenia z uprawnieniami administratora. Zmień katalog na **%windir%\system32\sysprep**, a następnie uruchom `sysprep.exe`.

    ![Otwórz okno wiersza polecenia](./media/createupload-vhd/sysprep_commandprompt.png)
3. **Narzędzie przygotowania systemu** zostanie wyświetlone okno dialogowe.

   ![Uruchom program Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. W **narzędzie przygotowania systemu**, wybierz pozycję **wprowadź systemu poza środowisko pola (OOBE)** i upewnij się, że **Generalize** jest zaznaczony.
5. W **opcje zamykania**, wybierz pozycję **zamknięcia**.
6. Kliknij przycisk **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Krok 2: Tworzenie konta magazynu i kontener
Należy korzystać z konta magazynu na platformie Azure tak ma miejsce, aby przekazać plik VHD. W tym kroku przedstawiono, jak utworzyć konto lub uzyskać potrzebne informacje z istniejącego konta. Zastąp zmienne &lsaquo; nawiasy &rsaquo; odpowiednimi informacjami.

1. Login

    ```powershell
    Add-AzureAccount
    ```

2. Ustaw subskrypcji platformy Azure.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Utwórz nowe konto magazynu. Nazwa konta magazynu powinna być unikatowa, 3 do 24 znaków. Nazwa może być dowolną kombinacją liter i cyfr. Należy również określić lokalizację, takich jak "Wschód nam"

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Nowe konto magazynu należy ustawić jako domyślny.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Utwórz nowy kontener.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-the-vhd-file"></a>Krok 3: Przekaż plik VHD
Użyj [Add-AzureVhd](https://docs.microsoft.com/powershell/module/azure/add-azurevhd) na przekazanie dysku VHD.

W oknie programu Azure PowerShell, używany w poprzednim kroku, wpisz następujące polecenie i Zastąp zmienne &lsaquo; nawiasy &rsaquo; odpowiednimi informacjami.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Krok 4: Dodawanie obrazu do listy niestandardowych obrazów
Użyj [AzureVMImage Dodaj](https://docs.microsoft.com/powershell/module/azure/add-azurevmimage) polecenia cmdlet, aby dodać obraz do listy obrazów niestandardowych.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Następne kroki
Możesz teraz [Tworzenie niestandardowych maszyny Wirtualnej](createportal.md) przy użyciu obrazu został przekazany.
