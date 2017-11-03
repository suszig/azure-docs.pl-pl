---
title: 'Azure AD Connect: Uwierzytelniania przekazywanego - blokady inteligentnej | Dokumentacja firmy Microsoft'
description: "W tym artykule opisano sposób ochrony sieci lokalnych kont usługi Azure Active Directory (Azure AD) przekazywanego uwierzytelniania przed atakami siłowymi haseł w chmurze."
services: active-directory
keywords: "Azure AD Connect przekazywanego uwierzytelniania, instalacji usługi Active Directory, wymaganych składników dla usługi Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 771741fd7da8c9b6932851851aaca148f9596643
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Uwierzytelniania przekazywanego usługi Azure Active Directory: Blokady inteligentnej

## <a name="overview"></a>Omówienie

Usługi Azure AD chroni przed atakami siłowymi hasła i uniemożliwia użytkownikom oryginalnego blokowanie poza ich usługi Office 365 i aplikacji SaaS. Ta funkcja o nazwie **blokady inteligentnej**, jest obsługiwana w przypadku korzystania z uwierzytelniania przekazywanego jako metodę logowania. Blokady inteligentnej jest domyślnie włączona dla wszystkich dzierżawców i kont użytkowników, są chronione przez cały czas; nie istnieje potrzeba aby ją włączyć.

Blokady inteligentnej działa przez śledzenie nieudanych prób logowania i po pewnym **próg blokady**, rozpoczynając **czas trwania blokady**. Wszelkich prób logowania z osoba atakująca czas trwania blokady są odrzucane. Jeśli ataku, kolejnych nieudanych prób zalogowania po zakończeniu czas trwania blokady wynik w dłuższym czasie trwania blokady.

>[!NOTE]
>Domyślna wartość progowa blokady to 10 nieudanych prób, a domyślny czas trwania blokady wynosi 60 sekund.

Blokady inteligentnej rozróżnia również logowania z oryginalnego użytkowników i osoby atakujące i tylko blokowane osoby atakujące w większości przypadków. Ta funkcja zapobiega osoby atakujące złośliwie zablokowania oryginalnego użytkowników. Firma Microsoft umożliwia zachowanie logowania, urządzenia użytkowników & przeglądarek i innymi sygnałami odróżnia oryginalnego użytkownika, osoby atakujące. Firma Microsoft stale umożliwiają zwiększenie nasze algorytmy.

Ponieważ uwierzytelniania przekazywanego przekazuje żądania weryfikacji haseł na lokalnej usługi Active Directory (AD), należy uniemożliwić osobom atakującym blokowania kont usługi AD użytkowników. Ponieważ masz zasad blokady konta AD (w szczególności [ **próg blokady konta** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) i [ **Zresetuj licznik blokady konta po zasady**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), należy skonfigurować progu blokady konta usługi Azure AD i wartości czasu trwania blokady odpowiednio do ataków w chmurze przed dotarciem lokalnej usługi AD.

>[!NOTE]
>Funkcji blokady inteligentnej jest wolny i jest _na_ domyślnie dla wszystkich klientów. Modyfikowanie progu blokady konta usługi Azure AD i czas trwania blokady wartości przy użyciu interfejsu API programu Graph musi jednak dzierżawy mają co najmniej jedną licencję usługi Azure AD Premium P2. Nie wymagają licencji usługi Azure AD Premium P2 _dla każdego użytkownika_ uzyskanie funkcji blokady inteligentnej przy użyciu uwierzytelniania przekazywanego.

Aby upewnić się, że użytkowników lokalnych kont usługi AD są również chronione, należy upewnić się, że:

1.  Progu blokady konta usługi Azure AD jest _mniej_ niż próg blokady konta przez usługi AD. Firma Microsoft zaleca, ustaw wartości, tak aby próg blokady konta przez usługi AD jest co najmniej dwie lub trzy razy progu blokady konta usługi Azure AD.
2.  Czas trwania blokady usługi Azure AD (reprezentowane w sekundach) jest _dłużej_ niż Directory resetowania konta blokady licznika po (reprezentowane w minutach).

>[!IMPORTANT]
>Obecnie Administrator nie można odblokować konta użytkowników chmury, jeśli zostały one zablokowane przez możliwość blokady inteligentnej. Mają one będzie czekać na czas trwania blokady wygaśnie.

## <a name="verify-your-ad-account-lockout-policies"></a>Sprawdź zasad blokady konta usługi AD

Użyj poniższych instrukcji, aby sprawdzić zasad blokady konta usługi AD:

1.  Otwórz narzędzie Zarządzanie zasadami grupy.
2.  Edytowanie zasad grupy, która jest stosowana do wszystkich użytkowników, na przykład domyślnych zasad domeny.
3.  Przejdź do zasady blokowania komputera Konfiguracja komputera\Zasady\Ustawienia systemu Windows\Ustawienia zabezpieczeń\Zasady konta\Zasady.
4.  Sprawdź wartości progu blokady konta i zresetuj licznik blokady konta po.

![Zasady blokowania kont usługi AD](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-needs-premium-license"></a>Użyj interfejsu API programu Graph do zarządzania wartości blokady inteligentnej Twojej dzierżawy (wymaga licencji Premium)

>[!IMPORTANT]
>Modyfikowanie progu blokady konta i czas trwania blokady wartości przy użyciu interfejsu API programu Graph usługi Azure AD to funkcja usługi Azure AD Premium P2. Również potrzebuje użytkownik musi być administratorem globalnym w dzierżawie.

Można użyć [Explorer wykres](https://developer.microsoft.com/graph/graph-explorer) mogą odczytywać, wartość i Aktualizuj wartości blokady inteligentnej usługi Azure AD. Jednak możesz również wykonać te operacje programowo.

### <a name="read-smart-lockout-values"></a>Inteligentne blokady odczytu wartości

Wykonaj następujące kroki, które można odczytać wartości blokady inteligentnej Twojej dzierżawy:

1. Zaloguj się do Eksploratora wykresu jako Administrator globalny dzierżawy. Jeśli zostanie wyświetlony monit, należy udzielić dostępu do żądanych uprawnień.
2. Kliknij polecenie "Modyfikuj uprawnienia" i wybierz uprawnienie "Directory.ReadWrite.All".
3. Skonfiguruj żądania interfejsu API programu Graph w następujący sposób: Ustaw wersję "BETA", typ żądania "GET" i adresu URL do `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kliknij przycisk "Uruchom zapytanie", aby zobaczyć wartości blokady inteligentnej swojej dzierżawy. Jeśli nie zdefiniowano wartości Twojej dzierżawy przed, pojawi się pusty zestaw.

### <a name="set-smart-lockout-values"></a>Ustaw wartości inteligentne blokady

Wykonaj następujące kroki, aby ustawić wartości blokady inteligentnej Twojej dzierżawy (po raz pierwszy tylko):

1. Zaloguj się do Eksploratora wykresu jako Administrator globalny dzierżawy. Jeśli zostanie wyświetlony monit, należy udzielić dostępu do żądanych uprawnień.
2. Kliknij polecenie "Modyfikuj uprawnienia" i wybierz uprawnienie "Directory.ReadWrite.All".
3. Skonfiguruj żądania interfejsu API programu Graph w następujący sposób: Ustaw wersję do wersji "BETA", typ "POST" i adres URL żądania `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Skopiuj i wklej następujący żądania JSON w polu "Treści żądania".
5. Kliknij przycisk "Uruchom zapytania" można ustawić wartości blokady inteligentnej swojej dzierżawy.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Jeśli nie są używane, można pozostawić **BannedPasswordList** i **EnableBannedPasswordCheck** wartości jako pusty ("") i "false" odpowiednio.

Sprawdź, czy ma ustawiony prawidłowo przy użyciu wartości blokady inteligentnej Twojej dzierżawy [te kroki](#read-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Aby zaktualizować inteligentne blokady

Wykonaj następujące kroki, aby zaktualizować wartości blokady inteligentnej Twojej dzierżawy (jeśli zostały skonfigurowane je przed):

1. Zaloguj się do Eksploratora wykresu jako Administrator globalny dzierżawy. Jeśli zostanie wyświetlony monit, należy udzielić dostępu do żądanych uprawnień.
2. Kliknij polecenie "Modyfikuj uprawnienia" i wybierz uprawnienie "Directory.ReadWrite.All".
3. [Wykonaj następujące kroki, które można odczytać wartości blokady inteligentnej Twojej dzierżawy](#read-smart-lockout-values). Kopiuj `id` wartość (GUID) elementu o nazwie "wyświetlanej" jako "PasswordRuleSettings".
4. Skonfiguruj żądania interfejsu API programu Graph w następujący sposób: Ustaw wersję "BETA", typ żądania "Poprawka" i adresu URL do `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -Użyj identyfikatora GUID z kroku 3 dla `<id>`.
5. Skopiuj i wklej następujący żądania JSON w polu "Treści żądania". Zmień wartości blokady inteligentnej zależnie od potrzeb.
6. Kliknij przycisk "Uruchom zapytania", aby zaktualizować wartości blokady inteligentnej swojej dzierżawy.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Sprawdź zaktualizowano poprawnie przy użyciu wartości blokady inteligentnej Twojej dzierżawy [te kroki](#read-smart-lockout-values).

## <a name="next-steps"></a>Następne kroki
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) — w przypadku zgłoszenia żądania nowych funkcji.
