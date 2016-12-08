---
title: "Wprowadzenie do języka Python i usług Azure Cloud Services | Microsoft Docs"
description: "Omówienie sposobu używania programu Python Tools for Visual Studio do tworzenia usług w chmurze platformy Azure, w tym ról Sieć Web i Proces roboczy."
services: cloud-services
documentationcenter: python
author: thraka
manager: timlt
editor: 
ms.assetid: 5489405d-6fa9-4b11-a161-609103cbdc18
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 11/16/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 9ad2f55c7db53459c17299ba5015783781c7cd63
ms.openlocfilehash: 6ec7c5ac984476e3645b45f5e8cf98cf0aa18e5d


---
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Role Sieć Web i Proces roboczy języka Python z programem Python Tools for Visual Studio

Ten artykuł zawiera omówienie sposobu użycia ról Sieć Web i Proces roboczy języka Python za pomocą programu [Python Tools for Visual Studio][Python Tools for Visual Studio]. Dowiesz się, jak używać programu Visual Studio do tworzenia i wdrażania podstawowej usługi w chmurze, która używa języka Python.

## <a name="prerequisites"></a>Wymagania wstępne
* Program Visual Studio w wersji 2013 lub 2015
* [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)
* [Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] lub [Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015]
* [32-bitowe środowisko Python w wersji 2.7][32-bitowe środowisko Python w wersji 2.7] lub [32-bitowe środowisko Python w wersji 3.5][32-bitowe środowisko Python w wersji 3.5]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Co to są role Sieć Web i Proces roboczy języka Python?
Platforma Azure udostępnia trzy modele obliczeniowe na potrzeby uruchamiania aplikacji: [funkcja Web Apps w usłudze App Service][execution model-web sites], [usługa Azure Virtual Machines][execution model-vms] i [usługi Azure Cloud Services][execution model-cloud services]. Wszystkie trzy modele obsługują język Python. Usługi Cloud Services, które obejmują role Sieć Web i Proces roboczy, udostępniają rozwiązanie typu *Platforma jako usługa (Platform as a Service, PaaS)*. W ramach usługi w chmurze rola Sieć Web zapewnia dedykowany serwer sieci Web usług Internet Information Services (IIS), podczas gdy rola Proces roboczy może uruchamiać asynchroniczne, długotrwałe lub ciągłe zadania niezależne od działań użytkownika lub danych wejściowych.

Aby uzyskać więcej informacji, zobacz [Co to jest usługa w chmurze?]

> [!NOTE]
> *Chcesz utworzyć prostą witrynę sieci Web?*
> Jeśli scenariusz obejmuje tylko prosty fronton witryny sieci Web, rozważ użycie lekkiej funkcji Web Apps w usłudze App Service. Możesz łatwo przeprowadzić uaktualnienie do usługi w chmurze w przypadku rozwoju witryny sieci Web lub zmiany wymagań. W <a href="/develop/python/">Centrum deweloperów języka Python</a> można znaleźć artykuły, które dotyczą funkcji Web Apps w usłudze App Service.
> <br />
> 
> 

## <a name="project-creation"></a>Tworzenie projektu
W programie Visual Studio możesz wybrać pozycję **Usługa w chmurze platformy Azure** w oknie dialogowym **Nowy projekt** w obszarze **Python**.

![Okno dialogowe Nowy projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

W Kreatorze usługi w chmurze platformy Azure można utworzyć nowe role Sieć Web i Proces roboczy.

![Okno dialogowe Usługa w chmurze platformy Azure](./media/cloud-services-python-ptvs/new-service-wizard.png)

Szablon roli Proces roboczy zawiera schematyczny kod służący do nawiązywania połączenia z kontem magazynu Azure lub z usługą Azure Service Bus.

![Rozwiązanie usługi w chmurze](./media/cloud-services-python-ptvs/worker.png)

W każdej chwili możliwe jest dodanie roli Sieć Web lub Proces roboczy do istniejącej usługi w chmurze.  Możesz dodawać istniejące projekty do rozwiązania lub tworzyć nowe.

![Polecenie Dodaj rolę](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Usługa w chmurze może zawierać role zaimplementowane w różnych językach.  Na przykład można mieć rolę Sieć Web języka Python zaimplementowaną za pomocą środowiska Django, języka Python lub roli Proces roboczy języka C#.  Między rolami można się w łatwy sposób komunikować za pomocą kolejek usługi Service Bus lub kolejek magazynu.

## <a name="install-python-on-the-cloud-service"></a>Instalowanie języka Python w usłudze w chmurze
> [!WARNING]
> Skrypty instalacji instalowane z programem Visual Studio (w momencie ostatniej aktualizacji artykułu) nie działają. W tej sekcji opisano sposób obejścia problemu.
> 
> 

Główny problem ze skryptami instalacji polega na tym, że nie instalują one języka Python. Najpierw należy zdefiniować dwa [zadania uruchamiania](cloud-services-startup-tasks.md) w pliku [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef). Pierwsze zadanie (**PrepPython.ps1**) pobiera i instaluje środowiska uruchomieniowe języka Python. Drugie zadanie (**PipInstaller.ps1**) uruchamia mechanizm pip, aby zainstalować wszystkie zależności.

Poniższe skrypty zostały napisane dla języka Python 3.5. Jeśli chcesz korzystać z wersji 2.x języka Python, ustaw plik zmiennej **PYTHON2** na **on** dla dwóch zadań uruchamiania i zadania środowiska uruchomieniowego: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.

```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>

  </Task>

</Startup>
```

Należy dodać zmienne **PYTHON2** i **PYPATH** do zadania uruchamiania procesu roboczego. Zmienna **PYPATH** jest używana tylko wtedy, gdy zmienna **PYTHON2** jest ustawiona na wartość **on**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Przykładowy plik ServiceDefinition.csdef
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Następnie należy utworzyć pliki **PrepPython.ps1** i **PipInstaller.ps1** w folderze **./bin** roli użytkownika.

#### <a name="preppythonps1"></a>PrepPython.ps1
Ten skrypt instaluje język Python. Jeśli zmienna środowiskowa **PYTHON2** jest ustawiona na wartość **on**, zostanie zainstalowany język Python 2.7. W przeciwnym razie zostanie zainstalowany język Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }

        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1
Ten skrypt wywołuje kod pip i instaluje wszystkie zależności w pliku **requirements.txt**. Jeśli zmienna środowiskowa **PYTHON2** jest ustawiona na wartość **on**, używany jest język Python 2.7. W przeciwnym razie używany jest język Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>Modyfikowanie skryptu LaunchWorker.ps1
> [!NOTE]
> W przypadku projektu **roli procesu roboczego** plik **LauncherWorker.ps1** jest wymagany do wykonania pliku uruchamiania. W projektach **roli sieci Web** plik uruchamiania jest zdefiniowany we właściwościach projektu.
> 
> 

Skrypt **Bin\LaunchWorker.ps1** pierwotnie został utworzony w celu wykonywania działań przygotowawczych, ale w praktyce nie działa. Zastąp zawartość tego pliku następującym skryptem.

Ten skrypt wywołuje plik **worker.py** z projektu języka Python. Jeśli zmienna środowiskowa **PYTHON2** jest ustawiona na wartość **on**, używany jest język Python 2.7. W przeciwnym razie używany jest język Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>ps.cmd
Szablony Visual Studio powinny utworzyć plik **ps.cmd** w folderze **./bin**. Ten skrypt powłoki wywołuje powyższe skrypty otoki PowerShell i zapewnia rejestrowanie na podstawie nazwy wywołanej otoki PowerShell. Jeśli ten plik nie został utworzony, jego zawartość powinna wyglądać następująco. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Uruchamianie lokalnie
Jeśli ustawisz projekt usługi w chmurze jako projekt startowy i naciśniesz klawisz F5, usługa w chmurze zostanie uruchomiona w lokalnym emulatorze platformy Azure.

Mimo że program PTVS obsługuje uruchamianie w emulatorze, debugowanie nie będzie działać (dotyczy to na przykład punktów przerwania).

Aby debugować role Sieć Web i Proces roboczy, możesz ustawić projekt roli jako projekt startowy i debugować go zamiast ról.  Można również ustawić wiele projektów startowych.  Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Ustaw projekty startowe**.

![Właściwości projektu startowego rozwiązania](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Publikowanie na platformie Azure
Aby przeprowadzić publikowanie, kliknij prawym przyciskiem myszy projekt usługi w chmurze w rozwiązaniu, a następnie wybierz pozycję **Publikuj**.

![Logowanie na potrzeby publikowania na platformie Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

Postępuj zgodnie z poleceniami kreatora. Jeśli trzeba, włącz pulpit zdalny. Pulpit zdalny jest przydatny w przypadku konieczności debugowania elementów.

Po zakończeniu konfigurowania ustawień kliknij pozycję **Publikuj**.

W oknie danych wyjściowych będzie wyświetlany postęp, a następnie zostanie wyświetlone okno Dziennik aktywności platformy Microsoft Azure.

![Okno Dziennik aktywności platformy Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Wdrożenie potrwa kilka minut, a następnie role Sieć Web i/lub Proces roboczy będą działały na platformie Azure!

### <a name="investigate-logs"></a>Sprawdzanie dzienników
Po uruchomieniu maszyny wirtualnej usługi w chmurze i zainstalowaniu języka Python można sprawdzić dzienniki pod kątem komunikatów o błędach. Te dzienniki znajdują się w folderze **C:\Resources\Directory\\{rola}\LogFiles**. Plik **PrepPython.err.txt** będzie zawierał co najmniej jeden błąd, jeśli skrypt próbuje wykryć instalację języka Python, a plik **PipInstaller.err.txt** może zgłaszać błąd nieaktualnej wersji kodu pip.

## <a name="next-steps"></a>Następne kroki
Bardziej szczegółowe informacje na temat pracy z rolami Sieć Web i Proces roboczy w ramach programu Python Tools for Visual Studio zawiera dokumentacja programu PTVS:

* [Projekty usługi w chmurze][Projekty usługi w chmurze]

Więcej szczegółów dotyczących korzystania z usług Azure na podstawie ról Sieć Web i Proces roboczy, czyli na przykład używanie usługi Azure Storage lub Service Bus, można znaleźć w następujących artykułach.

* [Blob Service][Blob Service]
* [Table Service][Table Service]
* [Queue Service][Queue Service]
* [Kolejki usługi Service Bus][Kolejki usługi Service Bus]
* [Tematy usługi Service Bus][Tematy usługi Service Bus]

<!--Link references-->

[Co to jest usługa w chmurze?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Centrum deweloperów języka Python]: /develop/python/

[Blob Service]: ../storage/storage-python-how-to-use-blob-storage.md
[Queue Service]: ../storage/storage-python-how-to-use-queue-storage.md
[Table Service]: ../storage/storage-python-how-to-use-table-storage.md
[Kolejki usługi Service Bus]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Tematy usługi Service Bus]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Dokumentacja narzędzi Python Tools for Visual Studio]: http://aka.ms/ptvsdocs
[Projekty usługi w chmurze]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[32-bitowe środowisko Python w wersji 2.7]: https://www.python.org/downloads/
[32-bitowe środowisko Python w wersji 3.5]: https://www.python.org/downloads/



<!--HONumber=Nov16_HO3-->


