<properties
    pageTitle="iOS でエンゲージメント API を使用する方法"
    description="最新の iOS SDK - iOS でエンゲージメント API を使用する方法"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2015"
    ms.author="piyushjo" />



# iOS でエンゲージメント API を使用する方法

このドキュメントは、「iOS でエンゲージメントを統合する方法」を補足し、エンゲージメント API を使用してアプリケーションの統計情報を報告する方法について詳しく説明します。

場合は、アプリケーションのセッション、アクティビティ、クラッシュ、および技術情報について報告するエンゲージメントのみが必要、し、最も簡単な方法があるすべてのカスタムを作成することに留意してください `UIViewController` から、対応するオブジェクトを継承 `EngagementViewController` クラスです。

アプリケーション固有のイベント、エラー、ジョブを報告する必要がある場合の例については、複数の操作を実行するかに実装されているものよりも、別の方法でアプリケーションのアクティビティを報告する必要がある場合、 `EngagementViewController` クラス、エンゲージメント API を使用する必要があります。

エンゲージメント API はによって提供される、 `EngagementAgent` クラスです。 呼び出してこのクラスのインスタンスを取得することができます、 `[EngagementAgent shared]` 静的メソッド (なお、 `EngagementAgent` 返されるオブジェクトはシングルトン)。

すべての API 呼び出しの前に、 `EngagementAgent` メソッドを呼び出してオブジェクトを初期化する必要があります `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION します。ドメイン};SdkKey {YOUR_SDK_KEY} を =;AppId = {YOUR_APPID}"];`

## エンゲージメントの概念

次の部分は、一般的なを絞り込む [モバイル エンゲージメントの概念](mobile-engagement-concepts.md) 、iOS プラットフォーム向けです。

### `セッション` と `アクティビティ`

*アクティビティ* つまりは通常、アプリケーションの 1 つの画面に関連付けられて、 *アクティビティ* 画面が表示され、画面を閉じるときに停止したときに開始: を使用して、エンゲージメント SDK を統合した場合、そうでは、 `EngagementViewController` クラスです。

ただし、*アクティビティ*はエンゲージメント API を使用して手動で制御することも可能です。 これにより、特定の画面をいくつかのサブ パートに分割して、この画面の使用状況の詳細情報を取得できます (たとえば、ダイアログがこの画面内で使用される頻度や時間を知ることができます)。

## アクティビティを報告する

### ユーザーが新しいアクティビティを開始する

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

呼び出す必要がある `startActivity()` ユーザー アクティビティが変更されるたびにします。 この関数の最初の呼び出しで、新しいユーザー セッションが開始します。

### ユーザーが現在のアクティビティを終了する

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] 必要があります **しない** 、複数のセッションにアプリケーションの用途の 1 つに分割する場合は、以外を自分では、この関数を呼び出します。 この関数を呼び出すと終了、現在のセッションで、すぐにため、後続の呼び出しに `startActivity()` 新しいセッションが開始します。 この関数は、アプリケーションを閉じると、SDK によって自動的に呼び出されます。

## イベントを報告する

### セッション イベント

通常、セッション イベントは、セッション中にユーザーによって実行されるアクションの報告に使用されます。

**余分なデータがない例:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**余分なデータがある例:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### スタンドアロン イベント

セッション イベントとは異なり、スタンドアロン イベントはセッションのコンテキスト外で使用されます。

**例:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## エラーの報告

### セッション エラー

通常、セッション エラーは、セッション中にユーザーに影響するエラーの報告に使用されます。

**例:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### スタンドアロン エラー

セッション エラーとは異なり、スタンドアロン エラーはセッションのコンテキスト外で使用できます。

**例:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## ジョブを報告する

**例:**

ログイン プロセスの実行時間を報告する場合を想定します。

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];
    
      [... sign in ...]
    
      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### ジョブ中のエラーを報告する

エラーは、現在のユーザー セッションに関連付ける代わりに、実行中のジョブに関連付けることができます。

**例:**

ログイン プロセス中にエラーを報告する場合を想定します。

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];
    
      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;
    
        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];
    
          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }
    
      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### ジョブ中のイベント

イベントは、現在のユーザー セッションではなく、実行中のジョブに関連付けることができます。

**例:**

ソーシャル ネットワークを持っていて、ジョブを使用して、ユーザーがサーバーに接続している合計時間を報告する場合を想定します。 ユーザーは友達からメッセージを受信できます。これがジョブ イベントです。

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## 追加のパラメーター

任意のデータをイベント、エラー、アクティビティ、ジョブに添付できます。

このデータは構造化することができ、iOS の NSDictionary クラスを使用します。

Extras に含められることに注意してください `配列 (NSArray、NSMutableArray)`, 、`番号 (NSNumber クラス)`, 、`文字列 (NSString、NSMutableString)`, 、`urls(NSURL)`, 、`データ (NSData、NSMutableData)` またはその他の `NSDictionary` インスタンス。
> [AZURE.NOTE] 追加のパラメーターは、JSON でシリアル化されます。 上記以外の別のオブジェクトを渡す場合は、クラス内に次のメソッドを実装する必要があります。
>
             -(NSString*) JSONRepresentation です。
>
> このメソッドは、JSON 表記のオブジェクトを返す必要があります。

### 例

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### 制限

#### キー

内の各キー、 `NSDictionary` 次の正規表現に一致する必要があります。

`^[a-zA-Z][a-zA-Z_0-9]*`

キーは、文字、数字、アンダー スコア (\_) が後に続く、少なくとも 1 つの文字で始まる必要があることを意味します。

#### サイズ

追加は、1 回の呼び出しにつき **1024** 文字に制限されます (エンゲージメント エージェントによって JSON でエンコードされた場合)。

前の例では、サーバーに送信される JSON は 58 文字です。

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## アプリケーションの情報を報告する

追跡情報 (またはその他のアプリケーション固有情報) を使用して手動で報告できます、 `sendAppInfo:` 関数です。

これらの情報は段階的に送信される可能性があることにご注意ください。特定のキーの最新の値のみが特定のデバイスに保持されます。

イベントの追加と同様に、 `NSDictionary` クラスは、配列またはサブディクショナリが、(JSON のシリアル化を使用して) 単純な文字列として処理することはアプリケーションの情報の要約に使用します。

**例:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### 制限

#### キー

内の各キー、 `NSDictionary` 次の正規表現に一致する必要があります。

`^[a-zA-Z][a-zA-Z_0-9]*`

キーは、文字、数字、アンダー スコア (\_) が後に続く、少なくとも 1 つの文字で始まる必要があることを意味します。

#### サイズ

アプリケーション情報は、1 回の呼び出しにつき **1024** 文字に制限されます (エンゲージメント エージェントによって JSON でエンコードされた場合)。

前の例では、サーバーに送信される JSON は 44 文字です。

    {"birthdate":"1983-12-07","gender":"female"}



