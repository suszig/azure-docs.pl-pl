---
title: "Rozwiązywanie problemów z błędami klienta Docker w systemie Windows za pomocą programu Visual Studio | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów występujących podczas tworzenia i wdrażania aplikacji sieci web do Docker w systemie Windows za pomocą programu Visual Studio przy użyciu programu Visual Studio."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 89fa04a1107b6abb49aefd68066443717ac9b731
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a>Rozwiązywanie problemów z programu Visual Studio Docker programowanie

Podczas pracy z programu Visual Studio Tools dla platformy Docker w wersji zapoznawczej, mogą wystąpić problemy z powodu charakteru wersji zapoznawczej.
Poniżej przedstawiono niektóre typowe problemy i rozwiązania.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Kontenery systemu Linux**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Tworzenie występują błędy podczas debugowania aplikacji sieci web lub konsoli .NET Core  

Może to być związane z nie udostępnianie dysku, na którym znajduje się projekt Docker dla systemu Windows.  Może zostać wyświetlony następujący błąd:

```
The "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with the default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
Aby rozwiązać ten problem:

1. Kliknij prawym przyciskiem myszy **Docker dla systemu Windows** w obszarze powiadomień, a następnie wybierz **ustawienia**.  
2. Wybierz **udostępnione dyski** i udostępnić dysk, na którym znajduje się projekt.

### <a name="windows-containers"></a>**Kontenery systemu Windows**

Następujące problemy są specyficzne dla aplikacji sieci web i konsoli w kontenerach Windows .NET Framework — profilowanie.

#### <a name="prerequisites"></a>Wymagania wstępne

1. Visual Studio 2017 RC (lub nowsza) .NET Core i Podgląd Docker obciążenia musi być zainstalowany.
2. Poprawek systemu Windows 10 Anniversary aktualizacji za pomocą najnowszej Windows Update. W szczególności [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) musi być zainstalowany. 
3. [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/) (kompilacji 1.13.0 lub nowszym) musi być zainstalowany.
4. **Przełącz się do systemu Windows kontenery** musi być zaznaczone. W obszarze powiadomień, kliknij przycisk **Docker dla systemu Windows**, a następnie wybierz **przełączyć się do systemu Windows kontenery**. Po ponownym uruchomieniu komputera, upewnij się, że to ustawienie jest zachowywane.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Dane wyjściowe konsoli nie jest wyświetlany w oknie danych wyjściowych programu Visual Studio podczas debugowania aplikacji konsoli

Jest to znany problem dotyczący debuger programu Visual Studio (msvsmon.exe), który obecnie nie jest przeznaczony dla tego scenariusza. Obsługa tego scenariusza, mogą zostać dołączone w przyszłej wersji. Aby wyświetlić dane wyjściowe z aplikacji konsoli w programie Visual Studio, użyj **Docker: Uruchom projekt**, który jest odpowiednikiem **uruchomić bez debugowania**.

#### <a name="debugging-web-applications-with-the-release-configuration-fails-with-403-forbidden-error"></a>Debugowanie aplikacji sieci web z wersji konfiguracji nie powiodło się błędem (403) zabroniony

Aby obejść ten problem, otwórz web.release.config w rozwiązaniu i Oznacz jako komentarz lub usunąć następujące wiersze:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Kontenery systemu Linux**

#### <a name="unable-to-validate-volume-mapping"></a>Nie można sprawdzić poprawności mapowania woluminu
Mapowanie woluminu jest wymagane na udostępnianie folderu aplikacji w kontenerze kod źródłowy i plików binarnych aplikacji.  Określony wolumin mapowania są zawarte w docker compose.dev.debug.yml i docker compose.dev.release.yml. Jak na komputerze hosta zostały zmienione pliki, kontenery odzwierciedlenia tych zmian w strukturze folderu podobne.

Aby włączyć mapowanie woluminu:

1. Kliknij przycisk **Moby** w obszarze powiadomień, a następnie wybierz **ustawienia**.
2. Wybierz **udostępnionych dysków**.
3. Wybierz dysk, który jest hostem projektu i dysku, na którym znajduje się % USERPROFILE %.
4. Kliknij przycisk **Zastosuj**.

Aby sprawdzić, czy działa mapowanie woluminu, ponowne skompilowanie i wybierz F5 z poziomu programu Visual Studio po co najmniej jednej stacji zostały udostępnione, lub uruchom polecenie w wierszu polecenia następujący kod.

> [!NOTE]
> W tym przykładzie przyjęto założenie, że folder użytkowników znajduje się na dysku C i został udostępniony.
> Sprawdź, czy w razie potrzeby, jeśli udostępniasz inny dysk.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Uruchom poniższy kod w kontenerze systemu Linux.

```
/ # ls
```

Powinny pojawić się katalogu z folderu publicznego i użytkowników. Jeśli żadne pliki nie są wyświetlane i folderze /c/Users/Public nie jest pusta, mapowanie wolumin nie jest prawidłowo skonfigurowany.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Przejdź do katalogu tunel, aby wyświetlić zawartość `/c/Users/Public` katalogu:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> Podczas pracy z maszyn wirtualnych systemu Linux kontenera systemu plików jest rozróżniana wielkość liter.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>Kompilacji: Wystąpił nieoczekiwany błąd zadania "PrepareForBuild"

Microsoft.DotNet.Docker.CommandLine.ClientException: Wystąpił błąd podczas próby nawiązania połączenia.

Sprawdź, czy działa host Docker domyślne. Otwórz wiersz polecenia i wykonać:

```
docker info
```

Jeśli to zwraca błąd, spróbuj uruchomić **Docker dla systemu Windows** aplikacji komputerowej. Jeśli aplikacja klasyczna jest uruchomiona, następnie **Moby** powinny być widoczne w obszarze powiadomień. Kliknij prawym przyciskiem myszy **Moby** , a następnie otwórz **ustawienia**. Kliknij przycisk **zresetować**, a następnie ponownie uruchom Docker.

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Występuje okna dialogowego błędu, gdy próbuje dodać obsługę Docker lub debugowania aplikacji platformy ASP.NET Core w kontenerze (F5)

Po odinstalowaniu i instalowanie rozszerzeń, pamięć podręczna Managed Extensibility Framework (MEF) w programie Visual Studio może ulec uszkodzeniu. W takiej sytuacji może spowodować różnych komunikaty o błędach, gdy użytkownik jest dodanie obsługi Docker i/lub próby uruchamiania lub debugowania aplikacji ASP.NET Core (F5). Jako celu tymczasowego obejścia problemu wykonaj następujące kroki, aby usunąć i ponownie wygenerować pamięć podręczna MEF.

1. Zamknij wszystkie wystąpienia programu Visual Studio.
1. Otwórz % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.
1. Usuń następujące foldery:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Otwórz program Visual Studio.
1. Spróbuj ponownie wykonać scenariusz.
