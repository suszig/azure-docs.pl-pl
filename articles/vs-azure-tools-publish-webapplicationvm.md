---
title: Publikowanie WebApplicationVM | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak wdrożyć aplikację sieci web do maszyny wirtualnej. Ten skrypt tworzy wymaganych zasobów w Twojej subskrypcji platformy Azure, jeśli nie istnieją."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 2738fc1dff50a177a227ae2c7719bd9a192d82ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publikowanie WebApplicationVM (skrypt programu Windows PowerShell)
Wdraża aplikację sieci web do maszyny wirtualnej. Skrypt tworzy wymaganych zasobów w Twojej subskrypcji platformy Azure, jeśli nie istnieją.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguracja
Ścieżka do pliku konfiguracji JSON, który opisuje szczegóły wdrożenia.

| Aliasy | Brak |
| --- | --- |
| Wymagana? |Wartość true |
| Stanowisko |o nazwie |
| Wartość domyślna |Brak |
| Akceptowanie danych wejściowych potoku? |wartość false |
| Akceptowanie symboli wieloznacznych? |wartość false |

### <a name="subscriptionname"></a>Nazwa subskrypcji
Nazwa subskrypcji platformy Azure, w którym chcesz utworzyć maszynę wirtualną.

| Aliasy | Brak |
| --- | --- |
| Wymagana? |wartość false |
| Stanowisko |o nazwie |
| Wartość domyślna |Używa pierwszej subskrypcji w pliku subskrypcji |
| Akceptowanie danych wejściowych potoku? |wartość false |
| Akceptowanie symboli wieloznacznych? |wartość false |

### <a name="webdeploypackage"></a>WebDeployPackage
Ścieżka do pakietu wdrożeniowego sieci web do publikowania do maszyny wirtualnej. Ten pakiet można utworzyć za pomocą kreatora Publikowanie w sieci Web w programie Visual Studio. Zobacz [jak: utworzyć pakiet wdrożeniowy sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Aliasy | Brak |
| --- | --- |
| Wymagana? |wartość false |
| Stanowisko |o nazwie |
| Wartość domyślna |Brak |
| Akceptowanie danych wejściowych potoku? |wartość false |
| Akceptowanie symboli wieloznacznych? |wartość false |

### <a name="allowuntrusted"></a>AllowUntrusted
Jeśli PRAWDA, Zezwalaj na korzystanie z certyfikatów, które nie są podpisane przez zaufanego głównego urzędu.

| Aliasy | Brak |
| --- | --- |
| Wymagana? |wartość false |
| Stanowisko |o nazwie |
| Wartość domyślna |wartość false |
| Akceptowanie danych wejściowych potoku? |wartość false |
| Akceptowanie symboli wieloznacznych? |wartość false |

### <a name="vmpassword"></a>VMPassword
Poświadczenia dla konta maszyny wirtualnej. Przykład: - VMPassword @{nazwa = "admin"; Hasło = "password"}

| Aliasy | Brak |
| --- | --- |
| Wymagana? |wartość false |
| Stanowisko |o nazwie |
| Wartość domyślna |Brak |
| Akceptowanie danych wejściowych potoku? |wartość false |
| Akceptowanie symboli wieloznacznych? |wartość false |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
Poświadczenia bazy danych SQL platformy Azure. Przykład: - DatabaseServerPassword @{nazwa = "admin"; Hasło = "password"}

| Aliasy | Brak |
| --- | --- |
| Wymagana? |wartość false |
| Stanowisko |o nazwie |
| Wartość domyślna |Brak |
| Akceptowanie danych wejściowych potoku? |wartość false |
| Akceptowanie symboli wieloznacznych? |wartość false |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Jeśli PRAWDA, Drukuj komunikaty z skrypt do strumienia wyjściowego.

| Aliasy | Brak |
| --- | --- |
| Wymagana? |wartość false |
| Stanowisko |o nazwie |
| Wartość domyślna |wartość false |
| Akceptowanie danych wejściowych potoku? |wartość false |
| Akceptowanie symboli wieloznacznych? |wartość false |

## <a name="remarks"></a>Uwagi
Pełny opis sposobów użycia skryptu do tworzenia środowisk do tworzenia i testowania, zobacz [za pomocą skryptów programu PowerShell systemu Windows do opublikowania deweloperów i środowisk testowych](vs-azure-tools-publishing-using-powershell-scripts.md).

Plik JSON konfiguracji określa szczegóły co ma zostać wdrożona. Zawiera informacje, które zostało określone podczas tworzenia projektu, takie jak nazwa grupy koligacji, obrazu wirtualnego dysku twardego i rozmiar maszyny wirtualnej. Także zawiera punkty końcowe na maszynie wirtualnej, baz danych, aby było możliwe, jeśli istnieje i parametry wdrażania w sieci web. Poniższy kod przedstawia przykładowy plik konfiguracji JSON:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Można edytować plik JSON konfiguracji do zmiany, jakie są obsługiwane. Maszyny wirtualne i usługi w chmurze są wymagane, ale w sekcji bazy danych jest opcjonalna.

