---
title: Tematy pomocy portalu rejestracji aplikacji | Dokumentacja firmy Microsoft
description: "Opis różnych funkcji w portalu rejestracji aplikacji firmy Microsoft."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: c60499c425a7fd800f7ca9a5bac1fed5af73b801
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="app-registration-reference"></a>Informacje dotyczące rejestracji aplikacji
Ten dokument zawiera kontekstu i opisy różnych funkcji w portalu rejestracji aplikacji Microsoft [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Moje aplikacje
Ta lista zawiera wszystkie aplikacje zarejestrowany do użytkowania z punktem końcowym v2.0 usługi Azure AD.  Te aplikacje mają możliwość do logowania użytkowników z obu konta osobiste z konta Microsoft i kont pracy/służbowych z usługi Azure Active Directory.  Aby dowiedzieć się więcej na temat punktu końcowego v2.0 usługi Azure AD, zobacz nasze [omówienie v2.0](active-directory-appmodel-v2-overview.md).  Te aplikacje można również zintegrować z punktu końcowego uwierzytelniania konta Microsoft, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Na żywo aplikacji zestawu SDK
Ta lista zawiera wszystkie aplikacje zarejestrowany do użytkowania wyłącznie z kontem Microsoft.  Nie są włączone do użytku w usłudze Azure Active Directory ani.  Jest to, gdzie można znaleźć wszystkie aplikacje, które wcześniej został zarejestrowany z portalu dla deweloperów MSA w `https://account.live.com/developers/applications`.  Wszystkie funkcje, które należy wcześniej wykonać `https://account.live.com/developers/applications` można teraz wykonać w tym nowego portalu `https://apps.dev.microsoft.com`.  Jeśli masz dodatkowe pytania dotyczące aplikacji konta Microsoft, skontaktuj się z nami.

## <a name="application-secrets"></a>Klucze tajne aplikacji
Hasła aplikacji są poświadczenia, które umożliwią niezawodnych aplikacji do wykonywania [uwierzytelnianie klienta](http://tools.ietf.org/html/rfc6749#section-2.3) z usługą Azure AD.  OAuth i OpenID Connect klucze tajne aplikacji jest często określana jako `client_secret`.  W protokole v2.0, każda aplikacja, która odbiera token zabezpieczający w lokalizacji adresowanego sieci web (przy użyciu `https` schemat) należy użyć klucz tajny aplikacji do identyfikacji do usługi Azure AD po realizacji tego tokenu zabezpieczającego.  Ponadto dowolnego klienta natywnego tego recieves tokeny na urządzenie będzie zabronione używana do uwierzytelniania klienta, klucz tajny aplikacji do magazynu kluczy tajnych w środowiskach niezabezpieczonych zniechęcić.

Każda aplikacja może zawierać dwóch hasła prawidłową aplikacją na dowolnym etapie w czasie.  Dzięki utrzymywaniu dwa klucze tajne, masz ablilty do przeprowadzania okresowe Przerzucanie klucza poprzez całego środowiska aplikacji.  Po dokonaniu migracji całości aplikacji do nowego klucza tajnego, możesz usunąć stary klucz tajny i udostępnić nową.

W tej chwili tylko dwa typy aplikacji kluczy tajnych są dozwolone w portalu rejestracji aplikacji.  Wybieranie **wygenerować nowe hasło** będzie Generowanie i przechowywanie wspólny klucz tajny w magazynie odpowiednich danych, którego można użyć w aplikacji.  Wybieranie **wygenerować nową parę klucz** utworzy nowe pary kluczy publicznych i prywatnych, które mogą być pobierane i używany do uwierzytelniania klienta do usługi Azure AD.

## <a name="profile"></a>Profil
Profil części portalu rejestracji aplikacji można dostosować stronie logowania aplikacji.  W tej chwili można zmienić logowania strony aplikacji logo, warunków adres URL usługi i zasady zachowania poufności informacji.  Logo musi być przezroczystym obrazem o wymiarach 48 x 48 lub 50 x 50 pikseli w formacie pliku GIF, PNG lub JPEG o maksymalnym rozmiarze 15 KB.  Spróbuj zmiana wartości i wyświetlanie wynikowy Zaloguj się na stronie!

## <a name="live-sdk-support"></a>Obsługa w zestawie SDK na żywo
Po włączeniu "Live SDK Obsługa" kluczy tajnych żadnych aplikacji, należy utworzyć będą udostępniane w obu programu Azure AD i magazyny danych Account firmy Microsoft.  Pozwoli to aplikacji łączyć bezpośrednio z usługą Microsoft Account (login.live.com).  Jeśli chcesz skompilować aplikację przy użyciu Account Microsoft bezpośrednio (a nie przy użyciu punktu końcowego v2.0 usługi Azure AD), użytkownik powinien upewnić się, czy włączona jest obsługa Live SDK.

Wyłączenie obsługi zestaw Live SDK będzie upewnij się, że klucz tajny aplikacji tylko są zapisywane na dane usługi Azure AD przechowywania.  Dane usługi Azure AD magazynu zawiera przepisy dotyczące korporacyjnej Zezwalaj, aby spełnić pewne standardy, takie jak FISMA zgodności.  Jeśli zostanie włączona obsługa zestaw Live SDK, aplikacja może nie zapewnienia zgodności z niektóre z tymi standardami.

Jeśli planujesz wyłącznie stale używać punktu końcowego v2.0 usługi Azure AD, można wyłączyć bezpiecznie zestaw Live SDK pomocy technicznej.

