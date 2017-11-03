---
title: "Konfigurowanie niestandardowej nazwy domeny dla swojego wystąpienia usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "W tym temacie opisano sposób konfigurowania niestandardowej nazwy domeny dla swojego wystąpienia usługi Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: a7abba87035016eee05b4ab663404f10127bba1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-custom-domain-name"></a>Konfigurowanie niestandardowej nazwy domeny 

Podczas tworzenia wystąpienia interfejsu API zarządzania (APIM) Azure przypisuje go do poddomeny azure api.net (na przykład `apim-service-name.azure-api.net`). Jednak mogą uwidaczniać APIM punktów końcowych przy użyciu z własnej nazwy domeny, takich jak **contoso.com**. W tym samouczku przedstawiono sposób odwzorowywania istniejącej nazwy DNS niestandardowe punkty końcowe udostępnianych przez wystąpienie usługi Azure API Management.


## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym artykule, musi mieć:

+ Aktywna subskrypcja platformy Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Wystąpienie APIM. Aby uzyskać więcej informacji, zobacz [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Nazwa domeny niestandardowej, która jest własnością użytkownika. Nazwa domeny niestandardowej, którego chcesz użyć, należy kupowane niezależnie i hostowanych na serwerze DNS. W tym temacie nie daje instrukcje na temat obsługi niestandardowej nazwy domeny.
+ Musi mieć ważny certyfikat za pomocą klucza publicznego i prywatnego (. PFX). Podmiotu lub alternatywnej nazwy podmiotu (SAN) musi być zgodna z nazwą domeny (umożliwia APIM bezpieczne Uwidacznianie adresów URL za pośrednictwem protokołu SSL).

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Użyj portalu Azure, aby ustawić niestandardową nazwę domeny

1. Przejdź do Twojego wystąpienia APIM w [portalu Azure](https://portal.azure.com/).
2. Wybierz **domen niestandardowych i SSL**.
    
    Istnieje wiele punktów końcowych, do których można przypisać niestandardową nazwę domeny. Obecnie dostępne są następujące punkty końcowe: 
    + **Serwer proxy** (domyślnie: `<apim-service-name>.azure-api.net`), 
    + **Portal** (domyślnie: `<apim-service-name>.portal.azure-api.net`),     
    + **Zarządzanie** (domyślnie: `<apim-service-name>.management.azure-api.net`), 
    + **Menedżer sterowania usługami** (domyślnie: `<apim-service-name>.scm.azure-api.net`).

    >[!NOTE]
    > Należy zaktualizować wszystkie punkty końcowe lub niektóre z nich. Zazwyczaj aktualizacji klientów **Proxy** (ten adres URL jest używany do wywołania interfejsu API za pośrednictwem interfejsu API zarządzania) i **Portal** (portalu dla deweloperów adres URL). **Zarządzanie** i **SCM** punkty końcowe są używane wewnętrznie APIM klientów i w związku z tym rzadziej przypisano niestandardowej nazwy domeny.
3. Wybierz punkt końcowy, który chcesz zaktualizować. 
4. W oknie po prawej stronie kliknij **niestandardowy**.

    + W **niestandardowej nazwy domeny**, określ nazwę, którego chcesz użyć. Na przykład `api.contoso.com`. <br/>Nazwy domen symbolu wieloznacznego (na przykład *. domena.com) również są obsługiwane.
    + W **certyfikatu**, Określ prawidłowy. Plik PFX, który chcesz przekazać. 
    + Jeśli certyfikat ma hasła, wprowadź go w **hasło** pola.
1. Kliknij przycisk Zastosuj.

    >[!NOTE]
    >Proces przypisywania certyfikatu może potrwać 15 minut.

## <a name="next-steps"></a>Następne kroki

[Uaktualnienie i skalowania usługi](upgrade-and-scale.md)