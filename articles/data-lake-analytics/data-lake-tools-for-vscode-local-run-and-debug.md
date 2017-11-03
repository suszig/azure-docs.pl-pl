---
title: 'Azure Data Lake Tools: Uruchamiania lokalnego skryptu U-SQL i debugowania lokalnego z kodem Visual Studio | Dokumentacja firmy Microsoft'
description: "Dowiedz się, jak używać usługi Azure Data Lake Tools dla Visual Studio Code przebiegu lokalnego do debugowania lokalnego."
Keywords: "VScode, usługi Azure Data Lake Tools, uruchamiania lokalnego debugowania, debugowania lokalnego pliku magazynu w wersji zapoznawczej, lokalnego przekazać do ścieżki do magazynu"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 78a5efb19f73192dcc95103abc70c14a3ce2e29a
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="u-sql-local-run-and-local-debug-for-windows-with-visual-studio-code"></a>Uruchamiania lokalnego skryptu U-SQL i lokalnego debugowania dla systemu Windows z kodem Visual Studio
W tym dokumencie Dowiedz się jak uruchamiać zadań U-SQL na maszynie lokalnej, aby przyspieszyć we wczesnej fazie kodowania lub Aby debugować kod lokalnie w programie Visual Studio Code. Aby uzyskać instrukcje dotyczące usługi Azure Data Lake narzędzia dla programu Visual Studio Code, zobacz [narzędzi użycia usługi Azure Data Lake Tools dla Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md). 


## <a name="set-up-the-u-sql-local-run-environment"></a>Konfigurowanie środowiska lokalnego uruchamiania skryptu U-SQL

1. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć palety polecenia, a następnie wprowadź **ADL: pobieranie lokalne Uruchom zależności** pobierania pakietów.  

   ![Pobieranie pakietów ADL LocalRun zależności](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Zlokalizuj pakiety zależności ze ścieżki pokazano **dane wyjściowe** okienku, a następnie zainstaluj BuildTools i Win10SDK 10240. Oto przykład ścieżki:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![Zlokalizuj pakiety zależności](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 do zainstalowania **BuildTools**, kliknij przycisk visualcppbuildtools_full.exe w folderze LocalRunDependency, a następnie postępuj zgodnie z instrukcjami kreatora.   

    ![Zainstaluj BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 do zainstalowania **Win10SDK 10240**, kliknij przycisk sdksetup.exe w folderze LocalRunDependency/Win10SDK_10.0.10240_2, a następnie postępuj zgodnie z instrukcjami kreatora.  

    ![Zainstaluj Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Ustawienie zmiennej środowiskowej. Ustaw **SCOPE_CPP_SDK** zmiennej środowiskowej:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. Uruchom ponownie system operacyjny, aby upewnić się, że ustawień zmiennych środowiskowych zostaną zastosowane.  

   ![Upewnij się, że zmienna środowiskowa SCOPE_CPP_SDK jest zainstalowany](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Uruchom usługę uruchamiania lokalnego i przesłać zadanie U-SQL do lokalnego konta 
Użytkownik po raz pierwszy, można użyć **ADL: pobieranie lokalne Uruchom zależności** pobierania lokalnego uruchamiania pakietów, jeśli nie masz [Konfigurowanie lokalnego środowiska uruchamiania skryptu U-SQL](#set-up-the-u-sql-local-run-environment).

1. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć palety polecenia, a następnie wprowadź **ADL: Uruchom usługę lokalnego uruchamiania**.   
2. Wybierz **Akceptuj** aby zaakceptować postanowienia licencyjne dotyczące oprogramowania firmy Microsoft po raz pierwszy. 

   ![Zaakceptuj postanowienia licencyjne dotyczące oprogramowania firmy Microsoft](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Zostanie otwarta konsola cmd. Dla użytkowników po raz pierwszy, musisz wprowadzić **3**, a następnie zlokalizuj ścieżkę do folderu lokalnego dla danych wejściowych i wyjściowych. Inne opcje można użyć wartości domyślnych. 

   ![Narzędzia Data Lake Tools dla Visual Studio Code lokalnego uruchamiania cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia, wpisz **ADL: Prześlij zadanie**, a następnie wybierz **lokalnego** można przesłać zadania do konta lokalnego.

   ![Narzędzia Data Lake Tools dla Visual Studio Code wybierz lokalnego](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Po przesłaniu zadania można wyświetlić szczegóły przesyłania. Zaznacz, aby wyświetlić szczegóły przesyłania **jobUrl** w **dane wyjściowe** okna. Można również wyświetlić stan zadania przesyłania z poziomu konsoli cmd. Wprowadź **7** w konsoli cmd, jeśli chcesz dowiedzieć się więcej szczegółów zadania.

   ![Data Lake Tools dla Visual Studio Code lokalnego uruchamiania wyjścia](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![narzędzi Data Lake Tools dla Visual Studio Code lokalnych cmd stan uruchomienia](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Uruchom lokalny debugowania dla zadania skryptu U-SQL  
Dla pierwszego użytkownika:

1. Użyj **ADL: pobieranie lokalne Uruchom zależności** pobierania lokalnego uruchamiania pakietów, jeśli nie masz [Konfigurowanie lokalnego środowiska uruchamiania skryptu U-SQL](#set-up-the-u-sql-local-run-environment).
2. Zainstaluj .NET Core SDK 2.0 zgodnie z sugestią podaną w oknie komunikatu, jeśli nie jest zainstalowany.
 
  ![Przypomnienie instaluje Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Instalacji C# dla programu Visual Studio Code jako widoczną w oknie komunikatu Jeśli nie jest zainstalowany. Kliknij przycisk **zainstalować** aby kontynuować, a następnie ponownie uruchom VSCode.

    ![Monitu, aby zainstalować C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Wykonaj poniższe kroki, aby przeprowadzić debugowania lokalnego:
  
1. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć palety polecenia, a następnie wprowadź **ADL: Uruchom usługę lokalnego uruchamiania**. Zostanie otwarta konsola cmd. Upewnij się, że **DataRoot** jest ustawiona.
2. Ustaw punkt przerwania w Twojej C# związane z kodem.
3. Powrót do edytora skryptów, kliknij prawym przyciskiem myszy i wybierz **ADL: debugowania lokalnego**.
    
   ![Narzędzia Data Lake Tools dla Visual Studio Code wyniku lokalnego debugowania](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Następne kroki
- Używanie usługi Azure Data Lake Tools dla Visual Studio Code, zobacz [narzędzi użycia usługi Azure Data Lake Tools dla Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- Do uzyskania wprowadzenie informacji na temat usługi Data Lake Analytics, zobacz [samouczek: rozpoczynanie pracy z usługą Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Aby informacji na temat narzędzia Data Lake Tools dla programu Visual Studio, zobacz [samouczek: skryptów U-SQL opracowanie przy użyciu narzędzi Data Lake Tools dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby uzyskać informacje na temat tworzenia zespołów, zobacz [opracowanie U-SQL zestawy dla usługi Azure Data Lake Analytics zadań](data-lake-analytics-u-sql-develop-assemblies.md).
