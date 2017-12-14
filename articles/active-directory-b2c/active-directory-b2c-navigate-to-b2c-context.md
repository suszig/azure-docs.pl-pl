---
title: "Azure Active Directory B2C: Przełączanie się na dzierżawę B2C | Microsoft Docs"
description: "Jak przełączyć się do kontekstu dzierżawy usługi Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 6c1fd08c52f33a062d06e0593cbbe00346bb44f1
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Przełączanie się do dzierżawy usługi Azure AD B2C

Aby skonfigurować usługę Azure AD B2C, musisz być w kontekście swojej dzierżawy usługi Azure AD B2C.

## <a name="log-into-azure-ad-b2c-tenant"></a>Logowanie się do dzierżawy usługi Azure AD B2C

Aby przejść do swojej dzierżawy usługi Azure AD B2C, musisz się zalogować do witryny Azure Portal jako administrator globalny dzierżawy usługi Azure AD B2C.

1. Zaloguj się do [Azure Portal](http://portal.azure.com).
1. Przełącz dzierżawy, klikając swój adres e-mail lub zdjęcie w prawym górnym rogu.
1. Z wyświetlonej listy `Directory` wybierz dzierżawę usługi Azure AD B2C, którą chcesz zarządzać.

Witryna Azure Portal zostanie odświeżona.  Od tej chwili kontekstem logowania w witrynie Azure Portal będzie Twoja dzierżawa usługi Azure AD B2C.

## <a name="navigate-to-the-b2c-features-blade"></a>Przechodzenie do bloku funkcji B2C

1. Kliknij pozycję **Przeglądaj** w obszarze nawigacji po lewej stronie.
1. Kliknij pozycję **> Więcej usług**, a następnie wyszukaj tekst `Azure AD B2C` w okienku nawigacyjnym po lewej stronie.  (Aby przypiąć tę pozycję do tablicy startowej po lewej stronie, kliknij gwiazdkę na lewo od usługi Azure AD B2C)
1. Kliknij pozycję **Azure AD B2C**, aby otworzyć blok funkcji B2C.
   
    ![Zrzut ekranu z blokiem przeglądania w poszukiwaniu funkcji B2C](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Tylko administrator globalny dzierżawy B2C może uzyskiwać dostęp do bloku funkcji B2C. Administrator globalny innej dzierżawy ani użytkownik dowolnej dzierżawy nie mogą uzyskać dostępu do tego bloku.  Na swoją dzierżawę B2C możesz przełączyć się, używając przełącznika dzierżawy w prawym górnym rogu witryny Azure Portal.
