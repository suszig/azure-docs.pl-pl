---
title: "Włącz debugowanie zdalne z ciągłego dostarczania | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak włączyć debugowanie zdalne w przypadku używania ciągłego dostarczania do wdrożenia na platformie Azure"
services: cloud-services
documentationcenter: .net
author: mikejo
manager: ghogen
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: mikejo
ms.openlocfilehash: 1a30b42e6e84edf9a7cef861aaf6a60e87c473d0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Włączanie zdalnego debugowania podczas korzystania z ciągłego dostarczania w celu publikowania w systemie Azure
Można włączyć debugowanie zdalne na platformie Azure dla usługi w chmurze lub maszyn wirtualnych, gdy używasz [ciągłego dostarczania](cloud-services-dotnet-continuous-delivery.md) do publikowania na platformie Azure, wykonaj następujące czynności.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Włączanie debugowania zdalnego dla usługi w chmurze
1. Na agenta kompilacji, konfigurowanie początkowej środowiska platformy Azure zgodnie z opisem w [kompilacji wiersza polecenia platformy Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Ponieważ zdalnego debugowania środowiska uruchomieniowego (msvsmon.exe) jest wymagany dla pakietu, należy zainstalować **Remote Tools for Visual Studio**.

   * [Remote Tools for Visual Studio 2017 r.](https://go.microsoft.com/fwlink/?LinkId=746570)
   * [Remote Tools for Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=615470)
   * [Remote Tools for Visual Studio 2013 z aktualizacją 5](https://www.microsoft.com/download/details.aspx?id=48156)
    
   Alternatywnie można kopiować pliki binarne debugowania zdalnego z systemu, które mają zainstalowanego programu Visual Studio.

3. Utwórz certyfikat zgodnie z opisem w [Omówienie certyfikatów dla usług w chmurze Azure](cloud-services-certs-create.md). Aktualizowanie pliku PFX i odcisk palca certyfikatu protokołu RDP i przekaż certyfikat do docelowej usługi w chmurze.
4. Należy użyć następujących opcji w wierszu polecenia programu MSBuild do tworzenia i pakietu z włączoną debugowania zdalnego. (Zastąp rzeczywiste ścieżki do plików systemu i projektu dla elementów oddzielona kąt).
   
   ```cmd
   msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"
   ```
   
   `VSX64RemoteDebuggerPath`jest ścieżka do folderu zawierającego msvsmon.exe w narzędzi Remote Tools dla programu Visual Studio.
   `RemoteDebuggerConnectorVersion`jest to wersja zestawu SDK platformy Azure w usłudze w chmurze. Ponadto powinna być zgodna z wersją zainstalowaną z programem Visual Studio.

5. Publikowanie do docelowej usługi w chmurze przy użyciu pliku pakietu i .cscfg generowane w poprzednim kroku.
6. Zaimportuj certyfikat (pfx) na komputer z programu Visual Studio z zestawem Azure SDK dla platformy .NET zainstalowane. Upewnij się zaimportować `CurrentUser\My` magazynu certyfikatów, w przeciwnym razie dołączanie debugera programu Visual Studio zakończy się niepowodzeniem.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Włączanie zdalnego debugowania dla maszyn wirtualnych
1. Utwórz maszynę wirtualną platformy Azure. Zobacz [Utwórz maszynę wirtualną z systemem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lub [tworzenie i zarządzanie nimi maszyn wirtualnych platformy Azure w programie Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. W portalu Azure] (http://go.microsoft.com/fwlink/p/?LinkID=269851), przejdź do maszyny wirtualnej w maszynie wirtualnej **odcisk PALCA certyfikatu protokołu RDP**. Ta wartość jest używana dla `ServerThumbprint` wartości w konfiguracji rozszerzenia.
3. Utwórz certyfikat klienta w sposób opisany w [Omówienie certyfikatów dla usług w chmurze Azure](cloud-services-certs-create.md) (Aktualizowanie PFX i odcisk palca certyfikatu protokołu RDP).
4. Instalowanie programu Azure Powershell (wersja 0.7.4 lub nowszym) w sposób opisany w [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).
5. Uruchom następujący skrypt, aby włączyć rozszerzenie RemoteDebug. Zamień ścieżki i danych osobistych własny, takie jak nazwa subskrypcji, nazwę usługi i odcisk palca.
   
   > [!NOTE]
   > Ten skrypt jest skonfigurowany dla programu Visual Studio 2015. Jeśli używasz programu Visual Studio 2013 lub Visual Studio 2017, zmodyfikuj `$referenceName` i `$extensionName` przypisania poniżej, aby `RemoteDebugVS2013` lub `RemoteDebugVS2017`.

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. Importowanie certyfikatu (pfx) na komputer z programu Visual Studio z zestawem Azure SDK dla platformy .NET zainstalowane.

