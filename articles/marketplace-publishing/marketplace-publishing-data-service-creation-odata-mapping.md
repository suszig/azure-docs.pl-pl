---
title: "Przewodnik dotyczący tworzenia usługi danych dla witryny Marketplace | Dokumentacja firmy Microsoft"
description: "Szczegółowe instrukcje dotyczące sposobu tworzenia, certyfikować i wdrażania usługi danych dla kupić w witrynie Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: a853b4dbd1952ba4ea8ee68ea3ca98f588bb71a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Mapowanie istniejącej usługi sieci web OData przy użyciu pliku CSDL
> [!IMPORTANT]
> **W tym momencie firma Microsoft nie są już dołączania żadnych nowych wydawców danych usługi. Nowe dataservices nie zostanie zatwierdzenia dla listy.** Jeśli masz aplikacji biznesowych SaaS chcesz publikować w AppSource więcej informacji można znaleźć [tutaj](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub więcej informacji można znaleźć usługi developer chcesz publikować w witrynie Azure Marketplace [tutaj](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Ten artykuł zawiera omówienie dotyczące sposobu używania CSDL do mapowania istniejącej usługi zgodne usługi OData. Wyjaśniono, jak można utworzyć mapowania dokumentu (CSDL), który przekształca wejściowych żądanie od klienta za pośrednictwem wywołania usługi i danych wyjściowych (danych) z powrotem do klienta za pośrednictwem OData zgodne źródła danych. Microsoft Azure Marketplace udostępnia usługi, aby użytkownicy końcowi przy użyciu protokołu OData. Usługi, które są udostępniane przez dostawców zawartości (właścicielom danych) są widoczne w różnych formularzy, takich jak REST protokołu SOAP, itp.

## <a name="what-is-a-csdl-and-its-structure"></a>Co to jest plik CSDL, a jego struktury?
CSDL (koncepcyjnej Schema Definition Language) jest specyfikacją definiowanie sposobu opisu usługi sieci web lub usługi baza danych w wspólne wyrażenia XML w portalu Azure Marketplace.

Proste omówienie **żądania przepływu:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Przepływ danych** znajduje się w odwrotnym kierunku:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Rysunek 1** diagramy jak klient może uzyskać danych z dostawcy zawartości (usługi) za pośrednictwem portalu Azure Marketplace.  CSDL jest używany przez składnik mapowania/przekształcenia obsłużyć żądanie i przekazywania danych między zawartości dostawcy usług i klienta.

*Rysunek 1: Szczegółowe przepływu z klienta do dostawcy zawartości za pośrednictwem portalu Azure Marketplace*

  ![Rysowanie](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

W tle na Atom, protokołu Atom i protokołu OData, w którym kompilacji rozszerzenia portalu Azure Marketplace, przejrzyj: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Fragment powyższego łącza: *"celem protokołu Open Data (zwanych OData) jest zapewnienie protokołu opartego na interfejsie REST dla operacji CRUD stylu (tworzenia, odczytu, aktualizacji i usuwania) względem zasobów udostępniony jako usługi danych. "Usługa danych" jest punktem końcowym w przypadku, gdy dane widoczne z co najmniej jeden "kolekcji" każdego z zero lub więcej "pozycje", które składają się z pary wartości o nazwie wpisany. OData jest opublikowane przez firmę Microsoft w obszarze standardów języka OASIS (organizacji w celu przejścia z strukturalnych informacji normy) tak, aby każdy użytkownik, który chce utworzyć serwerów, klientów lub narzędzia bez ograniczeń lub opłat licencyjnych. "*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Są trzy najistotniejsze, które muszą być zdefiniowane w pliku CSDL:
* **Punktu końcowego** z usługi dostawcy sieci Web adres URI usługi
* **Parametry danych** przekazywany jako dane wejściowe do dostawcy usług z definicjami parametrów są wysyłane do dostawcy zawartości usługi do typu danych.
* **Schemat** danych zwracanych do usługi żądanie schemat danych są dostarczane przez usługę dostawcy zawartości, w tym kontenerze, kolekcje/tabel, kolumn/zmiennych i typy danych.

Na poniższym diagramie przedstawiono omówienie przepływu, z którym klient wprowadza instrukcję OData (wywołanie usługi sieci web dostawcy zawartości) aby odzyskać dane/wyników.

  ![Rysowanie](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>kroki:
1. Klient wysyła żądanie za pośrednictwem wywołania usługi z parametry wejściowe zdefiniowane w pliku XML w portalu Azure Marketplace
2. Plik CSDL jest używany w celu zweryfikowania wywołania usługi.
   * Sformatowany zgłoszenia serwisowego są następnie wysyłane do usługi dostawcy zawartości w portalu Azure Marketplace
3. Usługi sieci Web wykonuje i preforms akcji zlecenie Http (tj. GET) dane są zwracane do portalu Azure Marketplace, gdzie żądanych danych (jeżeli istniał) jest ujawnia w formacie XML do klienta przy użyciu mapowania zdefiniowane w pliku CSDL.
4. Klient wysłaniu danych (jeżeli istniał) w formacie XML lub JSON

## <a name="definitions"></a>Definicje
### <a name="odata-atom-pub"></a>Protokołu OData ATOM
Ustaw rozszerzenie protokołu ATOM, gdzie każdy wpis reprezentuje jeden wiersz wyniku. Element zawartości wpisu została rozszerzona o zawierają wartości wiersza — jako pary wartości klucza. Więcej informacji znajduje się w tym miejscu: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>Plik CSDL - Conceptual Schema Definition Language
Umożliwia zdefiniowanie funkcji (SPROCs) i jednostek, które są dostępne za pośrednictwem bazy danych. Więcej informacji znaleźć tutaj: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [!TIP]
> Kliknij przycisk **inne wersje** listy rozwijanej i wybierz wersję, jeśli nie widzisz tego artykułu.
> 
> 

### <a name="edm---entry-data-model"></a>EDM - wpis modelu danych
* Omówienie: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* Podgląd: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* Typy danych: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

Poniżej przedstawiono szczegółowe od lewej do prawej przepływać z którym klient wprowadza instrukcję OData (wywołanie usługi sieci web dostawcy zawartości) do pobierania wyników/danych kopii:

  ![Rysowanie](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a>Podstawowe informacje o pliku CSDL
CSDL (koncepcyjnej Schema Definition Language) jest specyfikacją definiowanie sposobu opisu usługi sieci web lub usługi baza danych w wspólne wyrażenia XML w portalu Azure Marketplace. Plik CSDL zawiera opis krytycznej kawałków, który **umożliwia przekazywanie danych ze źródła danych w portalu Azure Marketplace.** Elementy główne są opisane w tym miejscu:

* Informacje o interfejsie opisem wszystkich funkcji dostępnych publicznie (FunctionImport węzeł)
* Informacje o typie danych dla wszystkich wiadomości requests(input) i responses(outputs) komunikatu (EntityContainer/EntitySet/EntityType węzłów)
* Powiązanie informacji na temat protokołu transportu być używane (węzeł nagłówka)
* Adres do lokalizacji określonej usługi (atrybut BaseURI)

Mówiąc CSDL reprezentuje kontraktu platformy - i niezależny od języka między żądający usługi i dostawcy usług. Przy użyciu pliku CSDL, klient może zlokalizować usługi bazy danych i usługi sieci web i wywołać żadnego ze swoich publicznie dostępnych funkcji.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Dotyczących pliku CSDL z bazą danych lub kolekcji
**Specyfikacja pliku CSDL**

Plik CSDL jest opis gramatyki języka XML opisu usługi sieci web. Specyfikacja sam jest podzielony na 4 główne elementy: EntitySet, FunctionImport; Przestrzeń nazw, a dla obiektu.

Aby ułatwić zrozumienie umożliwia tę warstwę abstrakcji dotyczą CSDL tabeli.

Pamiętaj;

  Plik CSDL reprezentuje kontraktu platformy - i niezależny od języka między **obiektu żądającego usługi** i **usługodawcy**. Przy użyciu pliku CSDL, **klienta** mogą zlokalizować **usługi bazy danych i usługi sieci web** i wywołać żadnego ze swoich publicznie dostępnych **funkcji.**

Usługi danych czterech części pliku CSDL można traktować pod względem bazy danych, tabeli, kolumny i procedura składowana.

Następujący dla usługi danych dotyczących:

* Obiekt EntityContainer ~ = bazy danych
* Obiekt EntitySet ~ = tabeli
* Obiekt EntityType ~ = kolumn
* Element FunctionImport ~ = procedury składowanej

**Dozwolone zlecenia HTTP**

* GET — zwraca wartości z bazy danych (zwraca kolekcję)
* POST — służy do przekazywania danych i opcjonalne wartości zwracanych z bazy danych (Utwórz nowy wpis w kolekcji, zwracany identyfikator/URI)
* Usuń — Usuwa dane z bazy danych (spowoduje usunięcie kolekcji)
* PUT — aktualizacji danych w bazie danych (Zastąp kolekcji lub utwórz taki)

## <a name="metadatamapping-document"></a>Dokument metadanych/mapowania
Dokument metadanych/mapowania jest używany do mapowania istniejących usług sieci web dostawcy zawartości, dzięki czemu może być udostępniany jako usługi sieci web OData w systemie Azure Marketplace. Jest on oparty na pliku CSDL i implementuje kilka rozszerzeń pliku CSDL w celu zaspokojenia potrzeb REST na podstawie usług sieci web za pośrednictwem portalu Azure Marketplace. Rozszerzenia znajdują się w [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) przestrzeni nazw.

Przykład pliku CSDL: (kopiowania i wklejania poniżej przykładowy plik CSDL w edytorze XML i zmiany zgodne z usługą.  Wklej do mapowania pliku CSDL karcie DataService podczas tworzenia usługi w [publikowania portalu Marketplace Azure](https://publish.windowsazure.com)).

**Warunki:** postanowień dotyczących pliku CSDL do [Portal publikowania](https://publish.windowsazure.com) warunki interfejsu użytkownika (PPUI).

* Oferują "Title" w PPUI odnosi się do MyWebOffer
* Moja firma w PPUI odnosi się do **wydawcy, nazwa wyświetlana** w [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) interfejsu użytkownika
* Interfejs API odnosi się do sieci Web lub usługi danych (Plan w PPUI)

**Hierarchia:** oferty, która ma plany właścicielem firmy (dostawcy zawartości), czyli service(s), który wiersz w górę z interfejsem API.

### <a name="webservice-csdl-example"></a>Przykład CSDL usługi sieci Web
Łączy się z usługą, która jest ujawniany przez punkt końcowy aplikacji sieci web (np. aplikacji C#)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> Wyświetl więcej przykładów CSDL usługi sieci Web w artykule [przykłady mapowania istniejące usługi OData za pomocą CSDLs sieci web](marketplace-publishing-data-service-creation-odata-mapping-examples.md)
> 
> 

### <a name="dataservice-csdl-example"></a>Przykład CSDL usługi danych
Nawiązuje połączenie z usługą Udostępnianie tabelę lub widok jako punkt końcowy w poniższym przykładzie przedstawiono dwa interfejsy API dla bazy danych na podstawie pliku CSDL interfejsu API (możesz użyć widoków zamiast tabel).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Zobacz też
* Jeśli interesuje Cię uczenia i zrozumienie określonych węzłów i ich parametry, przeczytaj ten artykuł [węzły mapowanie danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definicje i wyjaśnienia, przykłady i kontekstu przypadków użycia.
* Jeśli jesteś zrecenzować przykłady, przeczytaj ten artykuł [przykłady mapowanie danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-examples.md) aby zobaczyć przykładowy kod i zrozumieć Składnia kodu i kontekstu.
* Aby powrócić do określonej ścieżki do publikowania danych usługi Azure Marketplace, przeczytaj ten artykuł [Podręcznik publikowania danych usługi](marketplace-publishing-data-service-creation.md).

