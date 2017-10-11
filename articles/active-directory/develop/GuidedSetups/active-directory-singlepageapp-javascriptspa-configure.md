---
title: "Usługi Azure AD w wersji 2 JS SPA instrukcje konfiguracji — Konfigurowanie | Dokumentacja firmy Microsoft"
description: "Jak aplikacje JavaScript SPA można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: adff529bfdc40006666cc643d49a302d7f1d09ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
## <a name="register-your-application"></a>Rejestrowanie aplikacji

Istnieje wiele sposobów, aby utworzyć aplikację, wybierz jeden z nich:

### <a name="option-1-register-your-application-express-mode"></a>Opcja 1: Rejestrowanie aplikacji (tryb Express)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:

1.  Zarejestrować aplikację za pośrednictwem [portalu rejestracji aplikacji firmy Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Wprowadź nazwę aplikacji i poczty e-mail
3.  Upewnij się, opcja *instrukcje konfiguracji* zaznaczono
4.  Postępuj zgodnie z instrukcjami, aby uzyskać identyfikator aplikacji i wklej go w kodzie

### <a name="option-2-register-your-application-advanced-mode"></a>Opcja 2: Rejestrowanie aplikacji (Tryb zaawansowany)

1. Przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app) do rejestrowania aplikacji
2. Wprowadź nazwę aplikacji i poczty e-mail 
3. Upewnij się, opcja *instrukcje konfiguracji* nie jest zaznaczone
4.  Kliknij przycisk `Add Platform`, a następnie wybierz pozycję`Web`
5. Dodaj `Redirect URL` odpowiada adres URL aplikacji, oparty na serwerze sieci web. Znajduje się w sekcjach poniżej instrukcje na temat set / uzyskać adres URL przekierowania w Visual Studio i języka Python.
6. Kliknij przycisk *Zapisz*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio instrukcje pobrania adres URL przekierowania
> Postępuj zgodnie z instrukcjami, aby uzyskać adres URL przekierowania:
> 1.    W *Eksploratora rozwiązań*, wybierz projekt i przyjrzyj się `Properties` okna (Jeśli nie widzisz okna właściwości, naciśnij klawisz `F4`)
> 2.    Skopiuj wartości z `URL` do Schowka:<br/> ![Właściwości projektu](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Przywraca *portalu rejestracji aplikacji* i wkleić wartość jako `Redirect URL` i kliknij przycisk "Zapisz"

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Adres URL przekierowania ustawienie dla języka Python
> Dla języka Python można ustawić sieci web port serwera przy użyciu wiersza polecenia. Ten przewodnik instalacji korzysta z portu 8080 dla odwołania, ale działanie może używać innych dostępnych portów. W każdym przypadku postępuj zgodnie z instrukcjami poniżej, aby zdefiniować adres URL przekierowania w aplikacji informacje rejestracyjne:<br/>
> - Przywraca *portalu rejestracji aplikacji* i ustaw `http://localhost:8080/` jako `Redirect URL`, lub użyj `http://localhost:[port]/` używania niestandardowego portu TCP (gdzie *[port]* jest niestandardowy port TCP Liczba) i kliknij przycisk "Zapisz"


#### <a name="configure-your-javascript-spa"></a>Skonfiguruj SPA skrypt JavaScript

1.  Utwórz plik o nazwie `msalconfig.js` zawierający informacje o rejestracji aplikacji. Jeśli używasz programu Visual Studio, wybierz projekt (folder główny projekt), kliknij prawym przyciskiem myszy i wybierz: `Add`  >  `New Item`  >  `JavaScript File`. Nadaj jej nazwę`msalconfig.js`
2.  Dodaj następujący kod do Twojej `msalconfig.js` pliku:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Zastąp <code>Enter_the_Application_Id_here</code> wraz z identyfikatorem aplikacji został zarejestrowany
</li>
</ol>
