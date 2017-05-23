---
title: "Samouczek usługi Azure Cosmos DB: Tworzenie, wykonywanie zapytań i przechodzenie w konsoli Gremlin | Microsoft Docs"
description: "Przewodnik Szybki start usługi Azure Cosmos DB opisujący tworzenie wierzchołków, krawędzi i zapytań za pomocą interfejsu API programu Graph w usłudze Azure Cosmos DB."
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: terminal
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fb27ba1a70959ba92fbd021e9e42438081000e45
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-create-query-and-traverse-a-graph-in-the-gremlin-console"></a>Azure Cosmos DB: Tworzenie elementów, wykonywanie zapytań i przechodzenie grafu w konsoli Gremlin

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB, można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

Ten przewodnik Szybki start przedstawia sposób tworzenia konta usługi Azure Cosmos DB, bazy danych i grafu (kontenera) przy użyciu witryny Azure Portal oraz korzystania z [konsoli Gremlin](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) do pracy z danymi interfejsu API programu Graph (wersja zapoznawcza). W tym samouczku utworzysz wierzchołki i krawędzie oraz wykonasz względem nich zapytania, zaktualizujesz właściwość wierzchołka, przejdziesz graf i usuniesz wierzchołek.

![Usługa Azure Cosmos DB w konsoli Apache Gremlin](./media/create-graph-gremlin-console/gremlin-console.png)

Konsola Gremlin jest oparta na języku Groovy/Java i działa w systemach Linux, Mac i Windows. Możesz ją pobrać z [witryny Apache TinkerPop](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.4/apache-tinkerpop-gremlin-console-3.2.4-bin.zip).

## <a name="prerequisites"></a>Wymagania wstępne

Musisz dysponować subskrypcją platformy Azure, aby utworzyć konto usługi Azure Cosmos DB na potrzeby tego przewodnika Szybki start.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Należy również zainstalować [konsolę Gremlin](http://tinkerpop.apache.org/). Użyj wersji 3.2.4 lub nowszej.

## <a name="create-a-database-account"></a>Tworzenie konta bazy danych

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Dodawanie grafu

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## <a id="ConnectAppService"></a>Łączenie z usługą aplikacji
1. Przed uruchomieniem konsoli Gremlin utwórz lub zmodyfikuj plik konfiguracji *remote-secure.yaml* w katalogu *apache-tinkerpop-gremlin-console-3.2.4/conf*.
2. Wypełnij ustawienia konfiguracji *host*, *port*, *username*, *password*, *connectionPool* i *serializer*:

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Hosts|***.graphs.azure.com|Identyfikator URI usługi Graph, który można pobrać z witryny Azure Portal
    Port|443|Ustawiony numer 443
    Nazwa użytkownika|*Twoja nazwa użytkownika*|Zasób w postaci `/dbs/<db>/colls/<coll>`.
    Hasło|*Twój podstawowy klucz główny*|Twój podstawowy klucz główny dla usługi Azure Cosmos DB
    ConnectionPool|{enableSsl: true}|Ustawienie puli połączeń protokołu SSL
    Serializer|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Ustaw na tę wartość

3. W terminalu uruchom polecenie *bin/gremlin.bat* lub *bin/gremlin.sh*, aby uruchomić [konsolę Gremlin](http://tinkerpop.apache.org/docs/3.2.4/tutorials/getting-started/).
4. W terminalu uruchom polecenie *:remote connect tinkerpop.server conf/remote-secure.yaml*, aby nawiązać połączenie z usługą aplikacji.

Wspaniale! Teraz, po zakończeniu konfigurowania, zacznijmy uruchamianie poleceń konsoli.

## <a name="create-vertices-and-edges"></a>Tworzenie wierzchołków i krawędzi

Zacznijmy od dodania wierzchołków dla czterech osób: *Thomas*, *Mary Kay*, *Robin* i *Ben*.

Dane wejściowe (Thomas):

```
:> g.addV('person').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Dane wyjściowe:

```
==>[id:1eb91f79-94d7-4fd4-b026-18f707952f21,label:person,type:vertex,properties:[firstName:[[id:ec5fcfbe-040e-48c3-b961-31233c8b1801,value:Thomas]],lastName:[[id:86e5b580-0bca-4bc2-bc53-a46f92c1a182,value:Andersen]],age:[[id:2caeab3c-c66d-4098-b673-40a8101bb72a,value:44]]]]
```
Dane wejściowe (Mary Kay):

```
:> g.addV('person').property('firstName', 'Mary Kay').property('lastName', 'Andersen').property('age', 39)
```

Dane wyjściowe:

```
==>[id:899a9d37-6701-48fc-b0a1-90950be7e0f4,label:person,type:vertex,properties:[firstName:[[id:c79c5599-8646-47d1-9a49-3456200518ce,value:Mary Kay]],lastName:[[id:c1362095-9dcc-479d-ab21-86c1b6d4ffc1,value:Andersen]],age:[[id:0b530408-bfae-4e8f-98ad-c160cd6e6a8f,value:39]]]]
```

Dane wejściowe (Robin):

```
:> g.addV('person').property('firstName', 'Robin').property('lastName', 'Wakefield')
```

Dane wyjściowe:

```
==>[id:953aefd9-5a54-4033-9b3a-d4dc3049f720,label:person,type:vertex,properties:[firstName:[[id:bbda02e0-8a96-4ca1-943e-621acbb26824,value:Robin]],lastName:[[id:f0291ad3-05a3-40ec-aabb-6538a7c331e3,value:Wakefield]]]]
```

Dane wejściowe (Ben):

```
:> g.addV('person').property('firstName', 'Ben').property('lastName', 'Miller')
```

Dane wyjściowe:

```
==>[id:81c891d9-beca-4c87-9009-13a826c9ed9a,label:person,type:vertex,properties:[firstName:[[id:3a3b53d3-888c-46da-bb54-1c42194b1e18,value:Ben]],lastName:[[id:48c6dd50-79c4-4585-ab71-3bf998061958,value:Miller]]]]
```

Następnie dodajmy krawędzie dla relacji między tymi osobami.

Dane wejściowe (Thomas -> Mary Kay):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Mary Kay'))
```

Dane wyjściowe:

```
==>[id:c12bf9fb-96a1-4cb7-a3f8-431e196e702f,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:0d1fa428-780c-49a5-bd3a-a68d96391d5c,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Dane wejściowe (Thomas -> Robin):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Robin'))
```

Dane wyjściowe:

```
==>[id:58319bdd-1d3e-4f17-a106-0ddf18719d15,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:3e324073-ccfc-4ae1-8675-d450858ca116,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Dane wejściowe (Robin -> Ben):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Ben'))
```

Dane wyjściowe:

```
==>[id:889c4d3c-549e-4d35-bc21-a3d1bfa11e00,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:40fd641d-546e-412a-abcc-58fe53891aab,outV:3e324073-ccfc-4ae1-8675-d450858ca116]
```

## <a name="update-a-vertex"></a>Aktualizowanie wierzchołka

Zaktualizujmy wierzchołek *Thomas*, podając nowy wiek: *45*.

Dane wejściowe:
```
:> g.V().hasLabel('person').has('firstName', 'Thomas').property('age', 45)
```
Dane wyjściowe:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

## <a name="query-your-graph"></a>Wykonywanie zapytania względem grafu

Teraz uruchommy różne zapytania względem grafu.

Najpierw wypróbujmy zapytanie z filtrem, aby zwrócić tylko te osoby, które mają więcej niż 40 lat.

Dane wejściowe (zapytanie filtru):

```
:> g.V().hasLabel('person').has('age', gt(40))
```

Dane wyjściowe:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

Następnie wyświetlmy imiona osób, które mają więcej niż 40 lat.

Dane wejściowe (zapytanie filtru i projekcji):

```
:> g.V().hasLabel('person').has('age', gt(40)).values('firstName')
```

Dane wyjściowe:

```
==>Thomas
```

## <a name="traverse-your-graph"></a>Przechodzenie grafu

Przejdźmy graf w celu zwrócenia wszystkich znajomych Thomasa.

Dane wejściowe (znajomi Thomasa):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person')
```

Dane wyjściowe: 

```
==>[id:f04bc00b-cb56-46c4-a3bb-a5870c42f7ff,label:person,type:vertex,properties:[firstName:[[id:14feedec-b070-444e-b544-62be15c7167c,value:Mary Kay]],lastName:[[id:107ab421-7208-45d4-b969-bbc54481992a,value:Andersen]],age:[[id:4b08d6e4-58f5-45df-8e69-6b790b692e0a,value:39]]]]
==>[id:91605c63-4988-4b60-9a30-5144719ae326,label:person,type:vertex,properties:[firstName:[[id:f760e0e6-652a-481a-92b0-1767d9bf372e,value:Robin]],lastName:[[id:352a4caa-bad6-47e3-a7dc-90ff342cf870,value:Wakefield]]]]
```

Następnie uzyskajmy kolejną warstwę wierzchołków. Przejdźmy graf w celu zwrócenia wszystkich osób będących znajomymi znajomych Thomasa.

Dane wejściowe (znajomi znajomych Thomasa):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```
Dane wyjściowe:

```
==>[id:a801a0cb-ee85-44ee-a502-271685ef212e,label:person,type:vertex,properties:[firstName:[[id:b9489902-d29a-4673-8c09-c2b3fe7f8b94,value:Ben]],lastName:[[id:e084f933-9a4b-4dbc-8273-f0171265cf1d,value:Miller]]]]
```

## <a name="drop-a-vertex"></a>Usuwanie wierzchołka

Usuńmy teraz wierzchołek z bazy danych grafu.

Dane wejściowe (usunięcie wierzchołka Robin):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').drop()
```

## <a name="clear-your-graph"></a>Czyszczenie grafu

Na koniec usuńmy z bazy danych wszystkie wierzchołki i krawędzie.

Dane wejściowe:

```
:> g.V().drop()
```

Gratulacje! Pomyślnie ukończono samouczek interfejsu API programu Graph w usłudze Azure Cosmos DB!

## <a name="review-slas-in-the-azure-portal"></a>Przeglądanie umów SLA w witrynie Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie zamierzasz w przyszłości korzystać z tej aplikacji, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start:  

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę utworzonego zasobu. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę zasobu do usunięcia, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wyjaśniono, jak utworzyć konto usługi Azure Cosmos DB, jak utworzyć graf za pomocą Eksploratora danych oraz jak utworzyć wierzchołki i krawędzie, a także jak przejść graf za pomocą konsoli Gremlin. Teraz możesz tworzyć bardziej złożone zapytania i implementować zaawansowaną logikę przechodzenia grafu za pomocą języka Gremlin. 

> [!div class="nextstepaction"]
> [Wykonywanie zapytań przy użyciu języka Gremlin](tutorial-query-graph.md)

