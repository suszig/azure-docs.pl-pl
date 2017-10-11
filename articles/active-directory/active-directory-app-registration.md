---
title: "Rejestracja aplikacji usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak zarejestrować aplikację w usłudze Azure Active Directory za pomocą portalu Azure"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: priyamo
ms.reviewer: elisol
ms.openlocfilehash: 2f2817688beb2028fd0bba8522827d87a0097f21
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Zarejestrować aplikację z dzierżawą usługi Azure Active Directory

Aby zarejestrować aplikację z dzierżawą usługi Azure Active Directory (Azure AD), można użyć portalu Azure. Tworzy identyfikator aplikacji dla aplikacji i umożliwia odbierać tokeny.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz dzierżawy usługi Azure AD, wybierając konto w prawym górnym rogu strony.
3. W okienku nawigacji po lewej stronie wybierz **więcej usług**, kliknij przycisk **rejestracji aplikacji**i kliknij przycisk **Dodaj**.
4. Postępuj zgodnie z monitami i utwórz nową aplikację. Jeśli chcesz szczegółowe przykłady dla aplikacji sieci web lub natywnych aplikacji, zapoznaj się naszego [quickstarts](active-directory-developers-guide.md).
  * W przypadku aplikacji sieci Web, podaj **adres URL logowania**, który jest podstawowy adres URL aplikacji, w którym użytkownicy mogą rejestrować w np. `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Dla natywnych aplikacji, podaj **identyfikator URI przekierowania**, który używa usługi Azure AD do zwracania odpowiedzi tokenu. Wprowadź wartość specyficzną dla Twojej aplikacji, np. `http://MyFirstAADApp`
5. Po zakończeniu rejestracji usługi Azure AD przypisuje aplikacji identyfikatora klienta unikatowy identyfikator aplikacji.

## <a name="update-application-settings-from-the-azure-portal"></a>Aktualizowanie ustawień aplikacji z portalu Azure

Można łatwo zmodyfikować ustawienia istniejących aplikacji przy użyciu portalu Azure. Na przykład można skonfigurować adres URL odpowiedzi, który jest, gdzie usługa Azure AD wystawia token odpowiedzi. Możesz na przykład skonfigurować uprawnień dotyczących innych aplikacji, aby umożliwić aplikacji dostęp do interfejsu API programu Microsoft Graph. Można to zrobić wszystkich za pośrednictwem strony ustawień aplikacji.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz dzierżawy usługi Azure AD, wybierając konto w prawym górnym rogu strony.
3. W okienku nawigacji po lewej stronie wybierz **więcej usług**, kliknij przycisk **rejestracji aplikacji**i wybierz aplikację z listy.
4. Kliknij przycisk **ustawienia** otwarcie strony ustawień aplikacji.
  * **Właściwości** strona umożliwia modyfikowanie ogólne informacje dla aplikacji. W tym nazwy aplikacji, adres URL logowania jednokrotnego i adresu URL wylogowania.
  * **Adresy URL odpowiedzi** strony umożliwia dodanie adresu URL odpowiedzi, który jest, gdzie usługi Azure AD wysyła odpowiedzi tokenu.
  * **Właścicieli** strony umożliwia dodanie właściciele aplikacji.
  * **Uprawnienia** strony pozwala na skonfigurowanie uprawnień dla aplikacji. Na przykład, aby uzyskać dostęp do interfejsu API programu Microsoft Graph, kliknij przycisk **Dodaj** i wybierz **Microsoft Graph** w selektorze interfejsu API, a następnie wybierz uprawnienia wymagane, na przykład **Czytaj dane katalogu**.
  * **Klucze** strony umożliwia dodanie hasła aplikacji. Klucz tajny będą wyświetlane tylko w przypadku, gdy natychmiast po utworzeniu, tak Pamiętaj skopiować go do dalszego użytku.

## <a name="use-the-inline-manifest-editor"></a>Za pomocą wbudowanego edytora manifestu

Edytor manifestu wbudowanego umożliwia modyfikowanie niektórych właściwości aplikacji, które nie są widoczne bezpośrednio w portalu Azure. Na przykład można go zmodyfikować identyfikator URI aplikacji aplikacji lub włączyć przepływu niejawnego OAuth2.0 zamiast domyślnego autoryzacji grant kodu przepływu.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz dzierżawy usługi Azure AD, wybierając konto w prawym górnym rogu strony.
3. W okienku nawigacji po lewej stronie wybierz **więcej usług**, kliknij przycisk **rejestracji aplikacji**i wybierz aplikację z listy.
4. Kliknij przycisk **manifestu** ze strony aplikacji, aby otworzyć Edytor manifestu w tekście.
5. Bezpośrednio można wprowadzić zmiany w manifeście i zapisz go, gdy wszystko jest gotowe. Alternatywnie możesz pobrać manifest, aby otworzyć go w edytorze ulubionych i przekaż zaktualizowaną manifestu.

## <a name="next-steps"></a>Następne kroki

1. Zapoznaj się z [Quickstarts](active-directory-developers-guide.md) uzyskać szczegółowe wskazówki dotyczące aplikacji uwierzytelniania przy użyciu usługi Azure AD.
2. Zapoznaj się z naszą pełną listę przykłady kodu w [GitHub](https://github.com/azure-samples).
