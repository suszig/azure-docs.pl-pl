---
title: "ASP.NET w wersji 2 usługi Azure AD w sieci Web serwera pobieranie rozpoczęte — Config | Dokumentacja firmy Microsoft"
description: "Implementowanie logowania firmy Microsoft dla rozwiązania ASP.NET z aplikacji opartych na przeglądarce sieci web tradycyjnych przy użyciu standardowego protokołu OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 0c627802ccfba230dcde2dafffee26cb1c895791
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a>Tworzenie aplikacji (Express)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:
1. Zarejestrować aplikację za pośrednictwem [portalu rejestracji aplikacji firmy Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Wprowadź nazwę aplikacji i poczty e-mail
3.  Upewnij się, że zaznaczono opcję instrukcje konfiguracji
4.  Postępuj zgodnie z instrukcjami, aby dodać adres URL przekierowania do aplikacji

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Dodaj swoje informacje rejestracyjne aplikacji do rozwiązania (zaawansowane)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:
1. Przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app) do rejestrowania aplikacji
2. Wprowadź nazwę aplikacji i poczty e-mail 
3.  Upewnij się, że jest zaznaczona opcja instrukcje konfiguracji
4.  Kliknij przycisk `Add Platform`, a następnie wybierz pozycję`Web`
5.  Przejdź do programu Visual Studio i, w Eksploratorze rozwiązań wybierz projekt i przyjrzyj się okna właściwości (Jeśli nie widzisz okna właściwości, naciśnij klawisz F4)
6.  Zmiany, aby włączyć protokół SSL`True`
7.  Skopiuj adres URL protokołu SSL i dodać ten adres URL do listy adresów URL przekierowań w portalu rejestracji listę adresów URL przekierowań:<br/><br/>![Właściwości projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  Dodaj następujący kod w `web.config` znajduje się w folderze głównym w sekcji `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Zastąp `ClientId` wraz z identyfikatorem aplikacji został zarejestrowany
</li>
<li>
Zastąp `redirectUri` z adresem URL protokołu SSL projektu
</li>
</ol>
<!-- End Docs -->
