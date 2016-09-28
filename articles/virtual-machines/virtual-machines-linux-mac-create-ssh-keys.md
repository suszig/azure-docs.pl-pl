<properties
    pageTitle="Tworzenie kluczy SSH w systemie Linux i na komputerach Mac | Microsoft Azure"
    description="Generuj klucze SSH i korzystaj z nich w systemach Linux oraz na komputerach Mac dla usługi Resource Manager i klasycznych modeli wdrażania na platformie Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="v-livech"/>


# Tworzenie kluczy SSH w systemie Linux i na komputerach Mac dla maszyn wirtualnych systemu Linux platformy Azure

Para kluczy SSH umożliwia tworzenie na platformie Azure maszyn wirtualnych, które domyślnie używają kluczy SSH do uwierzytelniania, eliminując konieczność logowania przy użyciu haseł.  Hasła można złamać i narażają one maszyny wirtualne na bezlitosne próby ataków siłowych w celu odgadnięcia hasła. Maszyny wirtualne tworzone za pomocą szablonów platformy Azure lub interfejsu `azure-cli` mogą mieć dołączony publiczny klucz SSH w ramach wdrożenia, eliminując potrzebę konfiguracji po wdrożeniu.  Jeśli łączysz się z maszyną wirtualną z systemem Linux z poziomu systemu Windows, zobacz [ten dokument.](virtual-machines-linux-ssh-from-windows.md)

## Lista szybkich poleceń

W poniższych przykładach poleceń zastąp wartości między znakami &lt; i &gt; wartościami z własnego środowiska.

```bash
ssh-keygen -t rsa -b 2048 -C "<your_user@yourdomain.com>"
```

Wprowadź nazwę pliku, który zostanie zapisany w katalogu `~/.ssh/`:

```bash
<azure_fedora_id_rsa>
```

Wprowadź hasło dla azure_fedora_id_rsa:

```bash
<correct horse battery staple>
```

Dodaj nowo utworzony klucz do elementu `ssh-agent` w systemie Linux i na komputerze Mac (dodany także do pęku kluczy systemu OS X):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/azure_fedora_id_rsa
```

Skopiuj klucz publiczny SSH na serwer z systemem Linux:

```bash
ssh-copy-id -i ~/.ssh/azure_fedora_id_rsa.pub <youruser@yourserver.com>
```

Przetestuj logowanie, używając kluczy zamiast hasła:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/azure_fedora_id_rsa <youruser@yourserver.com>
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## Wprowadzenie

Publiczne i prywatne klucze SSH umożliwiają logowanie do serwerów z systemem Linux w najłatwiejszy sposób. [Kryptografia klucza publicznego](https://en.wikipedia.org/wiki/Public-key_cryptography) zapewnia znacznie bezpieczniejsze logowanie do maszyn wirtualnych z systemem Linux lub BSD na platformie Azure niż hasła, które mogą być dużo łatwiej złamane za pomocą ataków siłowych. Klucz publiczny można udostępniać wszystkim, ale tylko Ty (lub lokalna infrastruktura zabezpieczeń) posiadasz klucz prywatny.  Prywatny klucz SSH powinien być zabezpieczony [bardzo bezpiecznym hasłem ](https://www.xkcd.com/936/) (źródło:[xkcd.com](https://xkcd.com)).  To hasło służy wyłącznie do uzyskiwania dostępu do prywatnego klucza SSH i **nie jest** hasłem do konta użytkownika.  Dodanie hasła do Twojego klucza SSH koduje klucz prywatny, aby jego użycie nie było możliwe bez podania hasła.  Jeżeli osobie niepowołanej udałoby się wykraść Twój klucz prywatny, który nie został zabezpieczony hasłem, osoba ta mogłaby użyć tego klucza w celu zalogowania się na dowolne serwery z odpowiadającym mu kluczem publicznym.  Zabezpieczenie klucza prywatnego hasłem zapewnia dodatkową warstwę ochronną dla infrastruktury na platformie Azure, a osoba niepowołana nie może go użyć.



W tym artykule przedstawiono sposób tworzenia plików kluczy w formacie *ssh-rsa*, który jest zalecany w przypadku wdrożeń klasycznych i wdrożeń przy użyciu usługi Resource Manager.  Klucze *ssh-rsa* są wymagane w [portalu](https://portal.azure.com) w przypadku wdrożeń klasycznych i wdrożeń przy użyciu usługi Resource Manager.


## Tworzenie kluczy SSH

Platforma Azure wymaga, aby klucz publiczny i prywatny miały długość co najmniej 2048 bitów oraz format ssh-rsa. Aby utworzyć klucze, należy użyć funkcji `ssh-keygen`, która najpierw zadaje szereg pytań, a następnie zapisuje klucz prywatny i dopasowany klucz publiczny. Po utworzeniu maszyny wirtualnej platformy Azure klucz publiczny jest kopiowany do katalogu `~/.ssh/authorized_keys`.  Klucze SSH w katalogu `~/.ssh/authorized_keys` są używane jako wezwania klienta do dopasowania odpowiedniego klucza prywatnego w celu zalogowania za pomocą połączenia przy użyciu protokołu SSH.


## Korzystanie z programu ssh-keygen

To polecenie umożliwia utworzenie zabezpieczonej (zaszyfrowanej) pary kluczy SSH o długości 2048 bitów w standardzie RSA. W celu dokonania łatwej identyfikacji zostanie ona opatrzona komentarzem.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
```

_Opis polecenia_

`ssh-keygen` = program używany do tworzenia kluczy

`-t rsa` = typ klucza do utworzenia w [formacie RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = liczba bitów klucza

`-C "ahmet@fedoraVMAzure"` = komentarz dodany na końcu pliku klucza publicznego, aby umożliwić jego łatwą identyfikację.  Zwykle jako komentarz używany jest adres e-mail, ale można też użyć innych informacji, które sprawdzą się najlepiej w danej infrastrukturze.

### Korzystanie z kluczy PEM

W przypadku korzystania z klasycznego modelu wdrażana (klasyczny portal Azure lub interfejs wiersza polecenia usługi Azure Service Management `asm`) do uzyskania dostępu do maszyn wirtualnych z systemem Linux może być konieczne użycie kluczy SSH w formacie PEM.  Oto jak utworzyć klucz PEM przy użyciu istniejącego publicznego klucza SSH i istniejącego certyfikatu standardu x509.

Aby utworzyć klucz w formacie PEM przy użyciu istniejącego publicznego klucza SSH:

```bash
ssh-keygen -f id_rsa.pub -m 'PEM' -e > id_rsa.pem
```

## Przewodnik po poleceniu ssh-keygen

Każdy krok jest szczegółowo omówiony.  Zacznij od uruchomienia polecenia `ssh-keygen`.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in azure_fedora_id_rsa.
Your public key has been saved in azure_fedora_id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@fedoraVMAzure
The key's randomart image is:
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

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa`

Nazwa pary kluczy dla potrzeb tego artykułu.  Para kluczy o nazwie **id_rsa** jest domyślna. Niektóre narzędzia mogą wymagać podania nazwy **id_rsa** pliku klucza prywatnego, dlatego zaleca się jego utworzenie. Katalog `~/.ssh/` jest domyślną lokalizacją par kluczy SSH oraz pliku konfiguracyjnego SSH.

```bash
ahmet@fedora$ ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 azure_fedora_id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 azure_fedora_id_rsa.pub
```
Lista plików w katalogu `~/.ssh`. `ssh-keygen` tworzy katalog `~/.ssh`, jeśli taki jeszcze nie istnieje, a także ustawia poprawne prawo własności i atrybuty plików.

Hasło klucza:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` hasło (password) jest nazywane „passphrase”.  *Zdecydowanie* zalecane jest dodanie hasła do par kluczy. Bez hasła chroniącego parę kluczy każda osoba mająca plik klucza prywatnego może użyć go do logowania na dowolnym serwerze, który ma odpowiadający mu klucz publiczny. Dodanie hasła zwiększa ochronę w przypadku, gdy osoba niepowołana jest w stanie uzyskać dostęp do Twojego pliku klucza prywatnego. Dzięki temu zabezpieczeniu masz czas na dokonanie zmiany kluczy używanych do uwierzytelnienia.

## Korzystanie z programu ssh-agent do przechowywania hasła klucza prywatnego

Aby uniknąć wpisywania hasła do pliku klucza prywatnego przy każdym logowaniu przez protokół SSH, można skorzystać z programu `ssh-agent`, który przechowuje hasło w pamięci podręcznej. Jeśli korzystasz z komputera Mac, pęk kluczy systemu OS X bezpiecznie przechowa Twoje hasła do klucza prywatnego po wywołaniu programu `ssh-agent`.

Najpierw upewnij się, że program `ssh-agent` jest uruchomiony.

```bash
eval "$(ssh-agent -s)"
```

Następnie dodaj klucz prywatny do programu `ssh-agent` przy użyciu polecenia `ssh-add`.

```bash
ssh-add ~/.ssh/azure_fedora_id_rsa
```

Hasło klucza prywatnego jest teraz przechowywane w programie `ssh-agent`.

## Tworzenie i konfigurowanie pliku konfiguracyjnego SSH

Zalecanym najlepszym rozwiązaniem jest utworzenie i skonfigurowanie pliku `~/.ssh/config` w celu przyspieszenia logowania i optymalizacji zachowania klienta SSH.

W poniższym przykładzie pokazano standardową konfigurację. 

### Utwórz plik

```bash
touch ~/.ssh/config
```

### Edytuj plik, aby dodać nową konfigurację SSH:

```bash
vim ~/.ssh/config
```

### Przykład pliku `~/.ssh/config`:

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


### Opis pliku konfiguracji

`Host` = nazwa hosta wywołanego na terminalu.  `ssh fedora22` informuje protokół `SSH`, aby użyć wartości z bloku ustawień z etykietą `Host fedora22` UWAGA: Może to być jakakolwiek etykieta zrozumiała dla Ciebie, która jednocześnie nie jest nazwą hosta żadnego serwera.

`Hostname 102.160.203.241` = adres IP lub nazwa DNS serwera, do którego uzyskiwany jest dostęp.

`User git` = zdalne konto użytkownika, które ma zostać użyte.

`PubKeyAuthentication yes` = informuje protokół SSH, że chcesz użyć klucza SSH do logowania.

`IdentityFile /home/ahmet/.ssh/id_id_rsa` = prywatny klucz SSH i odpowiedni klucz publiczny, który ma zostać użyty do uwierzytelniania.


## Korzystanie z protokołu SSH w systemie Linux bez użycia hasła

Jeśli masz już parę kluczy SSH i skonfigurowany plik konfiguracji SSH, możesz szybko i bezpiecznie zalogować się do maszyny wirtualnej systemu Linux. Podczas pierwszego logowania do serwera przy użyciu klucza SSH polecenie monituje o wpisanie hasła do pliku danego klucza.

```bash
ssh fedora22
```

### Opis polecenia

Gdy polecenie `ssh fedora22` jest wykonywane, protokół SSH najpierw lokalizuje i ładuje wszystkie ustawienia z bloku `Host fedora22`, a następnie ładuje wszystkie pozostałe ustawienia z ostatniego bloku `Host *`.

## Następne kroki

W dalszej kolejności należy utworzyć maszyny wirtualne systemu Linux platformy Azure przy użyciu nowego klucza publicznego SSH.  Maszyny wirtualne platformy Azure, które zostały utworzone z publicznym kluczem SSH jako loginem, są lepiej chronione niż maszyny wirtualne utworzone z hasłami stosowanymi w domyślnej metodzie logowania.  W domyślnej konfiguracji maszyn wirtualnych platformy Azure utworzonych przy użyciu kluczy SSH hasła są wyłączone, aby uniknąć prób odgadnięcia hasła za pomocą ataków siłowych.

- [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu witryny Azure Portal](virtual-machines-linux-quick-create-portal.md)
- [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-quick-create-cli.md)



<!--HONumber=Sep16_HO3-->


