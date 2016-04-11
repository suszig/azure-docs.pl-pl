<properties 
   pageTitle="Service Bus ブローカー メッセージング .NET チュートリアル | Microsoft Azure"
   description="ブローカー メッセージング .NET チュートリアル。"
   services="service-bus"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/14/2015"
   ms.author="sethm" />

# Service Bus ブローカー メッセージング .NET チュートリアル

Azure Service Bus では、2 種類の包括的メッセージング ソリューションが提供されています。1 つ目は、クラウドで実行する一元的な "リレー" サービスによるもので、SOAP、WS-*、REST など、広範なトランスポート プロトコルと Web サービスの標準をサポートします。 クライアントは、オンプレミス サービスと直接接続する必要はありません。また、クライアントはオンプレミス サービスの場所を知っている必要はありません。オンプレミス サービス側では、ファイアウォールの着信ポートを開く必要がありません。

2 つ目のメッセージング ソリューションでは、"ブローカー" メッセージング機能を使用します。 この機能は、非同期的または分離されたメッセージング機能と考えることができ、Service Bus メッセージング インフラストラクチャを使用する発行/サブスクライブ、一時的な切り離し、負荷分散のシナリオをサポートします。 分離型通信には、クライアントとサーバーを必要に応じて接続し、非同期に操作を実行できるなど多数の利点があります。

このチュートリアルでは、Service Bus ブローカー メッセージングのコア コンポーネントの 1 つであるキューに関する概要と実践的使用例を説明します。 このチュートリアルの一連のトピックでは、メッセージの一覧を設定し、キューを作成して、そのキューにメッセージを送信するアプリケーションを作成します。 最後に、アプリケーションはキューからメッセージを受信して表示した後、リソースをクリーンアップして終了します。 Service Bus「リレー型」メッセージング機能を使用するアプリケーションを構築する方法を説明する対応するチュートリアルについては、 [Service Bus リレー メッセージのチュートリアル](service-bus-relay-tutorial.md)します。

## 概要と前提条件

キューでは、コンシューマーが競合している場合のメッセージ配信に先入先出法 (FIFO) を使用します。 FIFO では、通常、メッセージはキューに追加された順番に受信され、処理されます。このとき、メッセージを受信して処理できるメッセージ コンシューマーは、メッセージ 1 件につき 1 つだけです。 キューを使用する主なメリットが実現するためには *一時的な切り離し* アプリケーション コンポーネントの: つまり、プロデューサーとコンシューマーする必要はありませんの送信とキューにメッセージが永続的に格納されるため、同時にメッセージを受信します。 関連する利点が *負荷平準化*, 、異なるレートでメッセージを送受信するには、プロデューサーとコンシューマーを有効にします。

このチュートリアルを始める前に済ましておく必要がある管理および準備の手順を次に示します。 最初に、サービス名前空間を作成し、Shared Access Signature (SAS) キーを取得します。 サービス名前空間は、Service Bus によって公開される各アプリケーションのアプリケーション境界を提供します。 サービス名前空間が作成された時点で、SAS キーが生成されます。 サービス名前空間と SAS キーの組み合わせが、アプリケーションへのアクセスを Service Bus が認証する資格情報になります。

### サービス名前空間を作成し、SAS キーを取得する

1. サービス名前空間を作成する」の手順に従います [How To: 作成または Service Bus Service 名前空間を変更する](https://msdn.microsoft.com/library/azure/hh690931.aspx)です。

1. メイン ウィンドウで、 [Azure クラシック ポータル][], 、前の手順で作成した名前空間の名前をクリックします。

1. クリックして **構成**します。

1.  **共有アクセス署名の生成** セクションで、関連付けられたプライマリ キーを書き留めて、 **RootManagerSharedAccessKey** ポリシー、またはクリップボードにコピーします。 この値は、このチュートリアルの後半で使用します。

次の手順では、Visual Studio プロジェクトを作成し、メッセージのコンマ区切りの一覧を読み込む、厳密に型指定された 2 つのヘルパー関数を記述、 [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [リスト](https://msdn.microsoft.com/library/6sh2ey19.aspx) オブジェクトです。

### Visual Studio プロジェクトを作成する

1. [スタート] メニューでプログラムを右クリックし、管理者として Visual Studio を開きます **管理者として実行**します。

1. 新しいコンソール アプリケーション プロジェクトを作成します。 クリックして、 **ファイル** メニューをクリック **新規**, 、] をクリックし、 **プロジェクト**します。  **新しいプロジェクト** ダイアログ ボックスで、] をクリックして **Visual C# の場合** (場合 **Visual c#** 見当たらない場合は、下を見て **他の言語**)、] をクリックして、 **コンソール アプリケーション** テンプレートと名付けます **QueueSample**します。 既定値を使用して **場所**します。 クリックして **OK** プロジェクトを作成します。

1. NuGet パッケージ マネージャーを使用して、Service Bus ライブラリをプロジェクトに追加します。
    1. ソリューション エクスプ ローラーでプロジェクト フォルダーを右クリックし、クリックして **NuGet パッケージの管理**します。
    2.  **Nuget パッケージの管理** ダイアログ ボックスで、オンラインで検索 **Service Bus** ] をクリック **インストール**します。
<br />
1. ソリューション エクスプローラーで、Program.cs ファイルをダブルクリックして、Visual Studio エディターでそのファイルを開きます。 名前空間の名前を、既定の名前である `QueueSample` から `Microsoft.ServiceBus.Samples` に変更します。

    ```
    Microsoft.ServiceBus.Samples
    {
        …
    ```

1. `using` ステートメントを次のコードで示すように変更します。

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Data.csv という名前のテキスト ファイルを作成し、次のコンマ区切りテキストをコピーします。

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Data.csv ファイルを保存して閉じ、ファイルを保存した場所を憶えておきます。

1. ソリューション エクスプ ローラーで、プロジェクトの名前を右クリックし (この例では **QueueSample**)、をクリックして **追加**, 、] をクリックし、 **既存項目の**です。

1. 手順 6 で作成した Data.csv ファイルの場所に移動します。 [ファイル] メニューの [ **追加**します。 いることを確認 **のすべてのファイル (*.*)* * は、ファイルの種類の一覧で選択されます。

### メッセージのリストを解析する関数を作成する

1. 前に、 `Main()` メソッド、2 つの変数を宣言: 型のいずれかの **DataTable**, 、Data.csv にメッセージの一覧を格納します。 厳密に型指定された型リスト オブジェクト、もう一方でなければなりません [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)します。 後者は、チュートリアルの後の手順で使用する仲介型メッセージのリストです。

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        publicclass Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. 外部 `Main()`, 、定義、 `ParseCSV()` で Data.csv でメッセージの一覧を解析し、メッセージにするメソッド、 [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) テーブル、次のようにします。 このメソッドを返します、 **DataTable** オブジェクトです。

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. `Main()` メソッドに、`ParseCSVFile()` メソッドを呼び出すステートメントを追加します。

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### メッセージのリストを読み込む関数を作成する

1. 外部 `Main()`, 、定義、 `GenerateMessages()` を受け取るメソッド、 **DataTable** によって返されるオブジェクト `ParseCSVFile()` して厳密に型指定された仲介型メッセージの一覧にテーブルを読み込みます。 メソッドが戻ります、 **リスト** オブジェクトは、次の例に示すようにします。 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. `Main()` で、`ParseCSVFile()` の呼び出しのすぐ下に、`GenerateMessages()` メソッドを呼び出すステートメントを追加し、`ParseCSVFile()` からの戻り値を引数として渡します。

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### ユーザーの資格情報を取得する

1. 最初に、これらの値を保持するために、3 つのグローバル文字列変数を作成します。 これらの変数は前の変数宣言の直後で宣言します。次に例を示します。

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        publicclass Program
        {
    
            privatestatic DataTable issues;
            privatestatic List<BrokeredMessage> MessageList; 
            // Add these variablesprivatestaticstring ServiceNamespace;
            privatestaticstring sasKeyName = "RootManageSharedAccessKey";
            privatestaticstring sasKeyValue;
            …
    ```

1. 次に、サービス名前空間と SAS キーを受け取って格納する関数を作成します。 このメソッドを `Main()` の外側に追加します。 次に例を示します。 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the service namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Please enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. `Main()` で、`GenerateMessages()` の呼び出しのすぐ下に、`CollectUserInput()` メソッドを呼び出すステートメントを追加します。

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### ソリューションをビルドします。

 **ビルド** をクリックして Visual Studio のメニューで **ソリューションのビルド** または f6 キーを押してこれまで、正しく作業したことを確認します。

管理資格情報を作成する

これは、Service Bus メッセージング機能チュートリアルの 2 番目の手順です。 この手順では、アプリケーションの承認に使用する Shared Access Signature (SAS) 資格情報を作成するために使用する管理操作を定義します。

## 管理資格情報を作成する

この手順では、アプリケーションの承認に使用する Shared Access Signature (SAS) 資格情報を作成するために使用する管理操作を定義します。

1. わかりやすくするため、このチュートリアルではすべてのキュー操作を独立したメソッドに配置します。 `Program` クラスの `Main()` メソッドの下に `Queue()` メソッドを作成します。 次に例を示します。
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    staticvoid Queue()
    {
    }
    ```

1. 次の手順が使用して、SAS 資格情報を作成するには、 [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) オブジェクトです。 作成メソッドは、`CollectUserInput()` メソッドで取得した SAS キーの名前と値を受け取ります。 次のコードを `Queue()` メソッドに追加します。

    ```
    staticvoid Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```
### 名前空間マネージャーを作成する

1. 前の手順で取得した名前空間名と管理資格情報を含む URI を引数として使用して、新しい名前空間管理オブジェクトを作成します。 前の手順で追加したコードのすぐ下に、このコードを追加します。
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", <namespaceName>, string.Empty), credentials);
    ```

### 例

この時点で、コードは次のようになります。

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Queue();
    }

    static void Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

次の手順では、メッセージを送信するキューを作成します。

## キューにメッセージを送信する

この手順では、キューを作成した後、仲介型メッセージのリストに含まれるメッセージをそのキューに送信します。

### キューを作成し、キューにメッセージを送信する

1. 最初に、キューを作成します。 たとえば、`myQueue` という名前にして、最後の手順で追加した管理操作の直後で宣言します。

    ```
    QueueDescription myQueue;
    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. `Queue()` メソッドでは、新しく作成した Service Bus URI を引数として使用して、メッセージング ファクトリ オブジェクトを作成します。 1 つ前の手順で追加した管理操作の直後に次のコードを追加します。

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
    ```

1. 次に、使用して、キュー オブジェクトを作成、 [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) クラスです。 1 つ前の手順で追加したコードの直後に次のコードを追加します。

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. 次に、前に作成した仲介型メッセージのリストをループして各メッセージをキューに送信するコードを追加します。 1 つ前の手順で追加した `CreateQueueClient()` ステートメントの直後に次のコードを追加します。
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        myQueueClient.Send(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## キューからメッセージを受け取る

この手順では、前の手順で作成したキューからメッセージのリストを取得します。

### レシーバーを作成し、キューからメッセージを受け取る

 `Queue()` メソッドは、キューを反復処理しを使用してメッセージを受信、 [Microsoft.ServiceBus.Messaging.QueueClient.Receive](https://msdn.microsoft.com/library/azure/hh322678.aspx) メソッドは、各メッセージをコンソールに出力します。 1 つ前の手順で追加したコードの直後に、次のコードを追加します。

    ```
    Console.WriteLine("Now receiving messages from Queue.");
    BrokeredMessage message;
    while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
        {
            Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
            message.Complete();
    
            Console.WriteLine("Processing message (sleeping...)");
            Thread.Sleep(1000);
        }
    ```

### `Queue()` メソッドを終了し、リソースをクリーンアップする

前のコードの直後に、メッセージ ファクトリおよびキュー リソースをクリーンアップする次のコードを追加します。

    ```
    factory.Close();
    myQueueClient.Close();
    namespaceClient.DeleteQueue("IssueTrackingQueue");
    ```

### `Queue()` メソッドを呼び出す

最後に、`Main()` から `Queue()` メソッドを呼び出すステートメントを追加します。 Main() の最後に、次の強調表示されているコード行を追加します。
    
    ```
    public static void Main(string[] args)
    {
        // Collect user input
        CollectUserInput();
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    
        // Add this call
        Queue();
    }
    ```

### 例

次のコードが含まれていますが、完全な **QueueSample** アプリケーションです。

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Queue();
    }

    static void Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);

      QueueDescription myQueue;
      myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");

      MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
      QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

      // Send messages
      Console.WriteLine("Now sending messages to the Queue.");
      for (int count = 0; count < 6; count++)
      {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        myQueueClient.Send(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
      }

      Console.WriteLine("Now receiving messages from Queue.");
      BrokeredMessage message;
      while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
      {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
      }

      factory.Close();
      myQueueClient.Close();
      namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## QueueSample アプリケーションをビルドして実行する

上記の手順の完了後に、ビルドして実行する、 **QueueSample** アプリケーションです。

### QueueSample アプリケーションをビルドする

Visual Studio から、 **ビルド** ] メニューのをクリックして **ソリューションのビルド**, 、または F6 キーを押します。 エラーが発生する場合は、前の手順の最後に示されている完全な例を基にして、コードが正しいことを確認してください。

### QueueSample アプリケーションを実行する

1. アプリケーションを実行する前に必ずサービス名前空間を作成し、SAS キーの取得が」の説明に従って [概要と前提条件](#introduction-and-prerequisites)します。

1. ブラウザーを開きに移動、 [Azure クラシック ポータル][]します。

1. クリックして **Service Bus** 左側のツリーです。

1. 使用する名前空間の名前をクリックします。 ページの下部にあるをクリックして **接続情報**します。 SAS キーを含む接続文字列を書き留めておくか、クリップボードにコピーします。

1. Visual Studio から、 **デバッグ** ] メニューのをクリックして **[デバッグ開始]**, 、F5 キーを押します。 プロンプトが表示されたら、サービス名前空間の名前と、前の手順で取得したキーを入力します。

## 次のステップ

このチュートリアルでは、Service Bus の ブローカー メッセージング機能を使用して、Service Bus クライアント アプリケーションとサービスを構築する方法を紹介しました。 Service Bus を使用する類似のチュートリアル [リレー型メッセージング](service-bus-messaging-overview.md/#Relayed-messaging), を参照してください、 [Service Bus リレー メッセージのチュートリアル](service-bus-relay-tutorial.md)します。

詳細について [Service Bus](https://azure.microsoft.com/services/service-bus/), 、次のトピックを参照してください。

- [Service Bus メッセージングの概要](service-bus-messaging-overview.md)
- [Service Bus の基礎](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus のアーキテクチャ](service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com
