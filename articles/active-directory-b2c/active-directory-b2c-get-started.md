<properties
    pageTitle="Azure Active Directory B2C (wersja zapoznawcza): tworzenie dzierżawy usługi Azure Active Directory B2C | Microsoft Azure"
    description="Temat dotyczący sposobu tworzenia dzierżawy usługi Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="swkrish"/>

# Azure Active Directory B2C (wersja zapoznawcza): tworzenie dzierżawy usługi Azure Active Directory B2C

Aby rozpocząć korzystanie z usługi Microsoft Azure Active Directory (Azure AD) B2C, wykonaj trzy czynności opisane w tym artykule.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Krok 1. Tworzenie konta na potrzeby subskrypcji platformy Azure

Jeśli masz już subskrypcję platformy Azure, pomiń ten krok. W przeciwnym razie utwórz konto na potrzeby [subskrypcji platformy Azure](../active-directory/sign-up-organization.md) i uzyskaj dostęp do usługi Azure AD B2C.

> [AZURE.NOTE]
Obecnie można bezpłatnie korzystać z usługi Azure AD B2C w wersji zapoznawczej, ale jej użycie jest ograniczone do 50 000 użytkowników w dzierżawie. Do uzyskiwania dostępu do [klasycznego portalu Azure](http://manage.windowsazure.com/) jest wymagana subskrypcja platformy Azure.

## Krok 2. Tworzenie dzierżawy usługi Azure AD B2C

Wykonaj poniższe kroki, aby utworzyć nową dzierżawę usługi Azure AD B2C. Obecnie funkcji B2C nie można włączać w istniejących katalogach.

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator subskrypcji. Jest to konto służbowe lub szkolne bądź konto Microsoft zastosowane podczas tworzenia konta platformy Azure.
2. Kliknij pozycje **Nowy**  >  **Usługi aplikacji**  >  **Active Directory**  >  **Katalog**  >  **Utwórz niestandardowy**.

    ![Zrzut ekranu przedstawiający rozpoczęcie tworzenia dzierżawy](./media/active-directory-b2c-get-started/new-directory.png)

3. Wybierz wartości **Nazwa**, **Nazwa domeny** i **Kraj lub region** dzierżawy.
4. Zaznacz opcję **To jest katalog B2C**.
5. Kliknij znacznik wyboru, aby zakończyć czynność.

    ![Zrzut ekranu przedstawiający znacznik wyboru służący do tworzenia katalogu B2C](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Dzierżawa została utworzona i będzie wyświetlana w rozszerzeniu usługi Active Directory. Jesteś teraz również administratorem globalnym dzierżawy. W razie potrzeby możesz dodawać innych administratorów globalnych.

## Krok 3. Przechodzenie do bloku funkcji B2C w portalu Azure

1. Przejdź do rozszerzenia usługi Active Directory na pasku nawigacyjnym po lewej stronie.
2. Znajdź dzierżawę na karcie **Katalog** i kliknij ją.
3. Kliknij kartę **Konfigurowanie**.
4. Kliknij link **Zarządzaj ustawieniami B2C** w sekcji **Administracja usługą B2C**.

    ![Zrzut ekranu przedstawiający konfigurację katalogu dla usługi B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Na nowej karcie lub w nowym oknie przeglądarki zostanie otwarty blok funkcji B2C nowego portalu Azure.

    > [AZURE.IMPORTANT]
    Udostępnienie dzierżawy w portalu Azure może potrwać 2–3 minuty. Aby rozwiązać ten problem, po pewnym czasie wykonaj ponownie opisane kroki. Jeśli to nie pomoże, skontaktuj się z pomocą techniczną.

6. Przypnij ten blok do tablicy startowej, aby był łatwo dostępny. (Narzędzie przypinania jest oznaczone na czerwono w prawym górnym rogu bloku funkcji).

    ![Zrzut ekranu przedstawiający blok funkcji B2C](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    W [klasycznym portalu Azure](https://manage.windowsazure.com/) możesz zarządzać użytkownikami i grupami, konfiguracją samoobsługowego resetowania hasła i funkcjami znakowania firmy powiązanymi z dzierżawą.

## Następne kroki

Dowiedz się, jak zarejestrować aplikację za pomocą usługi Azure AD B2C i utworzyć aplikację Szybki start, zapoznając się z tematem [Azure Active Directory B2C (wersja zapoznawcza): rejestrowanie aplikacji](active-directory-b2c-app-registration.md).



<!--HONumber=Jun16_HO2-->


