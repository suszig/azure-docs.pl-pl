---
title: "Utwórz moduł Azure IoT krawędzi języku C# | Dokumentacja firmy Microsoft"
description: "W tym samouczku ilustrację jak napisać cz modułu konwertera danych przy użyciu najnowszych pakietów NuGet krawędzi IoT Azure, programu Visual Studio Code i C#."
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: "Azure iot, samouczek, modułu, nuget, vscode, csharp, krawędzi"
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.openlocfilehash: 7175ffc8de2c043593d61143b402484d33e4a8cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a>Utwórz moduł Azure IoT krawędzi z C & #x23;

Jak utworzyć moduł, dla których są wyświetlane w tym samouczku `Azure IoT Edge` przy użyciu `Visual Studio Code` i `C#`.

W tym samouczku będziemy przeprowadzenie konfiguracji środowiska i jak napisać [cz](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modułu konwertera danych przy użyciu najnowszych `Azure IoT Edge NuGet` pakietów. 

>[!NOTE]
Ten samouczek używa `.NET Core SDK`, który obsługuje zgodności między platformami. Następujące samouczek jest przeznaczony przy użyciu `Windows 10` systemu operacyjnego. Niektóre polecenia w tym samouczku mogą być różne w zależności od Twojego `development environment`. 

## <a name="prerequisites"></a>Wymagania wstępne

W tej sekcji możemy konfiguracji środowiska pod kątem `Azure IoT Edge` programowanie modułu. Dotyczy ona obu **64-bitowym systemie Windows** i **64-bitowego systemu Linux (8 Ubuntu/Debian)** systemów operacyjnych.

Następujące oprogramowanie jest wymagane:

- [Klient Git](https://git-scm.com/downloads)
- [Zestaw SDK dla platformy .NET Core](https://www.microsoft.com/net/core#windowscmd)
- [Visual Studio Code](https://code.visualstudio.com/)

Nie trzeba sklonować repozytorium dla tego przykładu, jednak cały kod przykładowy omówione w tym samouczku znajduje się w następujących repozytorium:

- `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a>Wprowadzenie

1. Zainstaluj `.NET Core SDK`.
2. Zainstaluj `Visual Studio Code` i `C# extension` z witryny Marketplace kodu programu Visual Studio.

Wyświetlić [krótki samouczek wideo](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) o tym, jak rozpocząć pracę przy użyciu `Visual Studio Code` i `.NET Core SDK`.

## <a name="creating-the-azure-iot-edge-converter-module"></a>Tworzenie modułu konwertera krawędzi IoT Azure

1. Zainicjuj nowy `.NET Core` klasy projektu biblioteki C#:
    - Otwórz okno wiersza polecenia (`Windows + R` -> `cmd` -> `enter`).
    - Przejdź do folderu, w którym chcesz utworzyć `C#` projektu.
    - Typ **dotnet nowej biblioteki klas -o IoTEdgeConverterModule -f netstandard1.3**. 
    - To polecenie tworzy pustą klasę o nazwie `Class1.cs` w katalogu projektów.
2. Przejdź do folderu, w którym właśnie utworzyliśmy projektu biblioteki klas, wpisując **cd IoTEdgeConverterModule**.
3. Otwórz projekt w `Visual Studio Code` , wpisując **kodu .**.
4. Po otwarciu projektu w `Visual Studio Code`, kliknij **IoTEdgeConverterModule.csproj** można otworzyć pliku, jak pokazano na poniższej ilustracji:

    ![Okno edycji w usłudze Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. Wstaw `XML` pokazano poniższy fragment kodu od zamknięcia obiektu blob `PropertyGroup` znacznika i zamykania `Project` tagów; wiersz sześć w poprzednim obrazu i Zapisz plik naciskając `Ctrl`  +  `S`.

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. Po zapisaniu `.csproj` pliku `Visual Studio Code` powinien monit o `unresolved dependencies` okna dialogowego, jak pokazano na poniższej ilustracji: 

    ![Okno dialogowe programu Visual Studio Code przywracania zależności](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    ) kliknij `Restore` Aby przywrócić wszystkie odwołania w projektach `.csproj` w tym pliku `PackageReferences` dodaliśmy. 

    (b) `Visual Studio Code` automatycznie tworzy `project.assets.json` pliku w projektach `obj` folderu. Ten plik zawiera informacje o zależności projektu, aby szybciej kolejnych operacji przywracania.
 
    >[!NOTE]
    `.NET Core Tools`znajdują się teraz na podstawie programu MSBuild. Co oznacza, że `.csproj` plik projektu jest tworzony zamiast `project.json`.

    - Jeśli `Visual Studio Code` Monituj użytkownika o jest prawidłowy, możemy go ręcznie. Otwórz `Visual Studio Code` zintegrowane okno terminalu, naciskając klawisz `Ctrl`  +  `backtick` kluczy lub przy użyciu menu `View`  ->  `Integrated Terminal`.
    - W `Integrated Terminal` typ okna **przywracania dotnet**.
    
7. Zmień nazwę `Class1.cs` pliku `BleConverterModule.cs`. 

    ) Aby zmienić nazwę pliku najpierw kliknąć plik, naciśnij klawisz `F2` klucza.
    
    (b) wpisz nazwę nowego **BleConverterModule**, jak pokazano na poniższej ilustracji:

    ![Visual Studio Code zmiana nazwy klasy](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. Zastąp istniejący kod w `BleConverterModule.cs` plik, kopiując i wklejając następujący fragment kodu do Twojej `BleConverterModule.cs` pliku.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. Zapisz plik, naciskając klawisz `Ctrl`  +  `S`.

10. Utwórz nowy plik o nazwie `Untitled-1` naciskając `Ctrl`  +  `N` kluczy, jak pokazano na poniższej ilustracji:

    ![Visual Studio Code nowy plik.](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. Do deserializacji `JSON` obiektu otrzymane od symulowane `BLE` urządzenie, skopiuj następujący kod do `Untitled-1` okna edytora pliku kodu. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. Zapisz plik jako `BleData.cs` naciskając `Ctrl`  +  `Shift`  +  `S` kluczy.
    - Na Zapisz jako okno dialogowe w `Save as Type` menu rozwijanego wybierz `C# (*.cs;*.csx)` jak pokazano na poniższej ilustracji:

    ![Visual Studio Code Zapisz jako okna dialogowego](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. Utwórz nowy plik o nazwie `Untitled-1` naciskając `Ctrl`  +  `N` kluczy.

14. Skopiuj i wklej poniższy fragment kodu do `Untitled-1` pliku. Ta klasa jest `Azure IoT Edge` moduł, który używamy danych wyjściowych danych otrzymywanych z naszych `BleConverterModule`.

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. Zapisz plik jako `DotNetPrinterModule.cs` naciskając `Ctrl`  +  `Shift`  +  `S`.
    - Na Zapisz jako okno dialogowe w `Save as Type` menu rozwijanego wybierz `C# (*.cs;*.csx)`.

16. Utwórz nowy plik o nazwie `Untitled-1` naciskając `Ctrl`  +  `N` kluczy.

17. Do deserializacji `JSON` obiektu otrzymane od `BleConverterModule`, Kopiuj i wklej poniższy kod fragment do `Untitled-1` pliku. 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. Zapisz plik jako `BleConverterData.cs` naciskając `Ctrl`  +  `Shift`  +  `S`.
    - Na Zapisz jako okno dialogowe w `Save as Type` menu rozwijanego wybierz `C# (*.cs;*.csx)`.

19. Utwórz nowy plik o nazwie `Untitled-1` naciskając `Ctrl`  +  `N` kluczy.

20. Skopiuj i wklej poniższy fragment kodu do `Untitled-1` pliku.

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. Zapisz plik jako `gw-config.json` naciskając `Ctrl`  +  `Shift`  +  `S`.
    - Na Zapisz jako okno dialogowe w `Save as Type` menu rozwijanego wybierz `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.

22. Aby umożliwić kopiowanie pliku konfiguracji do katalogu wyjściowego, należy zaktualizować `IoTEdgeConverterModule.csproj` przy użyciu następującego obiektu blob XML:

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - Zaktualizowany interfejs `IoTEdgeConverterModule.csproj` powinno wyglądać podobnie do poniższej ilustracji:

    ![Visual Studio Code zaktualizować pliku .csproj](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. Utwórz nowy plik o nazwie `Untitled-1` naciskając `Ctrl`  +  `N` kluczy.

24. Skopiuj i wklej poniższy fragment kodu do `Untitled-1` pliku.

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. Zapisz plik jako `binplace.ps1` naciskając `Ctrl`  +  `Shift`  +  `S`.
    - Na Zapisz jako okno dialogowe w `Save as Type` menu rozwijanego wybierz `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.

26. Tworzenie projektu przez naciśnięcie przycisku `Ctrl`  +  `Shift`  +  `B` kluczy. Podczas kompilowania projektu po raz pierwszy, `Visual Studio Code` monituje o `No build task defined.` okna dialogowego, jak pokazano na poniższej ilustracji:

    ![Zadanie kompilacji programu Visual Studio Code okna dialogowego](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    ) kliknij `Configure Build Task` przycisku.

    (b) w `Select a Task Runner` menu rozwijanego okna dialogowego. Wybierz `.NET Core` jak pokazano na poniższej ilustracji: 

    ![Visual Studio Code wybierz Okno zadań](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    Kliknięcie przycisku c) `.NET Core` tworzy element `tasks.json` plików w sieci `.vscode` katalogu i otwarcie pliku w `code editor` okna. Nie istnieje potrzeba do modyfikowania tego pliku, zamknij karcie.

27.  Otwórz `Visual Studio Code` zintegrowane okno terminalu, naciskając klawisz `Ctrl`  +  `backtick` kluczy lub przy użyciu menu `View`  ->  `Integrated Terminal` i typ **.\binplace.ps1** do `PowerShell` wiersza polecenia. To polecenie powoduje skopiowanie wszystkich naszych zależności do katalogu wyjściowego.

28. Przejdź do katalogu wyjściowego projekty w `Integrated Terminal` okna, wpisując **cd.\bin\Debug\netstandard1.3**.

29. Uruchom przykładowy projekt, wpisując **. \gw.exe gw — config.json** do `Integrated Terminal` okno. 
    - Jeśli zostały wykonane kroki ściśle w tym samouczku, teraz powinny być uruchomiono `Azure IoT Edge BLE Data Converter Module` przykładowy projekt, jak pokazano na poniższej ilustracji:
    
        ![Przykład symulowane urządzenie działa w programie Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - Jeśli chcesz zakończyć tę aplikację, naciśnij klawisz `<Enter>` klucza.

>[!IMPORTANT]
Nie zaleca się używania `Ctrl`  +  `C` zakończenie `IoT Edge` bramy aplikacji (czyli **gw.exe**). Ponieważ ta akcja może spowodować przerwanie wyjątkowo procesu.

