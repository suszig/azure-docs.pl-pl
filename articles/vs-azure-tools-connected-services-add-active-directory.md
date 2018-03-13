---
title: "Dodawanie usługi Azure Active Directory przy użyciu usług połączonych w programie Visual Studio | Dokumentacja firmy Microsoft"
description: "Dodawanie przy użyciu okna dialogowego programu Visual Studio Dodaj połączone usługi Azure Active Directory"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: a767c93fb271f3aa33d9556c69c511bcac7cb0d5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Dodawanie usługi Azure Active Directory przy użyciu usług połączonych w programie Visual Studio
Za pomocą usługi Azure Active Directory (Azure AD), może obsługiwać pojedynczego logowania jednokrotnego (SSO) dla aplikacji sieci web platformy ASP.NET MVC ani uwierzytelnianie usługi Active Directory w usługach interfejsu API sieci Web. Azure Active Directory Authentication użytkowników można za pomocą ich kont z usługi Azure Active Directory do nawiązania połączenia aplikacji sieci web. Zalety usługi Azure Active Directory uwierzytelniania z interfejsu API sieci Web obejmują zwiększone bezpieczeństwo danych podczas udostępnianie interfejsu API z aplikacji sieci web. Z usługą Azure AD nie masz do zarządzania systemem oddzielne z zarządzaniem konta i użytkownika.

## <a name="prerequisites"></a>Wymagania wstępne
- Konto platformy Azure — Jeśli nie masz konta platformy Azure, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Nawiązać połączenia z usługą Azure Active Directory za pomocą okna dialogowego połączenia usługi
1. W programie Visual Studio Utwórz lub Otwórz projekt platformy ASP.NET MVC lub projekt interfejsu API sieci Web platformy ASP.NET.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **usług połączonych** węzeł i z menu kontekstowego wybierz **dodać usług połączonych**.

1. Na **usług połączonych** wybierz pozycję **uwierzytelniania w usłudze Azure Active Directory**.
   
    ![Strona usług połączonych](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Na **wprowadzenie** strony **Konfigurowanie usługi Azure AD Authentication** kreatora wybierz **dalej**.
   
    ![Strona wprowadzenia](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. Na **jednokrotnego na** strony **Konfigurowanie usługi Azure AD Authentication** kreatora, wybierz domenę z **domeny** listy rozwijanej. Listy domen zawiera wszystkie domeny dostępny przez konta na liście w oknie dialogowym Ustawienia konta. Alternatywnie, można wprowadzić nazwę domeny, jeśli nie możesz znaleźć to, czego szukasz, takich jak `mydomain.onmicrosoft.com`. Można wybrać opcję, aby utworzyć aplikację usługi Azure Active Directory lub użyć ustawień z istniejącej aplikacji usługi Azure Active Directory. Wybierz **dalej** po zakończeniu.
   
    ![Jednokrotnego na stronie](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. Na **dostęp do katalogu** strony **Konfigurowanie usługi Azure AD Authentication** kreatora upewnij się, że **odczytuj dane katalogu** zaznaczenia pola wyboru. 
   
    ![Strona dostępu do katalogu](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Wybierz **Zakończ** dodać niezbędną konfigurację kodu i odwołania do włączenia projektu do uwierzytelniania usługi Azure AD. Domeny usługi Active Directory można wyświetlić na [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Visual Studio spowoduje wyświetlenie [co się stało](#how-your-project-is-modified) artykuł, aby pokazać, jak projekt został zmodyfikowany. Jeśli chcesz sprawdzić, czy wszystko działało, otwórz jeden z plików konfiguracyjnych zmodyfikowane i upewnij się, że ustawienia wymienione w artykule istnieją. 

## <a name="how-your-project-is-modified"></a>Jak zmienić jest projektu
Po uruchomieniu kreatora programu Visual Studio dodaje usługi Azure Active Directory i skojarzone odwołania do projektu. Pliki konfiguracji i plików kodu w projekcie są również zmodyfikowany w celu dodania pomocy technicznej dla usługi Azure AD. Określone zmiany, które powoduje, że program Visual Studio są zależne od typu projektu. Aby uzyskać szczegółowe informacje dotyczące sposobu zostaną zmodyfikowane projekty składnika ASP.NET MVC, zobacz [jakie projektów MVC — happened](http://go.microsoft.com/fwlink/p/?LinkID=513809). Dla projektów interfejsu API sieci Web, zobacz [co się stało — projekty interfejsu API sieci Web](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Następne kroki
* [Forum MSDN dotyczące usługi Azure Active Directory](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Dokumentacja usługi Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Wpis w blogu: Wprowadzenie do usługi Azure Active Directory](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

