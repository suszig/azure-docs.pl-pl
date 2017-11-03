---
title: "— Szybki Start Azure stosu Development Kit wdrożenia | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć Azure stosu Development Kit"
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/10/2017
ms.author: erikje
ms.custom: mvc
ms.openlocfilehash: 3d3f31775ae9384faeae20f108b71cea5dcc2934
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-development-kit-deployment-quickstart"></a>Wdrażanie zestawu Azure Stack Development Kit — przewodnik Szybki start

*Dotyczy: Azure stosu Development Kit*

[Azure stosu Development Kit](azure-stack-poc.md) jest środowiskiem badań i rozwoju, które można wdrożyć do oceny i Wykaż stosu Azure funkcji i usług. Można go pobrać działa prawidłowo, należy przygotować środowisko sprzętu i uruchamiać skrypty, niektóre (to potrwa kilka godzin). Po wykonaniu tej można Zaloguj się do portali administratora i użytkownika do zarządzania stosu Azure i przetestować oferty. 

1. [**Planowanie sprzętu, oprogramowania i sieci**](azure-stack-deploy.md). Komputer, który obsługuje zestaw deweloperski (Programowanie hosta kit) musi spełniać sprzętu, oprogramowania i wymagania dotyczące sieci. Należy również wybrać opcję przy użyciu usługi Azure Active Directory lub usług federacyjnych Active Directory. Należy przestrzegać wymagań wstępnych przed rozpoczęciem wdrażania, dzięki czemu proces instalacji uruchamia się sprawnie. 

2. [**Pobierać i wyodrębniać pakietu wdrożeniowego**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit). Możesz pobrać pakiet wdrożeniowy hosta zestawu rozwoju lub do innego komputera. Pliki wyodrębnionego wdrożenia podjąć 60 GB wolnego miejsca na dysku, więc za pomocą innego komputera może pomóc zmniejszyć wymagania sprzętowe dla hosta development kit.

3. [**Przygotowanie hosta zestawu programowanie** ](azure-stack-run-powershell-script.md#prepare-the-development-kit-host) za pomocą Instalatora. Po wykonaniu tego kroku hosta zestawu programowanie uruchomi się do Cloudbuilder.vhdx (pliki wirtualnego dysku twardego zawierającego rozruchowego systemu operacyjnego i stosu Azure instalacja).

4. [**Wdrażanie zestaw deweloperski** ](azure-stack-run-powershell-script.md#deploy-the-development-kit) na hoście development kit.

5. Wdrożenia stosu Azure korzysta z usługi Azure Active Directory, należy [zarejestrować stosu Azure w usłudze Azure](azure-stack-register.md) , co pozwala [pobieranie elementów witrynę Azure marketplace](azure-stack-download-azure-marketplace-item.md) stos Azure.

Po wykonaniu tych czynności, będziesz mieć Środowisko deweloperskie zestawu z portali zarówno administratora, jak i użytkowników. Następnie można [połączenia i zaloguj się na](azure-stack-connect-azure-stack.md) do portalu. Następnie można uruchomić wdrażania dostawców zasobów, tworzenie [oferuje](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions)i wypełniania stosu Azure [marketplace](azure-stack-marketplace.md).
