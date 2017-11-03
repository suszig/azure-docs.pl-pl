---
title: "Aplikacje wielodostępne z narzędzi elastycznej bazy danych i zabezpieczenia na poziomie wiersza"
description: "Użyj narzędzi elastycznej bazy danych z zabezpieczeniami na poziomie wiersza, aby skompilować aplikację z warstwą danych wysokiej skalowalności."
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: 4b4ec29bb53bdce413dadf7cb0751433b9ca686c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Aplikacje wielodostępne z narzędzi elastycznej bazy danych i zabezpieczenia na poziomie wiersza
[Narzędzi elastycznej bazy danych](sql-database-elastic-scale-get-started.md) i [zabezpieczeń na poziomie wiersza](https://msdn.microsoft.com/library/dn765131) oferują zaawansowany zestaw możliwości elastyczne i wydajne skalowanie warstwy danych wielodostępnych aplikacji z bazy danych SQL Azure. Aby uzyskać więcej informacji, zobacz [wzorce projektowe dla wielodostępnych aplikacji SaaS przy użyciu usługi Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) Aby uzyskać więcej informacji. 

W tym artykule przedstawiono sposób używania tych technologii jednocześnie do tworzenia aplikacji z warstwą danych skalowalnej obsługuje wielodostępne odłamków, za pomocą **ADO.NET SqlClient** i/lub **programu Entity Framework**.  

* **Narzędzi elastycznej bazy danych** umożliwia deweloperom skalowania warstwy danych aplikacji za pośrednictwem standardowych dzielenia na fragmenty rozwiązania przy użyciu zestawu .NET bibliotek i szablony usługi Azure. Zarządzanie odłamków z za pomocą biblioteki klienta elastycznej bazy danych pozwala zautomatyzować i uprościć wiele zadań infrastrukturalne zwykle skojarzone z dzielenia na fragmenty. 
* **Wiersz poziomu zabezpieczeń** umożliwia deweloperom przechowywania danych dla wielu dzierżawców w tej samej bazy danych za pomocą zasad zabezpieczeń, aby odfiltrować wiersze, które nie należą do dzierżawy wykonywanie zapytania. Scentralizowany dostęp logiki odświeżania w bazie danych, a nie w aplikacji upraszcza konserwację i zmniejsza ryzyko błędu ścieżce bazowej kodu aplikacji rozwoju. 

Razem z tych funkcji, aplikacji mogą korzystać z koszt zyski oszczędności i wydajności, dane są przechowywane dla wielu dzierżawców w bazie danych sam identyfikator niezależnego fragmentu. W tym samym czasie aplikacji nadal ma możliwość oferuje izolowanym, pojedynczego dzierżawcy fragmentów dla dzierżawcy "premium", którzy wymagają bardziej rygorystyczne gwarancji wydajności, ponieważ odłamków wielodostępne gwarantuje dystrybucji równy zasobów między dzierżawcami.  

Krótko mówiąc, elastyczne bazy danych biblioteki klienta [routingu zależne od danych](sql-database-elastic-scale-data-dependent-routing.md) interfejsów API automatycznie łączyć dzierżaw poprawny identyfikator niezależnego fragmentu bazy danych zawierającej klucz dzielenia na fragmenty (zazwyczaj "TenantId"). Po nawiązaniu połączenia zasady zabezpieczeń zabezpieczenia na poziomie wiersza w bazie danych zapewnia, że dzierżawców można tylko dostęp do wierszy zawierających ich identyfikatora dzierżawcy. Zakłada się, że wszystkie tabele zawierają kolumnę TenantId wskazują wiersze, które należą do każdego dzierżawcy. 

![Architektura aplikacji obsługi blogów][1]

## <a name="download-the-sample-project"></a>Pobierz przykładowy projekt
### <a name="prerequisites"></a>Wymagania wstępne
* Za pomocą programu Visual Studio (2012 lub nowszy) 
* Utwórz trzy bazy danych SQL Azure 
* Pobierz przykładowy projekt: [elastyczne narzędzia bazy danych dla bazy danych SQL Azure - odłamków wielodostępne](http://go.microsoft.com/?linkid=9888163)
  * Wprowadź informacje dla baz danych na początku **Program.cs** 

Ten projekt rozszerza opisanych w [elastyczne narzędzia bazy danych dla bazy danych SQL Azure - Entity Framework integracji](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) przez dodanie obsługi wielodostępne niezależnego fragmentu bazy danych. Tworzenia prostej aplikacji konsolowej do tworzenia blogów i wpisów, z czterech dzierżawców i dwóch niezależnych wielodostępnych baz danych jak pokazano na powyższym diagramie. 

Skompiluj i uruchom aplikację. Używa do ładowania menedżera map niezależnego fragmentu narzędzi elastycznej bazy danych i uruchom następujące testy: 

1. Przy użyciu programu Entity Framework i LINQ, Utwórz nowy blog, a następnie Wyświetl wszystkie blogi dla każdego dzierżawcy
2. Za pomocą ADO.NET SqlClient, wyświetlić wszystkie blogi dla dzierżawy
3. Próba wstawienia blogu dla niewłaściwego dzierżawcy sprawdzić, czy błąd jest zgłaszany  

Powiadomienie, że ponieważ zabezpieczenia na poziomie wiersza nie została jeszcze włączona w bazach danych niezależnego fragmentu, tych testów ujawnia problem: dzierżawcy będą mogli zobaczyć blogów, które nie należą do nich, a aplikacja nie będzie mógł Wstawianie blogu dla niewłaściwego dzierżawcy. W dalszej części tego artykułu opisano, jak rozwiązać te problemy przez wymuszania izolacji dzierżawców odświeżania. Istnieją dwa kroki: 

1. **Warstwy aplikacji**: modyfikowanie kodu aplikacji, aby zawsze ustawić bieżącego identyfikatora dzierżawcy w SESSION_CONTEXT po otwarciu połączenia. Przykładowy projekt zawiera już zostało to zrobione. 
2. **Warstwa danych**: Tworzenie zasad zabezpieczeń zabezpieczenia na poziomie wiersza każdego niezależnego fragmentu bazy danych oparte na TenantId przechowywane w SESSION_CONTEXT wierszy filtru. Należy to zrobić dla każdego użytkownika niezależnych baz danych, w przeciwnym razie wierszy w wielu dzierżawcy fragmentów nie są filtrowane. 

## <a name="step-1-application-tier-set-tenantid-in-the-sessioncontext"></a>Warstwy aplikacji w kroku 1): Ustaw TenantId w SESSION_CONTEXT
Po nawiązaniu połączenia z bazą danych niezależnych przy użyciu routingu interfejsów API zależne od danych biblioteki klienta elastycznej bazy danych, aplikacji nadal należy sprawdzić bazy danych, które TenantId używa tego połączenia, aby zabezpieczenia na poziomie wiersza zasady zabezpieczeń można odfiltrować wierszy należące do innych dzierżawców. Zalecanym sposobem przekazać te informacje jest przechowywanie bieżącego identyfikatora dzierżawy dla tego połączenia w [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Uwaga: Możesz też użyć [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), ale SESSION_CONTEXT jest lepszym rozwiązaniem, ponieważ jest łatwiejsza w użyciu, zwraca wartość NULL, domyślnie i obsługuje pary klucz wartość.)

### <a name="entity-framework"></a>Entity Framework
Dla aplikacji za pomocą programu Entity Framework, najprostszym podejście jest skonfigurowanie SESSION_CONTEXT w zastąpienie ElasticScaleContext opisanego w [danych zależne od routingu przy użyciu EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Przed powrotem połączenia przeprowadzana za pośrednictwem routingu zależne od danych, należy tworzyć i wykonywać SqlCommand, która ustawia "TenantId" w SESSION_CONTEXT do shardingKey określony dla tego połączenia. Dzięki temu wystarczy raz napisać kod, aby ustawić SESSION_CONTEXT. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data-dependent routing. This call opens a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call fails for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

Teraz SESSION_CONTEXT jest ustawiany automatycznie z określonego identyfikatora dzierżawcy przy każdym wywołaniu ElasticScaleContext: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient
Dla aplikacji za pomocą ADO.NET SqlClient Zalecanym podejściem jest tworzenie funkcji otoki wokół ShardMap.OpenConnectionForKey() ustawiający automatycznie "TenantId" w SESSION_CONTEXT do poprawne TenantId przed zwróceniem połączenia. Aby upewnić się, że SESSION_CONTEXT ma zawsze wartość, należy otworzyć tylko połączeń za pomocą tej funkcji otoki.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs are listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>Warstwa danych kroku 2): Tworzenie zasad zabezpieczeń na poziomie wiersza
### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Tworzenie zasad zabezpieczeń, aby filtrować wiersze, które mogą uzyskiwać dostęp do każdego dzierżawcy
Teraz, gdy aplikacja jest ustawienie SESSION_CONTEXT z bieżącego identyfikatora dzierżawcy przed wykonaniem kwerendy, zabezpieczenia na poziomie wiersza zasady zabezpieczeń można filtrować zapytania i wykluczyć wiersze, które mają różne identyfikatora dzierżawcy.  

Kontrola dostępu jest zaimplementowana w T-SQL: logika dostępu definiuje funkcję zdefiniowaną przez użytkownika i zasady zabezpieczeń powiązanie tej funkcji z dowolną liczbę tabel. Dla tego projektu funkcja sprawdza, czy aplikacji (zamiast innego użytkownika SQL) jest połączona z bazą danych, i że TenantId przechowywane w SESSION_CONTEXT TenantId danego wiersza. Predykat filtru umożliwia wiersze spełniające te warunki, które przechodzą przez filtr dla zapytania SELECT, UPDATE i DELETE; predykat bloku uniemożliwia wiersze, które naruszają te warunki przed wstawione lub zaktualizowane i. Jeśli nie ustawiono SESSION_CONTEXT, zwraca wartość NULL ani żadnych wierszy są widoczne lub możliwe do wstawienia. 

Aby włączyć zabezpieczenia na poziomie wiersza, należy wykonać T-SQL na wszystkich odłamków przy użyciu programu Visual Studio (SSDT), SSMS lub skrypt programu PowerShell dołączony do projektu (lub jeśli używasz [zadania elastyczne bazy danych](sql-database-elastic-jobs-overview.md), służy do automatyzacji wykonywania tego T-SQL na wszystkich odłamków): 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> Bardziej złożone projektów, które trzeba dodać predykat na setki tabel można użyć procedury przechowywane pomocnika, która automatycznie generuje zasady zabezpieczeń, dodawanie predykat dla wszystkich tabel w schemacie. Aby uzyskać więcej informacji, zobacz [zastosowania zabezpieczeń na poziomie wiersza do wszystkich tabel - pomocnika skryptu (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Teraz po uruchomieniu aplikacji przykładowej ponownie dzierżaw widoczne tylko wiersze, które należą do nich. Ponadto aplikacja nie można wstawić wierszy, które należą do dzierżawcy innych niż ten, który jest aktualnie połączony z niezależnego fragmentu bazy danych i nie można zaktualizować widocznych wierszy, które mają różne identyfikatora dzierżawcy. Jeśli aplikacja próbuje zrobić, DbUpdateException jest wywoływane.

Jeśli później Dodaj nową tabelę zmodyfikowane zasady zabezpieczeń i Dodaj filtr i zablokować predykaty w nowej tabeli: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Dodaj domyślne ograniczenia, aby automatycznie wypełnić TenantId dla operacji wstawienia
Możesz też zaznaczyć ograniczeniu domyślnym w każdej tabeli, aby automatycznie wypełnić TenantId o wartości przechowywanych obecnie we SESSION_CONTEXT podczas wstawiania wierszy. Na przykład: 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Teraz aplikacji nie trzeba określać identyfikatora dzierżawcy podczas wstawiania wierszy: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Użycie domyślne ograniczenia dla projektu programu Entity Framework, zaleca się, że kolumna identyfikatora dzierżawcy nie zostanie uwzględniony w modelu danych EF. Jest to spowodowane zapytania programu Entity Framework automatyczne określanie wartości domyślnych, które zastępują domyślne ograniczenia utworzone w T-SQL używających SESSION_CONTEXT. Aby użyć domyślnego ograniczenia w przykładowy projekt, na przykład, należy usunąć TenantId z DataClasses.cs (i wykonywania Add-Migration w konsoli Menedżera pakietów) i użyj T-SQL, aby upewnić się, że pole istnieje tylko w tabelach bazy danych. Dzięki temu podczas wstawiania danych, EF automatycznie dostarczyć niepoprawne wartości domyślne. 
> 
> 

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(Opcjonalnie) Włącz "administratora" można uzyskać dostępu do wszystkich wierszy
Niektóre aplikacje warto utworzyć "administratora" kto mogą uzyskiwać dostęp do wszystkich wierszy, na przykład, aby włączyć raportowanie dla wszystkich dzierżawców w odłamków wszystkich lub do wykonywania operacji podziału/Merge na odłamków obejmujących przesuwanie wierszy dzierżawy między bazami danych. Aby włączyć tę opcję, należy utworzyć nowego użytkownika SQL ("superuser" w tym przykładzie) w każdym niezależnego fragmentu bazy danych. Następnie należy zmienić zasady zabezpieczeń z nowej funkcji predykatu, która umożliwia użytkownikom dostęp do wszystkich wierszy:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Konserwacji
* **Dodawanie nowych odłamków**: wykonanie skryptu T-SQL, aby włączyć zabezpieczenia na poziomie wiersza na wszelkich nowych fragmentów, w przeciwnym razie kwerendy dotyczące tych fragmentów nie są filtrowane.
* **Dodawanie nowych tabel**: Dodaj predykatu filtru i bloku je do zasad zabezpieczeń na wszystkich odłamków zawsze, gdy nowa tabela została utworzona, w przeciwnym razie kwerendy w nowej tabeli nie są filtrowane. Ten można zautomatyzować za pomocą wyzwalacza DDL, zgodnie z opisem w [zastosowania zabezpieczeń na poziomie wiersza automatycznie na nowo utworzony tabele (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Podsumowanie
Narzędzi elastycznej bazy danych i zabezpieczenia na poziomie wiersza mogą być używane razem do skalowania aplikacji warstwy danych o obsługę zarówno wielodostępnych i pojedynczego dzierżawcy fragmentów. Wielodostępne odłamków może służyć do przechowywania danych wydajniej (szczególnie w przypadkach, gdy mają tylko kilka wierszy danych w dużej liczby dzierżawców), podczas jednego dzierżawcy fragmentów może służyć do obsługi dzierżawców premium z bardziej rygorystyczne wydajności i izolacja wymagania.  Aby uzyskać więcej informacji, zobacz [informacje dotyczące zabezpieczeń na poziomie wiersza](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Co to jest puli elastycznej platformy Azure?](sql-database-elastic-pool.md)
* [Scaling out with Azure SQL Database (Skalowanie w poziomie za pomocą usługi Azure SQL Database)](sql-database-elastic-scale-introduction.md)
* [Wzorce projektowe dla wielodostępnych aplikacji SaaS wykorzystujących usługę Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Uwierzytelnianie w aplikacjach wielodostępnych za pomocą usługi Azure AD i OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Aplikacja Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Pytania i żądania funkcji
Odpowiedzi na pytania, dotrzeć do nas na [forum bazy danych SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) i funkcja żądań, dodaj je do [forum opinii bazy danych SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


