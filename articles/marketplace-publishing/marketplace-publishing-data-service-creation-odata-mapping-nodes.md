---
title: "Przewodnik dotyczący tworzenia usługi danych dla witryny Marketplace | Dokumentacja firmy Microsoft"
description: "Szczegółowe instrukcje dotyczące sposobu tworzenia, certyfikować i wdrażania usługi danych dla kupić w witrynie Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 107baab2-5828-4158-abdf-59a580c80d37
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 8ff76ea21ba684ae2a2afcb74d66b4912d7be053
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Opis schematu węzłów do mapowania istniejącej usługi sieci web OData przy użyciu pliku CSDL
> [!IMPORTANT]
> **W tym momencie firma Microsoft nie są już dołączania żadnych nowych wydawców danych usługi. Nowe dataservices nie zostanie zatwierdzenia dla listy.** Jeśli masz aplikacji biznesowych SaaS chcesz publikować w AppSource więcej informacji można znaleźć [tutaj](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub więcej informacji można znaleźć usługi developer chcesz publikować w witrynie Azure Marketplace [tutaj](https://azure.microsoft.com/marketplace/programs/certified/).
>
>

Ten dokument pomaga wyjaśnienie struktury węzła do mapowania pliku CSDL protokołu OData. Należy pamiętać, że struktura węzła jest poprawnie sformułowany kod XML. Dlatego schematu głównego, nadrzędne i podrzędne stosuje się podczas projektowania mapowanie OData.

## <a name="ignored-elements"></a>Elementy została zignorowana
Poniżej przedstawiono wysokiego poziomu elementów CSDL (węzłów XML), które nie będą używane przez zaplecza Azure Marketplace podczas importowania metadanych usługi sieci web. Mogą być obecne, ale zostaną zignorowane.

| Element | Zakres |
| --- | --- |
| Za pomocą elementu |Węzeł, podrzędne węzły i wszystkie atrybuty |
| Documentation Element |Węzeł, podrzędne węzły i wszystkie atrybuty |
| Element ComplexType |Węzeł, podrzędne węzły i wszystkie atrybuty |
| Element skojarzenia |Węzeł, podrzędne węzły i wszystkie atrybuty |
| Rozszerzone właściwości |Węzeł, podrzędne węzły i wszystkie atrybuty |
| Obiekt EntityContainer |Następujące atrybuty są ignorowane: *rozszerza* i *AssociationSet* |
| Schemat |Następujące atrybuty są ignorowane: *Namespace* |
| Element FunctionImport |Następujące atrybuty są ignorowane: *tryb* (przyjęto wartość domyślną ln) |
| Dla obiektu |Tylko następujące węzły podrzędne są ignorowane: *klucza* i *PropertyRef* |

Poniżej opisano zmiany (dodany i zignorowane elementy) w różnych węzłach CSDL XML szczegółów.

## <a name="functionimport-node"></a>Element FunctionImport węzła
Węzeł FunctionImport reprezentuje jeden adres URL (punkt wejścia), który udostępnia usługi, aby użytkownik końcowy. Węzeł umożliwia opisujące, jak adres URL zostanie rozwiązana, parametry, które są dostępne dla użytkownika końcowego i jak te parametry są udostępniane.

Szczegółowe informacje o tym węźle znajdują się w [tutaj][MSDNFunctionImportLink](https://msdn.microsoft.com/library/cc716710.aspx)

Poniżej przedstawiono dodatkowe atrybuty (lub ma zostać dodany do atrybutów) który są udostępniane przez węzeł elementu FunctionImport:

**d:BaseUri** — szablon identyfikatora URI zasobu REST, która jest widoczna w portalu Marketplace. Marketplace używa tego szablonu do tworzenia zapytań dotyczących usługi sieci web REST. Szablon identyfikatora URI zawiera symbole zastępcze dla parametrów w formie {parameterName}, gdzie nazwa parametru jest nazwa parametru. Przykład. apiVersion = {apiVersion}.
Parametry nie mogą znajdować się jako parametry identyfikatora URI lub części ścieżki identyfikatora URI. W przypadku pojawienia się w ścieżce zawsze są obowiązkowe (nie można oznaczyć jako wartości null). *Przykład:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Nazwa** — nazwa funkcji zaimportowany.  Nie może być taka sama jak innych nazw zdefiniowanych w pliku CSDL.  Przykład. Name = "GetModelUsageFile"

**Obiekt EntitySet** *(opcjonalnie)* — Jeśli funkcja zwraca kolekcję typów jednostek, wartość **EntitySet** musi być zestawu jednostek do której należy kolekcji. W przeciwnym razie **EntitySet** atrybutu nie mogą być używane. *Przykład:*`EntitySet="GetUsageStatisticsEntitySet"`

**Obiekt ReturnType** *(opcjonalnie)* — Określa typ elementów zwróconych przez identyfikator URI.  Nie należy używać tego atrybutu, jeśli funkcja nie zwraca wartości. Poniżej przedstawiono obsługiwane typy:

* **Kolekcja (<Entity type name>)**: Określa kolekcję typów zdefiniowanych jednostek. Nazwa jest obecny w atrybucie nazwy węzła dla obiektu. Przykładem jest kolekcją (WXC. HourlyResult).
* **Nieprzetworzona (<mime type>)**: Określa raw dokumentu/blob, który jest zwracany do użytkownika. Przykładem jest Raw(image/jpeg) inne przykłady:

  * ReturnType="Raw(text/plain)"
  * Obiekt ReturnType = "kolekcji (sage. DeleteAllUsageFilesEntity) "*

**d:Paging** — określa sposób obsługi stronicowania przez zasób REST. Wartości parametrów, które są używane w obrębie nawiasów gałęzi produkcji, np. strony = {$page} & wartość elementu itemsperpage = {$size} dostępne opcje to:

* **Brak:** stronicowania nie jest dostępna
* **Pomiń:** stronicowania to wyrazić za pomocą logicznych "Pomiń" i "zajmuje" (u góry). Pomiń tę łódź jeża M elementów i podejmij zwraca dalej elementy N. Wartość parametru: $skip
* **Podejmij:** podjęcia zwraca dalej elementy N. Wartość parametru: $take
* **Wartość PageSize:** stronicowania to wyrazić za pomocą strony logicznej i rozmiaru (elementów na stronie). Strona reprezentuje bieżącej strony, która jest zwracana. Wartość parametru: $page
* **Rozmiar:** rozmiar reprezentuje liczbę elementów zwróconych dla każdej strony. Wartość parametru: $size

**d:AllowedHttpMethods** *(opcjonalnie)* — Określa, które zlecenie jest obsługiwany przez zasobu REST. Ponadto ogranicza akceptowane zlecenie z podaną wartością.  Domyślne = POST.  *Przykład:* `d:AllowedHttpMethods="GET"` dostępne opcje to:

* **GET:** zwykle używane do zwracania danych
* **POST:** zazwyczaj używany do wstawiania nowych danych
* **Umieść:** zwykle używane do aktualizowania danych
* **Usuń:** służy do usuwania danych

Węzły podrzędne dodatkowe (nie pasuje do żadnego z dokumentacją CSDL) w węźle FunctionImport są:

**d:RequestBody** *(opcjonalnie)* -treść żądania jest używany do wskazują, że żądanie oczekuje treści do wysłania. Parametry mogą być podawane w treści żądania. Są w nawiasach klamrowych, np. wyrażone {parameterName}. Te parametry są mapowane na podstawie danych wejściowych użytkownika do treści, która jest przekazywane do dostawcy zawartości usługi. RequestBody element ma atrybut o nazwie httpMethod. Ten atrybut umożliwia dwóch wartości:

* **POST:** używane, jeśli żądanie jest żądaniem POST protokołu HTTP
* **GET:** używane w przypadku żądania HTTP GET

    Przykład:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** i **d:Namespace** — ten węzeł zawiera opis przestrzenie nazw, które są zdefiniowane w pliku XML, który jest zwracany przez import funkcji (identyfikator URI punktu końcowego). Kod XML, który jest zwracany przez usługi zaplecza może zawierać dowolną liczbę nazw do odróżnienia zawartość, która jest zwracana. **Wszystkie te obszary nazw, jeśli używane w zapytaniach XPath d:Map lub d:Match musi występować.** Węzeł d:Namespaces zawiera zestaw/listy węzłów d:Namespace. Każde z nich wymieniono jednej przestrzeni nazw używany w odpowiedzi usługi wewnętrznej bazy danych. Atrybut węzła d:Namespace są następujące:

* **d:prefix:** prefiksu dla przestrzeni nazw, jak pokazano w wynikach XML zwrócony przez usługę, np. f:FirstName, f:LastName, gdzie f jest prefiks.
* **d:URI:** pełny identyfikator URI przestrzeni nazw używany w dokumencie wynik. Reprezentuje wartość, która odpowiada prefiks w czasie wykonywania.

**d:ErrorHandling** *(opcjonalnie)* — ten węzeł zawiera warunki do obsługi błędów. Każdego ze stanów jest weryfikowany pod kątem wynik zwracany przez usługę dostawcy zawartości. Jeśli warunek zgodny proponowanych kod błędu HTTP do użytkownika końcowego jest zwracany komunikat o błędzie.

**d:ErrorHandling** *(opcjonalnie)* i **d:Condition** *(opcjonalnie)* -węzła warunku zawiera jeden warunek, który jest sprawdzany w wynik zwracany przez Usługa dostawcy zawartości. Poniżej przedstawiono **wymagane** atrybuty:

* **d:match:** wyrażenie XPath, które sprawdza, czy dany węzeł/wartości jest obecny w dostawcy zawartości wyjściowy XML. Wyrażenie XPath jest wykonywane na dane wyjściowe i powinien zwrócić wartość true, jeśli wynikiem warunku jest dopasowania lub wartość false w przeciwnym razie wartość.
* **d:HttpStatusCode:** odpowiada kod stanu HTTP, który ma zostać zwrócony przez Marketplace w przypadku warunku. Marketplace signalizes błędy użytkownika za pomocą kodów stanu HTTP. Listę kodów stanu HTTP są dostępne pod adresem http://en.wikipedia.org/wiki/HTTP_status_code
* **d:ErrorMessage:** komunikat o błędzie zwracany — z kodem stanu HTTP — do użytkownika końcowego. Powinno to być przyjazne komunikat, który nie zawiera żadnych kluczy tajnych.

**d:TITLE** *(opcjonalnie)* — umożliwia opisujące nazwy funkcji. Wartość tytułu pochodzi z

* Atrybut opcjonalny mapy (xpath), który określa, gdzie można znaleźć tytuł w odpowiedź zwrócona z żądania obsługi.
* - Lub - title określony jako wartość węzła.

**d:Rights** *(opcjonalnie)* — prawa (np. o prawach autorskich) skojarzone z funkcją. Wartość prawa pochodzi od:

* Atrybut opcjonalny mapy (xpath), który określa, gdzie można znaleźć prawa odpowiedź zwrócona z żądania obsługi.
* - Lub - prawa określone jako wartość węzła.

**d:Description** *(opcjonalnie)* — krótki opis dla tej funkcji. Wartość Opis pochodzi z

* Atrybut opcjonalny mapy (xpath), który określa, gdzie można znaleźć opisu w odpowiedź zwrócona z żądania obsługi.
* - Lub -opis określony jako wartość węzła.

**d:EmitSelfLink** - *można znaleźć w powyższym przykładzie "Element FunctionImport"stronicowanie, za pośrednictwem zwrócone dane"*

**d:EncodeParameterValue** -opcjonalne rozszerzenie OData

**d:QueryResourceCost** -opcjonalne rozszerzenie OData

**d:map** -opcjonalne rozszerzenie OData

**d:headers** -opcjonalne rozszerzenie OData

**d:headers** -opcjonalne rozszerzenie OData

**d:Value** -opcjonalne rozszerzenie OData

**d:HttpStatusCode** -opcjonalne rozszerzenie OData

**d:ErrorMessage** -opcjonalne rozszerzenie OData

## <a name="parameter-node"></a>Parametr węzła
Ten węzeł reprezentuje jeden parametr, który jest udostępniany jako część szablon identyfikatora URI / żądań treści, która została określona w węźle elementu FunctionImport.

Znaleziono na stronie dokumentu bardzo przydatne szczegóły dotyczące węzła "Parameter Element" [tutaj](http://msdn.microsoft.com/library/ee473431.aspx) (Użyj **innych wersji** listy rozwijanej, aby wybrać inną wersję, jeśli to konieczne wyświetlić dokumentację). *Przykład:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Atrybut parametru | Jest wymagana | Wartość |
| --- | --- | --- |
| Nazwa |Tak |Nazwa parametru. Uwzględniana wielkość liter!  Wielkość liter BaseUri. **Przykład:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ |Tak |Typ parametru. Wartość musi być **EDMSimpleType** lub typ złożony, który znajduje się w zakresie modelu. Aby uzyskać więcej informacji zobacz "6 właściwość parametru obsługiwane typy".  (Wielkość liter! Pierwszy znak jest wielką, rest są małe litery).  Zobacz też, [koncepcyjny modelu typów (CSDL)][MSDNParameterLink](http://msdn.microsoft.com/library/bb399548.aspx). **Przykład:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Tryb |Nie |**W**, Out lub InOut, w zależności od tego, czy parametr jest danych wejściowych, wyjściowych lub parametr wejścia/wyjścia. (Tylko "W" są dostępne w portalu Azure Marketplace.) **Przykład:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| Element maxLength |Nie |Maksymalna dozwolona długość parametru. **Przykład:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| dokładność |Nie |Dokładność parametru. **Przykład:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Skalowanie |Nie |Skala parametru. **Przykład:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

Atrybuty, które zostały dodane do specyfikacji pliku CSDL są następujące:

| Atrybut parametru | Opis |
| --- | --- |
| **d:regex** *(opcjonalnie)* |Instrukcja wyrażeń regularnych, używany do sprawdzania poprawności wartość wejściowa dla parametru. Jeśli wartość wejściowa nie pasuje do instrukcji wartość zostanie odrzucone. Dzięki temu można także określić zestaw możliwych wartości, np. ^ [0-9] +? $ w celu zezwolenia tylko cyfry. **Przykład:** ' < Nazwa parametru = "name" tryb = "In" Type = "String" d: wartości null = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = "A nazwę, która nie może zawierać spacji ani znaków innych niż angielskie innych niż alfanumeryczne" d:SampleValues = "George |
| **d:Enum** *(opcjonalnie)* |Potok przecinkami lista wartości jest nieprawidłowa dla parametru. Typ wartości musi zgodna ze zdefiniowanym typem parametru. Przykład: "angielski |
| **d: Nullable** *(opcjonalnie)* |Umożliwia zdefiniowanie, czy parametr może mieć wartości null. Wartość domyślna to: true. Jednak parametry, które są dostępne jako część ścieżki w szablonie identyfikatora URI nie może mieć wartości null. Jeśli ten atrybut ma wartość false dla tych parametrów — dane wejściowe użytkownika zostanie zastąpiona. **Przykład:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(opcjonalnie)* |Przykładowa wartość do wyświetlenia jako Uwaga do klienta w interfejsie użytkownika.  Można dodać kilka wartości przy użyciu listy potoku oddzielone, tj. " |

## <a name="entitytype-node"></a>Dla obiektu węzła
Ten węzeł reprezentuje jeden z typów, które są zwracane z witryny Marketplace dla użytkownika końcowego. Zawiera ona także mapowania z danych wyjściowych, która jest zwracana przez usługę dostawcy zawartości do wartości, które są zwracane do użytkownika końcowego.

Szczegółowe informacje o tym węźle znajdują się w [tutaj](http://msdn.microsoft.com/library/bb399206.aspx) (Użyj **innych wersji** listy rozwijanej, aby wybrać inną wersję, jeśli to konieczne wyświetlić dokumentację.)

| Nazwa atrybutu | Jest wymagana | Wartość |
| --- | --- | --- |
| Nazwa |Tak |Nazwa typu jednostki. **Przykład:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| Typ BaseType |Nie |Nazwa innego typu jednostki, który jest typem podstawowym typu jednostki, który jest definiowany. **Przykład:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Atrybuty, które zostały dodane do specyfikacji pliku CSDL są następujące:

**d:map** — wyrażenie XPath wykonywane względem danych wyjściowych usługi. W tym miejscu zakłada się, czy dane wyjściowe usługi zawiera zestaw elementów, które powtórzyć, tak, gdy źródło danych ATOM istnieje zestaw określonych węzłów zapisu Powtarzaj. Każdy z tych powtarzające się węzły zawiera jeden rekord. Następnie określono wyrażenia XPath do punktu w poszczególnych węźle powtarzających się w wynikach usługi dostawcy zawartości, który zawiera wartości dla pojedynczego rekordu. Przykład danych wyjściowych z usługi:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

Wyrażenie XPath może być /foo/pasek, ponieważ każdy paska węzła jest węźle powtarzających się w danych wyjściowych i zawiera rzeczywistej zawartości, która jest zwracana do użytkownika końcowego.

**Klucz** -tego atrybutu jest ignorowana przez Marketplace. REST na podstawie usług sieci web, ogólnie rzecz biorąc, nie uwidacznia klucza podstawowego.

## <a name="property-node"></a>Właściwości węzła
Ten węzeł zawiera jedną właściwość rekordu.

Szczegółowe informacje o tym węźle znajdują się w [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (Użyj **innych wersji** listy rozwijanej, aby wybrać inną wersję, jeśli to konieczne wyświetlić dokumentację.) *Przykład:*`<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"     Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Wymagane | Wartość |
| --- | --- | --- |
| Nazwa |Tak |Nazwa właściwości. |
| Typ |Tak |Typ wartości właściwości. Typ wartości właściwości musi być **EDMSimpleType** lub typ złożony (wskazywanym przez w pełni kwalifikowaną nazwą), który znajduje się w zakresie modelu. Aby uzyskać więcej informacji Zobacz typy modelu koncepcyjnego (CSDL). |
| Dopuszczające wartości zerowe |Nie |**Wartość true,** (wartość domyślna) lub **False** w zależności od tego, czy właściwość może mieć wartości null. Uwaga: W wersji CSDL wskazywanym przez [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) przestrzeni nazw, właściwość typu złożonego musi mieć Nullable = "False". |
| Wartość domyślna |Nie |Wartość domyślna właściwości. |
| Element maxLength |Nie |Maksymalna długość wartości właściwości. |
| FixedLength |Nie |**Wartość true,** lub **False** w zależności od tego, czy wartość właściwości będą przechowywane jako ciąg o długości fiexed. |
| dokładność |Nie |Oznacza maksymalną liczbę cyfr do przechowania w wartość liczbową. |
| Skalowanie |Nie |Maksymalna liczba miejsc dziesiętnych do przechowania w wartość liczbową. |
| Unicode |Nie |**Wartość true,** lub **False** w zależności od tego, czy wartość właściwości być przechowywane w postaci ciągu Unicode. |
| Sortowanie |Nie |Ciąg, który określa kolejność sortowania do użycia w źródle danych. |
| Właściwość ConcurrencyMode |Nie |**Brak** (wartość domyślna) lub **stałe**. Jeśli wartość jest równa **stałe**, zostanie użyta wartość właściwości w sprawdzenie optymistycznej współbieżności. |

Poniżej przedstawiono dodatkowe atrybuty, które zostały dodane do specyfikacji pliku CSDL:

**d:map** — wyrażenie XPath wykonywane z usługą danych wyjściowych i wyodrębnia jedną właściwość danych wyjściowych. Określone wyrażenie XPath jest określana względem węźle powtarzających się zaznaczonego w węźle EntityType XPath. Użytkownik może również określić bezwzględny XPath umożliwia tym statycznych zasobów w każdym z węzłów wyjściowych, jak na przykład praw autorskich instrukcję, która jest są dostępne tylko raz w oryginalnej usługi output, ale powinien znajdować się w każdym wierszy w danych wyjściowych OData. Przykład z usługi:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Wyrażenie XPath w tym miejscu będą ./bar/baz0 uzyskać węzła baz0 z usługi dostawcy zawartości.

**d:CharMaxLength** — dla typu string, można określić maksymalną długość. Zobacz przykład CSDL usługi danych

**d:IsPrimaryKey** — wskazuje, czy kolumna klucza podstawowego w tabeli lub widoku. Zobacz przykład DataService CSDL.

**d:isExposed** — Określa, czy schemat tabeli jest widoczna (zazwyczaj true). Zobacz przykład CSDL usługi danych

**d:IsView** *(opcjonalnie)* — wartość true, jeśli jest oparta na widoku, a nie tabelę.  Zobacz przykład CSDL usługi danych

**d:Tableschema** — Zobacz przykład CSDL usługi danych

**d:ColumnName** — jest to nazwa kolumny w tabeli lub widoku.  Zobacz przykład CSDL usługi danych

**d:IsReturned** — jest wartość logiczna, która określa, czy usługa udostępnia tę wartość do klienta.  Zobacz przykład CSDL usługi danych

**d:IsQueryable** — jest wartość logiczna, która określa czy kolumna może być użyty w zapytaniu bazy danych.   Zobacz przykład CSDL usługi danych

**d:OrdinalPosition** — jest numeryczny pozycję kolumny wyglądu, x, w tabeli lub widoku, gdzie x jest z zakresu od 1 do liczby kolumn w tabeli.  Zobacz przykład CSDL usługi danych

**d:DatabaseDataType** — typ danych kolumny w bazie danych, tj. typ danych SQL. Zobacz przykład CSDL usługi danych

## <a name="supported-parametersproperty-types"></a>Typy obsługiwane parametry lub właściwości.
Poniżej przedstawiono obsługiwane typy parametrów i właściwości. (Z uwzględnieniem wielkości liter)

| Typy pierwotne | Opis |
| --- | --- |
| Wartość null |Reprezentuje braku wartości |
| Wartość logiczna |Reprezentuje matematyczne pojęcie logiki przechowywanymi w danych binarnych |
| Bajtów |Niepodpisane 8-bitową liczbę całkowitą |
| Data i godzina |Reprezentuje datę i godzinę z wartości z zakresu od północy 12:00:00, 1 stycznia, 1753 r. N.E. za pomocą 11:59:59 godzinach od, grudnia 9999 r. |
| Decimal |Reprezentuje wartości liczbowych za pomocą stałych precyzję i skalę. Tego typu można opisać liczbowa wartość z zakresu od 10 ujemna ^ 255 + 1 do 10 dodatnią ^ 255 -1 |
| O podwójnej precyzji |Reprezentuje zmiennoprzecinkowej numer z dokładnością do 15 cyfr, mogącej reprezentować wartości z przybliżonej zakresem granicach 2.23E-308 do 1.79E +308. **Użyj przecinka z powodu problemu eksportu Exel** |
| Pojedynczy |Reprezentuje zmiennoprzecinkowej numer 7 cyfr precyzji, mogącej reprezentować wartości z przybliżonej zakresem granicach 1.18e-38 za pomocą 3.40E + 38 |
| Identyfikator GUID |Reprezentuje unikatowy identyfikator (128-bitowy) 16-bajtową wartość |
| Int16 |Reprezentuje wartość całkowita 16-bitowych |
| Int32 |Reprezentuje wartość całkowita 32-bitowa |
| Int64 |Reprezentuje wartość całkowita 64-bitowa |
| Ciąg |Reprezentuje stałej - lub znak danych o zmiennej długości |

## <a name="see-also"></a>Zobacz też
* Jeśli interesuje Cię zrozumieć ogólny proces mapowania OData i cel, przeczytaj ten artykuł [danych usługi OData mapowania](marketplace-publishing-data-service-creation-odata-mapping.md) Aby przejrzeć definicje, struktur i instrukcje.
* Jeśli jesteś zrecenzować przykłady, przeczytaj ten artykuł [przykłady mapowanie danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-examples.md) aby zobaczyć przykładowy kod i zrozumieć Składnia kodu i kontekstu.
* Aby powrócić do określonej ścieżki do publikowania danych usługi Azure Marketplace, przeczytaj ten artykuł [Podręcznik publikowania danych usługi](marketplace-publishing-data-service-creation.md).
