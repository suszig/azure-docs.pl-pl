<properties
    pageTitle="Maven を使用した HBase アプリケーションのビルドと、Windows ベースの HDInsight へのデプロイ |Microsoft Azure"
    description="Apache Maven を使用して Java ベースの Apache HBase アプリケーションをビルドし、Windows ベースの Azure HDInsight クラスターにデプロイする方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="larryfr"/>

#Use Maven to build Java applications that use HBase with HDInsight (Hadoop) (HDInsight (Hadoop) 環境の HBase を使用する Java アプリケーションを Maven で構築)

作成およびビルドする方法について説明する [Apache HBase](http://hbase.apache.org/) Apache Maven を使って Java でアプリケーションです。 その後、このアプリケーションを Azure HDInsight (Hadoop) で使用します。

[Maven](http://maven.apache.org/) ソフトウェア、ドキュメントについては、Java プロジェクトのレポートを作成できるようにするソフトウェア プロジェクト管理および包含ツールです。 この記事では、このツールを使用して、Azure HDInsight クラスターでの HBase テーブルの作成、クエリ、および削除を実行する基本的な Java アプリケーションを作成する方法について説明します。

##必要条件

* [Java プラットフォーム JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 以降

* [Maven](http://maven.apache.org/)

* [Azure HDInsight クラスターと HBase](hdinsight-hbase-get-started.md#create-hbase-cluster)

##プロジェクトを作成する

1. 開発環境のコマンド ラインから、プロジェクトを作成する場所にディレクトリを変更します。例: `cd code\hdinsight`

2. 使用して、 __mvn__ コマンド、Maven でインストールされた、プロジェクトのスキャフォールディングを生成します。

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    現在のディレクトリに新しいディレクトリで指定された名前で作成、 __artifactID__ パラメーター (**hbaseapp** この例では)。このディレクトリには、次の項目が含まれます。

    * __pom.xml__: プロジェクト オブジェクト モデル ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) プロジェクトをビルドに使用される情報と構成の詳細が含まれています。

    * __src__: 格納されているディレクトリ、 __main \java\com\microsoft\examples__ ディレクトリ、アプリケーションを作成します。

3. 削除、 __src\test\java\com\microsoft\examples\apptest.java__ ファイルのこの例では使用されないためです。

##プロジェクト オブジェクト モデルを更新する

1. 編集、 __pom.xml__ ファイルし、次のコードを追加、 `<dependencies>` セクション。

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>0.98.4-hadoop2</version>
        </dependency>

    これは Maven に対して、プロジェクトが必要である __hbase クライアント__ バージョン __0.98.4-hadoop2__します。 これはコンパイル時に、既定の Maven リポジトリからダウンロードされます。 使用することができます、 [Maven セントラル リポジトリ検索](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) をこの依存関係の詳細を参照してください。

2. 次のコードを追加、 __pom.xml__ ファイルです。 これは、ファイルの `<project>...</project>` タグ内に配置する必要があります (たとえば `</dependencies>` と `</project>` の間)。

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                <source>1.6</source>
                <target>1.6</target>
              </configuration>
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
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                  <goals>
                    <goal>shade</goal>
                  </goals>
                </execution>
              </executions>
            </plugin>
          </plugins>
        </build>

    これにより、リソースが構成 (__conf \hbase-site.xml__,、) HBase の構成情報を格納しています。

    > [AZURE.NOTE] また、コードを介して構成値を設定することができます。 内のコメントを参照してください、 __CreateTable__ これを行う方法については、次の例です。

    これも構成されている、 [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) と [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/)します。 トポロジのコンパイルにはコンパイラ プラグインが使用されます。 シャードのプラグインは、Maven でビルドされる JAR パッケージ内のライセンスの重複を防ぐために使用されます。 ライセンス ファイルの重複は、HDInsight クラスターでの実行時に発生するエラーの原因となるためです。 maven-shade-plugin を `ApacheLicenseResourceTransformer` 実装で使用すると、エラーを回避できます。

    また、maven-shade-plugin は、アプリケーションで必要とされるすべての依存関係を含む uberjar (または fatjar) も生成します。

3. 保存、 __pom.xml__ ファイルです。

4. という名前の新しいディレクトリを作成する __conf__ で、 __hbaseapp__ ディレクトリ。  __Conf__ ディレクトリという名前の新しいファイルを作成 __hbase-site.xml__ し、内容として以下を使用します。

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
         * Copyright 2010 The Apache Software Foundation
         *
         * Licensed to the Apache Software Foundation (ASF) under one
         * or more contributor license agreements.  See the NOTICE file
         * distributed with this work for additional information
         * regarding copyright ownership.  The ASF licenses this file
         * to you under the Apache License, Version 2.0 (the
         * "License"); you may not use this file except in compliance
         * with the License.  You may obtain a copy of the License at
         *
         *     http://www.apache.org/licenses/LICENSE-2.0
         *
         * Unless required by applicable law or agreed to in writing, software
         * distributed under the License is distributed on an "AS IS" BASIS,
         * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
         * See the License for the specific language governing permissions and
         * limitations under the License.
         */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    このファイルは、HDInsight クラスター用の HBase 構成の読み込みに使用されます。

    > [AZURE.NOTE] これは、最小限の hbase-site.xml ファイルであり、HDInsight クラスター用の最小限の設定が含まれています。 完全バージョンの hbase-site.xml 構成ファイルを HDInsight で使用されるのでは、次を参照してください。 [Azure ポータルを使用して HDInsight で Hadoop の管理のクラスター](hdinsight-administer-use-management-portal.md#rdp)します。 hbase-site.xml ファイルは C:\apps\dist\hbase-&lt;version number>-hadoop2\conf ディレクトリにあります。 ファイル パスの "version number" の部分は、クラスターで HBase が更新されると変わります。

3. 保存、 __hbase-site.xml__ ファイルです。

##アプリケーションを作成する

1. 移動して、 __\src\main\java\com\microsoft\examples__ ディレクトリや名前変更、app.java ファイルを __CreateTable.java__します。

2. 開いている、 __CreateTable.java__ ファイルし、次に、既存の内容を置き換えます。

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            //   in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    これは、 __CreateTable__ という名前のテーブルを作成するクラス __人__ し定義済みユーザーを設定します。

3. 保存、 __CreateTable.java__ ファイルです。

4.  __\Src\main\java\com\microsoft\examples__ ディレクトリという名前の新しいファイルを作成する __SearchByEmail.java__します。 このファイルの内容として以下を使用します。

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

     __SearchByEmail__ クラスを使用する電子メール アドレスによって行を照会します。 正規表現フィルターが使用されるため、このクラスを使用するときに文字列または正規表現を指定できます。

5. 保存、 __SearchByEmail.java__ ファイルです。

6.  __\Src\main\hava\com\microsoft\examples__ ディレクトリという名前の新しいファイルを作成する __DeleteTable.java__します。 このファイルの内容として以下を使用します。

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    このクラスは、この例のクリーンアップを無効にし、によって作成されたテーブルを削除して、 __CreateTable__ クラスです。

7. 保存、 __DeleteTable.java__ ファイルです。

##アプリケーションをビルドおよびパッケージ化する

1. コマンド プロンプトを開き、ディレクトリに移動、 __hbaseapp__ ディレクトリ。

2. 次のコマンドを使用し、アプリケーションを含む JAR ファイルをビルドします。

        mvn clean package

    これにより、前のビルド アーティファクトを整理し、まだインストールされていない依存関係をダウンロードして、アプリケーションをビルドしてパッケージ化します。

3. コマンドが完了したら、 __hbaseapp \target__ ディレクトリのという名前のファイルが格納 __hbaseapp 1.0-SNAPSHOT.jar__します。

    > [AZURE.NOTE]  __Hbaseapp 1.0-SNAPSHOT.jar__ ファイルは、uber アプリケーションを実行するために必要なすべての依存関係を含む jar が (fat jar とも呼ばれます)。

##JAR ファイルをアップロードしてジョブを開始する

> [AZURE.NOTE] 」の説明に従って、HDInsight クラスターにファイルをアップロードする方法はたくさんあります [HDInsight での Hadoop ジョブ用データのアップロード](hdinsight-upload-data.md)します。 次の手順を使用して [Azure PowerShell](../install-configure-powershell.md)します。

1. インストールして、Azure PowerShell を構成するという名前の新しいファイルを作成後 __hbase-runner.psm1__します。 このファイルの内容として以下を使用します。

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccountName `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccountName `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
                -ResourceGroupName $resourceGroup `
                | %{ $_.Key1 }
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    このファイルには 2 つのモジュールが含まれます。

    * __Add-hdinsightfile__ - HDInsight へのファイルのアップロードに使用されます

    * __Start-hbaseexample__ - 前に作成したクラスの実行に使用されます

2. 保存、 __hbase-runner.psm1__ ファイルです。

3. 新しい Azure PowerShell ウィンドウを開き、ディレクトリに移動、 __hbaseapp__ ディレクトリ、および次のコマンドを実行します。

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    パスの場所を変更、 __hbase-runner.psm1__ ファイルが以前に作成します。 これにより、この Azure PowerShell セッションのモジュールが登録されます。

2. 次のコマンドを使用して、アップロード、 __hbaseapp 1.0-SNAPSHOT.jar__ を HDInsight クラスター。

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    置換 __hdinsightclustername__ 、HDInsight クラスターの名前に置き換えます。 このコマンドはアップロード、 __hbaseapp 1.0-SNAPSHOT.jar__ に、 __/jar ファイルの例__ 、HDInsight クラスターのプライマリ ストレージの場所。

3. ファイルをアップロードした後は、次のコードを使用して、使用した新しいテーブルを作成する、 __hbaseapp__:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    置換 __hdinsightclustername__ 、HDInsight クラスターの名前に置き換えます。

    このコマンドはという名前の新しいテーブルを作成 __人__ 、HDInsight クラスター。 このコマンドを実行しても、コンソール ウィンドウに出力結果は表示されません。

2. テーブル内のエントリを検索するには、次のコマンドを使用します。

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    置換 __hdinsightclustername__ 、HDInsight クラスターの名前に置き換えます。

    このコマンドを使用して、 **SearchByEmail** クラスすべての行を検索する場所、 __contactinformation__ 列ファミリと __電子メール__ ] 列には、文字列が含まれています。 __contoso.com__します。 次の結果が表示されます。

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

    使用して __fabrikam.com__ の `-emailRegex` を持つユーザーを返します __fabrikam.com__ 電子メール フィールドにします。 この検索は正規の式に基づくフィルターを使用して実装される、ために入力することも、正規表現など __^ r__, 、エントリ返す、電子メールが"r"という文字で始まる場所。

##テーブルを削除する

例では、完了したときに、Azure PowerShell セッションから次のコマンドを使用して、削除を __人__ この例で使用されるテーブル。

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

置換 __hdinsightclustername__ 、HDInsight クラスターの名前に置き換えます。

##トラブルシューティング

###Start-HBaseExample を使用したときに、結果が表示されないか、予期しない結果が表示される

`-showErr` パラメーターを使用して、ジョブの実行中に生成された標準エラー (STDERR) を表示します。

