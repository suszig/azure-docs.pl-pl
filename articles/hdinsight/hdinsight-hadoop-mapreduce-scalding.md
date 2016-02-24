<properties
 pageTitle="Maven での Scalding MapReduce ジョブの開発 | Microsoft Azure"
 description="Maven を使用して Scalding MapReduce ジョブを作成した後、HDInsight クラスター上の Hadoop にジョブをデプロイして実行する方法を説明します。"
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="paulettm"
 editor="cgronlun"
    tags="azure-portal"/>
<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="12/04/2015"
 ms.author="larryfr"/>

# HDInsight での Apache Hadoop による Scalding MapReduce ジョブの開発

Scalding は、Hadoop MapReduce ジョブの作成を容易にする Scala ライブラリです。 簡潔な構文とともに Scala との緊密な統合を提供します。

このドキュメントでは、Maven を使用して Scalding で記述された基本的なワード カウント MapReduce ジョブを作成する方法について説明します。 その後、HDInsight クラスターにこのジョブをデプロイして実行する方法を学習します。

## 前提条件

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
* **Windows または Linux は、HDInsight クラスターで Hadoop をベース**します。 参照してください [HDInsight でのプロビジョニングの Linux ベースの Hadoop](hdinsight-hadoop-provision-linux-clusters.md) または [プロビジョニング Windows ベースの Hadoop を HDInsight で](hdinsight-provision-clusters.md) の詳細。

* **[Maven](http://maven.apache.org/)**

* **[Java プラットフォーム JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 以降**

## プロジェクトの作成とビルド

1. 次のコマンドを使用して、新しい Maven プロジェクトを作成します。

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    このコマンドはという名前の新しいディレクトリを作成 **scaldingwordcount**, 、し、Scala アプリケーションのスキャフォールディングを作成します。

2.  **Scaldingwordcount** ディレクトリ、開く、 **pom.xml** ファイルし、次の内容を置き換えます。

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
          <modelVersion>4.0.0</modelVersion>
          <groupId>com.microsoft.example</groupId>
          <artifactId>scaldingwordcount</artifactId>
          <version>1.0-SNAPSHOT</version>
          <name>${project.artifactId}</name>
          <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
          </properties>
          <repositories>
            <repository>
              <id>conjars</id>
              <url>http://conjars.org/repo</url>
            </repository>
            <repository>
              <id>maven-central</id>
              <url>http://repo1.maven.org/maven2</url>
            </repository>
          </repositories>
          <dependencies>
            <dependency>
              <groupId>com.twitter</groupId>
              <artifactId>scalding-core_2.11</artifactId>
              <version>0.13.1</version>
            </dependency>
            <dependency>
              <groupId>org.apache.hadoop</groupId>
              <artifactId>hadoop-core</artifactId>
              <version>1.2.1</version>
              <scope>provided</scope>
            </dependency>
          </dependencies>
          <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
              <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                  <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                      <goal>add-source</goal>
                      <goal>compile</goal>
                    </goals>
                  </execution>
                </executions>
              </plugin>
              <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                  <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                  </transformers>
                  <filters>
                    <filter>
                      <artifact>*:*</artifact>
                      <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                      </excludes>
                    </filter>
                  </filters>
                </configuration>
                <executions>
                  <execution>
                    <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                    <configuration>
                      <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                          <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                      </transformers>
                    </configuration>
                  </execution>
                </executions>
              </plugin>
            </plugins>
          </build>
        </project>

    このファイルでは、プロジェクト、依存関係、およびプラグインが記述されています。 重要なエントリは次のとおりです。

    * **maven.compiler.source** と **maven.compiler.target**: このプロジェクトの Java のバージョンを設定

    * **リポジトリ**: このプロジェクトで使用される依存関係ファイルを含むリポジトリです

    * **scalding-core_2.11** と **hadoop コア**: このプロジェクトは Scalding と Hadoop の両方のコア パッケージによって異なります。

    * **maven のプラグイン scala**: scala アプリケーションをコンパイルするためのプラグイン

    * **maven shade-プラグイン**: シェードされた (ファット) jar を作成するプラグイン。 このプラグインはフィルターおよび変換を適用します。具体的には次のとおりです。

        * **フィルター**: 適用されたフィルターは、jar ファイルに組み込まれるメタ情報を変更します。 実行時に署名の例外が発生しないように、フィルターを使用して、依存関係で組み込まれる可能性のある、さまざまな署名ファイルを除外します。

        * **実行**: パッケージ フェーズ実行構成の指定、 **com.twitter.scalding.Tool** パッケージのメイン クラスとしてクラスです。 これを指定しない場合は、hadoop コマンドでジョブを実行するときに、com.twitter.scalding.Tool を、アプリケーション ロジックを含むクラスと共に指定する必要があります。

3. 削除、 **src/テスト** ようにして、テストを作成しない次の例ではディレクトリです。

4. 開いている、 **src/main/scala/com/microsoft/example/app.scala** ファイルし、次の内容を置き換えます。

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
          // 1. Read lines from the specified input location
          // 2. Extract individual words from each line
          // 3. Group words and count them
          // 4. Write output to the specified output location
          TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

          //Tokenizer to split sentance into words
          def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
          }
        }

    これにより、基本的なワード カウント ジョブが実装されます。

5. ファイルを保存して閉じます。

6. 次のコマンドを使用して、 **scaldingwordcount** ディレクトリをビルドし、アプリケーション パッケージを作成します。

        mvn package

    このジョブが完了すると、WordCount アプリケーションを含むパッケージは掲載されて **target/scaldingwordcount-1.0-SNAPSHOT.jar**します。

## Linux ベースのクラスターでのジョブの実行

> [AZURE.NOTE] 次の手順では、SSH および Hadoop コマンドを使用します。 MapReduce ジョブを実行するには、他の方法を参照してください。 [HDInsight で Hadoop MapReduce の使用](hdinsight-use-mapreduce.md)します。

1. 次のコマンドを使用して、HDInsight クラスターにパッケージをアップロードします。

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    これにより、ファイルがローカル システムからヘッド ノードにコピーされます。

    > [AZURE.NOTE] SSH アカウントをセキュリティで保護するパスワードを使用した場合は、パスワードを入力するように求められます。 SSH キーを使用している場合は、`-i` パラメーターと、秘密キーのパスを使用する必要があることがあります。 たとえば、`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.` のように指定します。

2. 次のコマンドを使用して、クラスターのヘッド ノードに接続します。

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] SSH アカウントをセキュリティで保護するパスワードを使用した場合は、パスワードを入力するように求められます。 SSH キーを使用している場合は、`-i` パラメーターと、秘密キーのパスを使用する必要があることがあります。 たとえば、`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net` のように指定します。

3. ヘッド ノードに接続した後、次のコマンドを使用してワード カウント ジョブを実行します。

        hadoop jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasb:///example/data/gutenberg/davinci.txt --output wasb:///example/wordcountout

    これにより、前に実装した WordCount クラスが実行されます。 `--hdfs` HDFS を使用するジョブに指示します。 `--input` 入力テキスト ファイルを指定します、 `--output` 出力の場所を指定します。

4. ジョブが完了した後、次の方法で出力を表示します。

        hadoop fs -text wasb:///example/wordcountout/part-00000

    次のような情報が表示されます。

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## Windows ベースのクラスターでのジョブの実行

> [AZURE.NOTE] 次の手順では、Windows PowerShell を使用します。 MapReduce ジョブを実行するには、他の方法を参照してください。 [HDInsight で Hadoop MapReduce の使用](hdinsight-use-mapreduce.md)します。

1. [Azure PowerShell インストールおよび構成](../powershell-install-configure.md)します。

2. Azure PowerShell を起動し、Azure アカウントにログインします。 資格情報を提供すると、コマンドがアカウントの情報を返します。

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. 複数のサブスクリプションがある場合、デプロイに使用するサブスクリプション ID を提供します。

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] 使用する `Get-AzureRMSubscription` 個々 のサブスクリプション Id が含まれているアカウントに関連付けられているすべてのサブスクリプションの一覧を取得します。

4. WordCount ジョブをアップロードして実行するには、次のスクリプトを使用します。 置換 `CLUSTERNAME` 、HDInsight の名前のクラスター、およびことを確認して `$fileToUpload` への正しいパスが、 __scaldingwordcount 1.0-SNAPSHOT.jar__ ファイルです。

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = "CLUSTERNAME"
        $fileToUpload = "scaldingwordcount-1.0-SNAPSHOT.jar"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"
        
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasb:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                       "--input", `
                       "wasb:///example/data/gutenberg/davinci.txt", `
                       "--output", `
                       "wasb:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: " + $job.JobId
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     スクリプトの実行時に、HDInsight クラスターの管理者のユーザー名とパスワードを入力するように求められます。 ジョブが実行されているときに発生したエラーは、コンソールに記録されます。
     
6. 出力をファイルにダウンロードは、ジョブが完了すると、 __output.txt__ 現在のディレクトリにします。 次のコマンドを実行して、結果を表示します。

        cat output.txt

    ファイルには次のような値が含まれます。

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## 次のステップ

これで、Scalding を使用して HDInsight 用の MapRedcue ジョブを作成する方法に関する説明は終わりです。次のリンクを使用して、Azure HDInsight を操作するその他の方法について調べることもできます。

* [HDInsight での Hive の使用](hdinsight-use-hive.md)

* [HDInsight での Pig の使用](hdinsight-use-pig.md)

* [HDInsight での MapReduce ジョブの使用](hdinsight-use-mapreduce.md)

