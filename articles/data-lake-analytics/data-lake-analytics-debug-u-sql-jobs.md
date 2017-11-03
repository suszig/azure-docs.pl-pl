---
title: "Debugowanie zadań U-SQL | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można debugować wierzchołek nie powiodło się U-SQL przy użyciu programu Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Debugowanie zdefiniowane przez użytkownika kodu C#, dla zadań U-SQL nie powiodło się

U-SQL zapewnia modelu rozszerzalności przy użyciu języka C#, więc można pisać swój kod, aby dodać funkcje, takie jak niestandardowe ekstraktory lub reduktor. Aby dowiedzieć się więcej, zobacz [Podręcznik programowania U-SQL](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). W praktyce dowolny kod może być konieczne debugowania i systemy danych big data mogą dostarczać tylko ograniczone środowiska uruchomieniowego debugowania informacje, takie jak pliki dziennika.

Azure Data Lake Tools dla programu Visual Studio udostępnia funkcję **nie można debugować wierzchołków**, co umożliwia sklonować zadanie zakończone niepowodzeniem z chmury na komputerze lokalnym do debugowania. Lokalne klonu przechwytuje środowiska chmury całego, w tym wszystkie dane wejściowe i kod użytkownika.

Poniżej film wideo przedstawia nie powiodło się wierzchołków debugowania w Azure Data Lake Tools dla programu Visual Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> Program Visual Studio wymaga następujących dwóch aktualizacji, jeśli nie są już zainstalowane: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) i [Universal C środowiska uruchomieniowego systemu Windows](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-to-local-machine"></a>Pobieranie nie powiodło się wierzchołków do komputera lokalnego

Po otwarciu zadanie zakończone niepowodzeniem w Azure Data Lake Tools dla programu Visual Studio jest wyświetlany żółty pasek alertów o szczegółowe komunikaty o błędach na karcie błąd.

1. Kliknij przycisk **Pobierz** do pobrania wszystkich wymaganych zasobów i strumienie wejściowe. Pobieranie nie zostało ukończone, kliknij przycisk **ponów**.

2. Kliknij przycisk **Otwórz** po zakończeniu pobierania do generowania środowisku debugowania lokalnego. Nowe wystąpienie programu Visual Studio z rozwiązaniem do debugowania jest automatycznie utworzony i otwarty.

![Azure Data Lake Analytics U-SQL debugowania programu visual studio pobierania wierzchołków](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

Zadania mogą obejmować źródła plików z kodem lub zarejestrowanych zestawy, a te dwa typy mają różne scenariusze debugowania.

- [Zadanie zakończone niepowodzeniem z kodem debugowania](#debug-job-failed-with-code-behind)
- [Debugowanie zakończonego niepowodzeniem zadania przy użyciu zestawów](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Debugowanie nie powiodło się z kodem — zadanie

Jeśli zadanie U-SQL zakończy się niepowodzeniem, a zadanie zawiera kod użytkownika (zwykle o nazwie `Script.usql.cs` projektu U-SQL), aby kod źródłowy jest importowany do debugowania rozwiązania.  Z tego miejsca można użyć programu Visual Studio debugowanie narzędzia (czujki, zmienne, itp.) do rozwiązania problemu.

> [!NOTE]
> Przed debugowania, należy sprawdzić **wspólnego języka środowiska uruchomieniowego wyjątki** w oknie Ustawienia wyjątków (**Ctrl + Alt + E**).

![Ustawienia programu visual studio debugowania w usłudze Azure Data Lake Analytics U-SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Naciśnij klawisz **F5** do uruchomienia kodu związane z kodem. Zostanie on uruchomiony, dopóki nie zostanie zatrzymana przez wyjątek.

2. Otwórz `ADLTool_Codebehind.usql.cs` pliku i ustaw punkty przerwania, naciśnij klawisz **F5** do debugowania kodu krok po kroku.

    ![Wyjątek debugowania w usłudze Azure Data Lake Analytics U-SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Zadanie nie powiodło się z zestawami debugowania

Jeśli używasz zestawy zarejestrowanych za pomocą skryptu U-SQL, system nie można pobrać kodu źródłowego automatycznie. W takim przypadku ręcznie dodać pliki kodu źródłowego te zestawy do rozwiązania.

### <a name="configure-the-solution"></a>Konfigurowanie rozwiązania

1. Kliknij prawym przyciskiem myszy **rozwiązania "VertexDebug" > Dodaj > istniejący projekt...**  Aby znaleźć te zestawy kodu źródłowego i dodać projektu do rozwiązania debugowania.

    ![Dodawanie usługi Azure Data Lake Analytics U-SQL debugowania projektu](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Kliknij prawym przyciskiem myszy **LocalVertexHost > właściwości** w rozwiązaniu i skopiuj **katalog roboczy** ścieżki.

3. Kliknij prawym przyciskiem myszy **projektu kodu źródłowego zestawu > właściwości**, wybierz pozycję **kompilacji** po lewej stronie, a następnie wklej skopiowany ścieżkę jako **wyjściowy > Ścieżka wyjściowa**.

    ![Ścieżka pliku pdb zestawu Azure Data Lake Analytics U-SQL debugowania](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Naciśnij klawisz **Ctrl + Alt + E**, sprawdź **wspólnego języka środowiska uruchomieniowego wyjątki** w oknie Ustawienia wyjątków.

### <a name="start-debug"></a>Uruchom debugowania

1. Kliknij prawym przyciskiem myszy **projektu kodu źródłowego zestawu > odbudować** pliki .pdb dane wyjściowe do `LocalVertexHost` katalog roboczy.

2. Naciśnij klawisz **F5** i projekt zostanie uruchomiony, dopóki nie zostanie zatrzymana przez wyjątek. Może zostać wyświetlony komunikat następujące ostrzeżenie, które można bezpiecznie zignorować. Może potrwać do minuty aby przejść do ekranu debugowania.

    ![Ostrzeżenie programu visual studio debugowania w usłudze Azure Data Lake Analytics U-SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Otwórz kod źródłowy i ustaw punkty przerwania, naciśnij klawisz **F5** do debugowania kodu krok po kroku.

Umożliwia także Visual Studio debugowanie narzędzia (czujki, zmienne, itp.) do rozwiązania problemu.

> [!NOTE]
> Odbuduj projekt kodu źródłowego zestawu każdorazowo po modyfikacji kod służący do generowania plików PDB zaktualizowane.

Po debugowaniu, jeśli projekt zakończy się pomyślnie w oknie danych wyjściowych zawiera następujący komunikat:

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL debugowania powiodło się](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a>Prześlij ponownie zadania

Po zakończeniu debugowania Prześlij zadanie zakończone niepowodzeniem.

1. W przypadku zadań z rozwiązaniami związane z kodem, skopiuj kod C# do pliku źródła kodu powiązanego (zazwyczaj `Script.usql.cs`).
2. W przypadku zadań z zestawami zarejestrować zestawy .dll zaktualizowane w bazie danych ADLA:
    1. W Eksploratorze serwera lub Eksploratorze chmury, rozwiń węzeł **konta ADLA > baz danych** węzła.
    2. Kliknij prawym przyciskiem myszy **zestawy** i Zarejestruj ponownie nowe zestawy dll z bazą danych ADLA: ![debugowania Azure Data Lake Analytics U-SQL Rejestrowanie zestawów](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. Prześlij swoją pracę.

## <a name="next-steps"></a>Następne kroki

- [Podręcznik programowania U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
- [Opracowywanie operatorów zdefiniowanych przez użytkownika U-SQL do zadania usługi Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Samouczek: tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
