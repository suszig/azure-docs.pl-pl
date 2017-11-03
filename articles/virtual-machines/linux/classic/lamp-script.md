---
title: "Za pomocą rozszerzenia CustomScript na maszynie Wirtualnej systemu Linux | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać rozszerzenia CustomScript do wdrażania aplikacji na maszynach wirtualnych systemu Linux na platformie Azure utworzonych przy użyciu klasycznego modelu wdrażania."
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.openlocfilehash: cb1fc9a44dc9e57d9cc9f1c546ad937d67e63c2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Wdrażanie aplikacji LAMP przy użyciu rozszerzenia Azure CustomScript dla systemu Linux
> [!IMPORTANT] 
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Aby uzyskać informacje o wdrażaniu stosu światła przy użyciu modelu Resource Manager, zobacz [tutaj](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Rozszerzenia CustomScript Microsoft Azure dla systemu Linux umożliwia dostosowywanie maszyn wirtualnych (VM), uruchamiając dowolny kod napisany w dowolnym języku skryptowym obsługiwane przez maszynę Wirtualną (na przykład, Python i Bash). Zapewnia bardzo elastyczne sposobem zautomatyzowania wdrożenia aplikacji na wielu komputerach.

Można wdrożyć rozszerzenia CustomScript przy użyciu portalu Azure, programu Windows PowerShell lub interfejsu wiersza polecenia platformy Azure (Azure CLI).

W tym artykule, które będą używane wiersza polecenia platformy Azure, aby wdrożyć prostą aplikację światła do maszyny Wirtualnej systemu Ubuntu utworzone przy użyciu klasycznego modelu wdrażania.

## <a name="prerequisites"></a>Wymagania wstępne
W tym przykładzie należy najpierw utworzyć dwie maszyny wirtualne Azure systemem Ubuntu 14.04 lub nowszej. Maszyny wirtualne są nazywane *maszyny wirtualnej skryptu* i *maszyny wirtualnej światła*. Użyj unikatowej nazwy podczas tworzenia maszyn wirtualnych. Jeden służy do uruchamiania polecenia interfejsu wiersza polecenia i jeden służy do wdrażania aplikacji światła.

Należy również konta usługi Azure Storage i klucz dostępu do niego (można uzyskać tę z portalu Azure).

Jeśli potrzebujesz, aby uzyskać pomoc przy tworzeniu maszyn wirtualnych systemu Linux na platformie Azure, zapoznaj się [tworzenia maszyny wirtualnej systemem Linux](createportal.md).

Ubuntu założenia instalacji, ale istnieje możliwość dostosowania instalacji dla dowolnej obsługiwanej distro systemu Linux.

Maszyny Wirtualnej vm skryptu musi mieć zainstalowane z połączeniem pracy Azure wiersza polecenia platformy Azure. Aby uzyskać pomoc na ten temat, zobacz [Instalowanie i Konfigurowanie interfejsu wiersza polecenia platformy Azure](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>Przekaż skryptu
Rozszerzenia CustomScript będą używane do uruchamiania skryptu na zdalnej maszyny Wirtualnej, aby zainstalować stosu światła i utworzyć stronę PHP. Aby uzyskać dostęp do skryptu z dowolnego miejsca będzie przekazać jako obiektów blob platformy Azure.

### <a name="script-overview"></a>Przegląd skryptu
Przykładowy skrypt instaluje stosu światła Ubuntu (w tym konfigurowania instalacji dyskretnej programu MySQL), zapisuje prostego pliku PHP i uruchamia Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Przekaż skryptu
Zapisz skrypt jako plik tekstowy, na przykład *install_lamp.sh*, a następnie przekaż go do magazynu Azure. Można to zrobić łatwo z wiersza polecenia platformy Azure. Poniższy przykład powoduje przekazanie pliku do kontenera magazynu o nazwie "skrypty". Kontener nie istnieje. należy najpierw utworzyć.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Utwórz również plik JSON, który opisano, jak pobrać skrypt z usługi Magazyn Azure. Zapisz jako *public_config.json* (zastępując "mystorage" z nazwą konta magazynu):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Wdrażanie rozszerzenia
Teraz służy następnego polecenia do wdrożenia za pomocą rozszerzenia CustomScript Linux do zdalnego maszyny Wirtualnej przy użyciu wiersza polecenia platformy Azure.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Poprzednie polecenie pobiera i uruchamia *install_lamp.sh* skryptu na Maszynie wirtualnej o nazwie *maszyny wirtualnej światła*.

Ponieważ aplikacja obejmuje serwer sieci web, pamiętaj, aby otworzyć port nasłuchujący HTTP na zdalnej maszynie Wirtualnej za pomocą poniższego polecenia.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Monitorowanie i rozwiązywanie problemów
Możesz sprawdzić na jak niestandardowe skrypt jest uruchamiany przez wyszukiwanie w pliku dziennika na zdalnej maszynie Wirtualnej. SSH *maszyny wirtualnej światła* i tail pliku dziennika za pomocą poniższego polecenia.

    cd /var/log/azure/customscript
    tail -f handler.log

Po uruchomieniu rozszerzenia CustomScript, możesz przejść do strony PHP utworzone informacji. Strona PHP, na przykład w tym artykule jest *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Dodatkowe zasoby
Te same czynności podstawowe umożliwia wdrażanie złożonych aplikacji. W tym przykładzie skrypt instalacji został zapisany jako publiczny obiektów blob w usłudze Azure Storage. Opcja bardziej bezpieczna będzie przechowywać skrypt instalacji jako bezpieczne obiektów blob z [bezpiecznego dostępu podpisu](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Dodatkowe zasoby dla wiersza polecenia platformy Azure, Linux i rozszerzenia CustomScript są wyświetlane obok.

[Automatyzowanie zadań dostosowania maszyny Wirtualnej systemu Linux za pomocą rozszerzenia CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Rozszerzenia Azure Linux (GitHub)](https://github.com/Azure/azure-linux-extensions)