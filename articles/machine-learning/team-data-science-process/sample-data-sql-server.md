---
title: "Przykładowe dane w programie SQL Server na platformie Azure | Dokumentacja firmy Microsoft"
description: "Przykładowe dane w programie SQL Server na platformie Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: fbd83ad59a9db1daca4ba16402031e2c1c5b7991
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="heading"></a>Przykładowe dane w programie SQL Server na platformie Azure
Ten dokument zamieszczono przykładowe dane przechowywane w programie SQL Server na platformie Azure przy użyciu SQL lub języka programowania Python. Ponadto jak przenieść próbki danych do usługi Azure Machine Learning przez zapisanie go w pliku, przekazać go do obiektów blob platformy Azure i odczytywania go do usługi Azure Machine Learning Studio.

Używa języka Python próbkowania [pyodbc](https://code.google.com/p/pyodbc/) ODBC — Biblioteka nawiązać połączenia z programem SQL Server na platformie Azure i [Pandas](http://pandas.pydata.org/) biblioteki w celu pobierania próbek.

> [!NOTE]
> Przykładowy kod SQL w tym dokumencie przyjęto założenie, że dane są w programie SQL Server na platformie Azure. Jeśli nie jest, zapoznaj się [przenoszenie danych do programu SQL Server na platformie Azure](move-sql-server-virtual-machine.md) tematu, aby uzyskać instrukcje dotyczące sposobu przenoszenia danych do programu SQL Server na platformie Azure.
> 
> 

Następujące **menu** linki do tematów opisujących sposób przykładowe dane z różnych środowiskach magazynu. 

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Dlaczego przykładowe dane?**
Jeśli zestaw danych, które mają być analizowanie jest duży, zazwyczaj jest dobrym rozwiązaniem w dół przykładowych danych, aby zmniejszyć jego rozmiar mniejsze, ale reprezentatywny i łatwiejsze w zarządzaniu. To ułatwia zrozumienie danych, badanie i inżynieria funkcji. Swoją rolę w [zespołu danych nauki procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) jest umożliwienie szybkiego prototypy funkcji przetwarzania danych i modeli uczenia maszynowego.

To zadanie próbkowania jest krokiem w [zespołu danych nauki procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="SQL"></a>Przy użyciu programu SQL
W tej sekcji opisano kilka metod, aby wykonać pobieranie próbek losowych z danymi w bazie danych przy użyciu programu SQL. Wybierz metodę, w oparciu o rozmiar danych i jego dystrybucji.

Dwa poniższe elementy pokazują, jak używać newid w programie SQL Server do wykonywania próbki. Wybór metody zależy od sposobu losowej próbki w celu (pk_id w poniższym przykładowym kodzie zakłada się, że klucz podstawowy generowane automatycznie).

1. Mniej rygorystyczne losowej próbki
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Więcej losowej próbki 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample może być użyta do próbkowania także przedstawiona poniżej. Może to być lepszym rozwiązaniem, jeśli rozmiar danych jest duży (przy założeniu, że nie jest skorelowany danych na różnych stronach) i dla zapytania do wykonania w odpowiednim czasie.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Można eksplorować i generowanie funkcji z tej próbki danych przez zapisanie go w nowej tabeli
> 
> 

### <a name="sql-aml"></a>Nawiązywanie połączenia z usługi Azure Machine Learning
Przykładowe zapytania powyżej można używać bezpośrednio w usłudze Azure Machine Learning [i zaimportuj dane] [ import-data] moduł dół przykładowe dane na bieżąco i przełączyć go do eksperymentu uczenia maszynowego Azure. Poniżej przedstawiono zrzut ekranu przy użyciu modułu czytnik do odczytu próbki danych:

![Czytnik sql][1]

## <a name="python"></a>Przy użyciu języka programowania Python
W tej sekcji przedstawiono przy użyciu [biblioteki pyodbc](https://code.google.com/p/pyodbc/) do ustanawiania połączenia ODBC w bazie danych programu SQL server w języku Python. Parametry połączenia bazy danych jest następujący: (zamiast servername, dbname, nazwę użytkownika i hasło z konfiguracją):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas](http://pandas.pydata.org/) biblioteki w języku Python zawiera bogaty zestaw struktur danych i narzędzia do analizy danych do manipulowania danymi programowania Python. Poniższy kod odczytuje 0,1% próbkę danych z tabeli w bazie danych Azure SQL w danych Pandas:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Teraz możesz pracować z próbki danych w ramce Pandas danych. 

### <a name="python-aml"></a>Nawiązywanie połączenia z usługi Azure Machine Learning
Następujący przykładowy kod umożliwia zapisują dane próbkowania w dół do pliku i przekaż go do obiektów blob platformy Azure. Dane w obiekcie blob mogą bezpośrednio odczytywać do eksperymentu Azure Machine Learning przy użyciu [i zaimportuj dane] [ import-data] modułu. Dostępne są następujące kroki: 

1. Zapis pandas ramki danych do pliku lokalnego
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Przekazywanie pliku lokalnego do obiektów blob platformy Azure
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Odczytywanie danych z obiektów blob platformy Azure przy użyciu usługi Azure Machine Learning [i zaimportuj dane] [ import-data] modułu, jak pokazano poniżej Przechwyć ekranu:

![Czytnik obiektów blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Proces nauki danych zespołu w przykładzie akcji
Przykład end-to-end wskazówki procesu nauki danych Team publicznego zestawu danych, przy użyciu zobacz [proces nauki danych zespołu w działaniu: przy użyciu programu SQL Server](sql-walkthrough.md).

[1]: ./media/sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
