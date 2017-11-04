---
title: "Azure AD Connect: Rozwiązywanie problemów z bezproblemowe logowanie jednokrotne | Dokumentacja firmy Microsoft"
description: "W tym temacie opisano sposoby rozwiązywania problemów z usługi Azure Active Directory bezproblemowe logowanie jednokrotne (Azure AD bezproblemowe logowanie Jednokrotne)."
services: active-directory
keywords: "Co to jest usługa Azure AD Connect, zainstaluj usługę Active Directory, wymaganych składników dla usługi Azure AD, SSO, Single Sign-on"
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
ms.openlocfilehash: b383a21500c753d8d2fe6747756541a3ff94ef02
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Rozwiązywanie problemów z usługi Azure Active Directory bezproblemowe logowanie jednokrotne

Ten artykuł pomaga informacje o typowych problemów dotyczących usługi Azure AD bezproblemowe logowanie jednokrotne.

## <a name="known-issues"></a>Znane problemy

- W niektórych przypadkach włączenie logowania jednokrotnego bezproblemowe może potrwać do 30 minut.
- Obsługa przeglądarki Edge nie jest dostępna.
- Uruchamianie klientom pakietu Office, szczególnie w sytuacjach współużytkowanego komputera, spowodować monity bardzo logowania dla użytkowników. Użytkownicy będą musieli wprowadzić często ich nazw użytkowników, ale nie hasła.
- Jeśli bezproblemowe logowanie Jednokrotne zakończy się powodzeniem, użytkownik nie zostanie podany, możliwość wyboru "Wylogowuj mnie". Ze względu na to zachowanie scenariuszy mapowania programu SharePoint i usługi OneDrive nie działają.
- Bezproblemowe logowanie Jednokrotne nie działa w trybie przeglądania prywatnym w programie Firefox.
- Bezproblemowe logowanie Jednokrotne nie działa w programie Internet Explorer, gdy jest włączony tryb rozszerzony ochrony.
- Bezproblemowe logowanie Jednokrotne nie działa w przeglądarkach dla urządzeń przenośnych w systemach iOS i Android.
- Jeśli planowana jest synchronizacja 30 lub większą liczbą lasów usługi AD, nie można włączyć bezproblemowe logowanie Jednokrotne za pomocą usługi Azure AD Connect. Jako rozwiązanie alternatywne można [ręcznie włączyć](#manual-reset-of-azure-ad-seamless-sso) tę funkcję na dzierżawy.
- Dodawanie adresów URL usługi Azure AD (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) do strefy "Zaufane witryny" zamiast "lokalny intranet" **uniemożliwia użytkownikom logowanie**.

## <a name="check-status-of-the-feature"></a>Sprawdź stan funkcji

Sprawdź, czy funkcja logowania jednokrotnego bezproblemowe jest nadal **włączone** w dzierżawie. Stan można sprawdzić, przechodząc do **Azure AD Connect** bloku na [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com/).

![Centrum administracyjne usługi Azure Active Directory — blok Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Przyczyny niepowodzenia logowania na Centrum administracyjnego usługi Azure Active Directory (wymaga licencji Premium)

Jeśli dzierżawy ma licencji usługi Azure AD Premium skojarzonych z nim, można też przyjrzeć się [raport aktywności logowania](../active-directory-reporting-activity-sign-ins.md) na [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com/).

![Centrum administracyjne usługi Active Directory platformy Azure — raport logowania](./media/active-directory-aadconnect-sso/sso9.png)

Przejdź do **usługi Azure Active Directory** -> **logowania** na [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com/) i kliknij przycisk działań logowania określonego użytkownika. Wyszukaj **kod błędu logowania w** pola. Wartość tego pola mapowania na Przyczyna niepowodzenia i rozpoznawanie przy użyciu poniższej tabeli:

|Kod błędu logowania|Przyczyna niepowodzenia logowania|Rozwiązanie
| --- | --- | ---
| 81001 | Bilet Kerberos użytkownika jest zbyt duży. | Zmniejsz członkostwa grupy użytkownika i spróbuj ponownie.
| 81002 | Nie można zweryfikować biletu Kerberos użytkownika. | Zobacz [Rozwiązywanie problemów z listy kontrolnej](#troubleshooting-checklist).
| 81003 | Nie można zweryfikować biletu Kerberos użytkownika. | Zobacz [Rozwiązywanie problemów z listy kontrolnej](#troubleshooting-checklist).
| 81004 | Próba uwierzytelniania Kerberos nie powiodła się. | Zobacz [Rozwiązywanie problemów z listy kontrolnej](#troubleshooting-checklist).
| 81008 | Nie można zweryfikować biletu Kerberos użytkownika. | Zobacz [Rozwiązywanie problemów z listy kontrolnej](#troubleshooting-checklist).
| 81009 | "Nie można sprawdzić poprawności biletów Kerberos użytkownika. | Zobacz [Rozwiązywanie problemów z listy kontrolnej](#troubleshooting-checklist).
| 81010 | Bezproblemowe logowanie jednokrotne nie powiodło się, ponieważ ważność biletu Kerberos użytkownika wygasła lub bilet jest nieprawidłowy. | Użytkownik musi zalogować się przy użyciu urządzenia przyłączone do domeny w sieci firmowej.
| 81011 | Nie można znaleźć obiektu użytkownika w oparciu o informacje z biletu Kerberos użytkownika. | Użyj usługi Azure AD Connect do synchronizowania informacje o użytkowniku w usłudze Azure Active Directory.
| 81012 | Użytkownik próbujący zalogować się do usługi Azure AD jest inny niż użytkownik zalogowany do urządzenia. | Zaloguj się z innego urządzenia.
| 81013 | Nie można znaleźć obiektu użytkownika w oparciu o informacje z biletu Kerberos użytkownika. |Użyj usługi Azure AD Connect do synchronizowania informacje o użytkowniku w usłudze Azure Active Directory. 

## <a name="troubleshooting-checklist"></a>Rozwiązywanie problemów z listy kontrolnej

Poniższa lista kontrolna umożliwia rozwiązywanie problemów bezproblemowe logowanie Jednokrotne:

- Sprawdź, czy funkcja logowania jednokrotnego bezproblemowe jest włączona w programie Azure AD Connect. Jeśli nie można włączyć funkcję (na przykład z powodu zablokowanych port), upewnij się, że masz wszystkie [wstępne](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) w miejscu.
- Sprawdź, czy obie te usługi Azure AD adresy URL (https://autologon.microsoftazuread-sso.com i https://aadg.windows.net.nsatc.net) są częścią ustawień strefy Intranet.
- Upewnij się, że urządzeń firmowych jest przyłączony do domeny usługi AD.
- Upewnij się, że użytkownik jest zalogowany do urządzenia przy użyciu konta domeny AD.
- Upewnij się, że konto użytkownika w lesie usługi AD, w którym zostało bezproblemowe logowanie Jednokrotne skonfigurowano.
- Upewnij się, że urządzenie jest podłączone do sieci firmowej.
- Upewnij się, że godzina na urządzeniu jest zsynchronizowany z czasem usługi Active Directory i kontrolerów domeny i jest w ciągu pięciu minut od siebie.
- Wyświetl listę istniejących biletów Kerberos na urządzeniu, używając **klist** polecenia z wiersza polecenia. Sprawdź, jeśli bilety wystawione dla `AZUREADSSOACCT` istnieją konta komputera. Bilety Kerberos użytkowników są zazwyczaj prawidłowe 12 godzin. W usłudze Active Directory, może mieć różne ustawienia.
- Wyczyść istniejące bilety Kerberos z urządzeniami przy użyciu **przeczyścić klist** polecenia, a następnie spróbuj ponownie.
- Aby ustalić, czy występują problemy związane z JavaScript, przejrzyj dzienniki konsoli przeglądarki (w obszarze "Developer Tools").
- Przegląd [kontroler domeny](#domain-controller-logs) również.

### <a name="domain-controller-logs"></a>Kontroler domeny

Jeśli inspekcja sukcesów jest włączona na kontrolerze domeny, a następnie za każdym razem, gdy użytkownik loguje się przy użyciu łatwego logowania jednokrotnego zapisu zabezpieczeń jest rejestrowane w dzienniku zdarzeń. Można znaleźć tych zdarzeń zabezpieczeń za pomocą następującej kwerendy (Znajdź zdarzenie **4769** skojarzone z kontem komputera **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Resetowanie ręczne funkcji

Jeśli rozwiązywania problemów nie pomogły, można ręcznie zresetować tę funkcję w dzierżawie. Wykonaj następujące kroki na serwerze lokalnym, na którym uruchomiony jest program Azure AD Connect:

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>: Krok 1 moduł bezproblemowe PowerShell logowania jednokrotnego

1. Najpierw należy pobrać i zainstalować [Microsoft Online Services Asystenta logowania](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Następnie Pobierz i zainstaluj [64-bitowy moduł usługi Azure Active Directory dla środowiska Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Przejdź do `%programfiles%\Microsoft Azure Active Directory Connect` folderu.
4. Zaimportuj moduł bezproblemowe PowerShell logowania jednokrotnego za pomocą tego polecenia: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>Krok 2: Pobieranie listy lasów usługi AD, dla których włączono bezproblemowe logowanie Jednokrotne

1. Uruchom program PowerShell jako Administrator. W programie PowerShell, wywołaj `New-AzureADSSOAuthenticationContext`. Po wyświetleniu monitu wprowadź poświadczenia administratora globalnego Twojej dzierżawy.
2. Wywołanie `Get-AzureADSSOStatus`. To polecenie zawiera listę lasów usługi AD (odszukaj na liście "Domeny"), na którym ta funkcja została włączona.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>Krok 3: Wyłącz bezproblemowe logowanie Jednokrotne dla każdego lasu usługi AD, skonfigurowanego go go na

1. Wywołanie `$creds = Get-Credential`. Po wyświetleniu monitu wprowadź poświadczenia administratora domeny dla danego lasu usługi AD.
2. Wywołanie `Disable-AzureADSSOForest -OnPremCredentials $creds`. To polecenie usuwa `AZUREADSSOACCT` konto komputera z lokalnego kontrolera domeny dla tego określonego lasu usługi AD.
3. Powtórz te czynności dla każdego lasu usługi AD, która po skonfigurowaniu funkcji na.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>Krok 4: Włącz bezproblemowe logowanie Jednokrotne dla każdego lasu usługi AD

1. Wywołanie `Enable-AzureADSSOForest`. Po wyświetleniu monitu wprowadź poświadczenia administratora domeny dla danego lasu usługi AD.
2. Powtórz te czynności dla każdego lasu usługi AD, który chcesz skonfigurować funkcję na.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Krok 5. Włącz tę funkcję na dzierżawy

Wywołanie `Enable-AzureADSSO` i wprowadź wartość "true" w `Enable: ` prompt włączyć tę funkcję w dzierżawie.
