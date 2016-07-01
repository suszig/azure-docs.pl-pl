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
    ms.date="05/16/2016"
    ms.author="v-livech"/>

# Tworzenie kluczy SSH w systemie Linux i na komputerach Mac dla maszyn wirtualnych systemu Linux platformy Azure

Aby utworzyć zabezpieczony hasłem klucz publiczny i prywatny SSH, należy otworzyć terminal na stacji roboczej.  Wygenerowane klucze SSH mogą zostać domyślnie użyte przy tworzeniu maszyn wirtualnych. Istnieje też możliwość dodania klucza publicznego do istniejących maszyn wirtualnych za pomocą interfejsu wiersza polecenia platformy Azure i szablonów platformy Azure.  Umożliwi to logowanie przez protokół SSH bez podawania hasła, przy użyciu bezpieczniejszego uwierzytelniania metodą kluczy zamiast haseł.

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

Najłatwiejszym sposobem logowania się do serwerów systemu Linux jest użycie kluczy publicznych i prywatnych SSH. [Szyfrowanie kluczem publicznym](https://en.wikipedia.org/wiki/Public-key_cryptography) jest bezpieczniejszym sposobem logowania do maszyny wirtualnej systemu Linux lub BSD na platformie Azure niż hasła, które mogą zostać złamane znacznie łatwiej metodą siłową. Klucz publiczny można udostępniać wszystkim, ale tylko Ty (lub lokalna infrastruktura zabezpieczeń) posiadasz klucz prywatny.  Wygenerowany klucz prywatny SSH będzie mieć [bezpieczne hasło](https://www.xkcd.com/936/) w celu jego ochrony. Hasło to umożliwia wyłącznie dostęp do klucza prywatnego SSH i **nie jest** hasłem do konta użytkownika.  Dodanie hasła do Twojego klucza SSH koduje klucz prywatny, aby jego użycie nie było możliwe bez podania hasła.  Jeżeli osobie niepowołanej udałoby się wykraść Twój klucz prywatny, który nie został zabezpieczony hasłem, osoba ta mogłaby użyć tego klucza w celu zalogowania się na Twoje serwery z zainstalowanym odpowiadającym mu kluczem publicznym.  Zabezpieczenie klucza prywatnego hasłem zapewnia dodatkową warstwę ochronną dla infrastruktury na platformie Azure, a osoba niepowołana nie może go użyć.


W tym artykule tworzone są pliki klucza w formacie *ssh-rsa*, które są zalecane podczas wdrażania w usłudze Resource Manager i wymagane w [portalu](https://portal.azure.com) zarówno w przypadku wdrożeń klasycznych, jak i wdrożeń realizowanych za pośrednictwem menedżera zasobów.


## Tworzenie kluczy SSH

Platforma Azure wymaga, aby klucz publiczny i prywatny miały długość co najmniej 2048 bitów oraz format ssh-rsa. Aby utworzyć parę kluczy, zostanie użyta funkcja `ssh-keygen`, która najpierw zadaje szereg pytań, a następnie zapisuje klucz prywatny i dopasowany klucz publiczny. Podczas tworzenia maszyny wirtualnej na platformie Azure przekazywana jest zawartość klucza publicznego, która jest kopiowana do maszyny wirtualnej systemu Linux, a następnie używana do uwierzytelniania podczas logowania razem z lokalnym, zabezpieczonym kluczem prywatnym.

## Korzystanie z programu ssh-keygen

To polecenie umożliwia utworzenie zabezpieczonej (zaszyfrowanej) pary kluczy SSH o długości 2048 bitów w standardzie RSA. W celu dokonania łatwej identyfikacji zostanie opatrzona komentarzem.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
```

_Opis polecenia_

`ssh-keygen` = program używany do tworzenia kluczy

`-t rsa` = typ klucza do utworzenia w [formacie RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = liczba bitów klucza

`-C "ahmet@fedoraVMAzure"` = komentarz dodany na końcu pliku klucza publicznego, aby umożliwić jego łatwą identyfikację.  Zwykle jako komentarz używany jest adres e-mail, ale można też użyć innych informacji, które sprawdzą się najlepiej w danej infrastrukturze.

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

Nazwa pary kluczy dla potrzeb tego artykułu.  Para kluczy o nazwie **id_rsa** jest domyślna. Niektóre narzędzia mogą wymagać podania nazwy **id_rsa** pliku klucza prywatnego, dlatego zaleca się jego utworzenie. (`~/.ssh/` jest typową domyślną lokalizacją dla wszystkich par kluczy SSH i pliku konfiguracyjnego SSH).

```bash
ahmet@fedora$ ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 azure_fedora_id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 azure_fedora_id_rsa.pub
```
Tutaj wyświetlone są Twoje nowe pary kluczy i ich uprawnienia. `ssh-keygen` tworzy katalog `~/.ssh`, jeśli taki jeszcze nie istnieje, a także ustawia poprawne prawo własności i atrybuty plików.

Hasło klucza:

`Enter passphrase (empty for no passphrase):`

Zaleca się dodanie hasła (w programie `ssh-keygen` używana jest nazwa „passphrase”) do posiadanych par kluczy. Bez hasła chroniącego parę kluczy każda osoba mająca kopię pliku klucza prywatnego może użyć go do logowania na dowolnym Twoim serwerze, który ma odpowiadający mu klucz publiczny. Dodanie hasła zwiększa ochronę w przypadku, gdy osoba niepowołana jest w stanie uzyskać dostęp do Twojego pliku klucza prywatnego. Dzięki temu zabezpieczeniu masz czas na dokonanie zmiany kluczy używanych do uwierzytelnienia.

## Korzystanie z programu ssh-agent do przechowywania hasła klucza prywatnego

Aby uniknąć wpisywania hasła do pliku klucza prywatnego przy każdym logowaniu przez protokół SSH, można skorzystać z programu `ssh-agent`, który przechowuje hasło w pamięci podręcznej. Zapewnia to szybkie i bezpieczne logowanie do maszyny wirtualnej systemu Linux.  Jeśli korzystasz z systemu OS X, pęk kluczy bezpiecznie przechowa Twoje hasła do klucza prywatnego po wywołaniu programu `ssh-agent`.

Najpierw upewnij się, że program `ssh-agent` jest uruchomiony.

```bash
eval "$(ssh-agent -s)"
```

Teraz dodaj klucz prywatny do programu `ssh-agent` za pomocą polecenia `ssh-add`. W systemie OS X spowoduje to ponowne uruchomienie pęku kluczy, który będzie przechowywał poświadczenia.

```bash
ssh-add ~/.ssh/azure_fedora_id_rsa
```

Hasło do klucza prywatnego jest teraz przechowywane, dzięki czemu nie trzeba wpisywać hasła klucza podczas każdego logowania przez protokół SSH.

## Tworzenie i konfigurowanie pliku konfiguracyjnego SSH

Chociaż nie jest to bezwzględnie koniecznie do uruchomienia maszyny wirtualnej z systemem Linux, zaleca się utworzenie i konfigurację pliku `~/.ssh/config`. Zapobiega on przypadkowemu użyciu haseł logowania do maszyn wirtualnych, automatyzuje użycie różnych par kluczy dla różnych maszyn wirtualnych platformy Azure oraz konfiguruje inne programy, takie jak **git**, pod kątem wielu serwerów.

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
  PubkeyAuthentication yes
  IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=no
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath /home/ahmet/.ssh/Connections/ssh-%r@%h:%p
  ControlPersist 4h
  StrictHostKeyChecking=no
  IdentityFile /home/ahmet/.ssh/id_rsa
  UseRoaming=no
```

Ta konfiguracja protokołu SSH udostępnia sekcje dla każdej usługi, aby umożliwić każdej z nich posiadanie własnej pary kluczy. Ustawienia domyślne są stosowane, jeśli host, do którego jest się zalogowanym, nie należy do żadnej z powyższych grup.


### Opis pliku konfiguracji

`Host` = nazwa hosta wywołanego na terminalu.  `ssh fedora22` informuje protokół `SSH`, aby użyć wartości z bloku ustawień z etykietą `Host fedora22` UWAGA: Może to być jakakolwiek etykieta zrozumiała dla Ciebie, która jednocześnie nie jest nazwą hosta żadnego serwera.

`Hostname 102.160.203.241` = adres IP lub nazwa DNS serwera, do którego jest się zalogowanym. Umożliwia przekierowanie na serwer i mapowanie pomiędzy zewnętrznym i wewnętrznym adresem IP.

`User git` = konto użytkownika zdalnego używane podczas logowania do maszyny wirtualnej platformy Azure.

`PubKeyAuthentication yes` = informuje protokół SSH, że chcesz użyć klucza SSH do logowania.

`IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa` = informuje protokół SSH, którą parę kluczy przekazać do serwera w celu uwierzytelnienia logowania.


## Korzystanie z protokołu SSH w systemie Linux bez użycia hasła

Jeśli masz już parę kluczy SSH i skonfigurowany plik konfiguracji SSH, możesz szybko i bezpiecznie zalogować się do maszyny wirtualnej systemu Linux. Podczas pierwszego logowania do serwera przy użyciu klucza SSH polecenie monituje o wpisanie hasła do pliku danego klucza.

```bash
ssh fedora22
```

### Opis polecenia

Gdy polecenie `ssh fedora22` jest wykonywane, protokół SSH najpierw lokalizuje i ładuje wszystkie ustawienia z bloku `Host fedora22`, a następnie ładuje wszystkie pozostałe ustawienia z ostatniego bloku `Host *`.

## Następne kroki

W dalszej kolejności należy utworzyć maszyny wirtualne systemu Linux platformy Azure przy użyciu nowego klucza publicznego SSH.  Maszyny wirtualne platformy Azure, które zostały utworzone z publicznym kluczem SSH jako loginem, są lepiej chronione niż maszyny utworzone z hasłami stosowanymi w domyślnej metodzie logowania.  Aby zapobiec próbom złamania hasła metodą siłową, maszyny wirtualne platformy Azure korzystające podczas logowania z kluczy SSH są domyślnie skonfigurowane tak, aby blokować logowania z użyciem haseł.

- [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu szablonu platformy Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu Portalu Azure](virtual-machines-linux-quick-create-portal.md)
- [Tworzenie bezpiecznej maszyny wirtualnej systemu Linux przy użyciu interfejsu wiersza polecenia platformy Azure](virtual-machines-linux-quick-create-cli.md)



<!--HONumber=Jun16_HO2-->


