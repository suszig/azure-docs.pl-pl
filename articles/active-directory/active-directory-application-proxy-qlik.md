---
title: Serwera Proxy aplikacji usługi Azure AD i znaczeniu Qlik | Dokumentacja firmy Microsoft
description: Włącz serwer Proxy aplikacji w portalu Azure i zainstaluj łączniki dla zwrotnego serwera proxy.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/20/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c34af889f65e0fa91f97655803af099dadcabc5d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="application-proxy-and-qlik-sense"></a>Serwer Proxy aplikacji i wykrywanie Qlik 
Azure Active Directory serwera Proxy aplikacji i znaczeniu Qlik ma współpracę ze sobą aby mieć pewność, że łatwo można używać serwera Proxy aplikacji do zapewniania dostępu zdalnego dla danego wdrożenia Qlik znaczeniu.  

## <a name="prerequisites"></a>Wymagania wstępne 
W pozostałej części w tym scenariuszu założono, należy wykonać następujące czynności:
 
- Skonfigurowane [znaczeniu Qlik](https://help.qlik.com/sense/1.1/Content/Introduction.htm). 
- Zainstalował łącznik serwera Proxy aplikacji 

## <a name="install-an-application-proxy-connector"></a>Instalowanie łącznika serwera Proxy aplikacji 
Jeśli już ma włączony serwer Proxy aplikacji, a ma zainstalowany łącznik, możesz pominąć tę sekcję i przenieść na [Dodaj aplikację do usługi Azure AD przy użyciu serwera Proxy aplikacji](application-proxy-ping-access.md). 

Łącznik serwera Proxy aplikacji jest usługi systemu Windows Server, który kieruje ruch z zdalnego pracowników do opublikowanej aplikacji. Aby uzyskać szczegółowe instrukcje dotyczące instalacji, zobacz [Włączanie serwera Proxy aplikacji w portalu Azure](active-directory-application-proxy-enable.md). 


1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/) jako administrator globalny. 
2. Wybierz usługę Azure Active Directory > serwer proxy aplikacji. 
3. Wybierz opcję Pobierz łącznik, aby rozpocząć pobieranie łącznika serwera Proxy aplikacji. Postępuj zgodnie z instrukcjami instalacji. 
 
>[!NOTE]
>Pobieranie łącznika automatycznie należy włączyć serwer Proxy aplikacji dla katalogu, ale jeśli nie możesz wybrać **Włączanie serwera Proxy aplikacji**. 
 
## <a name="publish-your-applications-in-azure"></a>Publikowanie własnych aplikacji na platformie Azure 
Aby opublikować QlikSense, należy opublikować dwóch aplikacji na platformie Azure.  

### <a name="application-1"></a>Aplikacja #1: 
Wykonaj następujące kroki, aby opublikować aplikację. Bardziej szczegółowe wskazówki kroki 1 – 8, zobacz [publikowanie aplikacji przy użyciu serwera Proxy aplikacji usługi Azure AD](application-proxy-publish-azure-portal.md). 


1. Jeśli nie w ostatniej sekcji, zaloguj się do portalu Azure jako administrator globalny. 
2. Wybierz **usługi Azure Active Directory** > **aplikacje dla przedsiębiorstw**. 
3. Wybierz **Dodaj** w górnej części bloku. 
4. Wybierz **lokalnej aplikacji**. 
5.       Wypełnij wymagane pola informacje o nowej aplikacji. Użyj poniższych wskazówek dla ustawienia: 
    - **Wewnętrzny adres URL**: Ta aplikacja ma wewnętrzny adres URL jest adresem URL QlikSense, sama. Na przykład **https&#58;//demo.qlikemm.com** 
    - **Metoda wstępnego uwierzytelnienia**: Azure Active Directory (zalecane, ale nie są wymagane) 
1.       Wybierz **Dodaj** w dolnej części bloku. Aplikacja zostanie dodany, a następnie otwiera menu szybki start. 
2.       Wybierz z menu szybki start **przypisać użytkownika do testowania**, i Dodaj przynajmniej jednego użytkownika do aplikacji. Upewnij się, że to konto testu ma dostęp do aplikacji lokalnych. 
3.       Wybierz **przypisać** można zapisać przypisanie użytkownika testu. 
4.       (Opcjonalnie) W bloku zarządzania aplikacjami wybierz rejestracji jednokrotnej. Wybierz **ograniczone delegowanie protokołu Kerberos** z listy rozwijanej, a następnie wypełnij wymagane pola, zgodnie z konfiguracją Qlik. Wybierz pozycję **Zapisz**. 

### <a name="application-2"></a>Aplikacja #2: 
Wykonaj te same czynności, jak w przypadku aplikacji #1, z następującymi wyjątkami: 

**Krok #5**: adres URL wewnętrznego powinno być teraz do adresu URL QlikSense port uwierzytelniania używany przez aplikację. Wartość domyślna to **4244** dla protokołu HTTPS i 4248 dla protokołu HTTP. Ex: **https&#58;//demo.qlik.com:4244** 
**krok nr 10:** nie Konfigurowanie logowania jednokrotnego i pozostawić **logowanie jednokrotne wyłączone**
 
 
## <a name="testing"></a>Testowanie 
Aplikacja jest teraz gotowy do testowania. Dostęp do zewnętrznego adresu URL używany do publikowania QlikSense w aplikacji #1 i logowania jako użytkownik przypisane do obydwu aplikacji.  

## <a name="next-steps"></a>Następne kroki

- [Publikowanie aplikacji przy użyciu serwera Proxy aplikacji](application-proxy-publish-azure-portal.md)
- [Praca z serwera Proxy aplikacji łączniki](active-directory-application-proxy-connectors-azure-portal.md).
