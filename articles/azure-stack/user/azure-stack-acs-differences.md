---
title: "Magazynu Azure stosu: Różnice i zagadnienia dotyczące"
description: "Opis różnic między Azure stosu magazynu i usługi Azure Storage oraz zagadnienia dotyczące wdrażania usługi Azure stosu."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
ms.reviwer: xiaofmao
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/08/2017
ms.author: jeffgilb
ms.openlocfilehash: 1dc099fa234e217b682c88f2214fe271c916eec2
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Magazynu Azure stosu: Różnice i zagadnienia dotyczące

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Magazyn Azure stosu to zbiór magazynu usługi w chmurze Microsoft Azure stosu. Usługa Azure Storage stosu udostępnia obiektów blob, tabeli, kolejki i funkcje zarządzania kontami z semantyki spójnej platformy Azure.

Ten artykuł zawiera podsumowanie znane różnice Azure stosu magazynu z usługi Magazyn Azure. Także podsumowanie innych kwestii, które należy wziąć pod uwagę podczas wdrażania usługi Azure stosu. Aby uzyskać informacje ogólne różnice między stosu Azure i usługi Azure, zobacz [kluczowe zagadnienia dotyczące](azure-stack-considerations.md) tematu.

## <a name="cheat-sheet-storage-differences"></a>Ściągawka: różnice magazynu

| Funkcja | Azure (globalna) | Azure Stack |
| --- | --- | --- |
|File Storage|Oparte na chmurze udziałów plików SMB obsługiwane|Nie jest jeszcze obsługiwane
|Szyfrowanie usługi Azure Storage dla danych magazynowanych|256-bitowe szyfrowanie AES|Nie jest jeszcze obsługiwane
|Typ konta magazynu|Ogólnego przeznaczenia i Azure kont magazynu obiektów Blob|Ogólnego przeznaczenia tylko
|Opcje replikacji|Magazyn lokalnie nadmiarowy, magazynu geograficznie nadmiarowego magazynu geograficznie nadmiarowego dostęp do odczytu i Magazyn strefowo nadmiarowy|Magazyn lokalnie nadmiarowy
|Premium Storage|W pełni obsługiwane|Można udostępnić, ale brak limitu wydajności lub gwarancji
|Dyski zarządzane|Premium i standardowa obsługiwane|Nie jest jeszcze obsługiwane
|Nazwa obiektu blob|1024 znaków (2048 bajtów)|880 znaków (1,760 w bajtach)
|Maksymalny rozmiar obiektu blob bloku|4,75 TB (100 MB X 50 000 bloków)|50 000 x 4 MB (około 195 GB)
|Kopiowanie migawki obiektu blob strony|Kopia zapasowa Azure niezarządzane maszyny Wirtualnej dysków dołączonych do uruchomionej maszyny Wirtualnej obsługiwane|Nie jest jeszcze obsługiwane
|Kopiuj przyrostowe migawki obiektu blob strony|Premium i standardowa Azure stronicowe obiekty BLOB obsługiwane|Nie jest jeszcze obsługiwane
|Maksymalny rozmiar obiektu blob strony|8 TB|1 TB
|Rozmiar strony obiektu blob strony|512 bajtów|4 KB
|Rozmiar klucza w tabeli klucz partycji i wiersza|1024 znaków (2048 bajtów)|400 znaków (800 w bajtach)

### <a name="metrics"></a>Metryki
Istnieją pewne różnice z metryki magazynu:
* Metryki magazynu danych transakcji nie odróżnia przepustowość sieci wewnętrznych lub zewnętrznych.
* Danych transakcji w metryki magazynu nie ma na maszynie wirtualnej dostęp do dysków zainstalowanych.

## <a name="api-version"></a>Wersja interfejsu API
Obsługiwane są następujące wersje z usługą Azure Storage stosu:

* Usługi danych magazynu Azure: [2015-04-05 wersja interfejsu API REST](https://docs.microsoft.com/rest/api/storageservices/Version-2015-04-05?redirectedfrom=MSDN)
* Usługi zarządzania magazynu Azure: [2015-05-01-preview, 2015-06-15 i 2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN) 

## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do narzędzi programistycznych magazyn Azure stosu](azure-stack-storage-dev.md)
* [Wprowadzenie do magazynu Azure stosu](azure-stack-storage-overview.md)

