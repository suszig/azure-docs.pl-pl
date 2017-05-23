---
title: "Tworzenie konta usługi Azure Cosmos DB | Microsoft Docs"
description: "Utwórz bazę danych NoSQL przy użyciu usługi Azure Cosmos DB. Wykonaj te instrukcje, aby utworzyć konto usługi Azure Cosmos DB i rozpocząć tworzenie błyskawicznej globalnej bazy danych NoSQL."
keywords: tworzenie bazy danych
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 0e7f8488-7bb7-463e-b6fd-3ae91a02c03a
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/17/2017
ms.author: mimig
redirect_url: https://aka.ms/acdbnetqa
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 13621d942f2880f4dd1523315f43099eca2607d8
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-create-an-azure-cosmos-db-nosql-account-using-the-azure-portal"></a>Jak utworzyć konto usługi Azure Cosmos DB NoSQL przy użyciu witryny Azure Portal
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](documentdb-create-account.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

Aby utworzyć bazę danych przy użyciu usługi Microsoft Azure Cosmos DB, są potrzebne:

* Konto platformy Azure. Jeśli go jeszcze nie masz, możesz utworzyć [bezpłatne konto platformy Azure](https://azure.microsoft.com/free).
* Utwórz konto usługi Azure Cosmos DB.  

Konto usługi Azure Cosmos DB można utworzyć za pomocą witryny Azure Portal, szablonów usługi Azure Resource Manager lub interfejsu wiersza polecenia platformy Azure. W tym artykule przedstawiono, jak utworzyć konto usługi Azure Cosmos DB przy użyciu witryny Azure Portal. Aby utworzyć konto przy użyciu usługi Azure Resource Manager lub interfejsu wiersza polecenia platformy Azure, zobacz artykuł [Automate Azure Cosmos DB database account creation](documentdb-automation-resource-manager-cli.md) (Automatyzacja tworzenia konta bazy danych usługi Azure Cosmos DB).

Jesteś nowym użytkownikiem usługi Azure Cosmos DB? Obejrzyj [to](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) czterominutowe nagranie, w którym Scott Hanselman pokazuje, jak wykonać typowe zadania w portalu online.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Na lewym pasku nawigacyjnym kliknij pozycje **Nowy** i **Bazy danych**, a następnie kliknij pozycję **Azure Cosmos DB**.

   ![Zrzut ekranu przedstawiający witrynę Azure Portal z wyróżnionymi poleceniami Więcej usług i NoSQL (Azure Cosmos DB)](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)  
3. W bloku **Nowe konto** określ odpowiednią konfigurację konta usługi Azure Cosmos DB.

    ![Zrzut ekranu bloku Nowa usługa Azure Cosmos DB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

   * W polu **Identyfikator** wprowadź nazwę, aby zidentyfikować konto usługi Azure Cosmos DB.  Po **zweryfikowaniu** wartości pola **Identyfikator** w polu tym zostanie wyświetlony zielony znacznik wyboru. Wartość pola **Identyfikator** jest używana jako nazwa hosta w identyfikatorze URI. Pole **Identyfikator** może zawierać tylko małe litery, cyfry i znak „-” w liczbie od 3 do 50 znaków. Pamiętaj, że ciąg *documents.azure.com* jest dołączany do wybranej nazwy punktu końcowego, by utworzyć punkt końcowy konta usługi Azure Cosmos DB.
   * W polu **Interfejs API usługi NoSQL** wybierz model programowania, który ma zostać użyty:

     * **DocumentDB**: interfejs API usługi DocumentDB jest dostępny za pośrednictwem [zestawów SDK](documentdb-sdk-dotnet.md) .NET, Java, Node.js, Python i JavaScript, a także w środowisku HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) i umożliwia dostęp programowy do wszystkich funkcji usługi DocumentDB.
     * **MongoDB**: usługa DocumentDB zapewnia także [obsługę na poziomie protokołu](documentdb-protocol-mongodb.md) interfejsów API usługi **MongoDB**. Po wybraniu opcji interfejsu API usługi MongoDB można używać istniejących zestawów SDK oraz [narzędzi](documentdb-mongodb-mongochef.md) usługi MongoDB do komunikacji z usługą DocumentDB. Istniejące aplikacje usługi MongoDB można [przenieść](documentdb-import-data.md) do środowiska DocumentDB [bez zmian kodu](documentdb-connect-mongodb-account.md), by używać w pełni zarządzanej bazy danych działającej jako usługa z obsługą nieograniczonego skalowania, globalnej replikacji i innych funkcji.
   * W polu **Subskrypcja** wybierz subskrypcję platformy Azure, która ma być używana dla konta usługi Azure Cosmos DB. Jeśli konto ma tylko jedną subskrypcję, jest wybierane jako domyślne.
   * W polu **Grupa zasobów** wybierz lub utwórz grupę zasobów dla konta usługi Azure Cosmos DB.  Domyślnie zostanie utworzona nowa grupa zasobów. Aby uzyskać więcej informacji, zobacz temat [Using the Azure portal to manage your Azure resources](../azure-portal/resource-group-portal.md) (Korzystanie z witryny Azure Portal do zarządzania zasobami Azure).
   * Użyj opcji **Lokalizacja**, aby określić lokalizację geograficzną, w której będzie hostowane konto usługi Azure Cosmos DB.
4. Po skonfigurowaniu opcji nowego konta usługi Azure Cosmos DB kliknij przycisk **Utwórz**. Aby sprawdzić stan wdrożenia, przejdź do centrum powiadomień.  

   ![Szybkie tworzenie baz danych — zrzut ekranu przedstawiający centrum powiadomień z informacją o trwającym tworzeniu konta usługi Azure Cosmos DB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)  

   ![Zrzut ekranu przedstawiający centrum powiadomień z informacją o pomyślnym utworzeniu i wdrożeniu w grupie zasobów konta usługi Azure Cosmos DB — powiadomienie kreatora bazy danych w trybie online](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)
5. Utworzone konto usługi Azure Cosmos DB jest gotowe do użycia z ustawieniami domyślnymi. Domyślna spójność konta usługi Azure Cosmos DB jest ustawiona na wartość **Sesja**.  Można ją dostosować, klikając pozycję **Domyślna spójność** w menu zasobów. Aby dowiedzieć się więcej na temat poziomów spójności oferowanych przez usługę Azure Cosmos DB, zobacz temat [Consistency levels in Azure Cosmos DB](documentdb-consistency-levels.md) (Poziomy spójności w usłudze Azure Cosmos DB).

   ![Zrzut ekranu przedstawiający blok Grupa zasobów — rozpoczęcie tworzenia aplikacji](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)  

   ![Zrzut ekranu przedstawiający blok poziomu spójności — spójność sesji](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create an Azure Cosmos DB account]: #Howto
[Next steps]: #NextSteps


## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz już konto usługi Azure Cosmos DB, kolejnym krokiem jest utworzenie kolekcji i bazy danych usługi Azure Cosmos DB.

Nową kolekcję i bazę danych można utworzyć, korzystając z jednego z następujących zasobów:

* witryny Azure Portal, zgodnie z opisem w artykule [Create a Azure Cosmos DB collection using the Azure portal](documentdb-create-collection.md) (Tworzenie kolekcji usługi Azure Cosmos DB za pomocą witryny Azure Portal);
* kompleksowych samouczków z przykładowymi danymi, dotyczących technologii [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) lub [Python](documentdb-python-application.md);
* przykładowego kodu w języku [.NET](documentdb-dotnet-samples.md#database-examples), [Node.js](documentdb-nodejs-samples.md#database-examples) lub [Python](documentdb-python-samples.md#database-examples) dostępnego w witrynie GitHub;
* zestawów SDK środowisk [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Node.js](documentdb-sdk-node.md), [Java](documentdb-sdk-java.md), [Python](documentdb-sdk-python.md) i [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx).

Po utworzeniu bazy danych i kolekcji należy [dodać dokumenty](documentdb-view-json-document-explorer.md) do kolekcji.

Gdy dokumenty znajdą się w kolekcji, można użyć [bazy danych SQL usługi DocumentDB](documentdb-sql-query.md) do [wykonywania zapytań](documentdb-sql-query.md#ExecutingSqlQueries) dotyczących tych dokumentów. Zapytania można wykonywać za pomocą narzędzia [Eksplorator zapytań](documentdb-query-collections-query-explorer.md) w witrynie Azure Portal, [interfejsu API REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) lub jednego z [zestawów SDK](documentdb-sdk-dotnet.md).

### <a name="learn-more"></a>Dowiedz się więcej
Aby dowiedzieć się więcej na temat usługi Azure Cosmos DB, zapoznaj się z tymi zasobami:

* [Wprowadzenie do usługi Azure Cosmos DB](../cosmos-db/introduction.md)

