---
title: "Omówienie protokołu .NET usługi Azure AD | Microsoft Docs"
description: "Jak używać wiadomości HTTP do autoryzacji dostępu do aplikacji sieci Web i interfejsów API sieci Web w dzierżawie za pomocą usługi Azure AD."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
translationtype: Human Translation
ms.sourcegitcommit: 8db20ca36190197abcb73200eeed79e004435af9
ms.openlocfilehash: d851dec517c162073ade6ffb4ebcf972c5054698


---
<!--TODO: Introduction -->

## <a name="register-your-application-with-your-ad-tenant"></a>Rejestrowanie aplikacji w dzierżawie usługi AD
Po pierwsze należy zarejestrować aplikację w dzierżawie usługi Azure Active Directory (Azure AD). Spowoduje to nadanie aplikacji identyfikatora oraz umożliwi jej otrzymywanie tokenów.

* Zaloguj się do [Azure Portal](https://portal.azure.com).
* Wybierz dzierżawę usługi Azure AD, klikając swoje konto w prawym górnym rogu strony.
* W okienku nawigacji po lewej stronie kliknij pozycję **Azure Active Directory**.
* Kliknij pozycję **Rejestracje aplikacji**, a następnie kliknij pozycję **Dodaj**.
* Postępuj zgodnie z monitami i utwórz nową aplikację. Na potrzeby tego samouczka nie ma znaczenia, czy jest to aplikacja sieci Web, czy aplikacja natywna, ale jeśli chcesz zapoznać się z konkretnymi przykładami dla aplikacji sieci Web lub aplikacji natywnych, zobacz nasze [przewodniki szybkiego startu](../articles/active-directory/active-directory-developers-guide.md).
  * W przypadku aplikacji sieci Web podaj **Adres URL logowania**, czyli podstawowy adres URL aplikacji, pod którym użytkownicy mogą się logować, np. `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * W przypadku aplikacji natywnych podaj **Identyfikator URI przekierowania**, którego usługa Azure AD będzie używać do zwracania odpowiedzi z tokenem. Wprowadź wartość specyficzną dla Twojej aplikacji, np. `http://MyFirstAADApp`
* Po zakończeniu rejestracji usługa Azure AD przypisze aplikacji unikatowy identyfikator klienta (identyfikator aplikacji). Ta wartość będzie potrzebna w kolejnych sekcjach, więc skopiuj ją ze strony aplikacji.



<!--HONumber=Dec16_HO4-->


