<properties
    pageTitle="Azure Active Directory B2C: rejestrowanie aplikacji | Microsoft Azure"
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
    ms.date="08/30/2016"
    ms.author="swkrish"/>



# Azure Active Directory B2C: rejestrowanie aplikacji

## Wymagania wstępne

Aby utworzyć aplikację, która akceptuje tworzenie kont i logowanie użytkowników, musisz najpierw zarejestrować aplikację w dzierżawie usługi Azure Active Directory B2C. Aby utworzyć własną dzierżawę, wykonaj kroki opisane w temacie [Tworzenie dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md). Wykonanie wszystkich czynności przedstawionych w tym artykule spowoduje przypięcie bloku funkcji B2C do tablicy startowej.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Przechodzenie do bloku funkcji B2C

Blok funkcji B2C przypięty do tablicy startowej będzie widoczny od razu po zalogowaniu do witryny [Azure Portal](https://portal.azure.com/) w roli administratora globalnego dzierżawy usługi B2C.

Do bloku można również przejść, klikając pozycję **Przeglądaj**, a następnie pozycję **Azure AD B2C** w lewym okienku nawigacji w [witryny Azure Portal](https://portal.azure.com/).

> [AZURE.IMPORTANT] Tylko administrator globalny dzierżawy B2C może uzyskiwać dostęp do bloku funkcji B2C. Administrator globalny innej dzierżawy ani użytkownik dowolnej dzierżawy nie mogą uzyskać dostępu do tego bloku.  Na swoją dzierżawę B2C możesz przełączyć się, używając przełącznika dzierżawy w prawym górnym rogu witryny Azure Portal.

## Rejestrowanie aplikacji

1. W bloku funkcji B2C w witrynie Azure Portal kliknij pozycję **Aplikacje**.
2. Kliknij pozycję **+Dodaj** w górnej części bloku.
3. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Aplikacja Contoso B2C”.
4. Jeśli piszesz aplikację opartą na sieci Web, przestaw przełącznik **Uwzględnij aplikację sieci Web/interfejs API sieci Web** na wartość **Tak**. **Adresy URL odpowiedzi** to punkty końcowe, w których usługa Azure AD B2C będzie zwracać wszystkie tokeny żądań aplikacji. Na przykład wprowadź wartość `https://localhost:44321/`. Jeśli aplikacja sieci Web będzie również wywoływała jakiś interfejs API sieci Web zabezpieczony za pomocą usługi Azure AD B2C, może być także konieczne utworzenie **klucza tajnego aplikacji** przez kliknięcie przycisku **Generuj klucz**.

    > [AZURE.NOTE] **Klucz tajny aplikacji** jest ważnym poświadczeniem zabezpieczeń i powinien być odpowiednio zabezpieczony.

5. Jeśli piszesz aplikację mobilną, przestaw przełącznik **Uwzględnij klienta natywnego** na wartość **Tak**. Skopiuj domyślny **identyfikator URI przekierowania**, który został utworzony automatycznie.
6. Kliknij pozycję **Utwórz**, aby zarejestrować aplikację.
7. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu.

> [AZURE.IMPORTANT] Aplikacjami utworzonymi w bloku funkcji B2C należy zarządzać w tej samej lokalizacji. Jeśli edytujesz aplikacje B2C przy użyciu programu PowerShell lub innego portalu, stają się one nieobsługiwane i prawdopodobnie nie będą działać w usłudze Azure AD B2C.

## Tworzenie aplikacji Szybki start

Po zarejestrowaniu aplikacji w usłudze Azure AD B2C możesz wykonać czynności opisane w jednym z naszych samouczków szybkiego startu, aby rozpocząć pracę. Poniżej przedstawiono kilka zaleceń:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]



<!--HONumber=Sep16_HO3-->


