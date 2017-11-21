---
title: 'Azure AD Connect: Uwierzytelniania przekazywanego - blokady inteligentnej | Dokumentacja firmy Microsoft'
description: "W tym artykule opisano sposób ochrony sieci lokalnych kont usługi Azure Active Directory (Azure AD) przekazywanego uwierzytelniania przed atakami siłowymi haseł w chmurze"
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
ms.openlocfilehash: 2e1468c6a576ab71b85e3f69e5914df6ee9e4d5a
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Uwierzytelniania przekazywanego usługi Azure Active Directory: Blokady inteligentnej

## <a name="overview"></a>Omówienie

Azure Active Directory (Azure AD) chroni przed atakami siłowymi hasła i uniemożliwia użytkownikom oryginalnego blokowanie poza ich usługi Office 365 i aplikacji SaaS. Ta funkcja o nazwie *blokady inteligentnej*, jest obsługiwana w przypadku korzystania z uwierzytelniania przekazywanego jako metodę logowania. Inteligentne blokada jest włączona domyślnie dla wszystkich dzierżawców i nieprzerwanie chroni kont użytkowników.

Blokady inteligentnej przechowuje informacje o nieudanych prób logowania. Po pewnym *próg blokady*, uruchamia *czas trwania blokady*. Blokady inteligentnej odrzuca wszystkie próby zalogować się w osoba atakująca czas trwania blokady. Jeśli nadal ataku, kolejnych nieudanych prób zalogowania po zakończeniu czas trwania blokady wynik w dłuższym czasie trwania blokady.

>[!NOTE]
>Blokady domyślny próg to 10 nieudanych prób. Domyślny czas trwania blokady wynosi 60 sekund.

Blokady inteligentnej rozróżnia również logowania z oryginalnego użytkowników i logowania przed atakami. W większości przypadków blokuje się tylko osoby atakujące. Ta funkcja zapobiega osoby atakujące złośliwie zablokowania oryginalnego użytkowników. Blokady inteligentnej używa poza zachowanie logowania, urządzenia i użytkownika przeglądarki, a inne sygnały w celu rozróżnienia oryginalnego użytkowników i osoby atakujące. Algorytmy są stale ulepszona.

Uwierzytelniania przekazywanego przekazuje żądania weryfikacji haseł do usługi Active Directory lokalnymi, dlatego należy całkowicie uniemożliwić osobom atakującym blokowania kont usługi Active Directory użytkowników. Usługi Active Directory ma własne zasady blokady konta, w szczególności [próg blokady konta](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) i [resetowania licznik blokady konta po](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx) zasad. Odpowiednio Skonfiguruj usługi Azure AD blokady próg i blokady wartości czasu trwania, aby odfiltrować ataków w chmurze przed dotarciem w lokalnej usłudze Active Directory.

>[!NOTE]
>Funkcji blokady inteligentnej jest wolny i jest _na_ domyślnie dla wszystkich klientów. Ale jeśli chcesz zmodyfikować za pomocą interfejsu API programu Graph usługi Azure AD blokady próg i blokady wartości czasu trwania dzierżawy musi mieć co najmniej jedną licencję usługi Azure AD Premium P2. Nie wymagają licencji usługi Azure AD Premium P2 _dla każdego użytkownika_ uzyskanie funkcji blokady inteligentnej przy użyciu uwierzytelniania przekazywanego.

Aby upewnić się, że użytkowników lokalnych kont usługi Active Directory są również chronione, należy upewnić się, że:

   * Progu blokady konta usługi Azure AD jest _mniej_ niż próg blokady konta usługi Active Directory. Ustaw wartości, tak aby próg blokady konta usługi Active Directory jest co najmniej dwa lub trzy razy dłuższy niż progu blokady konta usługi Azure AD.
   * Czas trwania blokady usługi Azure AD (reprezentowane w sekundach) jest _dłużej_ od usługi Active Directory Zresetuj licznik blokady konta po czas trwania (reprezentowane w minutach).

>[!IMPORTANT]
>Obecnie administrator nie można odblokować konta użytkowników chmury, jeśli zostały one zablokowane przez możliwość blokady inteligentnej. Administrator musi czekać na czas trwania blokady wygaśnie.

## <a name="verify-your-active-directory-account-lockout-policies"></a>Sprawdź zasady blokowania konta użytkownika usługi Active Directory

Użyj poniższych instrukcji, aby sprawdzić zasad blokady konta usługi Active Directory:

1.  Otwórz narzędzie Zarządzanie zasadami grupy.
2.  Edytuj ustawienia zasad grupy są stosowane do wszystkich użytkowników, na przykład **domyślne zasady domeny**.
3.  Przejdź do **Konfiguracja komputera** > **zasady** > **ustawienia systemu Windows** > **ustawienia zabezpieczeń**   >  **Zasady konta** > **zasad blokady konta**.
4.  Sprawdź Twojej **próg blokady konta** i **resetowania licznik blokady konta po** wartości.

![Zasady blokady konta w usłudze Active Directory](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-requires-a-premium-license"></a>Użyj interfejsu API programu Graph do zarządzania wartości blokady inteligentnej Twojej dzierżawy (wymaga licencji Premium)

>[!IMPORTANT]
>Modyfikowanie przy użyciu interfejsu API programu Graph usługi Azure AD blokady próg i blokady wartości czasu trwania to funkcja usługi Azure AD Premium P2. Również potrzebuje użytkownik musi być administratorem globalnym w dzierżawie.

Można użyć [Explorer wykres](https://developer.microsoft.com/graph/graph-explorer) mogą odczytywać, ustaw i zaktualizuj wartości Azure AD inteligentne blokady. Operacje te można również wykonać programowo.

### <a name="view-smart-lockout-values"></a>Wyświetlanie wartości inteligentne blokady

Wykonaj następujące kroki, aby wyświetlić wartości blokady inteligentnej Twojej dzierżawy:

1. Zaloguj się do Eksploratora wykresu jako administrator globalny dzierżawy. Jeśli zostanie wyświetlony monit, należy udzielić dostępu do żądanych uprawnień.
2. Wybierz **uprawnień do modyfikowania**, a następnie wybierz **Directory.ReadWrite.All** uprawnienia.
3. Skonfiguruj żądania interfejsu API programu Graph w następujący sposób: Ustaw wersję **BETA**, typ żądania do **UZYSKAĆ**i adres URL, który `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Wybierz **Uruchom kwerendę** wartości blokady inteligentnej swojej dzierżawy. Jeśli nie zdefiniowano wartości Twojej dzierżawy przed, pojawi się pusty zestaw.

### <a name="set-smart-lockout-values"></a>Ustaw wartości inteligentne blokady

Wykonaj następujące kroki, aby ustawić wartości blokady inteligentnej Twojej dzierżawy (wymagane tylko pierwszy raz):

1. Zaloguj się do Eksploratora wykresu jako administrator globalny dzierżawy. Jeśli zostanie wyświetlony monit, należy udzielić dostępu do żądanych uprawnień.
2. Wybierz **uprawnień do modyfikowania**, a następnie wybierz **Directory.ReadWrite.All** uprawnienia.
3. Skonfiguruj żądania interfejsu API programu Graph w następujący sposób: Ustaw wersję **BETA**, typ żądania do **POST**i adres URL, który `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Skopiuj i wklej następujący żądania JSON do **treści żądania** pola.
5. Wybierz **Uruchom kwerendę** można ustawić wartości blokady inteligentnej swojej dzierżawy.

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
>Jeśli nie są używane, można pozostawić **BannedPasswordList** i **EnableBannedPasswordCheck** wartości jako pusta (**""**) i **false** odpowiednio.

Sprawdź, czy wartości blokady inteligentnej Twojej dzierżawy mają ustawiony prawidłowo przy użyciu kroków w [wartości blokady inteligentnej widoku](#view-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Aby zaktualizować inteligentne blokady

Wykonaj następujące kroki, aby zaktualizować wartości blokady inteligentnej Twojej dzierżawy (jeśli zostaną ustawione przed):

1. Zaloguj się do Eksploratora wykresu jako administrator globalny dzierżawy. Jeśli zostanie wyświetlony monit, należy udzielić dostępu do żądanych uprawnień.
2. Wybierz **uprawnień do modyfikowania**, a następnie wybierz **Directory.ReadWrite.All** uprawnienia.
3. [Wykonaj następujące kroki, aby wyświetlić wartości blokady inteligentnej Twojej dzierżawy](#view-smart-lockout-values). Kopiuj `id` wartość (GUID) elementu z **displayName** jako **PasswordRuleSettings**.
4. Skonfiguruj żądania interfejsu API programu Graph w następujący sposób: Ustaw wersję **BETA**, typ żądania do **poprawka**i adres URL, który `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>`. Użyj identyfikatora GUID z kroku 3 `<id>`.
5. Skopiuj i wklej następujący żądania JSON do **treści żądania** pola. Zmień wartości blokady inteligentnej zależnie od potrzeb.
6. Wybierz **Uruchom kwerendę** można zaktualizować wartości blokady inteligentnej swojej dzierżawy.

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

Zweryfikuj, że wartości blokady inteligentnej Twojej dzierżawy ma zaktualizowany poprawnie za pomocą kroków w [wartości blokady inteligentnej widoku](#view-smart-lockout-values).

## <a name="next-steps"></a>Następne kroki
[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): plik nowego żądania funkcji za pomocą na Forum usługi Azure Active Directory.
