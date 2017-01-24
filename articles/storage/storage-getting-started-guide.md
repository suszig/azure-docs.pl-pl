---
title: "Szybkie wprowadzenie do usługi Azure Storage | Microsoft Docs"
description: "Szybko zapoznaj się z usługami Microsoft Azure Blobs, Table i Queues, korzystając z szablonów Szybki start usługi Azure Storage, programu Visual Studio i emulatora usługi Azure Storage. Uruchom pierwszą aplikację usługi Azure Storage w ciągu pięciu minut."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 582f76f8-c814-4a69-8a5c-1fd0e0d5d8f2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 12/08/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 12ce6b6bccf3ea2aa2945ddd775716f29cf01e1f
ms.openlocfilehash: 47b2623eb3b83220ef8e3cfafde06dab3ac3d22e


---
# <a name="get-started-with-azure-storage-in-five-minutes"></a>Szybkie wprowadzenie do usługi Azure Storage
## <a name="overview"></a>Omówienie
Rozpoczęcie tworzenia rozwiązań z wykorzystaniem usługi Azure Storage jest niezwykle proste. Ten samouczek pokazuje, jak szybko uruchomić aplikację usługi Azure Storage. Użyjesz szablonów Szybki start dołączonych do zestawu Azure SDK dla programu .NET. Szablony szybkiego startu zawierają gotowy do uruchomienia kod, który demonstruje wybrane, podstawowe scenariusze programistyczne związane z usługą Azure Storage.

Aby uzyskać więcej informacji na temat usługi Azure Storage przed rozpoczęciem pracy z kodem, zobacz [Następne kroki](#next-steps).

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem należy spełnić następujące wymagania wstępne:

1. Aby skompilować i utworzyć aplikację, musisz mieć wersję programu [Visual Studio](https://www.visualstudio.com/) zainstalowaną na komputerze.
2. Zainstaluj najnowszą wersję zestawu [Azure SDK dla programu .NET](https://azure.microsoft.com/downloads/). Zestaw SDK zawiera przykładowe projekty Szybki start Azure, emulator usługi Azure Storage oraz [Bibliotekę klienta usługi Azure Storage dla programu .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).
3. Upewnij się, że masz program [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) zainstalowany na komputerze, ponieważ jest wymagany w przykładowych projektach Szybki start Azure, których będziemy używać w tym samouczku.

    Jeśli nie masz pewności, jaka wersja programu .NET Framework jest zainstalowana na komputerze, zobacz [Poradnik: określanie zainstalowanych wersji programu .NET Framework](https://msdn.microsoft.com/vstudio/hh925568.aspx). Możesz również nacisnąć przycisk **Start** lub klawisz systemu Windows i wpisać **Panel sterowania**. Następnie kliknij pozycje **Programy** > **Programy i funkcje** i sprawdź, czy program .NET Framework 4.5 jest na liście zainstalowanych programów.
4. Będziesz potrzebować subskrypcji platformy Azure oraz konta usługi Azure Storage.

   * Aby uzyskać subskrypcję platformy Azure, zobacz tematy [Bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/), [Opcje zakupu](https://azure.microsoft.com/pricing/purchase-options/) i [Oferty członkowskie](https://azure.microsoft.com/pricing/member-offers/) (dla członków MSDN, Microsoft Partner Network, BizSpark i innych programów firmy Microsoft).
   * Aby utworzyć konto usługi Azure Storage, zobacz [Tworzenie konta magazynu](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Uruchamianie pierwszej aplikacji usługi Azure Storage w usłudze Azure Storage w chmurze
Jeśli masz już konto, możesz utworzyć prostą aplikację usługi Azure Storage, korzystając z jednego z przykładowych projektów Szybki start Azure w programie Visual Studio. Ten samouczek koncentruje się na przykładowych projektach usługi Azure Storage: **Azure Storage: obiekty Blob**, **Azure Storage: pliki**, **Azure Storage: kolejki** i **Azure Storage: tabele**:

1. Uruchom program Visual Studio.
2. W menu **Plik** kliknij pozycję **Nowy projekt**.
3. W oknie dialogowym **Nowy projekt** kliknij kolejno pozycje **Zainstalowane** > **Szablony** > **Visual C#** > **Chmura** > **Poradniki Szybki start** > **Data Services**.
    a. Wybierz jeden z następujących szablonów: **Azure Storage: obiekty Blob**, **Azure Storage: pliki**, **Azure Storage: kolejki** lub **Azure Storage: tabele**.
    b. Upewnij się, że program **.NET Framework 4.5** jest wybrany jako platforma docelowa.
    c. Określ nazwę projektu i utwórz nowe rozwiązanie Visual Studio, jak pokazano poniżej:

    ![Szybki start Azure][Image1]

Warto przejrzeć kod źródłowy przed uruchomieniem aplikacji. Aby przejrzeć kod, wybierz pozycję **Eksplorator rozwiązań** w menu **Widok** w programie Visual Studio. Następnie kliknij dwukrotnie plik Program.cs.

Uruchom przykładową aplikację:

1. W programie Visual Studio wybierz pozycję **Eksplorator rozwiązań** w menu **Widok**. Otwórz plik App.config i oznacz jako komentarz parametry połączenia dla emulatora usługi Azure Storage:

   `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2. Usuń znaczniki komentarza z parametrów połączenia dla usługi Azure Storage i podaj nazwę oraz klucz dostępu konta magazynu w pliku App.config:

   `<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

   Aby pobrać klucz dostępu do konta magazynu, zobacz [Zarządzanie kluczami dostępu do magazynu](storage-create-storage-account.md#manage-your-storage-access-keys).
3. Po podaniu nazwy i klucza dostępu konta magazynu w pliku App.config, przejdź do menu **Plik** i kliknij pozycję **Zapisz wszystko**, aby zapisać wszystkie pliki projektu.
4. W menu **Kompilacja** kliknij pozycję **Kompiluj rozwiązanie**.
5. W menu **Debugowanie** naciśnij klawisz **F11**, aby uruchomić rozwiązanie krok po kroku lub naciśnij klawisz **F5**, aby uruchomić rozwiązanie.

## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Uruchamianie pierwszej aplikacji usługi Azure Storage lokalnie z wykorzystaniem emulatora usługi Azure Storage
[Emulator usługi Azure Storage](storage-use-emulator.md) zapewnia środowisko lokalne, które emuluje obiekty Blob platformy Azure, kolejki i usługi tabel w celach deweloperskich. Możesz użyć emulatora magazynu, aby przetestować aplikację magazynu lokalnie, bez tworzenia subskrypcji platformy Azure lub konta magazynu i bez ponoszenia jakichkolwiek kosztów.

Aby wypróbować ten scenariusz, utwórz prostą aplikację usługi Azure Storage, korzystając z jednego z przykładowych projektów Szybki start Azure w programie Visual Studio. Ten samouczek koncentruje się na przykładowych projektach **Azure Blob Storage**, **Azure Table Storage** oraz **Azure Queue Storage**:

1. Uruchom program Visual Studio.
2. W menu **Plik** kliknij pozycję **Nowy projekt**.
3. W oknie dialogowym **Nowy projekt** kliknij kolejno pozycje **Zainstalowane** > **Szablony** > **Visual C#** > **Chmura** > **Poradniki Szybki start** > **Data Services**.
    a. Wybierz jeden z następujących szablonów: **Azure Storage: obiekty Blob**, **Azure Storage: pliki**, **Azure Storage: kolejki** lub **Azure Storage: tabele**.
    b. Upewnij się, że program **.NET Framework 4.5** jest wybrany jako platforma docelowa.
    c. Określ nazwę projektu i utwórz nowe rozwiązanie Visual Studio, jak pokazano poniżej:

    ![Szybki start Azure][Image1]

4. W programie Visual Studio wybierz pozycję **Eksplorator rozwiązań** w menu **Widok**. Otwórz plik App.config i oznacz jako komentarz parametry połączenia dla swojego konta usługi Azure Storage, jeśli konto zostało już dodane. Następnie usuń znaczniki komentarza z parametrów połączenia dla emulatora usługi Azure Storage:

   `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Warto przejrzeć kod źródłowy przed uruchomieniem aplikacji. Aby przejrzeć kod, wybierz pozycję **Eksplorator rozwiązań** w menu **Widok** w programie Visual Studio. Następnie kliknij dwukrotnie plik Program.cs.

Uruchom przykładową aplikację w emulatorze usługi Azure Storage:

1. Naciśnij przycisk **Start** lub klawisz systemu Windows, wyszukaj *Emulator usługi Microsoft Azure Storage* i uruchom aplikację. Po uruchomieniu emulatora w obszarze widoku zadań systemu Windows pojawi się ikona i powiadomienie.
2. W programie Visual Studio kliknij przycisk **Kompiluj rozwiązanie** w menu **Kompilacja**.
3. W menu **Debugowanie** naciśnij klawisz **F11**, aby uruchomić rozwiązanie krok po kroku, lub naciśnij klawisz **F5**, aby uruchomić rozwiązanie od początku do końca.

## <a name="next-steps"></a>Następne kroki
Zobacz następujące zasoby, aby dowiedzieć się więcej na temat usługi Azure Storage:

* [Wprowadzenie do usługi Microsoft Azure Storage](storage-introduction.md)
* [Rozpoczynanie pracy z programem Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Rozpoczynanie pracy z usługą Azure Blob Storage przy użyciu platformy .NET](storage-dotnet-how-to-use-blobs.md)
* [Rozpoczynanie pracy z usługą Azure Table Storage przy użyciu platformy .NET](storage-dotnet-how-to-use-tables.md)
* [Rozpoczynanie pracy z usługą Azure Queue Storage przy użyciu platformy .NET](storage-dotnet-how-to-use-queues.md)
* [Rozpoczynanie pracy z usługą Azure File Storage w systemie Windows](storage-dotnet-how-to-use-files.md)
* [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
* [Dokumentacja usługi Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Biblioteka klienta usługi Microsoft Azure Storage dla platformy .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Interfejs API REST usług Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png



<!--HONumber=Dec16_HO2-->


