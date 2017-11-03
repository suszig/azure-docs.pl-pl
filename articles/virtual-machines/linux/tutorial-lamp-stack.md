---
title: "Wdrażanie światła na maszynie wirtualnej systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Samouczek — instalacja stosu światła na maszynie Wirtualnej systemu Linux na platformie Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: c00e6a190633348411f47490808739d570cafd69
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a>Instalacja serwera sieci web światła w maszynie Wirtualnej platformy Azure
W tym artykule przedstawiono sposób wdrażania serwera sieci web Apache, MySQL i PHP (stos światła) na maszynie Wirtualnej systemu Ubuntu na platformie Azure. Jeśli wolisz NGINX serwera sieci web, zobacz [stosu LEMP](tutorial-lemp-stack.md) samouczka. Aby wyświetlić serwera światła w akcji, można opcjonalnie zainstalować i skonfigurować witrynę WordPress. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej systemu Ubuntu ("L" w stosie światła)
> * Otwieranie portu 80 na potrzeby ruchu w sieci Web
> * Instalowanie Apache, MySQL i PHP
> * Sprawdź, instalacja i Konfiguracja
> * Zainstaluj program WordPress na serwerze światła


Aby uzyskać więcej informacji na temat stosu światło, w tym zalecenia w środowisku produkcyjnym zobacz [dokumentacji Ubuntu](https://help.ubuntu.com/community/ApacheMySQLPHP).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Instalowanie Apache, MySQL i PHP

Uruchom następujące polecenie, aby zaktualizować źródła pakietu Ubuntu i zainstaluj Apache, MySQL i PHP. Należy pamiętać, daszek (^) na końcu polecenia.


```bash
sudo apt update && sudo apt install lamp-server^
```



Monit, aby zainstalować pakiety i innych zależności. Po wyświetleniu monitu ustawienia hasła głównego dla programu MySQL, a następnie [Enter], aby kontynuować. Postępuj zgodnie z monitami pozostałych. Ten proces instaluje minimalne wymagane rozszerzeń PHP potrzebne do korzystania z MySQL PHP. 

![Strona hasła głównego MySQL][1]

## <a name="verify-installation-and-configuration"></a>Sprawdź, instalacja i Konfiguracja


### <a name="apache"></a>Apache

Sprawdź wersję programu Apache przy użyciu następującego polecenia:
```bash
apache2 -v
```

Apache zainstalowana, i port 80 jest otwarty do maszyny Wirtualnej teraz są dostępne serwera sieci web z Internetu. Aby wyświetlić stronę domyślne Ubuntu Apache2, otwórz przeglądarkę sieci web, a następnie wprowadź publiczny adres IP maszyny wirtualnej. Użyj publicznego adresu IP używanego do SSH do maszyny Wirtualnej:

![Apache domyślnej strony][3]


### <a name="mysql"></a>MySQL

Sprawdź wersję programu MySQL przy użyciu następującego polecenia (należy pamiętać, kapitału `V` parametru):

```bash
mysql -V
```

Zaleca się uruchomienie następujący skrypt, aby pomóc w zabezpieczeniu instalacji MySQL:

```bash
mysql_secure_installation
```

Wprowadź hasło głównego MySQL i skonfiguruj ustawienia zabezpieczeń dla danego środowiska.

Jeśli chcesz utworzyć bazę danych MySQL, dodać użytkowników, lub zmienić ustawienia konfiguracji, zaloguj się do MySQL:

```bash
mysql -u root -p
```

Po zakończeniu zamknij wiersz mysql, wpisując `\q`.

### <a name="php"></a>PHP

Sprawdź wersję programu PHP za pomocą następującego polecenia:

```bash
php -v
```

Jeśli chcesz przetestować dalsze, Utwórz szybkie stronę informacji PHP do wyświetlania w przeglądarce. Poniższe polecenie tworzy stronę informacji PHP:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Teraz można sprawdzić stronę informacji PHP utworzony. Otwórz przeglądarkę i przejdź do `http://yourPublicIPAddress/info.php`. Należy zastąpić publicznego adresu IP maszyny Wirtualnej. Powinien być podobny do tego obrazu.

![Strona informacje o PHP][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a>Następne kroki

W tym samouczku wdrożono serwer światła na platformie Azure. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej systemu Ubuntu
> * Otwieranie portu 80 na potrzeby ruchu w sieci Web
> * Instalowanie Apache, MySQL i PHP
> * Sprawdź, instalacja i Konfiguracja
> * Zainstaluj program WordPress na serwerze światła

Przejście do następnym samouczku, aby dowiedzieć się, jak zabezpieczyć serwerów sieci web za pomocą certyfikatów SSL.

> [!div class="nextstepaction"]
> [Zabezpieczenia serwera sieci web przy użyciu protokołu SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png