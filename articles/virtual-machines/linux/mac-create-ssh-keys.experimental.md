---
title: Tworzenie pary kluczy SSH dla maszyn wirtualnych z systemem Linux na platformie Azure | Microsoft Docs
description: "Bezpiecznie utwórz parę kluczy SSH, prywatny i publiczny, dla maszyn wirtualnych z systemem Linux."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: rasquill
experiment_id: rasquill-ssh-20170308
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 19acd4efca7ef043f31b436b96f9129caee9591b
ms.lasthandoff: 04/03/2017



---

# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Tworzenie pary kluczy prywatnych i publicznych SSH dla maszyn wirtualnych z systemem Linux

W tym artykule przedstawiono sposób generowania pary plików prywatnych i publicznych kluczy RSA protokołu SSH w wersji 2 do użytku z maszynami wirtualnymi z systemem Linux.  Para kluczy SSH umożliwia tworzenie na platformie Azure maszyn wirtualnych, które domyślnie używają kluczy SSH do uwierzytelniania, eliminując konieczność logowania przy użyciu haseł.  Hasła można złamać i narażają one maszyny wirtualne na bezlitosne próby ataków siłowych w celu odgadnięcia hasła. Maszyny wirtualne tworzone za pomocą szablonów platformy Azure lub interfejsu `azure-cli` mogą mieć dołączony publiczny klucz SSH w ramach wdrożenia, co eliminuje potrzebę wykonania po wdrożeniu kroku konfiguracji polegającego na wyłączeniu logowania przy użyciu haseł dla protokołu SSH.

## <a name="quick-commands"></a>Szybkie polecenia

Uruchom następujące polecenia z poziomu powłoki Bash, zastępując przykłady własnymi wartościami.

Domyślnie plik klucza publicznego SSH jest tworzony w folderze `~/.ssh/id_rsa.pub`. Gdy po użyciu następującego polecenia pojawi się monit, utwórz hasło zabezpieczające klucz prywatny. (Klucz prywatny zostanie zaszyfrowany przy użyciu tego hasła).

```bash
ssh-keygen -t rsa -b 2048 
```

Dodaj nowo utworzony klucz do `ssh-agent`:

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> Powyższe polecenia działają prawie we wszystkich dystrybucjach systemu operacyjnego Linux. Mogą nie działać w kontenerach ze względu na radykalne ograniczenia środowiska. 

## <a name="detailed-walkthrough"></a>Szczegółowy przewodnik

Publiczne i prywatne klucze SSH umożliwiają logowanie do serwerów z systemem Linux w najłatwiejszy sposób. [Kryptografia klucza publicznego](https://en.wikipedia.org/wiki/Public-key_cryptography) zapewnia znacznie bezpieczniejsze logowanie do maszyn wirtualnych z systemem Linux lub BSD na platformie Azure niż hasła, które mogą być dużo łatwiej złamane za pomocą ataków siłowych.

> [!IMPORTANT]
> Klucz publiczny można udostępniać wszystkim, ale tylko Ty (lub lokalna infrastruktura zabezpieczeń) posiadasz klucz prywatny.  Prywatny klucz SSH powinien być zabezpieczony [bardzo bezpiecznym hasłem](https://www.xkcd.com/936/) (źródło:[xkcd.com](https://xkcd.com)).  To hasło służy wyłącznie do uzyskiwania dostępu do prywatnego klucza SSH i **nie jest** hasłem do konta użytkownika.  Dodanie hasła do klucza SSH spowoduje zaszyfrowanie klucza prywatnego przy użyciu 128-bitowego standardu AES, aby odszyfrowanie i użycie klucza nie było możliwe bez podania hasła.  Jeżeli osobie niepowołanej udałoby się wykraść Twój klucz prywatny, który nie został zabezpieczony hasłem, osoba ta mogłaby użyć tego klucza w celu zalogowania się na dowolne serwery z odpowiadającym mu kluczem publicznym.  Zabezpieczenie klucza prywatnego hasłem zapewnia dodatkową warstwę ochronną dla infrastruktury na platformie Azure, a osoba niepowołana nie może go użyć.

W tym artykule utworzono pliki prywatnych i publicznych kluczy RSA protokołu SSH w wersji 2, które są zalecane w przypadku wdrożeń przy użyciu usługi Resource Manager.  Klucze *ssh-rsa* są wymagane w [portalu](https://portal.azure.com) w przypadku zarówno wdrożeń klasycznych, jak i wdrożeń przy użyciu usługi Resource Manager.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Wyłączanie haseł SSH przy użyciu kluczy SSH

Platforma Azure wymaga, aby klucz publiczny i prywatny miały długość co najmniej 2048 bitów oraz format ssh-rsa. Aby utworzyć klucze, należy użyć funkcji `ssh-keygen`, która najpierw zadaje szereg pytań, a następnie zapisuje klucz prywatny i dopasowany klucz publiczny. Po utworzeniu maszyny wirtualnej platformy Azure klucz publiczny jest kopiowany do katalogu `~/.ssh/authorized_keys`.  Klucze SSH w katalogu `~/.ssh/authorized_keys` są używane jako wezwania klienta do dopasowania odpowiedniego klucza prywatnego w celu zalogowania za pomocą połączenia przy użyciu protokołu SSH.  W przypadku tworzenia maszyny wirtualnej platformy Azure z systemem Linux i możliwością uwierzytelniania za pomocą kluczy SSH platforma Azure konfiguruje serwer SSHD tak, aby zezwalał na użycie tylko kluczy SSH, a nie na logowanie przy użyciu hasła.  W związku z tym utworzenie maszyny wirtualnej platformy Azure z systemem Linux przy użyciu kluczy SSH może ułatwić zabezpieczenie wdrażania maszyny wirtualnej i nie trzeba będzie wtedy wykonywać typowego kroku konfiguracji po wdrażaniu, czyli wyłączenia haseł w pliku konfiguracji sshd_config.

## <a name="using-ssh-keygen"></a>Korzystanie z programu ssh-keygen

To polecenie umożliwia utworzenie zabezpieczonej (zaszyfrowanej) pary kluczy SSH o długości 2048 bitów w standardzie RSA. W celu dokonania łatwej identyfikacji zostanie ona opatrzona komentarzem.  

Klucze SSH są domyślnie przechowywane w katalogu `~/.ssh`.  Jeśli nie masz katalogu `~/.ssh`, polecenie `ssh-keygen` tworzy ten katalog z odpowiednimi uprawnieniami.

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

*Opis polecenia*

`ssh-keygen` — program używany do tworzenia kluczy

`-t rsa` — typ klucza do utworzenia, który jest w formacie RSA [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` — liczba bitów klucza


## <a name="classic-portal-and-x509-certs"></a>Portal klasyczny i certyfikaty X.509

Jeśli używasz [klasycznej witryny Azure Portal](https://manage.windowsazure.com/), do kluczy SSH potrzebne będą certyfikaty X.509.  Nie są dozwolone żadne inne typy publicznych kluczy SSH — *muszą* to być certyfikaty X.509.

Aby utworzyć certyfikat X.509 z istniejącego klucza prywatnego SSH-RSA:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Wdrażanie klasyczne przy użyciu programu `asm`

Jeśli używasz klasycznego modelu wdrażania (interfejsu wiersza polecenia usługi Azure Service Management `asm`), możesz użyć klucza publicznego SSH-RSA lub klucza w formacie RFC4716 w kontenerze **pem**.  Klucz publiczny SSH-RSA utworzono wcześniej w tym artykule przy użyciu polecenia `ssh-keygen`.

Aby utworzyć klucz w formacie RFC4716 przy użyciu istniejącego publicznego klucza SSH:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Przykład polecenia ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
The keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Zapisane pliki klucza:

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

Nazwa pary kluczy dla potrzeb tego artykułu.  Para kluczy o nazwie **id_rsa** jest domyślna. Niektóre narzędzia mogą wymagać podania nazwy **id_rsa** pliku klucza prywatnego, dlatego zaleca się jego utworzenie. Katalog `~/.ssh/` jest domyślną lokalizacją par kluczy SSH oraz pliku konfiguracyjnego SSH.  Jeśli nie określono pełnej ścieżki, polecenie `ssh-keygen` spowoduje utworzenie kluczy w bieżącym katalogu roboczym, a nie domyślnym katalogu `~/.ssh`.

Lista plików w katalogu `~/.ssh`.

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Hasło klucza:

`Enter passphrase (empty for no passphrase):`

Polecenie `ssh-keygen` odnosi się do hasła, za pomocą którego zaszyfrowano plik klucza prywatnego.  *Zdecydowanie* zalecane jest dodanie hasła do par kluczy. Bez hasła chroniącego klucz prywatny każda osoba mająca plik klucza może użyć go do logowania na dowolnym serwerze, który ma odpowiadający mu klucz publiczny. Dodanie hasła zwiększa ochronę w przypadku, gdy osoba niepowołana jest w stanie uzyskać dostęp do pliku klucza prywatnego. Dzięki temu zabezpieczeniu masz czas na dokonanie zmiany kluczy używanych do uwierzytelniania.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Korzystanie z programu ssh-agent do przechowywania hasła klucza prywatnego

Aby uniknąć wpisywania hasła do pliku klucza prywatnego przy każdym logowaniu przez protokół SSH, można skorzystać z programu `ssh-agent`, który przechowuje hasło w pamięci podręcznej. Jeśli korzystasz z komputera Mac, pęk kluczy systemu OS X bezpiecznie przechowa Twoje hasła do klucza prywatnego po wywołaniu programu `ssh-agent`.

Zweryfikuj program `ssh-agent` oraz polecenie `ssh-add` i użyj ich w celu poinformowania systemu SSH o plikach kluczy, aby nie trzeba było używać hasła interaktywnie.

```bash
eval "$(ssh-agent -s)"
```

Następnie dodaj klucz prywatny do programu `ssh-agent` przy użyciu polecenia `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Hasło klucza prywatnego jest teraz przechowywane w programie `ssh-agent`.

## <a name="using-ssh-copy-id-to-install-the-new-key"></a>Instalowanie nowego klucza przy użyciu polecenia `ssh-copy-id`
Jeśli już utworzono maszynę wirtualną, możesz zainstalować nowy klucz publiczny SSH na maszynie wirtualnej z systemem Linux za pomocą następującego polecenia, zastępując nazwę użytkownika maszyny wirtualnej i adres serwera własnymi wartościami:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Tworzenie i konfigurowanie pliku konfiguracyjnego SSH

Najlepszym rozwiązaniem jest utworzenie i skonfigurowanie pliku `~/.ssh/config` w celu przyspieszenia logowania i optymalizacji zachowania klienta SSH.

W poniższym przykładzie pokazano standardową konfigurację. 

### <a name="create-the-file"></a>Utwórz plik

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Edytuj plik, aby dodać nową konfigurację SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Przykład pliku `~/.ssh/config`:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Ta konfiguracja protokołu SSH udostępnia sekcje dla każdego serwera, aby umożliwić każdemu z nich posiadanie własnej pary kluczy. Ustawienia domyślne (`Host *`) są stosowane do hostów, które nie są zgodne z żadnym z określonych hostów we wcześniejszej części pliku konfiguracyjnego.

### <a name="config-file-explained"></a>Opis pliku konfiguracji

`Host` — nazwa hosta wywołanego na terminalu.  Polecenie `ssh fedora22` informuje protokół `SSH`, aby użyć wartości z bloku ustawień z etykietą `Host fedora22`. UWAGA: hostem może być jakakolwiek etykieta zrozumiała dla Ciebie, która jednocześnie nie jest faktyczną nazwą hosta żadnego serwera.

`Hostname 102.160.203.241` — adres IP lub nazwa DNS serwera, do którego uzyskiwany jest dostęp.

`User ahmet` — konto użytkownika zdalnego używane podczas logowania do serwera.

`PubKeyAuthentication yes` — informuje protokół SSH, że chcesz użyć klucza SSH do logowania.

`IdentityFile /home/ahmet/.ssh/id_id_rsa` — prywatny klucz SSH i odpowiedni klucz publiczny, który ma zostać użyty do uwierzytelniania.

## <a name="ssh-into-linux-without-a-password"></a>Korzystanie z protokołu SSH w systemie Linux bez użycia hasła

Jeśli masz już parę kluczy SSH i skonfigurowany plik konfiguracji SSH, możesz szybko i bezpiecznie zalogować się do maszyny wirtualnej systemu Linux. Podczas pierwszego logowania do serwera przy użyciu klucza SSH polecenie monituje o wpisanie hasła do pliku danego klucza.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Opis polecenia

Gdy polecenie `ssh fedora22` jest wykonywane, protokół SSH najpierw lokalizuje i ładuje wszystkie ustawienia z bloku `Host fedora22`, a następnie ładuje wszystkie pozostałe ustawienia z ostatniego bloku `Host *`.

## <a name="next-steps"></a>Następne kroki

W dalszej kolejności należy utworzyć maszyny wirtualne systemu Linux platformy Azure przy użyciu nowego klucza publicznego SSH.  Maszyny wirtualne platformy Azure, które zostały utworzone z publicznym kluczem SSH jako loginem, są lepiej chronione niż maszyny wirtualne utworzone z hasłami stosowanymi w domyślnej metodzie logowania.  W domyślnej konfiguracji maszyn wirtualnych platformy Azure utworzonych przy użyciu kluczy SSH hasła są wyłączone, aby uniknąć prób odgadnięcia hasła za pomocą ataków siłowych. Jeśli potrzebujesz więcej pomocy przy tworzeniu pary kluczy SSH lub potrzebujesz dodatkowych certyfikatów, na przykład do używania z portalem klasycznym, zobacz [Szczegółowe instrukcje dotyczące tworzenia par kluczy SSH i certyfikatów](create-ssh-keys-detailed.md).

* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu witryny Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

