---
title: "Należy ponownie wdrożyć stosu Azure | Dokumentacja firmy Microsoft"
description: "Należy ponownie wdrożyć Azure stosu."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: erikje
ms.openlocfilehash: 891cde9b16bbbb51729129b6ad7a0f3794307baa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="redeploy-azure-stack"></a>Należy ponownie wdrożyć Azure stosu
Aby ponownie wdrożyć stosu Azure, należy uruchomić za pośrednictwem od początku zgodnie z poniższym opisem.

## <a name="steps-to-redeploy-azure-stack"></a>Kroki, aby ponownie wdrożyć Azure stosu
1. Na hoście development kit Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień > Przejdź do skryptu asdk installer.ps1 > Uruchom go > kliknij **ponowny rozruch**.
2. Wybierz podstawowego systemu operacyjnego (nie **stosu Azure**) i kliknij przycisk **dalej**.
3. Po ponownym uruchomieniu hosta zestawu programowanie, usuń plik CloudBuilder.vhdx, który był używany w ramach poprzedniego wdrożenia.
4. [Wdrażanie zestaw deweloperski](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Następne kroki
[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)

