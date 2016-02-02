<properties 
    pageTitle="Windows ユニバーサルでエンゲージメント API を使用する方法" 
    description="Windows ユニバーサルでエンゲージメント API を使用する方法"            
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/07/2015" 
    ms.author="piyushjo" />


# Windows ユニバーサルでエンゲージメント API を使用する方法

このドキュメントは、ドキュメントへのアドオン [Windows ユニバーサルでエンゲージメントを統合する方法](../mobile-engagement-windows-store-integrate-engagement/): アプリケーションの統計情報を報告するエンゲージメント API を使用する方法についての詳細を提供しています。

のみをアプリケーションのセッション、アクティビティ、クラッシュ、および技術情報について報告するエンゲージメントを作成する場合、最も簡単な方法をすべてに留意してください、 `ページ` サブ クラスから継承、 `EngagementPage` クラスです。

アプリケーション固有のイベント、エラー、ジョブを報告する必要がある場合の例については、複数の操作を実行するかに実装されているものよりも、別の方法でアプリケーションのアクティビティを報告する必要がある場合、 `EngagementPage` クラス、エンゲージメント API を使用する必要があります。

エンゲージメント API はによって提供される、 `EngagementAgent` クラスです。 を通じてこれらのメソッドにアクセスできる `EngagementAgent.Instance`します。

エージェントのモジュールが初期化されていない場合でも、API に対する各呼び出しが遅延されると、エージェントが使用可能な場合にもう一度実行されます。

## エンゲージメントの概念

次の部分は、一般的なを絞り込む [モバイル エンゲージメントの概念](../mobile-engagement-concepts/) Windows ユニバーサル プラットフォームです。

### `セッション` と `アクティビティ`

*アクティビティ* つまりは通常、アプリケーションの 1 つのページに関連付けられて、 *アクティビティ* ページが表示され、ページを閉じるときに停止したときに開始: を使用して、エンゲージメント SDK を統合した場合、そうでは、 `EngagementPage` クラスです。

ただし、*アクティビティ*はエンゲージメント API を使用して手動で制御することも可能です。 これにより、このページの使用状況に関する詳しい情報を表示するために、いくつかのサブ部分で特定のページ (たとえばこのページ内でのダイアログの使用頻度と使用時間を知るために) を分割できます。

## アクティビティを報告する

### ユーザーが新しいアクティビティを開始する

#### リファレンス

            void StartActivity(string name, Dictionary<object, object> extras = null)

呼び出す必要がある `StartActivity()` ユーザー アクティビティが変更されるたびにします。 この関数の最初の呼び出しで、新しいユーザー セッションが開始します。
> [AZURE.IMPORTANT] アプリケーションが終了すると SDK は EndActivity メソッドを自動的に呼び出します。 したがって、EndActivity メソッドを呼び出すと現在のセッションが強制的に終了されるので、このメソッドを呼び出さないようにし、ユーザーのアクティビティが変わる際には常に StartActivity メソッドを呼び出すことを強くお勧めします。

#### 例

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### ユーザーが現在のアクティビティを終了する

#### リファレンス

            void EndActivity()

これは、アクティビティとセッションを終了します。 実際に何を実行しているのかがわからない場合は、このメソッドは呼び出さないでください。

#### 例

            EngagementAgent.Instance.EndActivity();

## ジョブを報告する

### ジョブを開始する

#### リファレンス

            void StartJob(string name, Dictionary<object, object> extras = null)

ジョブを使用して、一定期間、特定のタスクを追跡できます。

#### 例

            // An upload begins...
    
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
    
            EngagementAgent.Instance.StartJob("uploadData", extras);

### ジョブを終了する

#### リファレンス

            void EndJob(string name)

ジョブによって追跡されるタスクが終了するとすぐに、ジョブ名を指定して、このジョブの EndJob メソッドを呼び出す必要があります。

#### 例

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
    
            EngagementAgent.Instance.EndJob("uploadData");

## イベントを報告する

イベントには 3 種類あります。

-   スタンドアロン イベント
-   セッション イベント
-   ジョブ イベント

### スタンドアロン イベント

#### リファレンス

            void SendEvent(string name, Dictionary<object, object> extras = null)

スタンドアロン イベントは、セッションのコンテキストの外で発生します。

#### 例

            EngagementAgent.Instance.SendEvent("event", extra);

### セッション イベント

#### リファレンス

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

通常、セッション イベントは、セッション中にユーザーによって実行されるアクションの報告に使用されます。

#### 例

**データなし:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
    
            // or
    
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**データあり:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### ジョブ イベント

#### リファレンス

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

通常、ジョブ イベントは、ジョブ中にユーザーによって実行されるアクションの報告に使用されます。

#### 例

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## エラーの報告

エラーには 3 種類あります。

-   スタンドアロン エラー
-   セッション エラー
-   ジョブ エラー

### スタンドアロン エラー

#### リファレンス

            void SendError(string name, Dictionary<object, object> extras = null)

セッション エラーとは反対に、スタンドアロン エラーはセッションのコンテキストの外で発生します。

#### 例

            EngagementAgent.Instance.SendError("errorName", extras);

### セッション エラー

#### リファレンス

            void SendSessionError(string name, Dictionary<object, object> extras = null)

通常、セッション エラーは、セッション中にユーザーに影響するエラーの報告に使用されます。

#### 例

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### ジョブ エラー

#### リファレンス

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

エラーは、現在のユーザー セッションに関連付ける代わりに、実行中のジョブに関連付けることができます。

#### 例

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## クラッシュを報告する

エージェントは、クラッシュに対処する 2 つのメソッドを提供します。

### 例外を送信する

#### リファレンス

            void SendCrash(Exception e, bool terminateSession = false)

#### 例

呼び出すことで、いつでも例外を送信できます。

            EngagementAgent.Instance.SendCrash(aCatchedException);

オプションのパラメーターを使用して、クラッシュを送信するよりも、同時にエンゲージメント セッションを終了することも可能です。 それには次のように呼び出します。

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

その場合、セッションとジョブは、クラッシュを送信した後にだけ閉じられます。

### 未処理の例外を送信する

#### リファレンス

            void SendCrash(Exception e)

エンゲージメントは、エンゲージメントの自動**クラッシュ**レポートが**無効**になっている場合に、未処理の例外を送信するメソッドも提供します。 これは、アプリケーションの UnhandledException イベント ハンドラーの内部で使用する場合に特に便利です。

このメソッドは、呼び出された後に、**常に**エンゲージメントのセッションとジョブを終了します。

#### 例

これを使用して、独自の UnhandledExceptionEventArgs ハンドラーを実装できます。 たとえば、追加、 `Current_UnhandledException` のメソッド、 `App.xaml.cs` ファイル。

            // In your App.xaml.cs file
    
            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

"Public App(){}" の App.xaml.cs で次のように追加します。

            Application.Current.UnhandledException += Current_UnhandledException;

## デバイス Id

            String EngagementAgent.Instance.GetDeviceId()

このメソッドを呼び出すことで、エンゲージメント デバイス id を取得できます。

## Extras パラメーター

任意のデータをイベント、エラー、アクティビティ、ジョブにアタッチできます。 これらのデータは、ディクショナリを使用して構造化できます。 任意の型のキーと値を指定できます。

Extras データはシリアル化されるため、Extras に独自の型を挿入する場合、この型のデータ コントラクトを追加する必要があります。

### 例

新しい "Person" クラスを作成します。

            using System.Runtime.Serialization;
    
            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
    
                // Properties
    
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
    
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

次に、追加、 `人` インスタンスを extra です。

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
    
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] その他の型のオブジェクトを配置する場合、ToString() メソッドは、人間が判読できる文字列を返すように実装されていることを確認します。

### 制限

#### 構成する

オブジェクト内の各キーは、次の正規表現と一致する必要があります。

`^[a-zA-Z][a-zA-Z_0-9]*$`

キーは、文字、数字、アンダー スコア (\_) が後に続く、少なくとも 1 つの文字で始まる必要があることを意味します。

#### サイズ

Extras はセルあたり **1024** 文字に制限されています。

## アプリケーションの情報を報告する

### リファレンス

            void SendAppInfo(Dictionary<object, object> appInfos)

SendAppInfo() 関数を使用して追跡情報 (またはその他のアプリケーション固有情報) を手動で報告できます。

これらの情報は段階的に送信される可能性があることにご注意ください。特定のキーの最新の値のみが特定のデバイスに保持されます。イベント extras のように、dictionary \ を使用して<object, object\> 情報をアタッチします。

### 例

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };
    
            EngagementAgent.Instance.SendAppInfo(appInfo);

### 制限

#### 構成する

オブジェクト内の各キーは、次の正規表現と一致する必要があります。

`^[a-zA-Z][a-zA-Z_0-9]*$`

キーは、文字、数字、アンダー スコア (\_) が後に続く、少なくとも 1 つの文字で始まる必要があることを意味します。

#### サイズ

アプリケーションの情報は、呼び出しあたり **1024** 文字に制限されています。

前の例では、サーバーに送信される JSON は 44 文字です。

            {"birthdate":"1983-12-07","gender":"female"}



