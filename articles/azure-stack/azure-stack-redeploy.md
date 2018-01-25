---
title: "Należy ponownie wdrożyć stosu Azure | Dokumentacja firmy Microsoft"
description: "Należy ponownie wdrożyć Azure stosu."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 0dec5ea70376ff1c8cf488689f1a66190256f6ff
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="redeploy-azure-stack"></a>Należy ponownie wdrożyć Azure stosu
Jeśli wystąpi błąd podczas wdrażania usługi Azure stosu, można ponownie uruchomić Instalatora przy użyciu następującego polecenia programu PowerShell: `.\InstallAzureStackpoc.ps1 -rerun`. To polecenie będzie ponownie uruchomić Instalatora Azure stosu w momencie, który nie może on wcześniej bez konieczności rozpoczynania od początku. Jeśli zostanie wyświetlony ten sam błąd instalacji może być konieczne przeprowadzenie pełnej ponownego wdrażania na adres problem. 

Aby ponownie wdrożyć stosu Azure, należy uruchomić za pośrednictwem od początku zgodnie z poniższym opisem.

## <a name="steps-to-redeploy-azure-stack"></a>Kroki, aby ponownie wdrożyć Azure stosu
1. Na hoście development kit Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień > Przejdź do skryptu asdk installer.ps1 > Uruchom go > kliknij **ponowny rozruch**.
2. Wybierz podstawowego systemu operacyjnego (nie **stosu Azure**) i kliknij przycisk **dalej**.
3. Po ponownym uruchomieniu hosta zestawu programowanie, usuń plik CloudBuilder.vhdx, który był używany w ramach poprzedniego wdrożenia.
4. [Wdrażanie zestaw deweloperski](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Kolejne kroki
[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)

