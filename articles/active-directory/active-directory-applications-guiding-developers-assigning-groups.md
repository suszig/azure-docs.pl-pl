---
title: Przypisywanie grup do aplikacji Azure AD | Dokumentacja firmy Microsoft
description: "Jak zaimplementować przypisanie do grupy aplikacji Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: e0b0b87a454db96747f024e81882fe83d62fdbe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="assign-azure-active-directory-groups-to-an-application"></a>Przypisywanie grup usługi Azure Active Directory do aplikacji
Przed przypisaniem użytkowników i grup do aplikacji może wymagać przypisanie użytkownika. Aby dowiedzieć się, jak wymagają przypisania użytkownika, zobacz [wymagające przypisanie użytkownika](active-directory-applications-guiding-developers-requiring-user-assignment.md) artykułu.

W tym artykule przyjęto założenie, że utworzono już grup w usłudze active directory, używanego dla tej aplikacji.

## <a name="assigning-groups-to-an-application"></a>Przypisywanie grup do aplikacji
1. Zaloguj się do portalu Azure przy użyciu konta administratora.
2. Polecenie **wszystkie elementy** elementu w menu głównym.
3. Wybierz katalog, w używanej aplikacji.
4. Polecenie **aplikacji** kartę.
5. Wybierz aplikację z listy aplikacji skojarzonych z tym katalogiem.
6. Kliknij przycisk **użytkowników i grup** kartę.
7. Filtrowanie listy grup w usłudze active directory przy użyciu **grup** listy rozwijanej.
8. Wybierz grupę.
9. Kliknij przycisk **PRZYPISAĆ**.
10. Kliknij przycisk **tak** po wyświetleniu monitu.

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
