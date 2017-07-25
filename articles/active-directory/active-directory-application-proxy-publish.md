---
title: "Publikowanie aplikacji przy użyciu serwera proxy aplikacji usługi Azure AD | Microsoft Docs"
description: "Publikowanie aplikacji lokalnych w chmurze przy użyciu serwera proxy aplikacji usługi Azure AD w klasycznym portalu."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 526efc5b025f8050f40c49d0a07a9c2713509493
ms.contentlocale: pl-pl
ms.lasthandoff: 05/05/2017

---

# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publikowanie aplikacji przy użyciu serwera proxy aplikacji usługi Azure AD

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Klasyczna witryna Azure Portal](active-directory-application-proxy-publish.md)

Serwer proxy aplikacji usługi Azure AD pomaga wspierać pracowników zdalnych przez publikowanie lokalnych aplikacji w celu umożliwienia dostępu do nich za pośrednictwem Internetu. Przed rozpoczęciem pracy należy [włączyć serwer proxy aplikacji w klasycznym portalu Azure](active-directory-application-proxy-enable.md). W tym artykule przedstawiono kroki publikowania aplikacji działających w sieci lokalnej i zapewniania bezpiecznego dostępu zdalnego spoza tej sieci. Po ukończeniu tego artykułu możesz przystąpić do konfigurowania aplikacji przy użyciu spersonalizowanych informacji lub wymagań dotyczących zabezpieczeń.

> [!NOTE]
> Serwer proxy aplikacji jest funkcją dostępną tylko w przypadku uaktualnienia do wersji usługi Azure Active Directory w warstwie Premium lub Podstawowa. Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md). Jeśli chcesz używać serwera proxy aplikacji, możesz [publikować aplikacje w witrynie Azure Portal](application-proxy-publish-azure-portal.md).

## <a name="publish-an-app-using-the-wizard"></a>Publikowanie aplikacji za pomocą kreatora
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator.
2. Przejdź do obszaru Active Directory i wybierz katalog, w którym włączono serwer proxy aplikacji.
   
    ![Active Directory — ikona](./media/active-directory-application-proxy-publish/ad_icon.png)
3. Kliknij kartę **Aplikacje**, a następnie kliknij przycisk **Dodaj** w dolnej części ekranu
   
    ![Dodawanie aplikacji](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)
4. Wybierz pozycję **Opublikuj aplikację dostępną spoza sieci**.
   
    ![Opublikuj aplikację dostępną spoza sieci](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)
5. Podaj następujące informacje dotyczące aplikacji:
   
   * **Nazwa**: przyjazna nazwa aplikacji. Nazwa musi być unikatowa w katalogu.
   * **Wewnętrzny adres URL**: adres używany przez łącznik serwera proxy aplikacji do uzyskania dostępu do aplikacji z poziomu sieci prywatnej. Możesz wprowadzić określoną ścieżkę na serwerze zaplecza, która zostanie opublikowana, podczas gdy pozostała część serwera pozostanie nieopublikowana. W ten sposób można publikować różne lokacje na tym samym serwerze, nadając im różne nazwy i reguły dostępu.
     
     > [!TIP]
     > W przypadku publikowania ścieżki upewnij się, że zawiera ona wszystkie niezbędne obrazy, skrypty i arkusze stylów dla aplikacji. Na przykład jeśli aplikacja znajduje się w katalogu https://yourapp/app i korzysta z obrazów znajdujących się w katalogu https://yourapp/media, należy opublikować https://yourapp/ jako ścieżkę.
     > 
     > 
   * **Metoda uwierzytelniania wstępnego**: sposób, w jaki serwer proxy aplikacji weryfikuje użytkowników przed udzieleniem im dostępu do aplikacji. Wybierz jedną z opcji z menu rozwijanego.
     
     * Azure Active Directory: serwer proxy aplikacji przekierowuje użytkowników, aby zalogowali się w usłudze Azure AD, co umożliwia uwierzytelnienie ich uprawnień do katalogu i aplikacji.
     * Przekazywanie: uwierzytelnienie nie jest wymagane do uzyskania dostępu do tej aplikacji.
     
     ![Właściwości aplikacji](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  
6. Aby zakończyć działanie kreatora, kliknij znacznik wyboru w dolnej części ekranu. Aplikacja jest teraz zdefiniowana w usłudze Azure AD.

## <a name="assign-users-and-groups-to-the-application"></a>Przypisywanie użytkowników i grup do aplikacji
Aby użytkownicy mogli uzyskać dostęp do opublikowanej aplikacji, należy ich przypisać pojedynczo lub w grupach. (Pamiętaj, aby sobie także przypisać dostęp). W tym celu każdy użytkownik musi mieć licencję Azure w wersji Podstawowa lub wyższej. Można przypisać licencje pojedynczym użytkownikom lub grupom. Zobacz [Przypisywanie użytkowników do aplikacji](active-directory-applications-guiding-developers-assigning-users.md), aby uzyskać więcej szczegółów. 

W przypadku aplikacji wymagających uwierzytelnienia wstępnego spowoduje to przydzielenie uprawnień do używania aplikacji. W przypadku aplikacji niewymagających uwierzytelnienia wstępnego użytkownicy mogą nadal zostać przypisani do aplikacji, aby była ona wyświetlana na ich liście aplikacji, takiej jak MyApps.

1. Po zakończeniu pracy kreatora Dodawanie aplikacji zostanie wyświetlona strona Szybki start aplikacji. Aby zarządzać dostępem do aplikacji, wybierz pozycję **Użytkownicy i grupy**.
   
    ![Przypisywanie użytkowników na stronie Szybki start serwera proxy aplikacji — zrzut ekranu](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)
2. Wyszukaj określonych użytkowników w katalogu lub wyświetl wszystkich użytkowników. Aby wyświetlić wyniki, kliknij znacznik wyboru.
   
      ![Wyszukiwanie grup lub użytkowników — zrzut ekranu.](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)
3. Zaznacz użytkowników lub grupy do przypisania do aplikacji, a następnie kliknij pozycję **Przypisz**. Zostanie wyświetlony monit o potwierdzenie tej akcji.

> [!NOTE]
> W przypadku aplikacji korzystających ze zintegrowanego uwierzytelniania systemu Windows można przypisywać wyłącznie użytkowników synchronizowanych z lokalnej usługi Active Directory i grupy takich użytkowników. Użytkowników logujących się za pomocą konta Microsoft i gości nie można przypisywać do aplikacji opublikowanych przy użyciu serwera proxy aplikacji usługi Azure Active Directory. Upewnij się, że użytkownicy logują się za pomocą poświadczeń należących do tej samej domeny co publikowana aplikacja.
> 
> 

## <a name="test-your-published-application"></a>Testowanie opublikowanej aplikacji
Po opublikowaniu aplikacji można ją przetestować, przechodząc do opublikowanego adresu URL. Upewnij się, że można uzyskać do niej dostęp, jest poprawnie renderowana i wszystko działa zgodnie z oczekiwaniami. Jeśli wystąpią problemy lub komunikat o błędzie, spróbuj skorzystać z [przewodnika rozwiązywania problemów](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfigurowanie aplikacji
Na stronie Konfiguracja można modyfikować opublikowane aplikacje lub konfigurować opcje zaawansowane. Na tej stronie możesz dostosować aplikację, zmieniając jej nazwę lub przekazując logo. Możesz także zarządzać regułami dostępu, takimi jak metoda uwierzytelniania wstępnego czy uwierzytelnianie wieloskładnikowe.

![Konfiguracja zaawansowana](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)

Aplikacje opublikowane przy użyciu serwera proxy aplikacji usługi Azure Active Directory są wyświetlane na liście Aplikacje w usłudze Azure AD i można zarządzać nimi z tego poziomu.

Jeśli wyłączysz usługi serwera proxy aplikacji po opublikowaniu aplikacji, aplikacje te nie będą już dostępne spoza sieci prywatnej. Nie powoduje to usunięcia aplikacji.

Aby wyświetlić aplikację i sprawdzić, czy jest dostępna, kliknij dwukrotnie nazwę aplikacji. Jeśli usługa serwera proxy aplikacji jest wyłączona i aplikacja jest niedostępna, w górnej części ekranu zostanie wyświetlone ostrzeżenie.

Aby usunąć aplikację, zaznacz ją na liście, a następnie kliknij polecenie **Usuń**.

## <a name="next-steps"></a>Następne kroki
* [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)
* [Włączanie logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
* [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
* [Praca z aplikacjami obsługującymi oświadczenia](active-directory-application-proxy-claims-aware-apps.md)

Aby zapoznać się z najnowszymi informacjami i aktualizacjami, zobacz [blog dotyczący serwera proxy aplikacji](http://blogs.technet.com/b/applicationproxyblog/)


