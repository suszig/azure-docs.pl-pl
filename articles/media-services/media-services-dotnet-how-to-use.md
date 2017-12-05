---
title: "Jak skonfigurować komputer Media Services programowanie z platformą .NET"
description: "Więcej informacji na temat wymagania wstępne dotyczące usługi Media Services przy użyciu zestawu SDK usługi Media Services dla platformy .NET. Również informacje o sposobie tworzenia aplikacji programu Visual Studio."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: juliako
ms.openlocfilehash: f5dd263a2e925989069c3b0257cfafa4c43e6157
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="media-services-development-with-net"></a>Tworzenia usługi Media Services z platformą .NET
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

W tym temacie omówiono sposób rozpocząć tworzenie aplikacji usługi Media Services przy użyciu platformy .NET.

**Zestawu .NET SDK usługi Azure Media Services** biblioteki umożliwia program względem usługi Media Services przy użyciu platformy .NET. Aby ułatwić nawet tworzyć aplikacje za pomocą platformy .NET, **rozszerzenia SDK .NET usługi Azure Media Services** podano biblioteki. Ta biblioteka zawiera zestaw metod rozszerzeń i funkcje pomocnicze, które upraszczają kodu .NET. Zarówno biblioteki są dostępne za pośrednictwem **NuGet** i **GitHub**.

## <a name="prerequisites"></a>Wymagania wstępne
* Konto usługi Media Services w nowej lub istniejącej subskrypcji platformy Azure. Zobacz temat [Tworzenie konta usługi Media Services](media-services-portal-create-account.md).
* Systemy operacyjne: Windows 10, Windows 7, Windows 2008 R2 lub Windows 8.
* Program .NET framework 4.5.
* Program Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Tworzenie i konfigurowanie projektu programu Visual Studio
W tej sekcji przedstawiono sposób tworzenia projektu w programie Visual Studio i skonfigurować go do tworzenia usługi Media Services.  W tym przypadku projekt jest aplikacją konsoli Windows C#, ale te same kroki instalacji podane tutaj dotyczą innych typów projektów, które można utworzyć dla aplikacji usługi Media Services (na przykład aplikacji formularzy systemu Windows lub aplikacji sieci Web programu ASP.NET).

W tej sekcji przedstawiono sposób użycia **NuGet** można dodać rozszerzenia .NET SDK usługi Media Services i innych zależnych bibliotek.

Alternatywnie można uzyskać najnowsze bitów .NET SDK usługi Media Services z usługi GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) lub [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), Skompiluj rozwiązanie i dodać odwołań do projektu klienta. Wszystkie wymagane zależności uzyskać pobrane i wyodrębnione automatycznie.

1. Utwórz nową aplikację konsoli języka C# w programie Visual Studio. Wprowadź **nazwa**, **lokalizacji**, i **Nazwa rozwiązania**, a następnie kliknij przycisk OK.
2. Skompiluj rozwiązanie.
3. Użyj **NuGet** zainstalować i dodać **rozszerzenia SDK .NET usługi Azure Media Services** (**windowsazure.mediaservices.extensions**). Podczas instalacji tego pakietu instalowany jest również zestaw **.NET SDK usługi Media Services** oraz dodawane są wszystkie inne wymagane zależności.
   
    Upewnij się, że masz najnowszą wersję programu NuGet. Aby uzyskać dodatkowe instrukcje instalacji i informacje, zobacz [NuGet](http://nuget.codeplex.com/).

    1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu i wybierz polecenie **Zarządzaj pakietami NuGet**.

    2. Zostanie wyświetlone okno dialogowe Zarządzanie pakietami NuGet.

    3. W galerii Online, poszukaj rozszerzeń MediaServices Azure, wybierz **rozszerzenia SDK .NET usługi Azure Media Services** (**windowsazure.mediaservices.extensions**), a następnie kliknij przycisk  **Zainstaluj** przycisku.
   
    4. Projekt zostanie zmodyfikowana, a odwołania do rozszerzenia SDK .NET usługi Media Services SDK .NET usługi Media Services i innych zestawów zależnych zostaną dodane.
4. Aby promować czyszczący Środowisko deweloperskie, należy rozważyć włączenie, Przywracanie pakietu NuGet. Aby uzyskać więcej informacji, zobacz [Przywracanie pakietu NuGet "](http://docs.nuget.org/consume/package-restore).
5. Dodaj odwołanie do **System.Configuration** zestawu. Ten zestaw zawiera System.Configuration. **ConfigurationManager** klasy, która umożliwia dostęp do plików konfiguracji (na przykład App.config).
   
    1. Aby dodać odwołań za pomocą okna dialogowego Zarządzanie odwołaniami, kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań. Następnie kliknij przycisk **Dodaj**, następnie kliknij przycisk **odwołania...** .
   
    2. Zostanie wyświetlone okno dialogowe Zarządzanie odwołań.
    3. W obszarze zestawów struktury .NET, Znajdź i wybierz zestawu System.Configuration i naciśnij klawisz **OK**.
6. Otwórz plik App.config i Dodaj **appSettings** sekcji w pliku.     
   
    Ustaw wartości, które są wymagane do nawiązania połączenia interfejsu API usług Media Services. Aby uzyskać więcej informacji, zobacz [dostępu Azure Media Services API przy użyciu uwierzytelniania usługi Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

    Jeśli używasz [uwierzytelnianie użytkownika](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) pliku konfiguracji, prawdopodobnie będzie mieć wartości domeny dzierżawy usługi Azure AD i punkt końcowy interfejsu API REST usługi AMS.
    
    >[!Note]
    >Większość przykładów kodu w dokumentacji usługi Azure Media Services zestawu, użyj typu (interactive) użytkownika uwierzytelniania do nawiązania połączenia interfejsu API usług AMS. Ta metoda uwierzytelniania będzie działać do zarządzania i monitorowania natywnych aplikacji: aplikacje mobilne, aplikacje systemu Windows i aplikacji konsoli.
    
    >[!Important]
    > **Interakcyjne** metoda uwierzytelniania nie jest odpowiedni dla serwera, usług sieci web, interfejsów API typu aplikacji. Dla tych typów aplikacji, użyj **nazwy głównej usługi** metodę uwierzytelniania. Aby uzyskać więcej informacji, zobacz [dostęp do interfejsu API usług AMS przy użyciu uwierzytelniania usługi Azure AD](media-services-use-aad-auth-to-access-ams-api.md).

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

7. Zastąp istniejące instrukcje **using** na początku pliku Program.cs następującym kodem.
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

W tym momencie można przystąpić do rozpocząć tworzenie aplikacji usługi Media Services.    

## <a name="example"></a>Przykład

Oto przykład małych, który jest podłączany do interfejsu API usług AMS i wyświetla wszystkie dostępne procesory multimediów.
    
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    
            // List all available Media Processors
            foreach (var mp in _context.MediaProcessors)
            {
                Console.WriteLine(mp.Name);
            }
    
        }

##<a name="next-steps"></a>Następne kroki

Teraz [możesz połączyć się z interfejsu API usług AMS](media-services-use-aad-auth-to-access-ams-api.md) i uruchomić [tworzenie](media-services-dotnet-get-started.md).


## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

