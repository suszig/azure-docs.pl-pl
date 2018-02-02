---
title: "Utrwalanie plików w programie PowerShell w powłoce chmury Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Wskazówki dotyczące jak powłoki chmury Azure będzie się powtarzał plików."
services: azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: 74488b85ec524e4ad4c06a639a16ddbfd54b3154
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>Sposób działania programu PowerShell w powłoce chmury Azure (wersja zapoznawcza)
PowerShell w chmurze powłoki (wersja zapoznawcza) będzie się powtarzał plików za pomocą następujących metod: 
* Instalowanie udziału plików Azure określony jako `clouddrive` w Twojej `$Home` katalogu interakcji bezpośredniego udziału plików.

## <a name="list-cloud-drive-azure-file-shares"></a>Listy udziałów plików w chmurze Azure dysków
`Get-CloudDrive` Polecenie pobiera informacje o udział plików na platformę Azure obecnie zainstalowanych w wyniku dysku w chmurze w powłoce chmury. <br>
![Running Get-CloudDrive](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Odinstaluj obraz dysku w chmurze
Można odinstalować na udział plików na platformę Azure, który jest zainstalowany w chmurze powłoki w dowolnym momencie. Jeśli usunięto udział plików na platformę Azure, wyświetli się monit o utworzy i zainstaluje nowy udział plików na platformę Azure w następnej sesji.

`Dismount-CloudDrive` Polecenia odinstalowuje udziału plików na platformę Azure z bieżącego konta magazynu. Odinstalowywanie dysku w chmurze kończy bieżącej sesji. Użytkownik będzie monitowany o utworzy i zainstaluje nowy udział plików na platformę Azure podczas następnej sesji.
![Running Dismount-CloudDrive](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Kolejne kroki
[Przewodnik Szybki start po programie PowerShell](quickstart-powershell.md) <br>
[Więcej informacji na temat plików platformy Azure](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Więcej informacji na temat tagów magazynu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>