---
title: "Przykładów dla magazynu platformy Azure przy użyciu języka Java | Dokumentacja firmy Microsoft"
description: "Wyświetlanie, Pobierz i uruchom przykładowy kod i aplikacji usługi Azure Storage. Wykryj, wprowadzenie przykłady dla obiektów blob, kolejek, tabel i plików, za pomocą biblioteki klienta magazynu Java."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: fd27e1ac9a773e7b0f5245aa74acdb0521cd098c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-samples-using-java"></a>Przykładów dla magazynu platformy Azure przy użyciu języka Java

## <a name="java-sample-index"></a>Indeks przykładów Java

Poniższa tabela zawiera omówienie naszym repozytorium przykłady i scenariusze w każdej próbce. Kliknij łącza, aby wyświetlić odpowiednie przykładowy kod w serwisie GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Endpoint</th><th style="font-size:110%">Scenariusz</th><th style="font-size:110%">Przykładowy kod</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Obiekt blob</b></td>
<td>Dołącz obiektów Blob</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td> 
</tr> 
<tr> 
<td>Blokowych obiektów Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Szyfrowania po stronie klienta</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Wprowadzenie do korzystania z szyfrowania po stronie klienta usługi Azure w języku Java</a></td>
</tr> 
<tr> 
<td>Kopiowanie obiektu Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Tworzenie kontenera</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Usuwanie obiektów Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Usunąć kontenera</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Obiekt blob metadane/właściwości/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Kontener właściwości listy ACL/metadanych</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Get zakresów stron</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Przykładowe testów stronicowych obiektów Blob</a></td>
</tr> 
<tr> 
<td>Kontener obiektów Blob dzierżawy</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Kontener obiektów Blob listy</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td>Stronicowych obiektów Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr>
<tr> 
<td>SYGNATURY DOSTĘPU WSPÓŁDZIELONEGO</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">Przykładowe testy SAS</a></td>
</tr>   
<tr> 
<td>Właściwości usługi</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr>           
<tr> 
<td>Migawki obiektu Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Wprowadzenie do korzystania z usługi Azure Blob w języku Java</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Plik</b></td>
<td>Tworzenie plików/katalogów/udziałów</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr>
<tr> 
<td>Usuwanie plików/katalogów/udziałów</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr> 
<tr> 
<td>Metadanych właściwości katalogów</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr> 
<tr> 
<td>Pobierz pliki</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr> 
<tr> 
<td>Plik właściwości/metadanych/metryk</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr> 
<tr> 
<td>Właściwości usługi plików</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr> 
<tr> 
<td>Lista katalogów i plików</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr>
<tr> 
<td>Listy udziałów</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr>
<tr> 
<td>Udostępnianie właściwości/metadanych/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Wprowadzenie do korzystania z usługi Azure plików w języku Java</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Kolejki</b></td>
<td>Dodaj komunikat</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Przykłady biblioteki klienta Java magazynu</a></td> 
</tr> 
<tr> 
<td>Szyfrowania po stronie klienta</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Przykłady biblioteki klienta Java magazynu</a></td> 
</tr> 
<tr> 
<td>Tworzenie kolejki</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Wprowadzenie do korzystania z usługi Azure kolejek w języku Java</a></td> 
</tr> 
<tr> 
<td>Usuwanie kolejki/komunikatów</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Wprowadzenie do korzystania z usługi Azure kolejek w języku Java</a></td> 
</tr> 
<tr> 
<td>Wgląd do wiadomości</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Wprowadzenie do korzystania z usługi Azure kolejek w języku Java</a></td> 
</tr> 
<tr> 
<td>Kolejki ACL/metadanych/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Wprowadzenie do korzystania z usługi Azure kolejek w języku Java</a></td> 
</tr> 
<tr> 
<td>Właściwości usługi kolejki</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Wprowadzenie do korzystania z usługi Azure kolejek w języku Java</a></td> 
</tr> 
<tr> 
<td>Komunikat dotyczący aktualizacji</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Wprowadzenie do korzystania z usługi Azure kolejek w języku Java</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabela</b></td>
<td>Tworzenie tabeli</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Wprowadzenie do korzystania z usługi Azure tabel w języku Java</a></td> 
</tr> 
<tr> 
<td>Usuń jednostki/tabeli.</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Wprowadzenie do korzystania z usługi Azure tabel w języku Java</a></td> 
</tr> 
<tr> 
<td>Wstaw/Merge/zastępowania jednostki</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Przykłady biblioteki klienta Java magazynu</a></td> 
</tr> 
<tr> 
<td>Zapytanie jednostek</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Wprowadzenie do korzystania z usługi Azure tabel w języku Java</a></td> 
</tr> 
<tr> 
<td>Tabele kwerendy</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Wprowadzenie do korzystania z usługi Azure tabel w języku Java</a></td> 
</tr> 
<tr> 
<td>Właściwości listy ACL tabeli</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Wprowadzenie do korzystania z usługi Azure tabel w języku Java</a></td> 
</tr> 
<tr> 
<td>Aktualizowanie jednostek</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Przykłady biblioteki klienta Java magazynu</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Biblioteka przykłady kodu platformy Azure

Aby wyświetlić biblioteki kompletnego przykładu, przejdź do [przykłady kodu Azure](https://azure.microsoft.com/resources/samples/?service=storage) biblioteki, która zawiera przykłady dla usługi Azure Storage, który można pobrać i uruchom lokalnie. Biblioteka przykładowy kod zawiera przykładowy kod w formacie zip. Alternatywnie można wybrać i klonowania repozytorium GitHub dla każdej próbki.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Przewodniki z wprowadzeniem pobierania

Zapoznaj się z następujących przewodników, jeśli chcesz, aby uzyskać instrukcje dotyczące sposobu instalowania i rozpoczynanie pracy z biblioteki klienta magazynu Azure.

* [Wprowadzenie do korzystania z usługi Azure Blob w języku Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Wprowadzenie do korzystania z usługi Azure kolejek w języku Java](../storage-java-how-to-use-queue-storage.md)
* [Wprowadzenie do korzystania z usługi Azure tabel w języku Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Wprowadzenie do korzystania z usługi Azure plików w języku Java](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacje na próbkach dla innych języków:

* .NET: [przykłady magazynu platformy azure przy użyciu platformy .NET](../storage-samples-dotnet.md)
* Wszystkie inne języki: [przykłady usługi Azure Storage](../storage-samples.md)
