<properties
    pageTitle="Azure Active Directory B2C (wersja zapoznawcza): rejestracja aplikacji| Microsoft Azure"
    description="Jak zarejestrować aplikację w usłudze Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="swkrish"/>


# Azure Active Directory B2C (wersja zapoznawcza): rejestrowanie aplikacji

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Wymagania wstępne

Aby utworzyć aplikację, która akceptuje tworzenie kont i logowanie użytkowników, musisz najpierw zarejestrować aplikację w dzierżawie usługi Azure Active Directory B2C. Aby utworzyć własną dzierżawę, wykonaj kroki opisane w temacie [Tworzenie dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md). Wykonanie wszystkich czynności przedstawionych w tym artykule spowoduje przypięcie bloku funkcji B2C do tablicy startowej.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Przechodzenie do bloku funkcji B2C

Do bloku funkcji B2C można przejść z portalu Azure lub klasycznego portalu Azure.

### 1. Dostęp z poziomu portalu Azure

Blok funkcji B2C przypięty do tablicy startowej będzie widoczny od razu po zalogowaniu do [portalu Azure](https://portal.azure.com/) w roli administratora globalnego dzierżawy usługi B2C.

Do bloku można również przejść, klikając pozycję **Przeglądaj**, a następnie pozycję **Azure AD B2C** w lewym okienku nawigacji w [portalu Azure](https://portal.azure.com/).

Aby przejść bezpośrednio do bloku, należy użyć linku [https://portal.azure.com/ {dzierżawa}.onmicrosoft.com/?#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/](https://portal.azure.com/{tenant}.onmicrosoft.com/?#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/), gdzie element **{dzierżawa}** jest zastępowany nazwą zastosowaną podczas tworzenia dzierżawy (na przykład contosob2c). Ten link można dodać do zakładek do użytku w przyszłości.

    > [AZURE.IMPORTANT]
    Tylko administrator globalny dzierżawy B2C może uzyskiwać dostęp do bloku funkcji B2C. Administrator globalny innej dzierżawy ani użytkownik dowolnej dzierżawy nie mogą uzyskać dostępu do tego bloku.

### 2. Dostęp z poziomu klasycznego portalu Azure

Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator subskrypcji. (Jest to konto służbowe lub szkolne bądź konto Microsoft zastosowane podczas tworzenia konta platformy Azure). Przejdź do rozszerzenia Active Directory po lewej stronie i kliknij dzierżawę B2C. Na karcie **Szybki start** (pierwsza otwarta karta) kliknij pozycję **Zarządzaj ustawieniami B2C** w obszarze **Administrowanie**. Spowoduje to otwarcie bloku funkcji B2C w nowym oknie lub na nowej karcie przeglądarki.

Link **Zarządzaj ustawieniami B2C** można również znaleźć w sekcji **Administracja usługą B2C** na karcie **Konfigurowanie**.

## Rejestrowanie aplikacji

1. W bloku funkcji B2C w portalu Azure kliknij pozycję **Aplikacje**.
2. Kliknij pozycję **+Dodaj** w górnej części bloku.
3. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Aplikacja Contoso B2C”.
4. Jeśli piszesz aplikację opartą na sieci Web, przestaw przełącznik **Uwzględnij aplikację sieci Web/interfejs API sieci Web** na wartość **Tak**. **Adresy URL odpowiedzi** to punkty końcowe, w których usługa Azure AD B2C będzie zwracać wszystkie tokeny żądań aplikacji. Na przykład wprowadź wartość `https://localhost:44321/`. Jeśli aplikacja zawiera składnik po stronie serwera (interfejs API), który musi zostać zabezpieczony, utwórz (i skopiuj) również **klucz tajny aplikacji**, klikając przycisk **Wygeneruj klucz**.

    > [AZURE.NOTE]
    **Klucz tajny aplikacji** to ważne poświadczenie zabezpieczeń.

5. Jeśli piszesz aplikację mobilną, przestaw przełącznik **Uwzględnij klienta natywnego** na wartość **Tak**. Skopiuj domyślny **identyfikator URI przekierowania**, który został utworzony automatycznie.
6. Kliknij pozycję **Utwórz**, aby zarejestrować aplikację.
7. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu.

## Tworzenie aplikacji Szybki start

Po zarejestrowaniu aplikacji w usłudze Azure AD B2C możesz wykonać czynności opisane w jednym z naszych samouczków szybkiego startu, aby rozpocząć pracę. Poniżej przedstawiono kilka zaleceń:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]



<!--HONumber=Jun16_HO2-->


