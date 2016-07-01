<properties
   pageTitle="Używanie kluczy SSH z opartą na systemie Linux usługą Hadoop w systemie Linux, Unix lub OS X | Microsoft Azure"
   description=" Do usługi HDInsight w systemie Linux można uzyskać dostęp przy użyciu protokołu Secure Shell (SSH). Ten dokument zawiera informacje na temat używania protokołu SSH z usługą HDInsight przy użyciu klientów w systemie Linux, Unix lub OS X."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/25/2016"
   ms.author="larryfr"/>

#Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X

> [AZURE.SELECTOR]
- [System Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Systemy Linux, Unix i OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) pozwala na zdalne wykonywanie operacji na klastrach opartej na systemie Linux usługi HDInsight przy użyciu interfejsu wiersza polecenia. Ten dokument zawiera informacje na temat używania protokołu SSH z usługą HDInsight przy użyciu klientów w systemie Linux, Unix lub OS X.

> [AZURE.NOTE] W krokach podanych tym artykule przyjęto założenie, że używasz klienta z systemem Linux, Unix lub OS X. Chociaż te kroki można wykonać na kliencie z systemem Windows po zainstalowaniu pakietu dostarczającego programy `ssh` i `ssh-keygen` (np. Git dla systemu Windows), zalecamy, aby używając klientów z systemem Windows, korzystać z instrukcji z artykułu [Używanie SSH z opartą na systemie Linux usługą HDInsight (Hadoop) w systemie Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

##Wymagania wstępne

* **ssh-keygen** i **ssh** dla klientów z systemem Linux, Unix i OS X. Te narzędzia są zwykle dołączone do systemu operacyjnego lub dostępne w systemie zarządzania pakietami.

* Nowoczesna przeglądarka sieci Web, obsługująca język HTML5.

LUB

* [Interfejs wiersza polecenia platformy Azure](../xplat-cli-install.md).

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##Co to jest SSH?

SSH jest narzędziem do zdalnego logowania oraz wykonywania poleceń na serwerze zdalnym. W przypadku korzystania z usługi HDInsight opartej na systemie Linux protokół SSH ustanawia szyfrowane połączenie z głównym węzłem klastra i dostarcza wiersz polecenia służący do wpisywania poleceń. Polecenia są następnie wykonywane bezpośrednio na serwerze.

###Nazwa użytkownika SSH

Nazwa użytkownika SSH to nazwa używana do uwierzytelniania w klastrze usługi HDInsight. Po określeniu nazwy użytkownika SSH podczas tworzenia klastra dany użytkownik zostaje utworzony we wszystkich węzłach w klastrze. Po utworzeniu klastra można używać tej nazwy użytkownika do nawiązywania połączeń z głównymi węzłami klastra usługi HDInsight. Z węzłów głównych można następnie łączyć się z poszczególnymi węzłami procesów roboczych.

###Hasło SSH lub klucz publiczny

Użytkownik SSH może używać do uwierzytelniania hasła lub klucza publicznego. Hasło jest po prostu określonym ciągiem tekstu, a klucz publiczny jest częścią pary kluczy kryptograficznych wygenerowanej w celu unikatowej identyfikacji użytkownika.

Klucz jest bezpieczniejszy niż hasło, jednak jego wygenerowanie wymaga dodatkowych czynności, a pliki zawierające klucz muszą być przechowywane w bezpiecznej lokalizacji. Jeśli ktokolwiek uzyska dostęp do plików kluczy, uzyska również dostęp do Twojego konta. Natomiast utrata plików kluczy uniemożliwia zalogowanie się do konta.

Para kluczy składa się z klucza publicznego (który jest wysyłany do serwera HDInsight) i klucza prywatnego (który jest przechowywany na komputerze klienckim). Podczas łączenia z serwerem usługi HDInsight przy użyciu protokołu SSH klient SSH użyje klucza prywatnego na komputerze do wykonania uwierzytelnienia na serwerze.

##Tworzenie klucza SSH

Jeśli zamierzasz używać kluczy SSH w klastrze, skorzystaj z poniższych informacji. Jeśli planujesz używać hasła, możesz pominąć tę sekcję.

1. Otwórz sesję terminala i użyj następującego polecenia, aby sprawdzić, czy masz jakiekolwiek klucze SSH:

        ls -al ~/.ssh

    Poszukaj następujących plików na liście zawartości katalogu. Są to pospolite nazwy kluczy publicznych SSH.

    * id\_dsa.pub
    * id\_ecdsa.pub
    * id\_ed25519.pub
    * id\_rsa.pub

2. Jeśli nie chcesz używać istniejącego pliku lub nie masz istniejących kluczy SSH, wygeneruj nowy plik w następujący sposób:

        ssh-keygen -t rsa

    Zostanie wyświetlony monit z prośbą następujące informacje:

    * Lokalizacja pliku — domyślna lokalizacja to ~/.ssh/id\_rsa.
    * Hasło — zostanie wyświetlony monit o jego ponowne wprowadzenie.

        > [AZURE.NOTE] Zdecydowanie zalecamy użycie bezpiecznego hasła dla klucza. Jeśli zapomnisz hasła, jego odzyskanie nie będzie możliwe.

    Po zakończeniu działania polecenia otrzymasz dwa nowe pliki: klucz prywatny (na przykład **id\_rsa**) i klucz publiczny (na przykład **id\_rsa.pub**).

##Tworzenie klastra usługi HDInsight opartej na systemie Linux

Podczas tworzenia klastra usługi HDInsight opartej na systemie Linux należy podać wcześniej utworzony klucz publiczny. Istnieją dwa sposoby tworzenia klastra usługi HDInsight przy użyciu klientów z systemem Linux, Unix lub OS X:

* **Portal Azure** — tworzenie klastra przy użyciu portalu sieci Web.

* **Interfejs wiersza polecenia platformy Azure dla komputerów Mac oraz komputerów z systemem Linux i Windows** — tworzenie klastra przy użyciu poleceń wiersza polecenia.

Każda z tych metod wymaga hasła lub klucza publicznego. Aby uzyskać pełne informacje dotyczące tworzenia klastra usługi HDInsight opartej na systemie Linux, zobacz artykuł [Provision Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md) (Obsługa administracyjna klastrów usługi HDInsight opartej na systemie Linux).

###Portal Azure

Podczas używania [portalu Azure][preview-portal] do tworzenia klastra usługi HDInsight opartej na systemie Linux należy wprowadzić **NAZWĘ UŻYTKOWNIKA SSH** i wybrać wprowadzenie **HASŁA** lub **PUBLICZNEGO KLUCZA SSH**.

W przypadku wybrania **PUBLICZNEGO KLUCZA SSH** można albo wkleić klucz publiczny (zawarty w pliku z rozszerzeniem **.pub**) do pola __SSH PublicKey__, albo wybrać opcję __Wybierz plik__, aby przeglądać i wybrać plik klucza publicznego.

![Obraz formularza z monitem o klucz publiczny](./media/hdinsight-hadoop-linux-use-ssh-unix/ssh-key.png)

> [AZURE.NOTE] Plik klucza jest zwykłym plikiem tekstowym. Zawartość powinna wyglądać podobnie do następującej:
> ```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCelfkjrpYHYiks4TM+r1LVsTYQ4jAXXGeOAF9Vv/KGz90pgMk3VRJk4PEUSELfXKxP3NtsVwLVPN1l09utI/tKHQ6WL3qy89WVVVLiwzL7tfJ2B08Gmcw8mC/YoieT/YG+4I4oAgPEmim+6/F9S0lU2I2CuFBX9JzauX8n1Y9kWzTARST+ERx2hysyA5ObLv97Xe4C2CQvGE01LGAXkw2ffP9vI+emUM+VeYrf0q3w/b1o/COKbFVZ2IpEcJ8G2SLlNsHWXofWhOKQRi64TMxT7LLoohD61q2aWNKdaE4oQdiuo8TGnt4zWLEPjzjIYIEIZGk00HiQD+KCB5pxoVtp user@system
> ```

Spowoduje to utworzenie danych logowania dla określonego użytkownika przy użyciu podanego hasła lub klucza publicznego.

###Interfejs wiersza polecenia platformy Azure dla systemów Mac, Linux i Windows

Możesz użyć [interfejsu wiersza polecenia platformy Azure dla systemów Mac, Linux i Windows](../xplat-cli-install.md) do utworzenia nowego klastra przy użyciu polecenia `azure hdinsight cluster create`.

Aby uzyskać więcej informacji na temat używania tego polecenia, zobacz temat [Provision Hadoop Linux clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md) (Obsługa administracyjna klastrów Hadoop w usłudze HDInsight opartej na systemie Linux przy użyciu opcji niestandardowych).

##Łączenie z klastrem usługi HDInsight opartej na systemie Linux

Z poziomu sesji terminalowej użyj polecenia SSH do nawiązania połączenia z głównym węzłem klastra, podając adres i nazwę użytkownika:

* **Adres SSH** — istnieją dwa adresy, które mogą być użyte do nawiązania połączenia z klastrem przy użyciu protokołu SSH:

    * **Łączenie z węzłem głównym**: nazwa klastra z dołączonym sufiksem **-ssh.azurehdinsight.net**. Na przykład **mójklaster-ssh.azurehdinsight.net**.
    
    * **Łączenie z węzłem krawędzi**: jeśli klaster jest serwerem R w usłudze HDInsight, klaster zawiera również węzeł krawędzi, który jest dostępny pod adresem **RServer.CLUSTERNAME.ssh.azurehdinsight.net**, gdzie __CLUSTERNAME__ jest nazwą klastra.

* **Nazwa użytkownika** — nazwa użytkownika SSH podana podczas tworzenia klastra.

Poniższy przykład służy do łączenia z węzłem głównym 0 klastra **mycluster** jako użytkownik **me**:

    ssh me@mycluster-ssh.azurehdinsight.net

Jeśli użyto hasła do konta użytkownika, pojawi się monit o wprowadzenie tego hasła.

Jeśli użyto klucza SSH, który jest zabezpieczony za pomocą hasła, pojawi się monit o podanie hasła. W przeciwnym razie SSH spróbuje automatycznego uwierzytelniania przy użyciu jednego z lokalnych kluczy prywatnych na komputerze klienckim.

> [AZURE.NOTE] Jeśli SSH nie uwierzytelni się automatycznie przy użyciu poprawnego klucza prywatnego, użyj parametru **-i**, a następnie podaj ścieżkę do klucza prywatnego. Poniższy przykład załaduje klucz prywatny z pliku `~/.ssh/id_rsa`:
>
> `ssh -i ~/.ssh/id_rsa me@mycluster-ssh.azurehdinsight.net`

Jeśli łączysz się przy użyciu adresu węzła głównego, ale bez określania portu, domyślnym portem SSH będzie port 22, służący do łączenia się z węzłem głównym 0 w klastrze usługi HDInsight. W przypadku użycia portu 23 połączysz się z węzłem głównym 1. Aby uzyskać więcej informacji o węzłach głównych, zobacz artykuł [Availability and reliability of Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md) (Dostępność i niezawodność klastrów Hadoop w usłudze HDInsight).

###Łączenie z węzłami procesu roboczego

Węzły procesu roboczego nie są bezpośrednio dostępne poza centrum danych Azure, są jednak dostępne z węzła głównego klastra za pośrednictwem protokołu SSH.

Jeśli używasz klucza SSH do uwierzytelnienia konta użytkownika, musisz wykonać następujące czynności na komputerze klienckim:

1. Za pomocą edytora tekstów otwórz plik `~/.ssh/config`. Jeśli ten plik nie istnieje, możesz go utworzyć przy użyciu polecenia `touch ~/.ssh/config` wprowadzonego w terminalu.

2. Dodaj następującą zawartość do pliku. Zastąp *CLUSTERNAME* nazwą klastra usługi HDInsight:

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    Pozwoli to na skonfigurowanie agenta przekazywania SSH dla klastra usługi HDInsight.

3. Przetestuj agenta przekazywania SSH za pomocą następującego polecenia z terminala:

        echo "$SSH_AUTH_SOCK"

    Powinny zostać zwrócone informacje podobne do następujących:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Jeśli nic nie zostanie zwrócone, oznacza to, że program **ssh-agent** nie jest uruchomiony. Zapoznaj się z dokumentacją systemu operacyjnego, aby poznać konkretne kroki dotyczące instalowania i konfigurowania programu **ssh-agent**, lub zobacz artykuł [Używanie programu ssh-agent z ssh](http://mah.everybody.org/docs/ssh).

4. Po upewnieniu się, że program **ssh-agent** działa, należy wykonać następujące czynności, aby dodać klucz prywatny SSH do agenta:

        ssh-add ~/.ssh/id_rsa

    Jeśli klucz prywatny jest przechowywany w innym pliku, zastąp `~/.ssh/id_rsa` ścieżką do pliku.

Wykonaj poniższe czynności w celu nawiązania połączenia z węzłami procesów roboczych dla klastra.

> [AZURE.IMPORTANT] Jeśli używasz klucza SSH do uwierzytelniania konta, musisz wykonać wcześniejsze kroki, aby sprawdzić, czy agent przekazywania działa.

1. Połącz się z klastrem usługi HDInsight przy użyciu protokołu SSH, jak opisano wcześniej.

2. Po nawiązaniu połączenia użyj następującego polecenia do pobierania listy węzłów w klastrze. Zastąp ciąg *ADMINPASSWORD* hasłem do konta administratora klastra. Zastąp ciąg *CLUSTERNAME* nazwą klastra.

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    Spowoduje to zwrócenie informacji w formacie JSON dotyczących węzłów w klastrze, w tym danych `host_name`, które zawierają w pełni kwalifikowaną nazwę domeny (FQDN) każdego węzła. Oto przykład wpisu `host_name` zwróconego przez polecenie **curl**:

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

3. Po utworzeniu listy węzłów procesu roboczego, z którymi chcesz się połączyć, użyj następującego polecenia w sesji SSH na serwerze, aby nawiązać połączenie z węzłem procesu roboczego:

        ssh USERNAME@FQDN

    Zastąp ciąg *USERNAME* nazwą użytkownika SSH, a ciąg *FQDN* nazwą FQDN węzła procesu roboczego. Na przykład `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.

    > [AZURE.NOTE] Jeśli używasz hasła w celu uwierzytelniania sesji SSH, zostanie wyświetlony monit o ponowne wprowadzenie hasła. Jeśli używasz klucza SSH, połączenie powinno zostać zakończone bez żadnych monitów.

4. Po ustanowieniu sesji wiersz terminala zmieni się z `username@hn0-clustername` na `username@wk0-clustername`, aby wskazać, że masz połączenie z węzłem procesu roboczego. Dowolne polecenia wykonywane na tym etapie zostaną uruchomione w węźle procesu roboczego.

4. Po zakończeniu wykonywania akcji w węźle procesu roboczego użyj polecenia `exit`, aby zamknąć sesję węzła procesu roboczego. Spowoduje to powrót do wiersza `username@hn0-clustername`.

##Dodawanie większej liczby kont

1. Wygeneruj nowe klucze — publiczny i prywatny — dla nowego konta użytkownika, zgodnie z opisem w części [Tworzenie klucza SSH](#create-an-ssh-key-optional).

    > [AZURE.NOTE] Klucz prywatny powinien być generowany na kliencie używanym przez użytkownika do łączenia się z klastrem albo bezpiecznie przekazany do takiego klienta po utworzeniu.

1. W sesji SSH z klastrem dodaj nowego użytkownika przy użyciu następującego polecenia:

        sudo adduser --disabled-password <username>

    Zostanie utworzone nowe konto użytkownika, ale uwierzytelnianie hasłem zostanie wyłączone.

2. Utwórz katalog i pliki do przechowywania klucza za pomocą następujących poleceń:

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

3. Po otwarciu edytora nano skopiuj i wklej zawartość klucza publicznego dla nowego konta użytkownika. Na koniec naciśnij klawisze **Ctrl-X**, aby zapisać plik i zamknąć edytor.

    ![obraz edytora nano z przykładowym kluczem](./media/hdinsight-hadoop-linux-use-ssh-unix/nano.png)

4. Aby zmienić właściciela folderu .ssh i jego zawartości na nowe konto użytkownika, użyj następującego polecenia:

        sudo chown -hR <username>:<username> /home/<username>/.ssh

5. Teraz powinno być możliwe uwierzytelnienie na serwerze przy użyciu nowego konta użytkownika i klucza prywatnego.

##<a id="tunnel"></a>Tunelowanie SSH

Protokół SSH może również służyć do tunelowania żądań lokalnych, takich jak żądania sieci Web, do klastra usługi HDInsight. Żądania będą następnie kierowane do wymaganego zasobu, tak jakby pochodziły z węzła głównego klastra usługi HDInsight.

> [AZURE.IMPORTANT] Tunel SSH jest konieczny do uzyskiwania dostępu do interfejsu użytkownika sieci Web niektórych usług Hadoop. Na przykład interfejsy użytkownika usługi Historia zadań i Menedżera zasobów są dostępne wyłącznie przy użyciu tunelu SSH.

Aby uzyskać więcej informacji dotyczących tworzenia i używania tunelu SSH, zobacz artykuł [Use SSH Tunneling to access Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UI's](hdinsight-linux-ambari-ssh-tunnel.md) (Korzystanie z tunelowania SSH w celu uzyskania dostępu do interfejsów użytkownika sieci Web Ambari, ResourceManager, JobHistory, NameNode, Oozie i innych).

##Następne kroki

Po zapoznaniu się ze sposobem uwierzytelniania przy użyciu klucza SSH dowiedz się, jak korzystać z usługi MapReduce z Hadoop w usłudze HDInsight.

* [Korzystanie z programu Hive z usługą HDInsight](hdinsight-use-hive.md)

* [Korzystanie z języka Pig z usługą HDInsight](hdinsight-use-pig.md)

* [Korzystanie z zadań MapReduce z usługą HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Jun16_HO2-->


