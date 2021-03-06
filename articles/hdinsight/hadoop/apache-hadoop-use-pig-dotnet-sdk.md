---
title: "Uruchamianie zadań Apache Pig przy użyciu zestawu .NET SDK dla platformy Hadoop - Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać zestawu .NET SDK dla platformy Hadoop do przesyłania zadań Pig do platformy Hadoop w usłudze HDInsight."
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/29/2018
ms.author: larryfr
ms.openlocfilehash: 5510d64b518e4694caa8e35140907b55c01ecd8a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Uruchamianie zadań Pig przy użyciu zestawu .NET SDK dla platformy Hadoop w usłudze HDInsight

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Dowiedz się, jak używać zestawu .NET SDK dla platformy Hadoop do przesyłania zadań Apache Pig do platformy Hadoop w usłudze Azure HDInsight.

Zestaw .NET SDK usługi HDInsight zapewnia bibliotek klienta .NET, które ułatwia pracy z klastrami HDInsight ze środowiska .NET. Pig służy do tworzenia MapReduce operacji za pomocą modelowania szereg przekształcenia danych. W tym dokumencie Dowiedz się jak używać podstawowej aplikacji C# można przesłać zadania programu Pig do klastra usługi HDInsight.

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym artykule, są potrzebne.

* Klaster usługi Azure HDInsight (Hadoop w usłudze HDInsight) (Windows lub opartych na systemie Linux).

  > [!IMPORTANT]
  > Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz sekcję [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Wycofanie usługi HDInsight w systemie Windows).

* Program Visual Studio 2012, 2013, 2015 lub 2017 r.

## <a name="create-the-application"></a>Tworzenie aplikacji

Zestawu .NET SDK HDInsight udostępnia biblioteki klienta .NET, co ułatwia do pracy z klastrami HDInsight ze środowiska .NET.

1. Z **pliku** menu w programie Visual Studio, wybierz **nowy** , a następnie wybierz **projektu**.

2. Dla nowego projektu wpisz lub wybierz następujące wartości:

   | Właściwość | Wartość |
   | ------ | ------ |
   | Kategoria | Szablony/Visual C#/Windows |
   | Szablon | Aplikacja konsolowa |
   | Name (Nazwa) | SubmitPigJob |

3. Kliknij przycisk **OK**, aby utworzyć projekt.

4. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki** lub **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.

5. Aby zainstalować pakiety zestawu .NET SDK, użyj następującego polecenia:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. W Eksploratorze rozwiązań kliknij dwukrotnie **Program.cs** go otworzyć. Zamień istniejący kod poniżej.

    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitPigJob
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;

            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
            private const string ExistingClusterUsername = "<Cluster Username>";
            private const string ExistingClusterPassword = "<Cluster User Password>";

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. Aby uruchomić aplikację, naciśnij klawisz **F5**.

8. Aby zakończyć aplikację, naciśnij klawisz **ENTER**.

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać informacji na temat Pig w usłudze HDInsight, zobacz [Use Pig z usługą Hadoop w usłudze HDInsight](hdinsight-use-pig.md).

Aby uzyskać więcej informacji na temat używania usługi Hadoop w usłudze HDInsight można znaleźć w następujących dokumentach:

* [Korzystanie z programu Hive z usługą Hadoop w usłudze HDInsight](hdinsight-use-hive.md)
* [Używanie MapReduce z usługą Hadoop w usłudze HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
