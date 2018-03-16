---
title: "Usługi aplikacji w usłudze Azure stosu Update jeden | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat nowości w aktualizacji, jeden dla usługi aplikacji Azure stosu, znane problemy i pobierania aktualizacji."
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 0c33c8fdefbb27ba8414e58bed1b42ee7aaba88a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="app-service-on-azure-stack-update-one-release-notes"></a>Usługa aplikacji Azure stosu zaktualizować co informacje o wersji

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Te informacje o wersji opisano ulepszeń i poprawek w usłudze Azure App Service na Azure stosu Update 1 i znanych problemów. Znane problemy są podzielone na problemy z bezpośrednio do wdrożenia, proces aktualizacji i problemy z kompilacją (po instalacji).

> [!IMPORTANT]
> Zastosowanie aktualizacji 1802 systemu Azure stosu zintegrowane lub wdrożyć najnowszy zestaw deweloperski stosu Azure przed wdrożeniem usługi Azure App Service.
>
>

## <a name="build-reference"></a>Tworzenie odwołania

Usługa aplikacji Azure stosu Update 1 numer kompilacji jest **69.0.13698.9**

### <a name="prerequisites"></a>Wymagania wstępne

> [!IMPORTANT]
> Usługa aplikacji Azure na stosie Azure wymaga teraz [certyfikat uniwersalny podmiotu trzech](azure-stack-app-service-before-you-get-started.md#get-certificates) ze względu na ulepszenia w taki sposób, w jaki usługa rejestracji Jednokrotnej dla Kudu teraz obsługiwane w usłudze Azure App Service.  Nowy temat jest ** *. sso.appservice.<region>. <domainname>.<extension>**
>
>

Zapoznaj się [przed rozpoczęciem pracy dokumentacji](azure-stack-app-service-before-you-get-started.md) przed rozpoczęciem wdrażania.

### <a name="new-features-and-fixes"></a>Nowe funkcje i poprawki

Usługa aplikacji Azure na Azure stosu Update 1 zawiera następujące ulepszenia i poprawki:

- **Wysokiej dostępności z usługi Azure App Service** -1802 stosu Azure obciążeń włączone aktualizacje do wdrożenia na fault domen.  W związku z tym infrastruktury usługi aplikacji jest w stanie się odporność na uszkodzenia, co zostanie ona wdrożona w domenach awarii.  Domyślnie wszystkie nowe wdrożenia usługi Azure App Service zostanie mają tę możliwość, jednak wdrożeń ukończone przed 1802 stosu Azure zastosowanych aktualizacji można znaleźć w temacie [dokumentacji domena awarii usługi aplikacji](azure-stack-app-service-fault-domain-update.md)

- **Wdrażanie w istniejącej sieci wirtualnej** — klienci mogą teraz wdrożyć usługi aplikacji na stosie Azure w ramach istniejącej sieci wirtualnej.  Wdrażanie w istniejącej sieci wirtualnej umożliwia klientom łączenie się z programu SQL Server i serwer plików, wymaganych do usługi Azure App Service za pośrednictwem portów prywatnych.  Podczas wdrażania klientów można wybrać do wdrożenia w istniejącej sieci wirtualnej, jednak [należy utworzyć podsieci do użycia przez usługę App Service](azure-stack-app-service-before-you-get-started.md#virtual-network) przed ich wdrożeniem.

- Aktualizacje **dzierżawa usługi aplikacji, administrator, portali funkcje i narzędzia Kudu**.  Spójne z wersją zestawu SDK usługi Azure stosu portalu.

- **Aktualizacje następujące struktury aplikacji i narzędzi**:
    - Dodaje **.Net Core 2.0** obsługuje
    - Dodaje **Node.JS** wersji:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Dodaje **NPM** wersji:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Dodaje **PHP** aktualizacji:
        - 5.6.32
        - 7.0.26 (x86 i x64)
        - 7.1.12 (x86 i x64)
    - Zaktualizowano **Git dla systemu Windows** v 2.14.1
    - Zaktualizowano **Mercurial** do v4.5.0

  - Dodano obsługę **HTTPS tylko** funkcji w funkcji domeny niestandardowe w portalu dzierżawcy usługi aplikacji. 

  - Dodano Weryfikacja połączenia z magazynem w selektorze magazynu niestandardowego dla usługi Azure Functions 

#### <a name="fixes"></a>Poprawki

- Podczas tworzenia pakietu wdrożeniowego w trybie offline, klienci nie będą już przekazywane dostępu komunikat o błędzie podczas otwierania folderu z Instalatora usługi aplikacji

- Rozwiązane problemy podczas pracy funkcji domen niestandardowych w portalu dzierżawcy usługi aplikacji.

- Zapobiegaj klientów przy użyciu nazw zastrzeżonych administratora podczas instalacji

- Wdrożenia usługi App Service z włączoną **przyłączony do domeny** serwera plików

- Ulepszone pobieranie stosu Azure głównego certyfikatu w skrypcie i teraz sprawdzić poprawność certyfikatu głównego w Instalatorze usługi aplikacji.

- Nieprawidłowy stan stały zwracanych do usługi Azure Resource Manager, gdy subskrypcja jest usuwane że zasoby zawarte w przestrzeni nazw Microsoft.Web.

### <a name="known-issues-with-the-deployment-process"></a>Znane problemy z procesu wdrażania

- Nie są znane problemy dla wdrożenia usługi Azure App Service na Azure stosu Update 1.

### <a name="known-issues-with-the-update-process"></a>Znane problemy związane z procesem aktualizacji

- Nie są znane problemy aktualizacji usługi Azure App Service na Azure stosu Update 1.

### <a name="known-issues-post-installation"></a>Znane problemy (po instalacji)

- Nie są znane problemy dotyczące instalacji usługi Azure App Service na Azure stosu Update 1.

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Znane problemy dotyczące działania usługi Azure App Service na stosie Azure Administratorzy chmury

Zapoznaj się z dokumentacją w [1802 stosu Azure informacje o wersji](azure-stack-update-1802.md)

## <a name="see-also"></a>Zobacz także

- Omówienie usługi Azure App Service, zobacz [usłudze Azure App Service na stos Azure omówienie](azure-stack-app-service-overview.md).
- Aby uzyskać więcej informacji dotyczących sposobu przygotowania do wdrożenia usługi aplikacji Azure stosu, zobacz [przed rozpoczęciem pracy z usługi aplikacji Azure stosu](azure-stack-app-service-before-you-get-started.md).
