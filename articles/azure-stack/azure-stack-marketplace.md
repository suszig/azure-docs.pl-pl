---
title: "Publikowanie elementów marketplace niestandardowych w stosie Azure (operatorowi chmury) | Dokumentacja firmy Microsoft"
description: "Jako operator stosu Azure jak opublikować element marketplace niestandardowych w stosie Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: erikje
ms.openlocfilehash: 7b5f976eb2d51eb86761a2bd0be6adb45ca87681
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="the-azure-stack-marketplace-overview"></a>Omówienie usługi Azure Marketplace stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Witryny Marketplace to zbiór usług, aplikacji i zasobów dostosowany do stosu Azure, takich jak sieci, maszyny wirtualne, magazynu i tak dalej. Użytkownicy w tym miejscu pochodzić do tworzenia nowych zasobów i wdrażania nowej aplikacji. Go traktować jako zakupów katalog, w którym użytkownicy mogą przeglądać i wybierz elementy, które mają być użyte. Aby użyć elementu portalu Marketplace, użytkownicy musi subskrybować ofertę, który przyznaje im dostępu do elementu.

Jako operator stosu Azure zdecydujesz elementy, które można dodać (publikowanie) w portalu Marketplace. Można opublikować bazy danych, usługi aplikacji i tak dalej. Z tego powodu są widoczne dla wszystkich użytkowników. Możesz opublikować niestandardowe elementy, które możesz utworzyć. Można również opublikować elementy z rosnącym [listy elementów portalu Azure Marketplace](azure-stack-marketplace-azure-items.md). Podczas publikowania elementu portalu Marketplace, użytkownicy mogą zobaczyć ją w ciągu pięciu minut.

Aby otworzyć witryny Marketplace, kliknij przycisk **nowy**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Elementów portalu Marketplace
Element Azure Marketplace stosu jest usługi, aplikacji lub zasobów, które użytkownicy mogą pobrać i użyć. Wszystkie elementy Azure Marketplace stosu są widoczne dla wszystkich użytkowników.

Każdy element Marketplace zawiera:

* Szablon usługi Azure Resource Manager alokacji zasobów
* Metadane, takie jak ciągi, ikony i inne materiały marketingowe
* Formatowanie informacje do wyświetlania elementu w portalu

Każdy element publikowane w witrynie Marketplace używa formatu o nazwie pakietu galerii Azure (azpkg). Dodaj oddzielnie, wdrożenia lub środowisko uruchomieniowe zasobów (takich jak kod, pliki zip za pomocą oprogramowania, obrazy maszyny wirtualnej) Azure stosu nie jako część elementu portalu Marketplace. 

## <a name="next-steps"></a>Następne kroki
[Tworzenie i publikowanie elementu portalu marketplace](azure-stack-create-and-publish-marketplace-item.md)

