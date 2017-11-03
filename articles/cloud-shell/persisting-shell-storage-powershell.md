---
title: "Utrwalanie plików w powłoce chmury Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
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
ms.date: 07/17/2017
ms.author: damaerte
ms.openlocfilehash: 933af3860bfe087a0b4db7eff53d4b978a1475da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-cloud-shell-works"></a>Jak działa powłoki chmury
Powłoka chmury będzie się powtarzał plików za pomocą następujących metod: 
* Instalowanie udziału plików określony jako `clouddrive` w Twojej `$Home` katalogu interakcji bezpośredniego udziału plików.

## <a name="list-cloud-drive-file-shares"></a>Udziały plików na dysku w chmurze listy
`Get-CloudDrive` Polecenie pobiera informacje o udziału plików, które zostały aktualnie zainstalowanych w wyniku dysku w chmurze w powłoce chmury. <br>
![Get-CloudDrive uruchomiona](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Odinstaluj obraz dysku w chmurze
Można odinstalować udział pliku, który jest zamontowany do chmury powłoki w dowolnym momencie. Jeśli udział plików został usunięty, wyświetli się monit o utworzy i zainstaluje nowy udział plików w następnej sesji.

`Dismount-CloudDrive` Polecenia odinstalowuje udział plików z bieżącego konta magazynu. Odinstalowywanie dysku w chmurze kończy bieżącej sesji. Użytkownik będzie monitowany o utworzy i zainstaluje nowy udział plików podczas następnej sesji.
![Uruchomiona CloudDrive dezinstalacji](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Następne kroki
[Przewodnik Szybki start po programie PowerShell](quickstart-powershell.md) <br>
[Więcej informacji na temat usługi Magazyn plików Azure](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Więcej informacji na temat tagów magazynu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>