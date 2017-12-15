---
title: "Wdrażanie aplikacji w usłudze Azure App Service przy użyciu pliku ZIP | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć aplikację w usłudze Azure App Service przy użyciu pliku ZIP."
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: cephalin;sisirap
ms.openlocfilehash: a0e4df0ef0a1c873f1efcac1d8dbfe3cada18218
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-file"></a>Wdrażanie aplikacji w usłudze Azure App Service przy użyciu pliku ZIP

W tym artykule przedstawiono sposób wdrażania aplikacji sieci web przy użyciu pliku ZIP [usłudze Azure App Service](app-service-web-overview.md). 

To wdrożenie pliku ZIP używa tej samej usługi Kudu tego uprawnienia ciągłej integracji wdrożenia oparte na. Aparat kudu dla wdrożenia pliku ZIP obsługuje następujące funkcje: 

- Usuwanie plików pozostała z poprzedniego wdrożenia.
- Przywracanie pakietów opcję, aby włączyć domyślny proces kompilacji, w tym.
- [Dostosowanie wdrożenia](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), takie jak uruchamianie skryptów wdrażania.  
- Dzienniki wdrożenia. 

Aby uzyskać więcej informacji, zobacz [dokumentacji Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym artykule:

* [Utwórz aplikację usługi aplikacji](/azure/app-service/), lub użyć utworzonego w samouczku innej aplikacji.

## <a name="create-a-project-zip-file"></a>Utwórz plik ZIP projektu

>[!NOTE]
> Jeśli pobrano pliki w pliku ZIP, Wyodrębnij pliki. Na przykład, jeśli plik ZIP został pobrany z usługi GitHub, nie można wdrożyć tego pliku jako — jest. GitHub dodaje dodatkowe katalogi zagnieżdżone, które nie działają z usługi aplikacji. 
>

W lokalnej okno terminalu przejdź do katalogu głównego projektu aplikacji. 

Ten katalog powinien zawierać plik wejścia do aplikacji sieci web, takich jak _index.html_, _index.php_, i _app.js_. Może również zawierać pakiet zarządzania plików, takich jak _project.json_, _composer.json_, _package.json_, _bower.json_i _requirements.txt_.

Utwórz archiwum ZIP wszystkich elementów w projekcie. Polecenie używa domyślne narzędzie terminala:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="upload-zip-file-to-cloud-shell"></a>Przekaż plik ZIP do powłoki chmury

Jeśli wybierzesz opcję należy zamiast tego uruchomić wiersza polecenia platformy Azure z terminala lokalnego, Pomiń ten krok.

Wykonaj kroki opisane w tym miejscu, aby przesłać plik ZIP do powłoki chmury. 

[!INCLUDE [app-service-web-upload-zip.md](../../includes/app-service-web-upload-zip-no-h.md)]

Aby uzyskać więcej informacji, zobacz [utrwalić plików w powłoce chmury Azure](../cloud-shell/persisting-shell-storage.md).

## <a name="deploy-zip-file"></a>Wdrażanie plików ZIP

Wdrażanie przekazanego pliku ZIP do aplikacji sieci web przy użyciu [az aplikacji sieci Web wdrożenia źródła config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) polecenia. Jeśli wybierzesz nie użyć powłoki chmury, upewnij się, wersja interfejsu wiersza polecenia Azure jest 2.0.21 lub nowszym. Aby wyświetlić wersję Uruchom `az --version` w lokalnym okno terminalu. 

Poniższy przykład wdraża pliku ZIP, który został przekazany. Korzystając z lokalną instalacją wiersza polecenia platformy Azure, określ ścieżkę do pliku ZIP lokalnego dla `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

To polecenie wdraża pliki i katalogi z ZIP pliku do domyślnego folderu aplikacji usługi App Service (`\home\site\wwwroot`) i uruchamia ponownie aplikację. Jeśli żaden proces, dodatkowe niestandardowej kompilacji jest skonfigurowana, jest uruchamiany również. Aby uzyskać więcej informacji, zobacz [dokumentacji Kudu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

Aby wyświetlić listę wdrożenia dla tej aplikacji, należy użyć interfejsów API REST (zobacz następną sekcję). 

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="next-steps"></a>Następne kroki

W przypadku bardziej zaawansowanych scenariuszy wdrażania, spróbuj [wdrażanie na platformie Azure za pomocą narzędzia Git](app-service-deploy-local-git.md). Wdrożenie oparte na Git na platformie Azure umożliwia kontroli wersji, Przywracanie pakietu MSBuild i więcej.

## <a name="more-resources"></a>Więcej zasobów

* [Program kudu: Wdrażanie z pliku zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Poświadczenia wdrożenia usługi aplikacji Azure](app-service-deploy-ftp.md)
