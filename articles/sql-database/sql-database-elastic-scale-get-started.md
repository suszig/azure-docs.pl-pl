---
title: "Wprowadzenie do narzędzi elastycznej bazy danych | Dokumentacja firmy Microsoft"
description: "Podstawowe informacje dotyczące funkcji narzędzi elastycznej bazy danych, bazy danych SQL Azure, w tym przykładowej łatwe do uruchomienia aplikacji."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: 637463399593f4bc9ff5bfcbf67bf93b816efc7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Wprowadzenie do narzędzi elastycznej bazy danych
Ten dokument stanowi wprowadzenie do środowisko dewelopera ułatwiając uruchamianie przykładowej aplikacji. Próbka tworzy prostą aplikację podzielonej i Eksploruje kluczowych możliwości narzędzi elastycznej bazy danych. W przykładzie pokazano funkcje [biblioteki klienta elastycznej bazy danych](sql-database-elastic-database-client-library.md).

Aby zainstalować bibliotekę, przejdź do [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Biblioteka jest instalowany z przykładowej aplikacji opisanej w poniższej sekcji.

## <a name="prerequisites"></a>Wymagania wstępne
* Program Visual Studio 2012 lub nowszym z C#. Pobierz bezpłatną wersję w [programu Visual Studio pobiera](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 lub nowszej. Aby uzyskać najnowszą wersję, zobacz [instalowania NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="download-and-run-the-sample-app"></a>Pobieranie i uruchamianie aplikacji przykładowych
**Elastyczne narzędzia bazy danych dla bazy danych SQL Azure — wprowadzenie** Przykładowa aplikacja ilustruje najważniejszych aspektów środowisko programistyczne podzielonej aplikacji, które użyj narzędzi elastycznej bazy danych. Dotyczy on przypadków użycia klucza dla [zarządzania mapy niezależnego fragmentu](sql-database-elastic-scale-shard-map-management.md), [routingu zależne od danych](sql-database-elastic-scale-data-dependent-routing.md), i [zapytań wielu niezależnych](sql-database-elastic-scale-multishard-querying.md). Aby pobrać i uruchomić przykład, wykonaj następujące kroki: 

1. Pobierz [elastyczne narzędzia bazy danych dla bazy danych SQL Azure — wprowadzenie próbki](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) w witrynie MSDN. Rozpakuj próbki do wybranej lokalizacji.

2. Aby utworzyć projekt, otwórz **ElasticScaleStarterKit.sln** rozwiązania z **C#** katalogu.

3. W rozwiązaniu dla przykładowy projekt, otwórz **app.config** pliku. Następnie postępuj zgodnie z instrukcjami w pliku, aby dodać nazwę serwera bazy danych SQL Azure i Twoje informacje logowania (nazwa użytkownika i hasło).

4. Skompiluj i uruchom aplikację. Po wyświetleniu monitu włączenia programu Visual Studio pod kątem przywracania pakietów NuGet rozwiązania. Spowoduje to pobranie najnowszej wersji biblioteki klienta elastycznej bazy danych z pakietu NuGet.

5. Wypróbuj różne opcje, aby dowiedzieć się więcej o możliwościach biblioteki klienta. Należy zauważyć, kroki, które przyjmuje aplikacji w konsoli output i dowolnie eksplorowania kodu w tle.
   
    ![Postęp][4]

Gratulacje — pomyślnie skompilowane i uruchamianie pierwszej aplikacji podzielonej za pomocą narzędzi elastycznej bazy danych w bazie danych SQL. Do połączenia z bazą danych SQL i szybko wyświetlić na odłamków, utworzone próbki, należy użyć programu Visual Studio lub SQL Server Management Studio. Można zauważyć nowe przykładowe niezależnego fragmentu bazy danych i niezależnego fragmentu bazy danych Menedżera mapy, która utworzyła próbki.

> [!IMPORTANT]
> Firma Microsoft zaleca zawsze używać najnowszej wersji programu Management Studio, dzięki czemu można zachować synchronizację z aktualizacji na platformie Azure oraz bazy danych SQL. [Zaktualizuj program SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-the-code-sample"></a>Kluczowych przykładowy kod
* **Zarządzanie odłamków i niezależnych mapuje**: kod ilustruje sposób pracy z fragmentów, zakresy i mapowania w pliku **ShardManagementUtils.cs**. Aby uzyskać więcej informacji, zobacz [skalowania bazy danych przy użyciu Menedżera mapy niezależnego fragmentu](http://go.microsoft.com/?linkid=9862595).  

* **Zależne od danych routingu**: Routing transakcji, które mają prawo niezależnego fragmentu jest wyświetlany w obszarze **DataDependentRoutingSample.cs**. Aby uzyskać więcej informacji, zobacz [routingu zależne od danych](http://go.microsoft.com/?linkid=9862596). 

* **Obsługę zapytań wielu odłamków**: wykonywanie zapytań w odłamków przedstawiono w pliku **MultiShardQuerySample.cs**. Aby uzyskać więcej informacji, zobacz [zapytań wielu niezależnych](http://go.microsoft.com/?linkid=9862597).

* **Dodawanie pustego odłamków**: iteracyjne Dodawanie nowych odłamków pusty jest wykonywane przez kod w pliku **CreateShardSample.cs**. Aby uzyskać więcej informacji, zobacz [skalowania bazy danych przy użyciu Menedżera mapy niezależnego fragmentu](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Inne operacje elastycznego skalowania
* **Dzielenie istniejących niezależnego fragmentu**: możliwość podziału odłamków są dostarczane przez **narzędzia do scalania podziału**. Aby uzyskać więcej informacji, zobacz [przenoszenia danych między bazami danych w chmurze skalowalnych w poziomie](sql-database-elastic-scale-overview-split-and-merge.md).

* **Scalanie istniejącego odłamków**: scaleń niezależnego fragmentu są również wykonywane przy użyciu **narzędzia do scalania podziału**. Aby uzyskać więcej informacji, zobacz [przenoszenia danych między bazami danych w chmurze skalowalnych w poziomie](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Koszty
Mogą narzędzi elastycznej bazy danych. Korzystając z narzędzi elastycznej bazy danych, nie pojawi się żadnych dodatkowych kosztów na górze koszt użycie platformy Azure. 

Na przykład przykładowa aplikacja tworzy nowe bazy danych. Koszt to zależy od wybranej wersji bazy danych SQL i Azure użycia aplikacji.

Aby uzyskać informacje o cenach, zobacz [bazy danych SQL, szczegóły cennika](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat narzędzi elastycznej bazy danych zobacz następujące strony:

* Przykłady kodu: 
  * [Narzędzi elastycznej bazy danych SQL Azure — wprowadzenie](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Narzędzi elastycznej bazy danych SQL Azure - Entity Framework integracji](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Elastyczność niezależnego fragmentu w Centrum skryptów](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [elastycznego skalowania anonsu](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy: [wdrażanie skalowalnego w poziomie przy użyciu dzielenia na fragmenty wideo biblioteki klienta elastycznej bazy danych](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Kanał 9: [elastycznego skalowania omówienie wideo](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Forum dyskusyjne: [forum bazy danych SQL Azure](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Do pomiaru wydajności: [liczniki wydajności dla menedżera map niezależnego fragmentu](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png

