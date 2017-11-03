---
title: "Tworzenie zestawów języka U-SQL do zadania usługi Azure Data Lake Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć zestawy używanych i użyć ponownie w zadań usługi Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: c49f80f8dcd330d7f46726241e7178351b9cc28f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a>Tworzenie zestawów języka U-SQL do zadania usługi Azure Data Lake Analytics
Dowiedz się, jak włączyć kodu powiązanego do zestawów używanych i użyć ponownie w zadań usługi Data Lake Analytics. 

U-SQL ułatwia dodawanie niestandardowego kodu w językach .net, takich jak C#, VB.Net lub F #. Można nawet wdrażanie własnego środowiska uruchomieniowego do obsługi innych języków.

Najprostszym sposobem, aby użyć niestandardowego kodu jest do użycia narzędzi Data Lake Tools dla Visual Studio możliwości związane z kodem. Aby uzyskać więcej informacji, zobacz [samouczek: tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Istnieje kilka wady używania związane z kodem:

- Kod źródłowy pobiera przekazany do przesłania każdego skryptu.
- związane z kodem nie mogą być udostępniane przez inne zadania.

Aby rozwiązać te wady, można włączyć kodu powiązanego do zestawów i zarejestruj zestawy do katalogu Data Lake Analytics.

## <a name="prerequisites"></a>Wymagania wstępne
* Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 lub Visual Studio 2012 z zainstalowany program Visual C++
* Microsoft Azure SDK dla platformy .NET w wersji 2.5 lub nowszej.  Instalowanie przy użyciu Instalatora platformy sieci Web lub Instalator programu Visual Studio
* Konto usługi Data Lake Analytics.  Zobacz temat [Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-get-started-portal.md).
* Przejdź przez [wprowadzenie do usługi Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) samouczka.
* Połączenia z platformą Azure.
* Przekaż źródło danych, zobacz [wprowadzenie do usługi Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md). 

## <a name="develop-assemblies-for-u-sql"></a>Tworzenie zestawów dla U-SQL

**Aby utworzyć i przesłać zadanie U-SQL**

1. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
2. Rozwiń węzeł **zainstalowana**, **szablony**, **usługi Azure Data Lake**, **U-SQL(ADLA)**, wybierz pozycję **biblioteki klas (dla aplikacji U-SQL)** szablonu, a następnie kliknij przycisk **OK**.
3. Wpisz swój kod w Class1.cs.  Poniżej przedstawiono przykładowy kod.

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. Kliknij przycisk **kompilacji** menu, a następnie kliknij przycisk **Kompiluj rozwiązanie** do tworzenia biblioteki dll.

## <a name="register-assemblies"></a>Rejestrowanie zestawów

Zobacz [Analytics(U-SQL) Lake danych użyj katalogu](data-lake-analytics-use-u-sql-catalog.md).


## <a name="use-the-assemblies"></a>Korzystanie z zestawów

Zobacz [użycia usługi Azure Data Lake Tools dla programu Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

## <a name="see-also"></a>Zobacz też
* [Wprowadzenie do usługi Data Lake Analytics przy użyciu programu PowerShell](data-lake-analytics-get-started-powershell.md)
* [Wprowadzenie do usługi Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-get-started-portal.md)
* [Użyj narzędzi Data Lake Tools dla programu Visual Studio do tworzenia aplikacji U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
* [Użyj Data Lake Analytics(U-SQL) katalogu](data-lake-analytics-use-u-sql-catalog.md)
* [Korzystanie z narzędzi Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)