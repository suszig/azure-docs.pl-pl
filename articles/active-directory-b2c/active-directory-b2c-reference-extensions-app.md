---
title: Aplikacja rozszerzenia — usługa Azure AD B2C | Dokumentacja firmy Microsoft
description: Przywrócenie aplikacji rozszerzeń b2c
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 9/06/2017
ms.author: davidmu
ms.openlocfilehash: c07aba797118af2cc8283509944eda8b41d499b3
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-ad-b2c-extensions-app"></a>Usługa Azure AD B2C: Rozszerzeń aplikacji

Podczas tworzenia katalogu usługi Azure AD B2C aplikacja o nazwie `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` jest tworzony automatycznie wewnątrz nowego katalogu. Ta aplikacja, nazywany **b2c rozszerzeń aplikacji**, jest widoczny w *rejestracji aplikacji*. Jest on używany przez usługę Azure AD B2C do przechowywania informacji o użytkownikach i atrybuty niestandardowe. Usunięcie aplikacji usługi Azure AD B2C nie będzie działać prawidłowo, i będzie mieć wpływ na środowisko produkcyjne.

> [!IMPORTANT]
> Nie należy usuwać aplikacji rozszerzeń b2c, jeśli zamierzasz natychmiast usunąć dzierżawy. Jeśli aplikacja pozostaje usunięte przez dłużej niż 30 dni, informacje o użytkowniku zostaną trwale utracone.

## <a name="verifying-that-the-extensions-app-is-present"></a>Weryfikowanie, czy znajduje się w aplikacji rozszerzenia

Aby sprawdzić, czy znajduje się w aplikacji rozszerzeń b2c:

1. W dzierżawie usługi Azure AD B2C, kliknij **wszystkie usługi** w menu nawigacji po lewej stronie.
1. Wyszukaj i Otwórz **rejestracji aplikacji**.
1. Wyszukaj aplikację, która rozpoczyna się od **b2c rozszerzeń aplikacji**

## <a name="recover-the-extensions-app"></a>Odzyskiwanie aplikacji rozszerzenia

Jeśli przypadkowo usunięto aplikacji rozszerzeń b2c, masz 30 dni, aby go odzyskać. Możesz przywrócić aplikacji przy użyciu interfejsu API programu Graph:

1. Przejdź do [ https://graphexplorer.azurewebsites.net/ ](https://graphexplorer.azurewebsites.net/).
1. Zaloguj się do witryny jako administrator globalny katalogu usługi Azure AD B2C, której chcesz przywrócić usuniętego aplikacji dla. Ten administrator globalny musi mieć adres e-mail jest podobny do następującego: `username@{yourTenant}.onmicrosoft.com`.
1. Wystawiać HTTP GET względem adresu URL `https://graph.windows.net/myorganization/deletedApplications` z interfejsu api-version = 1.6. Ta operacja spowoduje wyświetlenie listy wszystkich aplikacji, które zostały usunięte w ciągu ostatnich 30 dni.
1. Znajdź aplikację na liście, których nazwa rozpoczyna się od "ruch aplikacji b2c rozszerzenia" i skopiuj jej `objectid` wartości właściwości.
1. Wystawiać POST protokołu HTTP względem adresu URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Zastąp `{OBJECTID}` część adresu URL z `objectid` z poprzedniego kroku. 

Teraz powinno być możliwe do [przywróconej aplikacja](#verifying-that-the-extensions-app-is-present) w portalu Azure.

> [!NOTE]
> Aplikację można przywrócić tylko, jeśli został on usunięty w ciągu ostatnich 30 dni. Jeśli został on więcej niż 30 dni, dane zostaną trwale utracone. Aby uzyskać pomoc pliku biletu pomocy technicznej.