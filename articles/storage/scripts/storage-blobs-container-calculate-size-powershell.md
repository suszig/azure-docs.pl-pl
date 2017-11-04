---
title: "Skrypt programu PowerShell Azure przykładowe — obliczania rozmiaru kontenera obiektu blob | Dokumentacja firmy Microsoft"
description: "Obliczania rozmiaru kontenera w magazynie obiektów Blob Azure przez sumowanie rozmiar każdego z jego obiektów blob."
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 10/23/2017
ms.author: fryu
ms.openlocfilehash: 46f3eac0129da41062caba2da090f9e532505d67
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Obliczania rozmiaru kontenera magazynu obiektów Blob

Ten skrypt oblicza rozmiar kontenera w magazynie obiektów Blob Azure przez sumowanie rozmiar obiektów blob w kontenerze.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="understand-the-size-of-blob-storage-container"></a>Zrozumienie rozmiar kontenera magazynu obiektów Blob

Całkowity rozmiar kontenera magazynu obiektów Blob obejmuje rozmiar pojemnik i rozmiar wszystkich obiektów blob w kontenerze.

Poniżej opisano sposób obliczania pojemności dla obiekt Blob kontenerów i obiektów blob. W poniższych Len(X) oznacza liczbę znaków w ciągu.

### <a name="blob-containers"></a>Kontenerów obiektów blob

Poniżej przedstawiono sposób oszacować ilość miejsca w magazynie zużytych według kontenera obiektów blob:

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

Poniżej przedstawiono podział:
* 48 bajtów koszty dla każdego kontenera obejmuje czasu ostatniej modyfikacji, uprawnienia ustawienia publicznego i niektóre metadane systemu.
* Nazwa kontenera jest przechowywany jako Unicode tak zająć liczbę znaków i pomnożenia 2.
* Dla każdego kontenera obiektów blob metadane przechowywane przechowujemy długość nazwy (przechowywany w formacie ASCII), a także długość wartości ciągu.
* 512 bajtów na identyfikator podpisany obejmują nazwą podpisanych identyfikatora, uprawnień, czas wygaśnięcia i godzina rozpoczęcia.

### <a name="blobs"></a>Obiekty blob

Poniżej przedstawiono sposób oszacować ilość miejsca w magazynie zużytych według obiektów blob:

* Obiekt Blob blokowy (podstawowy obiektów blob lub migawki)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
SizeInBytes(data in unique committed data blocks stored) +
SizeInBytes(data in uncommitted data blocks)
`

* Stronicowych obiektów Blob (podstawowy obiektów blob lub migawki)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
number of nonconsecutive page ranges with data * 12 bytes +
SizeInBytes(data in unique pages stored)
`

Poniżej przedstawiono podział:

* 124 bajtów koszty dla obiekt blob, który zawiera czasu ostatniej modyfikacji, rozmiar, Cache-Control, Content-Type, Content-Language, kodowania zawartości, Content-MD5 uprawnienia, migawki informacji, dzierżawy i niektóre metadane systemu.
* Nazwa obiektu blob jest przechowywany jako Unicode więc wykonać liczbę znaków i wielu przez 2.
* Następnie dla każdego metadane przechowywane, długość nazwy (przechowywany w formacie ASCII), a także długość wartości ciągu.
* Następnie dla blokowych obiektów blob
    * 8 bajtów zablokowanych
    * Liczba bloków razy identyfikator bloku rozmiar w bajtach
    * Powiększony o rozmiar danych we wszystkich blokach zatwierdzonej i niezatwierdzone. Uwaga: w przypadku używania migawek ten rozmiar zawiera tylko unikatowe dane dla tego obiektu blob podstawowej lub migawki. Jeśli niezatwierdzone bloki nie są używane po upływie tygodnia, zostaną one bezużytecznych, a następnie w tym czasie ich będzie już wchodzą w skład rozliczeń po.
* Następnie dla stronicowych obiektów blob
    * Liczba zakresów niesąsiadujące strony z danymi czasu 12 bajtów. Jest to liczba zakresów unikatową stronę podczas wywoływania funkcji GetPageRanges API są wyświetlane.
    * Powiększony o rozmiar danych w bajtach wszystkich przechowywanych stron. Uwaga, gdy migawki są używane, ten rozmiar zawiera tylko unikatowe strony podstawowej obiektów blob lub migawki obiektu blob są zliczane.

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących rozliczeń magazynu Azure, zobacz [opis systemu Windows Azure magazynu rozliczeń](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowego miejsca do magazynowania znajdują się w [przykłady środowiska PowerShell dla usługi Azure Storage](../blobs/storage-samples-blobs-powershell.md).
