---
title: "Wymagania wstępne platformy Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o wymaganiach wstępnych potrzebne Rozpoczynanie pracy z usługą Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 3fdef7bb58a5cd5dfbe4d37d9baf9c8e392ebe42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-prerequisites"></a>Wymagania wstępne usługi Azure Data Catalog

Należy zadbać o kilka rzeczy przed rozpoczęciem konfigurowania usługi Azure Data Catalog. Nie martw się, ten proces nie długo.

## <a name="azure-subscription"></a>Subskrypcja platformy Azure
Aby skonfigurować usługi Data Catalog, musi być właścicielem lub współwłaściciel subskrypcji platformy Azure.

Subskrypcje platformy Azure ułatwić organizowanie dostęp do zasobów usługi w chmurze, takich jak Data Catalog. Subskrypcje można także kontrolować sposób użycia zasobów jest zgłaszane, rozliczane i uregulowaniu płatności. Każda subskrypcja może mieć oddzielne ustawienia rozliczeń i płatności, więc można dodać subskrypcji i plany, które zależą od działów, projektów, biuro regionalne i tak dalej. Każda usługa w chmurze należy do subskrypcji, i musisz mieć subskrypcję przed skonfigurowaniem usługi Data Catalog. Aby dowiedzieć się więcej, zobacz artykuł [Manage accounts, subscriptions, and administrative roles](../active-directory/active-directory-assign-admin-roles.md) (Zarządzanie kontami, subskrypcjami i rolami administracyjnymi).

## <a name="azure-active-directory"></a>Usługa Azure Active Directory
Aby skonfigurować usługi Data Catalog, muszą być podpisane za pomocą konta użytkownika usługi Azure Active Directory (Azure AD).

Usługa Azure AD umożliwia firmom łatwe zarządzanie tożsamościami i dostępem, zarówno w chmurze, jak i lokalnie. Użytkownicy mogą używać jednego konta firmowego lub szkolnego dla pojedynczego logowania do dowolnego chmury i lokalnych aplikacji sieci web. Data Catalog używa usługi Azure AD do uwierzytelniania logowania. Aby dowiedzieć się więcej, zobacz [co to jest usługa Azure Active Directory?](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Za pomocą [portalu Azure](http://portal.azure.com/), aby móc zalogować się za pomocą osobistego konta Microsoft lub Azure Active Directory konto służbowe. Aby skonfigurować usługi Data Catalog za pomocą portalu Azure lub [portalu wykazu danych](http://www.azuredatacatalog.com), należy zalogować się przy użyciu konta usługi Azure Active Directory, a nie konta osobistego.
>
>

## <a name="active-directory-policy-configuration"></a>Konfiguracja zasad w usłudze Active Directory
Może wystąpić sytuacja, w których można Zaloguj się do portalu wykazu danych, ale podczas próby Zaloguj się do narzędzia rejestracji źródła danych, możesz napotkać komunikat o błędzie, który uniemożliwia rejestrowanie. To zachowanie problem może wystąpić tylko wtedy, gdy komputer znajduje się w sieci firmowej lub może wystąpić, tylko wtedy, gdy nawiązywane z spoza sieci firmowej.

Narzędzia rejestracji źródła danych korzysta z uwierzytelniania formularzy można sprawdzić poprawności poświadczeń użytkownika w usłudze Active Directory. Aby pomóc Ci się zalogować pomyślnym, administrator usługi Active Directory należy włączyć uwierzytelnianie formularzy w ramach globalnych zasad uwierzytelniania.

W ramach globalnych zasad uwierzytelniania metody uwierzytelniania można włączyć oddzielnie w intranecie i ekstranecie połączenia, jak pokazano na poniższym zrzucie ekranu. Mogą wystąpić błędy logowania, jeśli nie włączono uwierzytelniania formularzy dla sieci, z którym się łączysz.

 ![Usługi Active Directory globalnych zasad uwierzytelniania](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, zobacz [Konfigurowanie zasad uwierzytelniania](https://technet.microsoft.com/library/dn486781.aspx).
