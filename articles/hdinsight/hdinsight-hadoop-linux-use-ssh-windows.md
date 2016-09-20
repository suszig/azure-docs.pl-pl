<properties
   pageTitle="Używanie kluczy SSH z usługą Hadoop w klastrach opartych na systemie Linux w systemie Windows | Microsoft Azure"
   description="Poznaj sposoby tworzenia i używania kluczy SSH w celu uwierzytelniania w klastrach usługi HDInsight opartych na systemie Linux. Nawiązywanie połączeń z klastrami w klientach opartych na systemie Windows przy użyciu klienta SSH PuTTY."
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
   ms.date="08/30/2016"
   ms.author="larryfr"/>

#Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux, Unix i OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

[Bezpieczna powłoka (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) umożliwia zdalne wykonywanie operacji na klastrach usługi HDInsight opartych na systemie Linux przy użyciu interfejsu wiersza polecenia. Ten dokument zawiera informacje na temat łączenia się z usługą HDInsight z komputerów klienckich z systemem Windows przy użyciu klienta SSH o nazwie PuTTY.

> [AZURE.NOTE] W krokach przedstawionych w tym artykule przyjęto założenie, że używany jest klient z systemem Windows. Jeśli używasz klienta z systemem Linux, Unix lub OS X, zobacz temat [Korzystanie z protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight w systemach Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
>
> Jeśli masz system Windows 10 i korzystasz z pakietu [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about), możesz wykonać kroki opisane w dokumencie [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

##Wymagania wstępne

* Programy **PuTTy** i **PuTTYGen** dla klientów z systemem Windows. Te narzędzia są dostępne pod adresem [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

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

1. Otwórz program PuTTYGen.

2. W obszarze **Type of key to generate** (Typ klucza do wygenerowania) wybierz opcję **SSH-2 RSA**, a następnie kliknij przycisk **Generate** (Generuj).

    ![Interfejs PuTTYGen](./media/hdinsight-hadoop-linux-use-ssh-windows/puttygen.png)

3. Poruszaj myszą w obszarze poniżej paska postępu, aż pasek się wypełni. Przesuwanie wskaźnika myszy generuje losowe dane używane do generowania klucza.

    ![poruszanie myszą](./media/hdinsight-hadoop-linux-use-ssh-windows/movingmouse.png)

    Klucz publiczny zostanie wyświetlony po wygenerowaniu.

4. Aby zwiększyć bezpieczeństwo, możesz wprowadzić hasło w polu **Key passphrase** (Hasło klucza), a następnie wpisać tę samą wartość w polu **Confirm passphrase** (Potwierdź hasło).

    ![hasło](./media/hdinsight-hadoop-linux-use-ssh-windows/key.png)

    > [AZURE.NOTE] Zdecydowanie zalecamy użycie bezpiecznego hasła dla klucza. Jeśli zapomnisz hasła, jego odzyskanie nie będzie możliwe.

5. Kliknij przycisk **Save private key** (Zapisz klucz prywatny), aby zapisać klucz w pliku **ppk**. Ten klucz będzie używany do uwierzytelniania w klastrze usługi HDInsight opartej na systemie Linux.

    > [AZURE.NOTE] Klucz należy przechowywać w bezpiecznej lokalizacji, ponieważ umożliwia on uzyskanie dostępu do klastra usługi HDInsight opartej na systemie Linux.

6. Kliknij przycisk **Save public key** (Zapisz klucz publiczny), aby zapisać klucz w pliku **txt**. Umożliwi to ponowne użycie klucza publicznego w przyszłości podczas tworzenia dodatkowych klastrów usługi HDInsight opartej na systemie Linux.

    > [AZURE.NOTE] Klucz publiczny jest również wyświetlany w górnej części programu PuTTYGen. Możesz kliknąć prawym przyciskiem myszy to pole, skopiować wartość, a następnie wkleić ją w formularzu podczas tworzenia klastra przy użyciu Portalu Azure.

##Tworzenie klastra usługi HDInsight opartej na systemie Linux

Podczas tworzenia klastra usługi HDInsight opartej na systemie Linux należy podać wcześniej utworzony klucz publiczny. Istnieją dwa sposoby tworzenia klastra usługi HDInsight opartej na systemie Linux na komputerach klienckich z systemem Windows:

* **Azure Portal** — tworzenie klastra przy użyciu portalu sieci Web.

* **Interfejs wiersza polecenia platformy Azure dla komputerów Mac oraz komputerów z systemem Linux i Windows** — tworzenie klastra przy użyciu poleceń wiersza polecenia.

Każda z tych metod będzie wymagać klucza publicznego. Aby uzyskać pełne informacje dotyczące tworzenia klastra usługi HDInsight opartej na systemie Linux, zobacz artykuł [Provision Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md) (Obsługa administracyjna klastrów usługi HDInsight opartej na systemie Linux).

###Azure Portal

Podczas używania witryny [Azure Portal][preview-portal] do tworzenia klastra usługi HDInsight opartej na systemie Linux należy wprowadzić dane w polu **Nazwa użytkownika SSH** i wybrać opcję wprowadzenia hasła w polu **HASŁO** lub klucza publicznego w polu **KLUCZ PUBLICZNY SSH**.

W przypadku wybrania opcji **KLUCZ PUBLICZNY SSH** można wkleić klucz publiczny wyświetlany w polu __Public key for pasting into OpenSSH authorized\_keys file__ (Klucz publiczny do wklejenia do autoryzowanego pliku kluczy OpenSSH) w programie PuttyGen w polu __Klucz publiczny SSH__ albo wybrać opcję __Wybierz plik__, aby przejść do pliku zawierającego klucz publiczny.

![Obraz formularza z monitem o klucz publiczny](./media/hdinsight-hadoop-linux-use-ssh-windows/ssh-key.png)

Zostanie utworzony identyfikator logowania dla określonego użytkownika i włączone uwierzytelnianie przy użyciu hasła lub klucza SSH.

###Interfejs wiersza polecenia platformy Azure dla komputerów Mac oraz komputerów z systemem Linux i Windows

Możesz użyć [interfejsu wiersza polecenia platformy Azure dla systemów Mac, Linux i Windows](../xplat-cli-install.md) do utworzenia nowego klastra przy użyciu polecenia `azure hdinsight cluster create`.

Aby uzyskać więcej informacji na temat używania tego polecenia, zobacz temat [Provision Hadoop Linux clusters in HDInsight using custom options](hdinsight-hadoop-provision-linux-clusters.md) (Obsługa administracyjna klastrów Hadoop w usłudze HDInsight opartej na systemie Linux przy użyciu opcji niestandardowych).

##Łączenie z klastrem usługi HDInsight opartej na systemie Linux

1. Otwórz program PuTTY.

    ![interfejs programu putty](./media/hdinsight-hadoop-linux-use-ssh-windows/putty.png)

2. Jeśli podczas tworzenia konta użytkownika podano klucz SSH, należy wykonać następujący krok, aby wybrać klucz prywatny używany podczas uwierzytelniania w klastrze:

    W obszarze **Category** (Kategoria) rozwiń listę **Connection** (Połączenie), rozwiń listę **SSH** i wybierz pozycję **Auth** (Uwierzytelnianie). Na koniec kliknij przycisk **Browse** (Przeglądaj) i wybierz plik ppk, który zawiera klucz prywatny.

    ![interfejs programu putty, wybieranie klucza prywatnego](./media/hdinsight-hadoop-linux-use-ssh-windows/puttykey.png)

3. W obszarze **Category** (Kategoria) wybierz opcję **Session** (Sesja). Na ekranie **Basic options for your PuTTY session** (Podstawowe opcje sesji programu PuTTY) wprowadź adres SSH serwera usługi HDInsight w polu **Host name (or IP address)** (Nazwa hosta lub adres IP). Istnieją dwa możliwe adresy SSH, których możesz używać podczas łączenia się z klastrem:

    * __Adres węzła głównego__: aby nawiązać połączenie z węzłem głównym klastra, użyj nazwy klastra z sufiksem **-ssh.azurehdinsight.net**. Na przykład **mójklaster-ssh.azurehdinsight.net**.
    
    * __Adres węzła krawędzi__: jeśli łączysz się z serwerem R Server w klastrze usługi HDInsight, możesz nawiązać połączenie z węzłem krawędzi serwera R Server przy użyciu adresu __RServer.CLUSTERNAME.ssh.azurehdinsight.net__, gdzie CLUSTERNAME jest nazwą danego klastra. Na przykład __RServer.mycluster.ssh.azurehdinsight.net__.

    ![interfejs programu putty z wprowadzonym adresem ssh](./media/hdinsight-hadoop-linux-use-ssh-windows/puttyaddress.png)

4. Aby zapisać informacje o połączeniu w celu późniejszego użycia, wprowadź nazwę dla tego połączenia w obszarze **Saved Sessions** (Zapisane sesje), a następnie kliknij przycisk **Save** (Zapisz). Połączenie zostanie dodane do listy zapisanych sesji.

5. Kliknij przycisk **Open** (Otwórz), aby nawiązać połączenie z klastrem.

    > [AZURE.NOTE] Jeśli łączysz się z klastrem po raz pierwszy, zostanie wyświetlony alert zabezpieczeń. Jest to normalne. Wybierz opcję **Yes** (Tak), aby zbuforować klucz RSA2 serwera i kontynuować.

6. Po wyświetleniu monitu wprowadź nazwę użytkownika podaną podczas tworzenia klastra. Jeśli podano hasło dla użytkownika, zostanie wyświetlony monit o wprowadzenie tego hasła.

> [AZURE.NOTE] W powyższych krokach założono korzystanie z portu 22, co powoduje ustanowienie połączenia z węzłem głównym 0 w klastrze usługi HDInsight. W przypadku użycia portu 23 połączysz się z węzłem głównym 1. Aby uzyskać więcej informacji o węzłach głównych, zobacz artykuł [Availability and reliability of Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md) (Dostępność i niezawodność klastrów Hadoop w usłudze HDInsight).

###Łączenie z węzłami procesu roboczego

Węzły procesu roboczego nie są bezpośrednio dostępne poza centrum danych Azure, są jednak dostępne z węzła głównego klastra za pośrednictwem protokołu SSH.

Jeśli podczas tworzenia konta użytkownika podano klucz SSH, w celu nawiązania połączenia z węzłami procesów roboczych należy wykonać następujące kroki, aby użyć klucza prywatnego podczas uwierzytelniania w klastrze.

1. Zainstaluj program Pageant dostępny na stronie [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). To narzędzie służy do buforowania kluczy SSH dla programu PuTTY.

2. Uruchom program Pageant. Spowoduje to zminimalizowanie programu do ikony na pasku stanu. Kliknij ikonę prawym przyciskiem myszy i wybierz polecenie **Add Key** (Dodaj klucz).

    ![dodawanie klucza](./media/hdinsight-hadoop-linux-use-ssh-windows/addkey.png)

3. Po wyświetleniu okna dialogowego przeglądania wybierz plik ppk, który zawiera klucz, a następnie kliknij przycisk **Open** (Otwórz). Spowoduje to dodanie klucza do programu Pageant, który dostarczy go do programu PuTTY podczas nawiązywania połączenia z klastrem.

    > [AZURE.IMPORTANT] Jeśli do zabezpieczenia konta jest używany klucz SSH, musisz wykonać poprzednie kroki, zanim będzie można nawiązać połączenia z węzłami procesu roboczego.

4. Otwórz program PuTTY.

5. Jeśli do uwierzytelniania jest używany klucz SSH, w sekcji **Category** (Kategoria) rozwiń listę **Connection** (Połączenie), rozwiń listę **SSH**, a następnie wybierz opcję **Auth** (Uwierzytelnianie).

    W sekcji **Authentication parameters** (Parametry uwierzytelniania) włącz opcję **Allow agent forwarding** (Zezwalaj na przekazywanie przez agenta). Dzięki temu program PuTTY może automatycznie przekazywać uwierzytelnianie certyfikatu za pośrednictwem połączenia do węzła głównego klastra podczas łączenia się z węzłami procesów roboczych.

    ![zezwalanie na przekazywanie przez agenta](./media/hdinsight-hadoop-linux-use-ssh-windows/allowforwarding.png)

6. Nawiąż połączenie z klastrem zgodnie z wcześniejszym opisem. Jeśli do uwierzytelniania jest używany klucz SSH, nie musisz go wybierać — klucz SSH dodany do programu Pageant będzie używany do uwierzytelniania w klastrze.

7. Po nawiązaniu połączenia użyj następującego polecenia w celu pobrania listy węzłów w klastrze. Zastąp ciąg *ADMINPASSWORD* hasłem do konta administratora klastra. Zastąp ciąg *CLUSTERNAME* nazwą klastra.

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    Spowoduje to zwrócenie informacji w formacie JSON dotyczących węzłów w klastrze, w tym danych `host_name`, które zawierają w pełni kwalifikowaną nazwę domeny (FQDN) każdego węzła. Oto przykład wpisu `host_name` zwróconego przez polecenie **curl**:

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

8. Po utworzeniu listy węzłów procesu roboczego, z którymi chcesz się połączyć, użyj następującego polecenia w sesji programu PuTTY, aby nawiązać połączenie z węzłem procesu roboczego:

        ssh USERNAME@FQDN

    Zastąp ciąg *USERNAME* nazwą użytkownika SSH, a ciąg *FQDN* nazwą FQDN węzła procesu roboczego. Na przykład `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.

    > [AZURE.NOTE] Jeśli używasz hasła w celu uwierzytelniania sesji SSH, zostanie wyświetlony monit o ponowne wprowadzenie hasła. Jeśli używasz klucza SSH, połączenie powinno zostać zakończone bez żadnych monitów.

9. Po ustanowieniu sesji informacja w wierszu sesji programu PuTTY zmieni się z `username@hn0-clustername` na `username@wn0-clustername`, wskazując istnienie połączenia z węzłem procesu roboczego. Dowolne polecenia wykonywane na tym etapie zostaną uruchomione w węźle procesu roboczego.

10. Po zakończeniu wykonywania akcji w węźle procesu roboczego użyj polecenia `exit`, aby zamknąć sesję węzła procesu roboczego. Spowoduje to powrót do wiersza `username@hn0-clustername`.

##Dodawanie większej liczby kont

Jeśli potrzebujesz dodać więcej kont do klastra, wykonaj następujące czynności:

1. Wygeneruj nowy klucz publiczny i klucz prywatny dla nowego konta użytkownika, zgodnie z wcześniejszym opisem.

2. W sesji SSH z klastrem dodaj nowego użytkownika przy użyciu następującego polecenia:

        sudo adduser --disabled-password <username>

    Zostanie utworzone nowe konto użytkownika, ale uwierzytelnianie hasłem zostanie wyłączone.

3. Utwórz katalog i pliki do przechowywania klucza za pomocą następujących poleceń:

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

4. Po otwarciu edytora nano skopiuj i wklej zawartość klucza publicznego dla nowego konta użytkownika. Na koniec naciśnij klawisze **Ctrl-X**, aby zapisać plik i zamknąć edytor.

    ![obraz edytora nano z przykładowym kluczem](./media/hdinsight-hadoop-linux-use-ssh-windows/nano.png)

5. Aby zmienić właściciela folderu .ssh i jego zawartości na nowe konto użytkownika, użyj następującego polecenia:

        sudo chown -hR <username>:<username> /home/<username>/.ssh

6. Teraz powinno być możliwe uwierzytelnienie na serwerze przy użyciu nowego konta użytkownika i klucza prywatnego.

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



<!--HONumber=sep16_HO1-->


