---
title: "Usługi Azure AD v2 Windows Desktop pobieranie rozpoczęte — Konfiguracja | Dokumentacja firmy Microsoft"
description: "Jak aplikacje .NET pulpitu systemu Windows (XAML) można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4065727aef04d7969d438c6ef79127bb44568be1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
## <a name="set-up-your-project"></a>Konfigurowanie projektu

Ta sekcja zawiera instrukcje krok po kroku dotyczące sposobu tworzenia nowego projektu aby zademonstrować sposób integracji aplikacji .NET pulpitu systemu Windows (XAML) z *logowania z firmą Microsoft* aby mogła zbadać interfejsów API sieci Web, która wymaga tokenu.

Aplikacji utworzonych przez ten przewodnik przedstawia przycisk Wykres i wyświetlić wyniki na ekranie i przycisk wylogowania.

> Preferowane jest zamiast tego Pobierz ten przykładowy projekt programu Visual Studio? [Pobieranie projektu](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) i przejść [kroku konfiguracji](#create-an-application-express) skonfigurować przykładowy kod przed wykonaniem.


### <a name="create-your-application"></a>Tworzenie aplikacji
1. W programie Visual Studio:`File` > `New` > `Project`<br/>
2. W obszarze *szablony*, wybierz pozycję`Visual C#`
3. Wybierz `WPF App` (lub *aplikacji WPF* w zależności od wersji programu Visual Studio)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Dodaj do projektu biblioteki uwierzytelniania firmy Microsoft (MSAL)
1. W programie Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Skopiuj/Wklej następujące opcje w oknie Konsola Menedżera pakietów:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> Powyżej pakiet instaluje biblioteki uwierzytelniania firmy Microsoft (MSAL). MSAL obsługuje pobieranie, buforowanie i odświeżanie toskens użytkownika, które umożliwiają dostęp do interfejsów API chronione przez usługi Azure Active Directory w wersji 2.

## <a name="add-the-code-to-initialize-msal"></a>Dodaj kod, aby zainicjować MSAL
Ten krok ułatwia tworzenie klasy do obsługi interakcji z biblioteką MSAL, takie jak Obsługa tokenów.

1. Otwórz `App.xaml.cs` plik i dodać odwołanie do biblioteki MSAL do klasy:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Aktualizacja klasy aplikacji do następującego:
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a>Tworzenie aplikacji interfejsu użytkownika
Poniższa sekcja przedstawia, jak aplikacja może zapytać serwera chronionego wewnętrznej bazy danych, takich jak Microsoft Graph. Plik MainWindow.xaml należy utworzyć automatycznie w ramach szablonu projektu. Otwórz ten plik ten plik, a następnie postępuj zgodnie z instrukcjami poniżej:

Zastąp aplikacji `<Grid>` być następujące:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
