---
title: 'Usługi Azure Active Directory B2C: Wbudowane zasady | Dokumentacja firmy Microsoft'
description: Temat w ramach rozszerzalnych zasad usługi Azure Active Directory B2C i o sposobach tworzenia różnych typów zasad
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 01/26/2017
ms.author: davidmu
ms.openlocfilehash: 35b31c46b350474caa1472829d705b4d068d9642
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Usługi Azure Active Directory B2C: Wbudowane zasady


Rozszerzona platforma zasad usługi Azure Active Directory (Azure AD) B2C jest siły podstawowe usługi. Zasady pełni opisano funkcje tożsamości konsumentów takich jak konta, logowania lub edytowanie profilu. Na przykład zasad rejestracji umożliwia kontrolowanie zachowania, konfigurując następujące ustawienia:

* Typy kont (kont społecznościowych takimi jak Facebook) lub kont lokalnych, takie jak adresy e-mail, których klienci mogą korzystać zalogowania się do aplikacji
* Atrybuty (na przykład imię, kod pocztowy i rozmiarze buta) mają być zbierane od konsumenta podczas tworzenia konta
* Korzystanie z usługi Azure Multi-Factor Authentication
* Wygląd i działanie wszystkie strony
* Informacje (które manifesty jako oświadczenia w tokenie) czy po otrzymaniu kiedy zasad, uruchom zakończenie

Można utworzyć wiele zasad o różnych typach w dzierżawie i używać ich w aplikacji, zgodnie z potrzebami. Zasady mogą być ponownie używane w aplikacjach. Tego rodzaju elastyczności umożliwia deweloperom definiowanie i modyfikowanie środowiska tożsamości użytkownika z minimalnym lub Brak zmian do ich kodu.

Zasady są dostępne do użycia przy użyciu interfejsu dewelopera proste. Wyzwala zasady przy użyciu standardowego żądania uwierzytelniania HTTP (przekazywanie parametru zasad w żądaniu) i otrzymuje token dostosowane odpowiedzi aplikacji. Jedyną różnicą między żądań, które wywołują zasad logowania i żądań, które wywołują zasad rejestracji jest na przykład nazwę zasad, która jest używana w "p" parametru ciągu zapytania:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

## <a name="create-a-sign-up-or-sign-in-policy"></a>Tworzenie zasad rejestracji lub logowania

Ta zasada obsługuje zarówno konsumenta rejestracji i logowania, korzystając z pojedynczą konfiguracją. Konsumenci są doprowadziło do prawej ścieżki (rejestracji lub logowania), w zależności od kontekstu. Omówiono także zawartość tokenów, które otrzymają aplikacji po pomyślnej napędza rejestracje lub logowania.  Przykład kodu dla zasad rejestracji i logowania jest [dostępne tutaj](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Jest zalecane w przypadku użycia tej zasady protokołu zasad rejestracji i logowania zasad.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Tworzenie zasad rejestracji

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Tworzenie zasad logowania

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Tworzenie zasad edytowania profilów

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Tworzenie zasad resetowania haseł

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a>Często zadawane pytania

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Jak połączyć zasady rejestracji lub logowania za pomocą zasad resetowania hasła
Podczas tworzenia zasady rejestracji i logowania (za pomocą kont lokalnych), zobacz **nie pamiętam hasła?** łącza na pierwszej stronie środowisko. Kliknięcie tego łącza nie automatycznie wyzwalacza hasła zasady resetowania. 

Zamiast tego kodu błędu **`AADB2C90118`** jest zwracana do aplikacji. Twoja aplikacja powinna obsługiwać tego kodu błędu za pomocą zasad resetowania hasła określone. Aby uzyskać więcej informacji, zobacz [przykład przedstawiający metodę łączenia zasad](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Należy użyć zasad rejestracji i logowania lub zasad rejestracji i logowania zasady?
Zalecane jest użycie zasad rejestracji lub logowania za pośrednictwem zasad rejestracji i logowania zasad.  

Zasady rejestracji i logowania ma więcej możliwości niż zasad logowania. Również umożliwia dostosowywanie interfejsu użytkownika strony i ma lepszą obsługę lokalizacji. 

Nie należy do zlokalizowania zasad tylko potrzeby znakowania możliwości personalizacji i mają hasła, zaleca się zasad logowania resetowania wbudowanych.

## <a name="next-steps"></a>Kolejne kroki
* [Token sesji i konfiguracji rejestracji jednokrotnej](active-directory-b2c-token-session-sso.md)
* [Wyłączyć weryfikację wiadomości e-mail podczas tworzenia konta użytkownika](active-directory-b2c-reference-disable-ev.md)

