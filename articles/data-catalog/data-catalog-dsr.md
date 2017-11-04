---
title: "Obsługiwane źródeł danych w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "W tym artykule wymieniono specyfikacje aktualnie obsługiwanych źródeł danych."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/15/2017
ms.author: maroche
ms.openlocfilehash: 1f637acffdbf31f6c69124282f50dc5830ad9f5b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a>Obsługiwanych źródeł danych w wykazie danych Azure

Metadane można opublikować za pomocą publiczny interfejs API lub przez kliknięcie — po rejestracji narzędzie, lub ręcznie wprowadzić informacje bezpośrednio do usługi Azure Data Catalog sieci web portalu. Poniższa tabela zawiera podsumowanie wszystkich źródeł danych, które są obsługiwane przez katalog, w obecnie oraz możliwości publikowania dla każdego. Liście są również narzędzia danych zewnętrznych, które każdego źródła danych można uruchomić na naszym doświadczeniu portal "Otwórz w". Druga tabela zawiera specyfikację techniczne każdej właściwości połączenia źródła danych.


## <a name="list-of-supported-data-sources"></a>Listę obsługiwanych źródeł danych

<table>
    <tr>
       <td><b>Obiekt źródła danych</b></td>
       <td><b>Interfejs API</b></td>
       <td><b>Ręczne wprowadzanie</b></td>
       <td><b>Narzędzie do rejestracji</b></td>
       <td><b>Otwórz w narzędziach</b></td>
       <td><b>Uwagi</b></td>
    </tr>
    <tr>
      <td>Katalog usługi Azure Data Lake Store</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Data Lake — magazyn plików</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Blob Storage</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Katalog magazynu Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela magazynu Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td>Katalog systemu plików HDFS</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>System plików HDFS pliku</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabelę programu hive</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok gałęzi</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela bazy danych programu Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok bazy danych programu Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Inne (ogólnego zasobów)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela magazynu danych SQL Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Narzędzia danych programu SQL Server programu Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok SQL Data Warehouse</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Narzędzia danych programu SQL Server programu Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Wymiar usług SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wskaźnika KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Miara usług SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Raport programu SQL Server Reporting Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Przeglądarka</font></td>
      <td><font size=2>Tylko serwerów w trybie macierzystym. Tryb programu SharePoint nie jest obsługiwany.</font></td>
    </tr>
    <tr>
      <td>Tabeli programu SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Narzędzia danych programu SQL Server programu Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Narzędzia danych programu SQL Server programu Excel, usłudze Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela programu Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok programu Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok SAP HANA</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela bazy danych DB2</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok bazy danych DB2</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Pliku</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Katalogu FTP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Plik FTP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Raport HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Punkt końcowy HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Plik HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zestaw jednostek OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Funkcję OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela PostgreSQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok PostgreSQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok SAP HANA</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> Obiekt SalesForce</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Listy programu SharePoint </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure DB rozwiązania Cosmos kolekcji</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Ogólny tabeli ODBC</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Ogólny widok ODBC</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela Cassandra</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikuj jako ogólnego zasobów ODBC</font></td>
    </tr>
    <tr>
      <td>Widok Cassandra</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikuj jako ogólnego zasobów ODBC</font></td>
    </tr>
    <tr>
      <td>Tabela programu Sybase</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Widok programu Sybase</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabela bazy danych MongoDB</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikuj jako ogólnego zasobów ODBC</font></td>
    </tr>
    <tr>
      <td>Widok bazy danych MongoDB</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikuj jako ogólnego zasobów ODBC</font></td>
    </tr>
</table>

Jeśli potrzebujesz pomocy technicznej dla dodatkowych źródeł, należy przesłać żądanie funkcji [forum usługi Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


## <a name="data-source-reference-specification"></a>Specyfikacja odwołanie do źródła danych
> [!NOTE]
> **Struktury DSL** kolumny w tabeli poniżej wymieniono tylko właściwości połączenia "address" w zbiorze właściwości używanych przez usługi Azure Data Catalog. Oznacza to zbiór właściwości "address" może zawierać innych właściwości połączenia źródła danych, które Azure Data Catalog będzie się powtarzać, ale nie używa.

<table>
    <tr>
       <td><b>Typ źródła</b></td>
       <td><b>Typ zasobu</b></td>
       <td><b>Typy obiektów</b></td>
       <td><b>Struktura DSL<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Kontener</td>
      <td>Data Lake</td>
      <td>
        <font size=2>Protokół: webhdfs <br>Uwierzytelnianie: {podstawowe, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Tabela</td>
      <td>Plik w katalogu</td>
      <td>
        <font size=2>Protokół: webhdfs <br>Uwierzytelnianie: {podstawowe, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Kontener</td>
      <td>Kontener</td>
      <td>
        <font size=2>Protokół: obiektach blob azure <br>Uwierzytelnianie: {azure klawisza dostępu} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontener</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabela</td>
      <td>Obiekt blob, katalogu</td>
      <td>
        <font size=2>Protokół: obiektach blob azure <br>Uwierzytelnianie: {azure klawisza dostępu} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontener <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nazwa</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Kontener</td>
      <td>Kontener</td>
      <td>
        <font size=2>Protokół: azure — tabele <br>Uwierzytelnianie: {azure klawisza dostępu} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabela</td>
      <td>Tabela</td>
      <td>
        <font size=2>Protokół: azure — tabele <br>Uwierzytelnianie: {azure klawisza dostępu} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nazwa</font>
      </td>
    </tr>
    <tr>
      <td>Rozwiązania cosmos</td>
      <td>Kontener</td>
      <td>Klaster wirtualny</td>
      <td>
        <font size=2>Protokół: rozwiązania cosmos <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Rozwiązania cosmos</td>
      <td>Tabela</td>
      <td>Strumień, zestaw strumienia widoku</td>
      <td>
        <font size=2>Protokół: rozwiązania cosmos <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Kontener</td>
      <td>Lokacji</td>
      <td>
        <font size=2>Protokół: http <br>Uwierzytelnianie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Raport</td>
      <td>Raport, pulpit nawigacyjny</td>
      <td>
        <font size=2>Protokół: http <br>Uwierzytelnianie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: bazy danych db2 <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: bazy danych db2 <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat</font>
      </td>
    </tr>
    <tr>
      <td>System plików</td>
      <td>Tabela</td>
      <td>Plik</td>
      <td>
        <font size=2>Protokół: plik <br>Uwierzytelnianie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ścieżka</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabela</td>
      <td>Plik w katalogu</td>
      <td>
        <font size=2>Protokół: ftp <br>Uwierzytelnianie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop rozproszonego systemu plików</td>
      <td>Kontener</td>
      <td>Klaster</td>
      <td>
        <font size=2>Protokół: webhdfs <br>Uwierzytelnianie: {podstawowe, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop rozproszonego systemu plików</td>
      <td>Tabela</td>
      <td>Plik w katalogu</td>
      <td>
        <font size=2>Protokół: webhdfs <br>Uwierzytelnianie: {podstawowe, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: gałęzi <br>Uwierzytelnianie: {HDInsight, basic, username, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: gałęzi <br>Uwierzytelnianie: {HDInsight, basic, username, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Kontener</td>
      <td>Lokacji</td>
      <td>
        <font size=2>Protokół: http <br>Uwierzytelnianie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Raport</td>
      <td>Raport, pulpit nawigacyjny</td>
      <td>
        <font size=2>Protokół: http <br>Uwierzytelnianie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Tabela</td>
      <td>Punkt końcowy, plik</td>
      <td>
        <font size=2>Protokół: http <br>Uwierzytelnianie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: mysql <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: mysql <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Kontener</td>
      <td>Kontenera jednostek</td>
      <td>
        <font size=2>Protokół: odata <br>Uwierzytelnianie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabela</td>
      <td>Zestaw jednostek, funkcja</td>
      <td>
        <font size=2>Protokół: odata <br>Uwierzytelnianie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zasobów</font>
      </td>
    </tr>
    <tr>
      <td>Baza danych Oracle</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: oracle <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Baza danych Oracle</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: oracle <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: postgresql <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: postgresql <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Kontener</td>
      <td>Lokacji</td>
      <td>
        <font size=2>Protokół: http <br>Uwierzytelnianie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Raport</td>
      <td>Raport, pulpit nawigacyjny</td>
      <td>
        <font size=2>Protokół: http <br>Uwierzytelnianie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Dodatek Power Query</td>
      <td>Tabela</td>
      <td>Dane zestawu połączonych danych</td>
      <td>
        <font size=2>Protokół: dodatku power query <br>Uwierzytelnianie: {oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>SalesForce</td>
      <td>Tabela</td>
      <td>Obiekt</td>
      <td>
        <font size=2>Protokół: salesforce-com <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klasy <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nazwa elementu</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Kontener</td>
      <td>Serwer</td>
      <td>
        <font size=2>Protokół: sap hana-sql <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Tabela</td>
      <td>Widok</td>
      <td>
        <font size=2>Protokół: sap hana-sql <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Sharepoint</td>
      <td>Tabela</td>
      <td>List</td>
      <td>
        <font size=2>Protokołów: listy programu sharepoint <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Polecenie</td>
      <td>Procedura składowana</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Funkcja zwracająca tabelę</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Oprogramowanie SQL Server</td>
      <td>Polecenie</td>
      <td>Procedura składowana</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Oprogramowanie SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Funkcja zwracająca tabelę</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Oprogramowanie SQL Server</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Oprogramowanie SQL Server</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: tds <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowe</td>
      <td>Kontener</td>
      <td>Model</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowe</td>
      <td>WSKAŹNIK KPI</td>
      <td>WSKAŹNIK KPI</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowe</td>
      <td>Miary</td>
      <td>Miary</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowe</td>
      <td>Tabela</td>
      <td>Wymiar</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {wymiaru}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelarycznym</td>
      <td>Kontener</td>
      <td>Model</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelarycznym</td>
      <td>WSKAŹNIK KPI</td>
      <td>WSKAŹNIK KPI</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelarycznym</td>
      <td>Miary</td>
      <td>Miary</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelarycznym</td>
      <td>Tabela</td>
      <td>Tabela</td>
      <td>
        <font size=2>Protokół: usługi analysis services <br>Uwierzytelnianie: {systemu windows, basic anonimowe, Brak} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {Table}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Kontener</td>
      <td>Serwer</td>
      <td>
        <font size=2>Protokołów: usług raportowania <br>Uwierzytelnianie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Raport</td>
      <td>Raport</td>
      <td>
        <font size=2>Protokołów: usług raportowania <br>Uwierzytelnianie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ścieżka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: teradata <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: teradata <br>Uwierzytelnianie: {protokołu, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Kontener</td>
      <td>Model</td>
      <td>
        <font size="2">Protokół: mssql-mds <br>Uwierzytelnianie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Tabela</td>
      <td>Jednostka</td>
      <td>
        <font size="2">Protokół: mssql-mds <br>Uwierzytelnianie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;jednostki</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: bazie danych dokumentów <br>Uwierzytelnianie: {azure klawisza dostępu} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Collection</td>
      <td>Collection</td>
      <td>
        <font size=2>Protokół: bazie danych dokumentów <br>Uwierzytelnianie: {azure klawisza dostępu} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Kolekcja</font>
      </td>
    </tr>
    <tr>
      <td>Ogólne ODBC</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: odbc <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Opcje <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Ogólne ODBC</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: odbc <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Opcje <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Kontener</td>
      <td>Database (Baza danych)</td>
      <td>
        <font size=2>Protokół: sybase <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Tabela</td>
      <td>Tabela, widok</td>
      <td>
        <font size=2>Protokół: sybase <br>Uwierzytelnianie: {podstawowe, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schemat <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Inne (żadne z powyższych)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protokół: ogólny zasobów <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assetId</font>
      </td>
    </tr>
</table>
