<properties
 pageTitle="Linux VM で Microsoft HPC Pack を使用してNAMD を実行する| Microsoft Azure"
 description="Microsoft HPC Pack クラスターを Azure にデプロイし、複数の Linux コンピューティング ノード上で charmrun を使用して NAMD シミュレーションを実行します。"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
 ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="12/02/2015"
 ms.author="danlep"/>

# Azure の Linux コンピューティング ノード上で Microsoft HPC Pack を使用して NAMD を実行する

この記事は、複数の Linux 計算ノードと共に Azure 上の Microsoft HPC Pack クラスターをデプロイして実行する方法を示します、 [NAMD](http://www.ks.uiuc.edu/Research/namd/) ジョブが **charmrun** を計算し、大規模な biomolecular システムの構造を視覚化します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。



NAMD (ナノスケール分子力学プログラム) とは、ウイルス、セル構造体、および巨大タンパク質など、最大で数百万個の原子を含む大規模な生体分子系を高いパフォーマンスでシミュレーションするために設計された並列分子動力学パッケージです。 NAMD は、通常のシミュレーションでは数百個のコアに対応し、大規模なシミュレーションでは 500,000 個を超えるコアに対応します。

Microsoft HPC Pack は、Microsoft Azure 仮想マシンのクラスター上で各種の大規模な HPC および並列アプリケーション (MPI アプリケーションなど) を実行する機能を備えています。 Microsoft HPC Pack 2012 R2 Update 2 以降、HPC Pack では、HPC Pack クラスターにデプロイされた Linux 計算ノード VM で Linux HPC アプリケーションを実行する機能もサポートしています。 参照してください [Azure で HPC Pack クラスターで Linux 計算ノードを使ってみる](virtual-machines-linux-cluster-hpcpack.md) の概要についてです。


## 前提条件

* **Linux での HPC Pack クラスター計算ノード** -を参照してください [Azure で HPC Pack クラスターで Linux 計算ノードを使ってみる](virtual-machines-linux-cluster-hpcpack.md) の前提条件と Linux の HPC Pack クラスターを展開する手順は、Azure Marketplace で Azure PowerShell スクリプトと HPC Pack イメージを使用して、Azure 上のノードを計算します。

    Windows Server 2012 R2 ヘッド ノードおよび 4 つの Large サイズ (A3) CentOS 6.6 コンピューティング ノードから成る Azure ベースの HPC Pack クラスターをデプロイする際に、スクリプトと一緒に使用することができる XML 構成ファイルのサンプルを次に示します。 該当する値は、実際のサブスクリプションとサービス名に置き換えてください。

    ```
    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionName>Subscription-1</SubscriptionName>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>West US</Location>  
      <VNet>
        <VNetName>MyVNet</VNetName>
        <SubnetName>Subnet-1</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpclab.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>CentOS66HN</VMName>
        <ServiceName>MyHPCService</ServiceName>
        <VMSize>Large</VMSize>
        <EnableRESTAPI />
        <EnableWebPortal />
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
        <ServiceName>MyLnxCNService</ServiceName>
        <VMSize>Large</VMSize>
        <NodeCount>4</NodeCount>
        <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>    
```


* **NAMD software and tutorial files** - Download NAMD software for Linux from the [NAMD](http://www.ks.uiuc.edu/Research/namd/) site. This article is based on NAMD version 2.10, and uses the [Linux-x86_64 (64-bit Intel/AMD with Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archive, which you'll use to run NAMD on multiple Linux compute nodes in a cluster network. Also download the [NAMD tutorial files](http://www.ks.uiuc.edu/Training/Tutorials/#namd). Follow the instructions later in this article to extract the archive and the tutorial samples on the cluster head node.

* **VMD** (optional) - To see the results of your NAMD job, download and install the molecular visualization program [VMD](http://www.ks.uiuc.edu/Research/vmd/) on a computer of your choice. The current version is 1.9.2. See the VMD download site to get started.  


## Set up mutual trust between compute nodes
Running a cross-node job on multiple Linux nodes requires the nodes to trust each other (by **rsh** or **ssh**). When you create the HPC Pack cluster with the Microsoft HPC Pack IaaS deployment script, the script automatically sets up permanent mutual trust for the administrator account you specify. For non-administrator users you create in the cluster's domain, you have to set up temporary mutual trust among the nodes when a job is allocated to them, and destroy the relationship after the job is complete. To do this for each user, provide an RSA key pair to the cluster which HPC Pack uses to establish the trust relationship.

### Generate an RSA key pair
It's easy to generate an RSA key pair, which contains a public key and a private key, by running the Linux **ssh-keygen** command.

1.  Log on to a Linux computer.

2.  Run the following command.

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Press **Enter** to use the default settings until the command is completed. Do not enter a passphrase here; when prompted for a password, just press **Enter**.

    ![Generate an RSA key pair][keygen]

3.  Change directory to the ~/.ssh directory. The private key is stored in id_rsa and the public key in id_rsa.pub.

    ![Private and public keys][keys]

### Add the key pair to the HPC Pack cluster
1.  Make a Remote Desktop connnection to your head node with your HPC Pack administrator account (the administrator account you set up when you ran the deployment script).

2. Use standard Windows Server procedures to create a domain user account in the cluster's Active Directory domain. For example, use the Active Directory User and Computers tool on the head node. The examples in this article assume you create a domain user named hpclab\hpcuser.

2.  Create a file named C:\cred.xml and copy the RSA key data into it. You can find an example in the sample files at the end of this article.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Open a Command Prompt and enter the following command to set the credentials data for the hpclab\hpcuser account. You use the **extendeddata** parameter to pass the name of C:\cred.xml file you created for the key data.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    This command completes successfully without output. After setting the credentials for the user accounts you need to run jobs, store the cred.xml file in a secure location, or delete it.

5.  If you generated the RSA key pair on one of your Linux nodes, remember to delete the keys after you finish using them. HPC Pack does not set up mutual trust if it finds an existing id_rsa file or id_rsa.pub file.

>[AZURE.IMPORTANT] We don’t recommend running a Linux job as a cluster administrator on a shared cluster, because a job submitted by an administrator runs under the root account on the Linux nodes. A job submitted by a non-administrator user runs under a local Linux user account with the same name as the job user, and HPC Pack sets up mutual trust for this Linux user across all the nodes allocated to the job. You can set up the Linux user manually on the Linux nodes before running the job, or HPC Pack creates the user automatically when the job is submitted. If HPC Pack creates the user, HPC Pack deletes it after the job completes. The keys are removed after job completion on the nodes to reduce security threats.

## Set up a file share for Linux nodes

Now set up a standard SMB share on a folder on the head node, and mount the shared folder on all Linux nodes to allow the Linux nodes to access NAMD files with a common path. See the file sharing options and steps in [Get started with Linux compute nodes in an HPC Pack Cluster in Azure](virtual-machines-linux-cluster-hpcpack.md). (We recommend mounting a shared folder on the head node in this article because CentOS 6.6 Linux nodes don’t currently support the Azure File service, which provides similar features. For more about mounting an Azure File share, see [Persisting connections to Microsoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).)

1.  Create a folder on the head node, and share it to everyone by setting Read/Write privileges. In this example, \\\\CentOS66HN\Namd is the name of the folder, where CentOS66HN is the host name of the head node.

2. Extract the NAMD files in the folder by using a Windows version of **tar** or another Windows utility that operates on .tar archives. Extract the NAMD tar archive to \\\\CentOS66HN\Namd\namd2, and extract the tutorial files under \\\\CentOS66HN\Namd\namd2\namdsample.

2.  Open a Windows PowerShell window and run the following commands to mount the shared folder.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

The first command creates a folder named /namd2 on all nodes in the LinuxNodes group. The second command mounts the shared folder //CentOS66HN/Namd/namd2 onto the folder with dir_mode and file_mode bits set to 777. The *username* and *password* in the command should be the credentials of a user on the head node.

>[AZURE.NOTE]The “\`” symbol in the second command is an escape symbol for PowerShell. “\`,” means the “,” (comma character) is a part of the command.


## Prepare to run a NAMD job

 Your  NAMD job needs a *nodelist* file for **charmrun** to know the number of nodes to use when starting NAMD processes. You'll write a Bash script that generates the nodelist file and runs **charmrun** with this nodelist file. You can then submit a NAMD job in HPC Cluster Manager that calls this script.

### Environment variables and nodelist file
Information about nodes and cores is in the $CCP_NODES_CORES environment variable, which is automatically set by the HPC Pack head node when the job is activated. The format for the $CCP_NODES_CORES variable is as follows:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
```

This lists the total number of nodes, node names, and number of cores on each node that are allocated to the job. For example, if the job needs 10 cores to run, the value of $CCP_NODES_CORES will be similar to:

```
3 CENTOS66LN 00 4 CENTOS66LN 01 4 CENTOS66LN 03 2
```

Following is the information in the nodelist file, which the script will generate:

```
メインのグループ
host <Name of node1> + + の cpu <Cores of node1>
host <Name of node2> + + の cpu <Cores of node2>
…
```

For example:

```
メインのグループ
CENTOS66LN 00 のホストでは cpu 4
ホスト CENTOS66LN-01 では cpu 4
CENTOS66LN 03 のホストでは cpu 2
```
### Bash script to create a nodelist file

Using a text editor of your choice, create the following Bash script in the folder containing the NAMD program files and name it hpccharmrun.sh. A complete example is in the sample files at the end of this article. This bash script does the following things.

>[AZURE.TIP] Save your script as a text file with Linux line endings (LF only, not CR LF). This ensures that it runs properly on the Linux nodes.

1.  Define some variables.

    ```
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Get node information from the environment variables. $NODESCORES stores a list of split words from $CCP_NODES_CORES. $COUNT is the size of $NODESCORES.

    ```
    # Get node information from the environment variables
    # CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```

3.  If the $CCP_NODES_CORES variable is not set, just start **charmrun** directly. (This should only occur when you run this script directly on your Linux nodes.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Or create a nodelist file for **charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  実行 **charmrun** nodelist ファイルを使用して、リターン ステータスを取得し、最後に nodelist ファイルを削除します。

    ${CCP_NUMCPUS} は、HPC Pack ヘッド ノードによって設定されるもう一つの環境変数です。 この環境変数には、このジョブに割り当てられたコアの合計数が格納されます。 charmrun のプロセス数を指定するために使用します。

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  終了、 **charmrun** ステータスを返します。

    ```
    exit ${RTNSTS}
    ```

## NAMD ジョブの送信

これで、HPC クラスター マネージャーで NAMD ジョブを送信する準備が整いました。

1.  クラスター ヘッド ノードに接続し、HPC クラスター マネージャーを開始します。

2.   **ノード管理**, 、Linux 計算ノードが確実に、 **オンライン** 状態です。 間違っている場合は、選択し、をクリックして **オンライン**します。

2.   **ジョブ管理**, 、クリックして **新しいジョブ**します。

3.  ジョブの名前を入力します。 *hpccharmrun*します。

    ![新しい HPC ジョブ][namd_job]

4.   **ジョブの詳細** ] ページで、[ **ジョブ リソース**, 、としてリソースの種類を選択 **ノード** し、設定、 **最小** 3 にします。 この例では、3 つの Linux ノードでジョブを実行します。各ノードには 4 つのコアがあります。

    ![ジョブのリソース][job_resources]

5.   **タスクの詳細と I/O リダイレクト** ] ページで、ジョブに新しいタスクを追加して、次の値を設定します。

    * **コマンド ライン** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

    * **作業ディレクトリ** -/namd2

    * **最小** - 3

    ![タスクの詳細][task_details]

    >[AZURE.NOTE] 作業ディレクトリの設定がここで **charmrun** 各ノードで同じ作業ディレクトリに移動しようとしています。 作業ディレクトリが設定されていない場合、HPC Pack は、Linux ノードの 1 つに作成された無作為に命名されたフォルダーでコマンドを開始します。 これにより、他のノードで次のエラーが発生します。
`/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` これを回避するには、すべてのノードでの作業ディレクトリとしてアクセスできるフォルダーのパスを指定します。

5.  クリックして **送信** このジョブを実行します。

    既定では、HPC Pack は、ログオンした現在のユーザー アカウントとしてジョブを送信します。 ダイアログ ボックスをクリックした後に、ユーザー名とパスワードを入力するように求める **送信**します。

    ![ジョブの資格情報][資格情報]

    条件によっては、HPC Pack は前に入力されたユーザー情報を記憶していて、このダイアログ ボックスを表示しません。 このダイアログ ボックスが HPC Pack によって再び表示されるようにするには、コマンド ウィンドウに次のコマンドを入力し、ジョブを送信します。

    ```
    hpccred delcreds
    ```

6.  ジョブが終了するまで数分かかります。

7.  ジョブのログを見つける \\<headnodeName>\Namd\namd2\namd2_hpccharmrun.log と出力ファイルに \\<headnode>\Namd\namd2\namdsample\1-2-sphere\ します。

8.  必要に応じて、VMD を起動してジョブの結果を表示します。 この記事では、NAMD を視覚化するための手順 (この場合は、水球体のユビキチン タンパク質分子) については説明しません。 参照してください [NAMD チュートリアル](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) 詳細です。

    ![ジョブの結果][vmd_view]

## サンプル ファイル

### サンプル hpccharmrun.sh スクリプト

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 
### サンプル cred.xml ファイル

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```




<!--Image references-->
[keygen]: ./media/virtual-machines-linux-cluster-hpcpack-namd/keygen.png
[keys]: ./media/virtual-machines-linux-cluster-hpcpack-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-cluster-hpcpack-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-cluster-hpcpack-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-cluster-hpcpack-namd/creds.png
[task_details]: ./media/virtual-machines-linux-cluster-hpcpack-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-cluster-hpcpack-namd/vmd_view.png

