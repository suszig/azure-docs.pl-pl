<properties
    pageTitle="Publikowanie aplikacji przy użyciu serwera proxy aplikacji usługi Azure AD | Microsoft Azure"
    description="Publikowanie aplikacji lokalnych w chmurze przy użyciu serwera proxy aplikacji usługi Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="kgremban"/>


# Publikowanie aplikacji przy użyciu serwera proxy aplikacji usługi Azure AD


Po włączeniu serwera proxy aplikacji usługi Microsoft Azure Active Directory można publikować aplikacje lokalne, aby umożliwić użytkownikom zdalnym dostęp do tych aplikacji spoza sieci prywatnej.

W tym artykule przedstawiono kroki publikowania aplikacji działających w sieci lokalnej i zapewniania bezpiecznego dostępu zdalnego spoza tej sieci. Jeśli nie skonfigurowano jeszcze serwera proxy aplikacji i nie zainstalowano żadnych łączników, przed przejściem dalej wykonaj czynności opisane w temacie [Włączanie serwera proxy aplikacji w portalu Azure](active-directory-application-proxy-enable.md).

Jeśli korzystasz z serwera proxy aplikacji usługi Azure AD po raz pierwszy, zalecamy przetestowanie łącznika przez opublikowanie witryny sieci Web z poziomu sieci prywatnej przed rozpoczęciem publikowania aplikacji.

> [AZURE.NOTE] Serwer proxy aplikacji jest funkcją dostępną tylko w przypadku uaktualnienia do wersji usługi Azure Active Directory w warstwie Premium lub Podstawowa. Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).

## Publikowanie aplikacji za pomocą kreatora

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator.
2. Przejdź do obszaru Active Directory i wybierz katalog, w którym włączono serwer proxy aplikacji.

    ![Active Directory — ikona](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Kliknij kartę **Aplikacje**, a następnie kliknij przycisk **Dodaj** w dolnej części ekranu

    ![Dodawanie aplikacji](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Wybierz pozycję **Opublikuj aplikację dostępną spoza sieci**.

    ![Opublikuj aplikację dostępną spoza sieci](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Podaj następujące informacje dotyczące aplikacji:

    - **Nazwa**: przyjazna nazwa aplikacji. Nazwa musi być unikatowa w katalogu.
    - **Wewnętrzny adres URL**: adres używany przez łącznik serwera proxy aplikacji do uzyskania dostępu do aplikacji z poziomu sieci prywatnej. Możesz wprowadzić określoną ścieżkę na serwerze zaplecza, która zostanie opublikowana, podczas gdy pozostała część serwera pozostanie nieopublikowana. W ten sposób można publikować różne lokacje na tym samym serwerze, nadając im różne nazwy i reguły dostępu.
    - **Metoda uwierzytelniania wstępnego**: sposób, w jaki serwer proxy aplikacji zweryfikuje użytkowników przed udzieleniem im dostępu do aplikacji. Wybierz jedną z opcji z menu rozwijanego.

        - Azure Active Directory: serwer proxy aplikacji przekieruje użytkowników, aby zalogowali się w usłudze Azure AD, co umożliwi uwierzytelnienie ich uprawnień do katalogu i aplikacji.
        - Przekazywanie: uwierzytelnienie nie jest wymagane do uzyskania dostępu do tej aplikacji.

    ![Właściwości aplikacji](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Aby zakończyć działanie kreatora, kliknij znacznik wyboru w dolnej części ekranu. Aplikacja jest teraz zdefiniowana w usłudze Azure AD.


## Przypisywanie użytkowników i grup do aplikacji

Aby użytkownicy mogli uzyskać dostęp do opublikowanej aplikacji, należy ich przypisać pojedynczo lub w grupach. W przypadku aplikacji wymagających uwierzytelnienia wstępnego spowoduje to przydzielenie uprawnień do używania aplikacji. W przypadku aplikacji niewymagających uwierzytelnienia wstępnego użytkownicy nie potrzebują uprawnień, ale mimo to muszą być przypisani do aplikacji, aby była ona wyświetlana na ich liście aplikacji.

1. Po zakończeniu pracy kreatora Dodawanie aplikacji zostanie wyświetlona strona Szybki start aplikacji. Aby zarządzać dostępem do aplikacji, wybierz pozycję **Użytkownicy i grupy**.

    ![Przypisywanie użytkowników na stronie Szybki start serwera proxy aplikacji — zrzut ekranu](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Wyszukaj określonych użytkowników w katalogu lub wyświetl wszystkich użytkowników. Kliknij znacznik wyboru, aby wyświetlić wyniki.

    ![Wyszukiwanie grup lub użytkowników — zrzut ekranu.](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Zaznacz użytkowników lub grupy do przypisania do aplikacji, a następnie kliknij pozycję **Przypisz**. Zostanie wyświetlony monit o potwierdzenie tej czynności.

> [AZURE.NOTE] W przypadku aplikacji korzystających ze zintegrowanego uwierzytelniania systemu Windows można przypisywać wyłącznie użytkowników synchronizowanych z lokalnej usługi Active Directory i grupy takich użytkowników. Użytkowników logujących się za pomocą konta Microsoft i gości nie można przypisywać do aplikacji opublikowanych przy użyciu serwera proxy aplikacji usługi Azure Active Directory. Upewnij się, że użytkownicy logują się za pomocą poświadczeń należących do tej samej domeny co publikowana aplikacja.


## Konfiguracja zaawansowana

Na stronie Konfiguracja można modyfikować opublikowane aplikacje lub konfigurować opcje zaawansowane. Na tej stronie możesz dostosować aplikację, zmieniając jej nazwę lub przekazując logo. Możesz także zarządzać regułami dostępu, takimi jak metoda uwierzytelniania wstępnego czy uwierzytelnianie wieloskładnikowe.

![Konfiguracja zaawansowana](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Aplikacje opublikowane przy użyciu serwera proxy aplikacji usługi Azure Active Directory są wyświetlane na liście Aplikacje w usłudze Azure AD i można zarządzać nimi z tego poziomu.

Jeśli wyłączysz usługi serwera proxy aplikacji po opublikowaniu aplikacji, aplikacje te nie zostaną usunięte, ale nie będą już dostępne spoza sieci prywatnej.

Aby wyświetlić aplikację i sprawdzić, czy jest dostępna, kliknij dwukrotnie nazwę aplikacji. Jeśli usługa serwera proxy aplikacji jest wyłączona i aplikacja jest niedostępna, w górnej części ekranu zostanie wyświetlone ostrzeżenie.

Aby usunąć aplikację, zaznacz ją na liście, a następnie kliknij polecenie **Usuń**.

## Następne kroki

- [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)
- [Włączanie logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
- [Praca z aplikacjami obsługującymi oświadczenia](active-directory-application-proxy-claims-aware-apps.md)

Aby zapoznać się z najnowszymi informacjami i aktualizacjami, zobacz [blog dotyczący serwera proxy aplikacji](http://blogs.technet.com/b/applicationproxyblog/)



<!--HONumber=Jun16_HO2-->


