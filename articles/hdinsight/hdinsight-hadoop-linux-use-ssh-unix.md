---
title: "Korzystanie z protokołu SSH z usługą HDInsight (Hadoop) w systemie Windows, Linux, Unix lub OS X | Microsoft Docs"
description: " Do usługi HDInsight można uzyskać dostęp przy użyciu protokołu Secure Shell (SSH). Ten dokument zawiera informacje na temat używania protokołu SSH do łączenia się z usługą HDInsight przy użyciu klientów systemów Windows, Linux, Unix lub OS X."
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
ms.date: 04/03/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: 248e820ccd2c68a8500aab3233c5beea3c8cc868
ms.lasthandoff: 04/04/2017

---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a>Łączenie się z usługą HDInsight (Hadoop) przy użyciu protokołu SSH

Dowiedz się, jak nawiązać bezpieczne połączenia z usługą HDInsight przy użyciu protokołu [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell). Usługa HDInsight może używać systemu Linux (Ubuntu) jako systemu operacyjnego dla węzłów klastra. Protokół SSH umożliwia nawiązywanie połączeń z węzłami głównymi i węzłami krawędzi klastra opartego na systemie Linux oraz uruchamianie poleceń bezpośrednio w tych węzłach.

Poniższa tabela zawiera informacje dotyczące adresów i portów, które są wymagane podczas nawiązywania połączenia z usługą HDInsight przy użyciu protokołu SSH:

| Adres | Port | Element docelowy połączenia |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | Węzeł krawędzi (program R Server w usłudze HDInsight) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | Węzeł krawędzi (inny dowolny typ klastra, jeśli istnieje węzeł krawędzi) |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Podstawowy węzeł główny |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Dodatkowy węzeł główny |

> [!NOTE]
> Element `<edgenodename>` należy zastąpić nazwą węzła krawędzi.
>
> Element `<clustername>` należy zastąpić nazwą klastra usługi HDInsight.
>
> Zalecamy, aby __zawsze nawiązywać połączenie z węzłem krawędzi__, jeśli jest dostępny. Węzły główne hostują usługi, które mają krytyczne znaczenie dla kondycji klastra. W węźle krawędzi działają tylko obciążenia umieszczone przez użytkownika.
>
> Więcej informacji dotyczących używania węzłów krawędzi można znaleźć w temacie [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node) (Używanie węzłów krawędzi w usłudze HDInsight).

## <a name="ssh-clients"></a>Klienci SSH

Większość systemów operacyjnych udostępnia klienta `ssh`. System Microsoft Windows domyślnie nie udostępnia klienta SSH. Klient SSH dla systemu Windows jest dostępny w każdym z następujących pakietów:

* [Bash on Ubuntu on Windows 10](https://msdn.microsoft.com/commandline/wsl/about): polecenie `ssh` jest dostępne za pośrednictwem funkcji Bash w wierszu polecenia systemu Windows.

* [Git (https://git-scm.com/)](https://git-scm.com/): polecenie `ssh` jest dostępne za pośrednictwem wiersza polecenia GitBash.

* [GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/): polecenie `ssh` jest dostępne za pośrednictwem wiersza polecenia powłoki Git Shell. W rozwiązaniu GitHub Desktop można skonfigurować wiersz polecenia dla powłoki Git Shell, wybierając funkcję Bash, wiersz polecenia systemu Windows lub program PowerShell.

* [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): obecnie zespół ds. programu PowerShell pracuje nad przeniesieniem pakietu OpenSSH do systemu Windows. Dodatkowo są udostępniane wersje testowe.

    > [!WARNING]
    > Pakiet OpenSSH zawiera składnik serwera SSH — `sshd`. Składnik ten pozwala uruchomić w systemie serwer SSH i umożliwia innym osobom połączenie się z tym serwerem. Nie konfiguruj tego składnika ani nie otwieraj portu 22, chyba że chcesz hostować w systemie serwer SSH. Składnik ten nie jest wymagany do komunikowania się z usługą HDInsight.

Dostępnych jest również kilka graficznych klientów SSH, takich jak [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) i [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/). Mimo że ci klienci umożliwiają nawiązywanie połączeń z usługą HDInsight, proces łączenia się z serwerem jest inny niż w przypadku narzędzia `ssh`. Aby uzyskać więcej informacji, zobacz dokumentację graficznego klienta, którego używasz.

## <a id="sshkey"></a>Uwierzytelnianie: klucze SSH

Klucze SSH używają [kryptografii klucza publicznego](https://en.wikipedia.org/wiki/Public-key_cryptography) do zabezpieczenia klastra. Klucze SSH są bezpieczniejsze niż hasła i umożliwiają łatwe zabezpieczenie klastra usługi HDInsight.

Jeśli konto SSH zostanie zabezpieczone za pomocą klucza, podczas nawiązywania połączenia klient musi podać zgodny klucz prywatny:

* Większość klientów można skonfigurować do używania __domyślnego klucza__. Na przykład w środowiskach Linux i Unix klient `ssh` szuka klucza prywatnego w lokalizacji `~/.ssh/id_rsa`.

* Można określić __ścieżkę do klucza prywatnego__. W przypadku klienta `ssh` do określenia ścieżki do klucza prywatnego służy parametr `-i`. Na przykład `ssh -i ~/.ssh/hdinsight sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Jeśli masz __wiele kluczy prywatnych__, których używasz dla różnych serwerów, możesz automatycznie wybierać klucz za pomocą takich narzędzi, jak [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent).

> [!IMPORTANT]
>
> W przypadku zabezpieczenia klucza prywatnego za pomocą hasła musisz je podać przed użyciem klucza. Takie narzędzia, jak `ssh-agent`, umożliwiają wygodne przechowywanie haseł w pamięci podręcznej.

### <a name="create-an-ssh-key-pair"></a>Tworzenie pary kluczy SSH

Do tworzenia plików klucza publicznego i prywatnego służy polecenie `ssh-keygen`. Następujące polecenie generuje parę 2048-bitowych kluczy RSA, która pozwala połączyć się z usługą HDInsight:

    ssh-keygen -t rsa -b 2048

W trakcie procesu tworzenia kluczy zostaną wyświetlone monity o podanie informacji. Mogą one dotyczyć na przykład hasła lub lokalizacji przechowywania kluczy. Po zakończeniu tego procesu zostaną utworzone dwa pliki: klucz publiczny i klucz prywatny.

* __Klucz publiczny__ służy do tworzenia klastra usługi HDInsight i ma rozszerzenie `.pub`.

* __Klucz prywatny__ służy do uwierzytelniania klienta w klastrze usługi HDInsight.

> [!IMPORTANT]
> Klucze można zabezpieczyć przy użyciu hasła. W praktyce hasłem zabezpiecza się klucz prywatny. Nawet w przypadku uzyskania klucza prywatnego przez inną osobę do jego użycia jest potrzebne hasło.

### <a name="create-hdinsight-using-the-public-key"></a>Tworzenie klastrów usługi HDInsight przy użyciu klucza publicznego

| Metoda tworzenia | Sposób użycia klucza publicznego |
| ------- | ------- |
| **Witryna Azure Portal** | Usuń zaznaczenie pola __Użyj tego samego hasła podczas logowania do klastra__, a następnie wybierz opcję __Klucz publiczny__ jako typ uwierzytelniania SSH. Na koniec wybierz plik klucza publicznego lub wklej zawartość tekstową pliku w polu __Klucz publiczny SSH__.</br>![Okno dialogowe dotyczące klucza publicznego SSH w procesie tworzenia klastra usługi HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| **Azure PowerShell** | Użyj parametru `-SshPublicKey` polecenia cmdlet `New-AzureRmHdinsightCluster` i przekaż zawartość klucza publicznego jako ciąg.|
| **Interfejs wiersza polecenia platformy Azure 1.0** | Użyj parametru `--sshPublicKey` polecenia `azure hdinsight cluster create` i przekaż zawartość klucza publicznego jako ciąg. |
| **Szablon usługi Resource Manager** | Przykład użycia kluczy SSH razem z szablonem można znaleźć w temacie [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) (Wdrażanie usługi HDInsight w systemie Linux przy użyciu klucza SSH). Element `publicKeys` w pliku [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) umożliwia przekazanie kluczy do platformy Azure podczas tworzenia klastra. |

## <a id="sshpassword"></a>Uwierzytelnianie: hasło

Konta SSH mogą być chronione przy użyciu hasła. Podczas łączenia się z usługą HDInsight przy użyciu protokołu SSH pojawia się monit o podanie hasła.

> [!WARNING]
> W przypadku protokołu SSH nie zalecamy korzystania z uwierzytelniania za pomocą hasła. Hasła można złamać i są one podatne na ataki siłowe. Zamiast tego zalecamy używanie [kluczy SSH w celu uwierzytelniania](#sshkey).

### <a name="create-hdinsight-using-a-password"></a>Tworzenie klastrów usługi HDInsight przy użyciu hasła

| Metoda tworzenia | Sposób określenia hasła |
| --------------- | ---------------- |
| **Azure Portal** | Domyślnie hasło do konta użytkownika SSH jest takie samo jak hasło do konta umożliwiającego logowanie do klastra. Aby użyć innego hasła, usuń zaznaczenie pola __Użyj tego samego hasła podczas logowania do klastra__, a następnie wprowadź hasło w polu __Hasło SSH__.</br>![Okno dialogowe dotyczące hasła SSH w procesie tworzenia klastra usługi HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| **Azure PowerShell** | Użyj parametru `--SshCredential` polecenia cmdlet `New-AzureRmHdinsightCluster` i przekaż obiekt `PSCredential` zawierający nazwę użytkownika i hasło do konta SSH. |
| **Interfejs wiersza polecenia platformy Azure 1.0** | Użyj parametru `--sshPassword` polecenia `azure hdinsight cluster create` i podaj hasło. |
| **Szablon usługi Resource Manager** | Przykład użycia hasła razem z szablonem można znaleźć w temacie [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) (Wdrażanie usługi HDInsight w systemie Linux przy użyciu hasła SSH). Element `linuxOperatingSystemProfile` w pliku [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) umożliwia przekazanie nazwy konta SSH i hasła do platformy Azure podczas tworzenia klastra.|

### <a name="change-the-ssh-password"></a>Zmienianie hasła SSH

Informacje dotyczące zmiany hasła użytkownika konta SSH można znaleźć w sekcji __Change passwords__ (Zmienianie haseł) dokumentu [Manage HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) (Zarządzanie usługą HDInsight).

## <a id="domainjoined"></a>Uwierzytelnianie: przyłączony do domeny klaster usługi HDInsight

Jeśli używasz __przyłączonego do domeny klastra usługi HDInsight__, po nawiązaniu połączenia przy użyciu protokołu SSH musisz uruchomić polecenie `kinit`. Spowoduje to wyświetlenie monitu o podanie nazwy użytkownika domeny i hasła oraz uwierzytelnienie sesji w domenie usługi Azure Active Directory skojarzonej z klastrem.

Aby uzyskać więcej informacji, zobacz [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) (Konfigurowanie przyłączonej do domeny usługi HDInsight).

## <a name="connect-to-worker-and-zookeeper-nodes"></a>Łączenie się z węzłami procesu roboczego i węzłami dozorcy

Węzły procesu roboczego i węzły dozorcy nie są bezpośrednio dostępne z Internetu, ale można łączyć się z nimi z poziomu węzłów głównych lub węzłów krawędzi klastra. Poniżej przedstawiono ogólny zarys czynności, które należy wykonać w celu nawiązania połączenia z innymi węzłami:

1. Połącz się z węzłem głównym lub węzłem krawędzi za pomocą protokołu SSH:

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. Z poziomu połączenia SSH z węzłem głównym lub węzłem krawędzi połącz się z węzłem procesu roboczego w klastrze za pomocą polecenia `ssh`:

        ssh sshuser@wn0-myhdi

    Aby pobrać listę nazw domen węzłów w klastrze, zobacz przykłady w dokumencie [Manage HDInsight by using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) (Zarządzanie usługą HDInsight przy użyciu interfejsu API REST Ambari).

Jeśli konto SSH jest zabezpieczone __hasłem__, będzie konieczne wprowadzenie hasła. Następnie połączenie zostanie nawiązane.

Pozwoli to na skonfigurowanie agenta przekazywania SSH dla klastra usługi HDInsight.Jeśli konto SSH jest zabezpieczone przy użyciu __kluczy SSH__, upewnij się, że w środowisku lokalnym skonfigurowano przekazywanie przez agenta SSH.

> [!NOTE]
> Innym sposobem uzyskiwania bezpośredniego dostępu do wszystkich węzłów w klastrze jest zainstalowanie usługi HDInsight w usłudze Azure Virtual Network. Pozwoli to na przyłączenie komputera zdalnego do tej samej sieci wirtualnej i uzyskiwanie bezpośredniego dostępu do wszystkich węzłów w klastrze.
>
> Aby uzyskać więcej informacji, zobacz [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md) (Używanie sieci wirtualnej z usługą HDInsight).

### <a name="configure-ssh-agent-forwarding"></a>Konfigurowanie przekazywania przez agenta SSH

> [!IMPORTANT]
> W poniższej procedurze założono korzystanie z systemu opartego na systemie Linux/UNIX, ale działa ona również w przypadku funkcji Bash w systemie Windows 10. Jeśli te czynności nie działają w Twoim systemie, zapoznaj się z dokumentacją używanego klienta SSH.

1. Za pomocą edytora tekstów otwórz plik `~/.ssh/config`. Jeśli ten plik nie istnieje, możesz go utworzyć, wprowadzając polecenie `touch ~/.ssh/config` w wierszu polecenia.

2. Dodaj następujący tekst do pliku `config`.

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    Informacje w sekcji __Host__ zastąp adresem węzła, z którym łączysz się przy użyciu protokołu SSH. W poprzednim przykładzie użyto węzła krawędzi. Wpis ten umożliwia skonfigurowanie przekazywania przez agenta SSH dla określonego węzła.

3. Przetestuj agenta przekazywania SSH za pomocą następującego polecenia z terminala:

        echo "$SSH_AUTH_SOCK"

    To polecenie zwraca informacje podobne do następującego tekstu:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Jeśli nie zostaną zwrócone żadne informacje, oznacza to, że agent `ssh-agent` nie działa. Zapoznaj się z informacjami na temat skryptów uruchamiania agenta na stronie [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) (Korzystanie z agenta SSH z protokołem SSH) lub z dokumentacją używanego klienta SSH, aby uzyskać szczegółowe instrukcje instalowania i konfigurowania agenta `ssh-agent`.

4. Po upewnieniu się, że program **ssh-agent** działa, należy wykonać następujące czynności, aby dodać klucz prywatny SSH do agenta:

        ssh-add ~/.ssh/id_rsa

    Jeśli klucz prywatny jest przechowywany w innym pliku, zastąp `~/.ssh/id_rsa` ścieżką do pliku.

5. Połącz się z węzłem krawędzi lub węzłami głównymi klastra przy użyciu protokołu SSH. Następnie połącz się z węzłem procesu roboczego lub węzłem dozorcy przy użyciu polecenia SSH. Połączenie zostanie nawiązane przy użyciu przekazanego klucza.

## <a name="next-steps"></a>Następne kroki

* [Use SSH tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) (Używanie tunelowania SSH z usługą HDInsight)
* [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md) (Używanie sieci wirtualnej z usługą HDInsight)
* [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node) (Używanie węzłów krawędzi w usłudze HDInsight)
