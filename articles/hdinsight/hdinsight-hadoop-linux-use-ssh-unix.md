---
title: "Korzystanie z protokołu SSH z usługą HDInsight (Hadoop) w systemie Windows, Linux, Unix lub OS X | Microsoft Docs"
description: " Do usługi HDInsight można uzyskać dostęp przy użyciu protokołu Secure Shell (SSH). Ten dokument zawiera informacje na temat używania protokołu SSH z usługą HDInsight przy użyciu klientów w systemie Windows, Linux, Unix lub OS X."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: 37409ad3f50cdd4a7a384c96a57a35ef8c83fb8f


---
# <a name="use-ssh-with-hdinsight-hadoop-from-windows-linux-unix-or-os-x"></a>Korzystanie z protokołu SSH z usługą HDInsight (Hadoop) w systemie Windows, Linux, Unix lub OS X

> [!div class="op_single_selector"]
> * [PuTTY (Windows)](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [SSH (Windows, Linux, Unix, OS X)](hdinsight-hadoop-linux-use-ssh-unix.md)

Protokół [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) umożliwia logowanie do klastra usługi HDInsight opartego na systemie Linux i uruchamianie poleceń przy użyciu interfejsu wiersza polecenia. Ten dokument zawiera podstawowe informacje o protokole SSH oraz szczegółowe informacje na temat używania protokołu SSH z usługą HDInsight.

## <a name="what-is-ssh"></a>Co to jest SSH?

SSH to kryptograficzny protokół sieciowy umożliwiający bezpieczną komunikację z serwerem zdalnym za pośrednictwem niezabezpieczonej sieci. Protokół SSH jest używany do bezpiecznego logowania na serwerze zdalnym za pomocą wiersza polecenia. W tym przypadku jest to węzeł główny lub węzeł krawędzi klastra usługi HDInsight.

Protokół SSH może również służyć do tunelowania ruchu sieciowego z klienta do klastra usługi HDInsight. Dzięki użyciu tunelu można uzyskiwać dostęp do usług w klastrze usługi HDInsight, które nie są bezpośrednio połączone z Internetem. Aby uzyskać więcej informacji o korzystaniu z tunelowania SSH z usługą HDInsight, zobacz artykuł [Use SSH tunnelling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) (Używanie tunelowania SSH z usługą HDInsight).

## <a name="ssh-clients"></a>Klienci SSH

Wiele systemów operacyjnych oferuje funkcje klienta SSH za pośrednictwem narzędzi wiersza polecenia `ssh` i `scp`.

* __ssh__: ogólny klient SSH, za pomocą którego można ustanawiać sesje zdalne wiersza polecenia i tworzyć tunele.
* __scp__: narzędzie do kopiowania plików między systemami lokalnymi a zdalnymi za pośrednictwem protokołu SSH.

Przed udostępnieniem aktualizacji rocznicowej systemu Windows 10 system Windows nie udostępniał klienta SSH. Ta wersja systemu Windows zawiera funkcję dla deweloperów Bash w systemie Windows 10, udostępniającą polecenia `ssh`, `scp` i inne polecenia systemu Linux. Aby uzyskać więcej informacji na temat korzystania z funkcji Bash w systemie Windows 10, zobacz [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about) (Funkcja Bash dla Ubuntu w systemie Windows).

Jeśli korzystasz z systemu Windows, ale nie masz dostępu do funkcji Bash w systemie Windows 10, zalecamy użycie następujących klientów SSH:

* [Git dla systemu Windows](https://git-for-windows.github.io/): udostępnia narzędzia wiersza polecenia `ssh` i `scp`.
* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/): graficzny klient SSH.
* [MobaXterm](http://mobaxterm.mobatek.net/): graficzny klient SSH.
* [Cygwin](https://cygwin.com/): udostępnia narzędzia wiersza polecenia `ssh` i `scp`.

> [!NOTE]
> W procedurach przedstawionych w tym artykule przyjęto założenie, że masz dostęp do polecenia `ssh`. Jeśli korzystasz z klienta takiego jak PuTTY lub MobaXterm, zapoznaj się z dokumentacją tego produktu, aby znaleźć odpowiednie polecenie i parametry.

## <a name="ssh-authentication"></a>Uwierzytelnianie SSH

Połączenie SSH może korzystać z uwierzytelniania przy użyciu hasła lub [kryptografii klucza publicznego (https://en.wikipedia.org/wiki/Public-key_cryptography)](https://en.wikipedia.org/wiki/Public-key_cryptography). Użycie klucza to najbezpieczniejsza opcja, ponieważ ta metoda nie jest narażona na wiele rodzajów ataków, na które narażone jest zabezpieczenie hasłem. Jednak tworzenie kluczy i zarządzanie nimi jest bardziej skomplikowane niż korzystanie z hasła.

Zastosowanie kryptografii klucza publicznego wymaga utworzenia pary kluczy: _publicznego_ i _prywatnego_.

* **Klucz publiczny** jest ładowany do węzłów klastra usługi HDInsight lub dowolnej innej usługi, z którą chcesz używać kryptografii klucza publicznego.

* **Klucz prywatny** jest przedstawiany klastrowi usługi HDInsight podczas logowania przy użyciu klienta SSH w celu zweryfikowania Twojej tożsamości. Klucz prywatny należy chronić. Nie należy go udostępniać.

    Można zastosować dodatkowe zabezpieczenie, tworząc hasło klucza prywatnego. Użycie klucza będzie wymagało podania tego hasła.

### <a name="create-a-public-and-private-key"></a>Tworzenie klucza publicznego i prywatnego

Użycie narzędzia `ssh-keygen` to najprostszy sposób tworzenia pary kluczy, publicznego i prywatnego, do użycia z usługą HDInsight. W wierszu polecenia wpisz następujące polecenie, aby utworzyć nową parę kluczy do użycia z usługą HDInsight:

> [!NOTE]
> Jeśli korzystasz z klienta SSH graficznym interfejsem użytkownika, takiego jak MobaXTerm lub PuTTY, zapoznaj się z dokumentacją tego klienta, aby dowiedzieć się, jak wygenerować klucze.

    ssh-keygen -t rsa -b 2048

Zostanie wyświetlony monit o podanie następujących informacji:

* Lokalizacja pliku: domyślna lokalizacja to `~/.ssh/id_rsa`.

* Opcjonalne hasło: jeśli wprowadzisz hasło, będzie konieczne ponowne wprowadzenie go podczas uwierzytelniania w klastrze usługi HDInsight.

> [!IMPORTANT]
> Jest to hasło klucza prywatnego. Przy każdym użyciu klucza prywatnego w celu uwierzytelnienia należy wprowadzić to hasło. W przeciwnym razie użycie klucza nie będzie możliwe. Jeśli inna osoba wejdzie w posiadanie Twojego klucza prywatnego, nie będzie mogła go użyć, nie znając hasła.
>
> Jeśli zapomnisz hasła, jego zresetowanie ani odzyskanie nie będzie możliwe.

Po zakończeniu wykonywania polecenia zostaną utworzone dwa nowe pliki:

* __id\_rsa__: ten plik zawiera klucz prywatny.

    > [!WARNING]
    > Należy ograniczyć dostęp do tego pliku, aby uniemożliwić nieuprawniony dostęp do usług zabezpieczonych za pomocą klucza publicznego.

* __id\_rsa.pub__: ten plik zawiera klucz publiczny. Tego pliku użyjesz podczas tworzenia klastra usługi HDInsight.

    > [!NOTE]
    > Nie trzeba ograniczać dostępu do klucza _publicznego_. Sam klucz publiczny może posłużyć tylko do zweryfikowania klucza prywatnego. Usługi takie jak serwer SSH korzystają z klucza publicznego w celu zweryfikowania Twojej tożsamości podczas uwierzytelniania za pomocą klucza prywatnego.

## <a name="configure-ssh-on-hdinsight"></a>Konfigurowanie protokołu SSH w usłudze HDInsight

Podczas tworzenia klastra usługi HDInsight opartego na systemie Linux należy podać _nazwę użytkownika SSH_ oraz _hasło_ lub _klucz publiczny_. Podczas tworzenia klastra te informacje są używane do utworzenia nazwy logowania w węzłach klastra usługi HDInsight. Hasło lub klucz publiczny zabezpiecza konto użytkownika.

Aby uzyskać więcej informacji na temat konfigurowania uwierzytelniania SSH podczas tworzenia klastra, zobacz jeden z następujących dokumentów:

* [Create HDInsight using the Azure portal](hdinsight-hadoop-create-linux-clusters-portal.md) (Tworzenie klastrów usługi HDInsight za pomocą usługi Azure Portal)
* [Create HDInsight using the Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) (Tworzenie klastrów usługi HDInsight przy użyciu interfejsu wiersza polecenia platformy Azure)
* [Create HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) (Tworzenie klastrów usługi HDInsight przy użyciu programu Azure PowerShell)
* [Create HDInsight using Azure Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Tworzenie klastrów usługi HDInsight przy użyciu szablonów usługi Azure Resource Manager)
* [Create HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) (Tworzenie klastrów usługi HDInsight przy użyciu zestawu SDK dla platformy .NET)
* [Create HDInsight using REST](hdinsight-hadoop-create-linux-clusters-curl-rest.md) (Tworzenie klastrów usługi HDInsight przy użyciu usług REST)

### <a name="additional-ssh-users"></a>Dodatkowi użytkownicy SSH

Dodanie kolejnych użytkowników SSH do klastra po jego utworzeniu jest możliwe, ale niezalecane.

* Należy ręcznie dodać nowych użytkowników SSH do każdego węzła klastra.

* Nowi użytkownicy SSH mają taki sam dostęp do usługi HDInsight jak użytkownik domyślny. Nie można w żaden sposób ograniczyć dostępu do danych lub zadań w usłudze HDInsight na podstawie konta użytkownika SSH.

Aby ograniczyć dostęp dla poszczególnych użytkowników, należy użyć klastra usługi HDInsight przyłączonego do domeny. Usługa HDInsight przyłączona do domeny korzysta z usługi Active Directory na potrzeby kontroli dostępu do zasobów klastra.

Korzystanie z klastra usługi HDInsight przyłączonego do domeny umożliwia uwierzytelnianie za pomocą usługi Active Directory po nawiązaniu połączenia za pośrednictwem protokołu SSH. Wielu użytkowników może połączyć się, korzystając z protokołu SSH, a następnie uwierzytelnić się na swoich kontach usługi Active Directory. Aby uzyskać więcej informacji, zobacz sekcję [Usługa HDInsight przyłączona do domeny](#domainjoined).

##<a name="a-idconnecta-connect-to-hdinsight"></a><a id="connect"></a> Łączenie się z usługą HDInsight

Chociaż serwer SSH jest uruchomiony na wszystkich węzłach klastra usługi HDInsight, za pośrednictwem publicznego Internetu można łączyć się tylko z węzłami głównymi i węzłami krawędzi.

* Aby połączyć się z _węzłami głównymi_, użyj polecenia `CLUSTERNAME-ssh.azurehdinsight.net`, zastępując wartość__CLUSTERNAME__ nazwą klastra usługi HDInsight. Łącząc się przez port 22 (domyślny dla protokołu SSH), nawiążesz połączenie z podstawowym węzłem głównym. Port 23 umożliwia połączenie z pomocniczym węzłem głównym.

* Aby połączyć się z _węzłem krawędzi_, użyj polecenia `EDGENAME.CLUSTERNAME-ssh.azurehdinsight.net`, zastępując wartość__EDGENAME__ nazwą węzła krawędzi, a wartość __CLUSTERNAME__ nazwą klastra usługi HDInsight. Łącząc się z węzłem krawędzi, użyj portu 22.

Poniższe przykłady pokazują, jak połączyć się z węzłami głównymi i węzłem krawędzi klastra o nazwie __myhdi__ przy użyciu nazwy użytkownika SSH __sshuser__. Nazwa węzła krawędzi to __myedge__.

| Aby wykonać tę czynność... | Użyj polecenia... |
| ----- | ----- |
| Łączenie z podstawowym węzłem głównym | `ssh sshuser@myhdi-ssh.azurehdinsight.net` |
| Łączenie z pomocniczym węzłem głównym | `ssh -p 23 sshuser@myhdi-ssh.azurehdinsight.net` |
| Łączenie z węzłem krawędzi | `ssh sshuser@edge.myhdi-ssh.azurehdinsight.net` |

Jeśli konto SSH jest zabezpieczone hasłem, zostanie wyświetlony monit o wprowadzenie tego hasła.

Jeśli konto SSH jest zabezpieczone kluczem publicznym, może być konieczne wskazanie ścieżki do odpowiedniego klucza prywatnego przy użyciu przełącznika `-i`. Poniższy przykład przedstawia sposób użycia przełącznika `-i`:

    ssh -i /path/to/public.key sshuser@myhdi-ssh.azurehdinsight.net

### <a name="connect-to-other-nodes"></a>Łączenie z innymi węzłami

Węzły procesów roboczych i węzły dozorcy nie są bezpośrednio dostępne spoza klastra, są jednak dostępne z węzłów głównych lub z węzłów krawędzi klastra. Poniżej przedstawiono ogólnie czynności, które należy w tym celu wykonać:

1. Połącz się z węzłem głównym lub węzłem krawędzi za pomocą protokołu SSH:

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Z poziomu połączenia SSH z węzłem głównym lub węzłem krawędzi połącz się z węzłem procesu roboczego w klastrze za pomocą polecenia `ssh`:

        ssh sshuser@wn0-myhdi

    Jeśli chcesz pobrać listę węzłów procesów roboczych w klastrze, zobacz przykład pobierania w pełni kwalifikowanej nazwy domeny węzłów klastra w dokumencie [Manage HDInsight by using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) (Zarządzanie usługą HDInsight przy użyciu interfejsu API REST Ambari).

Jeśli konto SSH jest zabezpieczone hasłem, będzie konieczne wprowadzenie hasła. Następnie połączenie zostanie nawiązane.

Jeśli do uwierzytelnienia konta użytkownika używasz klucza SSH, upewnij się, że w środowisku lokalnym skonfigurowano agenta przekazywania SSH.

> [!IMPORTANT]
> W poniższej procedurze założono korzystanie z systemu opartego na systemie Linux/UNIX, ale działa ona również w przypadku funkcji Bash w systemie Windows 10. Jeśli te czynności nie działają w Twoim systemie, zapoznaj się z dokumentacją używanego klienta SSH.

1. Za pomocą edytora tekstów otwórz plik `~/.ssh/config`. Jeśli ten plik nie istnieje, możesz go utworzyć, wprowadzając polecenie `touch ~/.ssh/config` w wierszu polecenia.

2. Dodaj następującą zawartość do pliku. Zastąp *CLUSTERNAME* nazwą klastra usługi HDInsight:

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    Pozwoli to na skonfigurowanie agenta przekazywania SSH dla klastra usługi HDInsight.

3. Przetestuj agenta przekazywania SSH za pomocą następującego polecenia z terminala:

        echo "$SSH_AUTH_SOCK"

    To polecenie zwraca informacje podobne do następującego tekstu:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Jeśli żadne informacje nie zostaną zwrócone, oznacza to, że agent `ssh-agent` nie działa. Zapoznaj się z informacjami na temat skryptów uruchamiania agenta na stronie [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) (Korzystanie z agenta SSH z protokołem SSH) lub z dokumentacją używanego klienta SSH, aby uzyskać szczegółowe instrukcje instalowania i konfigurowania agenta `ssh-agent`.

4. Po upewnieniu się, że program **ssh-agent** działa, należy wykonać następujące czynności, aby dodać klucz prywatny SSH do agenta:

        ssh-add ~/.ssh/id_rsa

    Jeśli klucz prywatny jest przechowywany w innym pliku, zastąp `~/.ssh/id_rsa` ścieżką do pliku.

<a id="domainjoined"></a>
### <a name="domain-joined-hdinsight"></a>Usługa HDInsight przyłączona do domeny

[Przyłączona do domeny usługa HDInsight](hdinsight-domain-joined-introduction.md) integruje uwierzytelnianie Kerberos z usługą Hadoop. Ponieważ użytkownik SSH nie jest użytkownikiem domeny usługi Active Directory, nie można uruchamiać poleceń platformy Hadoop bez uwierzytelnienia w usłudze Active Directory. Wykonaj poniższe czynności, aby uwierzytelnić sesję SSH w usłudze Active Directory:

1. Połącz się z przyłączonym do domeny klastrem usługi HDInsight za pomocą protokołu SSH, zgodnie z opisem w sekcji [Łączenie się z usługą HDInsight](#connect). Na przykład za pomocą następującego polecenia możesz połączyć się z klastrem usługi HDInsight o nazwie __myhdi__ przy użyciu konta SSH o nazwie __sshuser__.

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Użyj następującego polecenia, aby uwierzytelnić się za pomocą nazwy użytkownika domeny i hasła:

        kinit

     Po wyświetleniu monitu wprowadź nazwę użytkownika domeny oraz hasło użytkownika domeny.

    Aby uzyskać więcej informacji na temat sposobu konfigurowania użytkowników domeny dla przyłączonych do domeny klastrów usługi HDInsight, zobacz artykuł [Configure Domain-joined HDInisight clusters](hdinsight-domain-joined-configure.md) (Konfigurowanie przyłączonych do domeny klastrów usługi HDInisight).

Po uwierzytelnieniu za pomocą polecenia `kinit` możesz używać poleceń platformy Hadoop, takich jak `hdfs dfs -ls /` i `hive`.

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>Tunelowanie SSH

Protokół SSH może również służyć do tunelowania żądań lokalnych, takich jak żądania sieci Web, do klastra usługi HDInsight. Żądania będą następnie kierowane do żądanego zasobu, tak jakby pochodziły z węzła głównego klastra usługi HDInsight.

> [!IMPORTANT]
> Tunel SSH jest konieczny do uzyskiwania dostępu do interfejsu użytkownika sieci Web niektórych usług Hadoop. Na przykład interfejsy użytkownika usługi Historia zadań i Menedżera zasobów są dostępne wyłącznie przy użyciu tunelu SSH.

Aby uzyskać więcej informacji dotyczących tworzenia i używania tunelu SSH, zobacz artykuł [Use SSH Tunneling to access Ambari web UI, JobHistory, NameNode, Oozie, and other web UI's](hdinsight-linux-ambari-ssh-tunnel.md) (Korzystanie z tunelowania SSH w celu uzyskania dostępu do interfejsów użytkownika sieci Web Ambari, JobHistory, NameNode, Oozie i innych).

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu się ze sposobem uwierzytelniania przy użyciu klucza SSH dowiedz się, jak korzystać z usługi MapReduce z Hadoop w usłudze HDInsight.

* [Korzystanie z programu Hive z usługą HDInsight](hdinsight-use-hive.md)
* [Korzystanie z języka Pig z usługą HDInsight](hdinsight-use-pig.md)
* [Korzystanie z zadań MapReduce z usługą HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Feb17_HO3-->


