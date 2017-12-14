---
title: "Osadź powłoki chmury Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się osadzić powłoki chmury Azure."
services: cloud-shell
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 7c06a51e7f9f402b2ec10e440ca98125a7f2a7cf
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="embed-azure-cloud-shell"></a>Osadź powłoki w chmurze Azure

Osadzanie powłoki chmury umożliwia deweloperom i zapisywania zawartości bezpośrednio Otwórz powłokę chmury z dedykowany adres URL, [shell.azure.com](https://shell.azure.com). Umożliwia to odblokowanie pełną moc uwierzytelniania chmury powłoki, narzędzi i aktualne narzędzia Azure PowerShell interfejsu wiersza polecenia/Azure.

## <a name="how-to"></a>Porady

Integrowanie przycisk uruchamiania powłoki chmury pliki markdown przez skopiowanie następujące czynności:

```markdown
[![Launch Cloud Shell](https:shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

HTML do osadzenia wyskakujących powłoki chmura znajduje się poniżej:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="next-steps"></a>Następne kroki
[Bash w chmurze powłoki Szybki Start](quickstart.md)<br>
[PowerShell w chmurze powłoki Szybki Start](quickstart-powershell.md)