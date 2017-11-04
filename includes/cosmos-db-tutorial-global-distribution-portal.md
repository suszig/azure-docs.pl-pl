
Informacje na temat bazy danych Azure rozwiązania Cosmos globalne dystrybucji w tym Azure piątek wideo z Scott Hanselman i Ramana Karthik główny Menedżer zespołu inżynieryjnego.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Aby uzyskać więcej informacji na temat sposobu globalnej replikacji bazy danych działa w usłudze Azure DB rozwiązania Cosmos, zobacz [dystrybucji danych globalnie z rozwiązania Cosmos DB](../articles/cosmos-db/distribute-data-globally.md).

## <a id="addregion"></a>Dodawanie regionów globalna baza danych przy użyciu portalu Azure
Azure DB rozwiązania Cosmos jest dostępna we wszystkich [regiony platformy Azure] [ azureregions] na całym świecie. Po wybraniu domyślnego poziomu spójności dla konta bazy danych, można skojarzyć jeden lub więcej regionów (w zależności od wybrana domyślna spójność dystrybucji globalnych i poziomu potrzeb).

1. W [portalu Azure](https://portal.azure.com/), na pasku po lewej stronie kliknij **bazy danych Azure rozwiązania Cosmos**.
2. W **bazy danych Azure rozwiązania Cosmos** bloku, wybierz konto bazy danych do zmodyfikowania.
3. W bloku konta kliknij **globalnie replikacji danych** z menu.
4. W **globalnie replikacji danych** bloku, wybierz regionów, aby dodać lub usunąć, klikając regionów na mapie, a następnie kliknij **zapisać**. Brak koszt do dodawania regionów, zobacz [cennikiem](https://azure.microsoft.com/pricing/details/cosmos-db/) lub [dystrybucji danych globalnie z bazy danych Azure rozwiązania Cosmos](../articles/cosmos-db/distribute-data-globally.md) artykułu, aby uzyskać więcej informacji.
   
    ![Kliknij przycisk regionów na mapie, aby dodać lub usunąć je][1]
    
Po dodaniu drugiego region **ręczną pracę awaryjną** jest włączona opcja **globalnie replikacji danych** bloku w portalu. Ta opcja umożliwia testowanie procesu pracy awaryjnej lub zmienić regionu podstawowego zapisu. Po dodaniu obszarem trzeci **priorytetów trybu Failover** opcja jest włączona na tym samym bloku, dzięki czemu można zmienić kolejność pracy awaryjnej dla odczytów.  

### <a name="selecting-global-database-regions"></a>Wybieranie regionów globalna baza danych
Istnieją dwa typowych scenariuszy dotyczących konfigurowania dwóch lub więcej regionów:

1. Dostarczanie małe opóźnienia dostępu do danych dla użytkowników końcowych, niezależnie od tego, gdzie znajdują się na całym świecie
2. Dodawanie regionalnych odporności ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii (BCDR)

Do dostarczania małym opóźnieniu dla użytkowników końcowych, zaleca się wdrożenie zarówno aplikacji i Dodaj Azure DB rozwiązania Cosmos w regionach, które jest zgodne, do których aplikacji użytkownicy znajdują się.

Dla BCDR, zalecane jest dodawanie regionów na podstawie par region opisanego w [firm ciągłości i odzyskiwanie po awarii (BCDR): łączyć regiony platformy Azure] [ bcdr] artykułu.

<!--

## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
