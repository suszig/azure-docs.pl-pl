---
title: "Wdrażanie LEMP na maszynie wirtualnej systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Samouczek — instalacja stosu LEMP na maszynie Wirtualnej systemu Linux na platformie Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: 87d60ae51aaa33b709d272605419fd85eeb5d93d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Zainstaluj serwer sieci web LEMP na maszynie Wirtualnej platformy Azure
W tym artykule przedstawiono sposób wdrażania serwera sieci web NGINX, MySQL i PHP (stos LEMP) na maszynie Wirtualnej systemu Ubuntu na platformie Azure. Stos LEMP stanowi alternatywę dla popularnych [stosu światła](tutorial-lamp-stack.md), które można także zainstalować na platformie Azure. Aby wyświetlić serwera LEMP w akcji, można opcjonalnie zainstalować i skonfigurować witrynę WordPress. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej systemu Ubuntu ("L" w stosie LEMP)
> * Otwieranie portu 80 na potrzeby ruchu w sieci Web
> * Zainstaluj NGINX, MySQL i PHP
> * Sprawdź, instalacja i Konfiguracja
> * Zainstaluj program WordPress na serwerze LEMP


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Zainstaluj NGINX, MySQL i PHP

Uruchom następujące polecenie, aby zaktualizować źródła pakietu Ubuntu i zainstaluj NGINX, MySQL i PHP. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

Monit, aby zainstalować pakiety i innych zależności. Po wyświetleniu monitu ustawienia hasła głównego dla programu MySQL, a następnie [Enter], aby kontynuować. Postępuj zgodnie z monitami pozostałych. Ten proces instaluje minimalne wymagane rozszerzeń PHP potrzebne do korzystania z MySQL PHP. 

![Strona hasła głównego MySQL][1]

## <a name="verify-installation-and-configuration"></a>Sprawdź, instalacja i Konfiguracja


### <a name="nginx"></a>NGINX

Sprawdź wersję programu NGINX przy użyciu następującego polecenia:
```bash
nginx -v
```

Z NGINX zainstalowane, a port 80 jest otwarty do maszyny Wirtualnej teraz są dostępne serwera sieci web z Internetu. Aby wyświetlić stronę powitalną NGINX, otwórz przeglądarkę sieci web, a następnie wprowadź publiczny adres IP maszyny wirtualnej. Użyj publicznego adresu IP używanego do SSH do maszyny Wirtualnej:

![NGINX domyślnej strony][3]


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

Skonfiguruj NGINX, aby użyć Menedżera procesu FastCGI PHP (PHP FPM). Uruchom następujące polecenia, aby utworzyć kopię zapasową oryginalnego pliku konfiguracji blok serwera NGINX, a następnie edytuj oryginalny plik w wybranym edytorze:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

W edytorze, Zastąp zawartość `/etc/nginx/sites-available/default` następującym kodem. Zobacz komentarze opis ustawienia. Zastąp publicznego adresu IP dla maszyny Wirtualnej *yourPublicIPAddress*i pozostawić pozostałe ustawienia. Następnie zapisz plik.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Sprawdź konfigurację NGINX błędy składniowe:

```bash
sudo nginx -t
```

Jeśli składnia jest poprawna, uruchom ponownie NGINX za pomocą następującego polecenia:

```bash
sudo service nginx restart
```

Jeśli chcesz przetestować dalsze, Utwórz szybkie stronę informacji PHP do wyświetlania w przeglądarce. Poniższe polecenie tworzy stronę informacji PHP:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Teraz można sprawdzić stronę informacji PHP utworzony. Otwórz przeglądarkę i przejdź do `http://yourPublicIPAddress/info.php`. Należy zastąpić publicznego adresu IP maszyny Wirtualnej. Powinien być podobny do tego obrazu.

![Strona informacje o PHP][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Następne kroki

W tym samouczku wdrożono serwer LEMP na platformie Azure. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej systemu Ubuntu
> * Otwieranie portu 80 na potrzeby ruchu w sieci Web
> * Zainstaluj NGINX, MySQL i PHP
> * Sprawdź, instalacja i Konfiguracja
> * Zainstaluj program WordPress na stosie LEMP

Przejście do następnym samouczku, aby dowiedzieć się, jak zabezpieczyć serwerów sieci web za pomocą certyfikatów SSL.

> [!div class="nextstepaction"]
> [Zabezpieczenia serwera sieci web przy użyciu protokołu SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
