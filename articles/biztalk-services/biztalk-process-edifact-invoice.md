<properties
   pageTitle="チュートリアル: Azure BizTalk Services を使用して EDIFACT 請求書を処理する | Microsoft Azure BizTalk Services"
   description="Box コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="12/02/2015"
   ms.author="Deonhe"/>

# チュートリアル: Azure BizTalk Services を使用して EDIFACT 請求書を処理する
BizTalk Services ポータルを使用して、X12 契約と EDIFACT 契約を構成し、デプロイできます。 このチュートリアルでは、EDIFACT 契約を作成し、取引先間で請求書を交換する方法について説明します。 このチュートリアルは、EDIFACT メッセージを交換している Northwind と Contoso という 2 社の取引先を対象とするエンド ツー エンドのビジネス ソリューションに基づいて作成されています。  

## このチュートリアルをベースにしたサンプル
このチュートリアルは、サンプルでは、書き込まれる **を送信する EDIFACT 請求書サービスを使用して BizTalk**, からをダウンロードすることが、 [MSDN コード ギャラリー](http://go.microsoft.com/fwlink/?LinkId=401005)します。 このサンプルを参照しながらチュートリアルを進めると、サンプルがどのように作成されたかを理解できます。 また、このチュートリアルを使用して、ソリューションを自分でゼロから作成することもできます。 ソリューションがどのように作成されるかを理解できるように、このチュートリアルでは後者のアプローチを目標としています。 このチュートリアルは、できる限りサンプルと整合するよう配慮されており、アーティファクト (スキーマや変換など) にはサンプルで使用されるものと同じ名前を使用しています。  

>[AZURE.NOTE] このソリューションでは、EAI ブリッジから EDI ブリッジにメッセージを送信するでは、のでが再利用、 [チェーン サンプル BizTalk サービス ブリッジ](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) サンプルです。  

## このソリューションの内容

このソリューションでは、Northwind が Contoso から EDIFACT 請求書を受け取ります。 請求書が標準の EDI 形式ではないため、 Northwind に送信する前に、請求書を EDIFACT 請求書 (INVOIC とも呼ばれます) ドキュメントに変換する必要があります。 受信後、Northwind は EDIFACT 請求書を処理し、制御メッセージ (CONTRL とも呼ばれます) を Contoso に返す必要があります。

![][1]  

このビジネス シナリオを達成するために、Contoso は Microsoft Azure BizTalk Services が備える機能を使用します。

*   Contoso は EAI ブリッジを使用して、元の請求書を EDIFACT INVOIC に変換します。

*   BizTalk Services ポータルで構成された契約の一部としてデプロイされている EDI 送信ブリッジに、EAI ブリッジからメッセージが送信されます。

*   EDI 送信ブリッジは EDIFACT INVOIC を処理し、Northwind にルーティングします。

*   請求書を受信すると、Northwind は、契約の一部としてデプロイされている EDI 受信ブリッジに CONTRL メッセージを返します。  

> [AZURE.NOTE] 必要に応じて、このソリューションでは、バッチ処理を使用して、各請求書を個別に送信する代わりに、バッチで請求書を送信する方法も示します。  

シナリオの実行にあたって、Contoso から Northwind への請求書の送信と、Northwind からの受信確認の受信に、Service Bus のキューを使用します。 このキューはクライアント アプリケーションを使用して作成できます。クライアント アプリケーションはダウンロードで入手できるほか、このチュートリアルの過程で取得できるサンプル パッケージにも含まれています。  

## 前提条件

*   Service Bus 名前空間が必要です。 名前空間の作成手順については、次を参照してください。 [How To: 作成または Service Bus Service 名前空間を変更する](https://msdn.microsoft.com/library/hh690931.aspx)です。 お知らせがあると想定既にプロビジョニングされると、Service Bus 名前空間と呼ばれる **edifactbts**します。

*   BizTalk Services サブスクリプションが必要です。 手順については、次を参照してください。 [Azure クラシック ポータルを使用して BizTalk サービスを作成する](http://go.microsoft.com/fwlink/?LinkID=302280)です。 このチュートリアルと仮定と呼ばれる、BizTalk サービスのサブスクリプションがある **contosowabs**します。

*   BizTalk Services ポータルに BizTalk Services サブスクリプションを登録する必要があります。 手順については、次を参照してください [BizTalk Services ポータルで BizTalk サービスの展開を登録する。](https://msdn.microsoft.com/library/hh689837.aspx)

*   Visual Studio をインストールする必要があります。

*   BizTalk Services SDK をインストールする必要があります。 SDK をダウンロードする [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## 手順1. Service Bus キューを作成する  
このソリューションでは、Service Bus キューを使用して、取引先間でメッセージを交換します。 Contoso と Northwind はメッセージをキューに送り、EAI ブリッジと EDI ブリッジがこのキューからメッセージを取得して処理します。 このソリューションでは、次の 3 つの Service Bus キューが必要です。

*   **northwindreceive** – Northwind は、このキューで Contoso から請求書を受信します。

*   **contosoreceive** – Contoso は、このキューで Northwind から受信確認を受信します。

*   **中断** – すべての保留メッセージは、このキューに送られます。 処理中にエラーが発生すると、メッセージは保留になります。

サンプル パッケージに含まれているクライアント アプリケーションを使用して、これらの Service Bus キューを作成できます。  

1.  このサンプルをダウンロードした場所から開く **チュートリアルを送信する請求書を使用して BizTalk Services EDI Bridges.sln**します。

2.  キーを押して **f5 キーを押して** をビルドし、開始、 **チュートリアル クライアント** アプリケーションです。

3.  表示された画面で、Service Bus ACS 名前空間、発行者名、発行者キーを入力します。

    ![][2]  
4.  Service Bus 名前空間に 3 つのキューが作成されることがメッセージ ボックスに表示されます。 Click **OK**.

5.  チュートリアル クライアントを実行したままにしておきます。 開く] をクリック **Service Bus** > **_、Service Bus 名前空間_** > **キュー**, 、3 つのキューが作成されていることを確認します。  

## 手順 2. 取引先契約を作成してデプロイする
Contoso と Northwind の間の取引先契約を作成します。 取引先契約とは、2 つのビジネス パートナーの間の取引契約を定義するものです。使用するメッセージ スキーマやメッセージング プロトコルなどが含まれます。取引先アグリーメントには EDI ブリッジ 2 つは、取引先にメッセージを送信する 1 つにはが含まれています (と呼ばれる、 **EDI 送信ブリッジ**)、取引先からメッセージを受信するもう 1 つ (と呼ばれる、 **EDI 受信ブリッジ**)。

このソリューションのコンテキストでは、EDI 送信ブリッジは契約の送信側に相当し、Contoso から Northwind に EDIFACT 請求書を送信するために使用されます。 同様に、EDI 受信ブリッジは契約の受信側に相当し、Northwind からの受信確認を受信するために使用されます。  

### 取引先を作成する

最初に、Contoso と Northwind 用に取引先を作成します。  

1.  BizTalk Services ポータルでの **パートナー** ] タブ、[ **追加**します。

2.  新しいパートナー] ページで、次のように入力します。 **Contoso** クリックしてパートナー名として **保存**します。

3.  2 番目のパートナーを作成する手順を繰り返して **Northwind**します。  

### 契約を作成する
取引先のビジネス プロファイルの間に取引先契約を作成します。 このソリューションでは、パートナーの作成時に自動的に作成される既定のパートナー プロファイルを使用します。  

1.  BizTalk サービス ポータルで、クリックして **契約** > **追加**します。

2.  新しいアグリーメントの [ **全般設定** ] ページで、次の図のように値を指定してクリックして **続行**します。

    ![][3]  

    クリックした後 **続行**, 、2 つのタブ **受信の設定** と **送信の設定** 使用可能になります。

3.  Contoso と Northwind の間に送信契約を作成します。 この契約によって、Contoso が EDIFACT 請求書を Northwind に送信する方法が制御されます。

    1.  クリックして **送信設定**します。

    2.  上の既定値を保持、 **受信 URL**, 、**変換**, 、および **バッチ処理** タブです。

    3.   **プロトコル** ] タブの [、 **スキーマ** セクションで、アップロード、 **EFACT_D93A_INVOIC.xsd** スキーマです。 このスキーマは、サンプル パッケージと共に提供されています。

        ![][4]  
    4.   **トランスポート** ] タブで、Service Bus キューの詳細を指定します。 使用して送信側アグリーメントについて、 **northwindreceive** を Northwind に EDIFACT 請求書を送信するキューを **中断** 処理中に失敗し、中断したメッセージのルーティング先キューです。 これらのキューを作成した [手順 1: Service Bus キューを作成する](#BKMK_Queue)です。

        ![][5]  

         **トランスポート設定 > トランスポートの種類** と **メッセージ保留の設定 > トランスポートの種類**, Azure Service Bus を選択して、図のように、値を指定します。

4.  Contoso と Northwind の間に受信契約を作成します。 この契約によって、Contoso が Northwind から受信確認を受信する方法が制御されます。

    1.  クリックして **設定を受け取る**します。

    2.  上の既定値を保持、 **トランスポート** と **変換** タブです。

    3.   **プロトコル** ] タブの [、 **スキーマ** セクションで、アップロード、 **EFACT_4.1_CONTRL.xsd** スキーマです。 このスキーマは、サンプル パッケージと共に提供されています。

    4.   **ルート** ] タブで、Northwind からのみ肯定応答が Contoso にルーティングされるようにするフィルターを作成します。  **ルートの設定**, 、クリックして **追加** ルーティング フィルターを作成します。

        ![][6]  
        1.  値を指定 **ルール名**, 、**ルート ルール**, 、および **ルーティング先** 、図のようにします。

        2.  クリックして **保存**します。

    5.   **ルート** 再度] タブで、保留肯定応答 (処理中に失敗する確認) の送り先を指定します。 Azure Service Bus にトランスポートの種類を設定、ルーティング先を **キュー**, 、認証の種類を **Shared Access Signature** (SAS) は、Service Bus 名前空間の SAS 接続文字列を指定しとキューの名前を入力 **中断**します。

5.  最後に、クリックして **展開** 、アグリーメントをデプロイします。 送信契約と受信契約がデプロイされるエンドポイントを書き留めます。

    *    **送信の設定** ] タブの [ **受信 URL**, 、端点に注意してください。 EDI 送信ブリッジを使用して Contoso から Northwind にメッセージを送信するには、このエンドポイントにメッセージを送信する必要があります。

    *    **受信の設定** ] タブの [ **トランスポート**, 、端点に注意してください。 EDI 受信ブリッジを使用して Northwind から Contoso にメッセージを送信するには、このエンドポイントにメッセージを送信する必要があります。  

## 手順 3. BizTalk Services プロジェクトを作成してデプロイする

前の手順で、EDIFACT 請求書と受信確認を処理するために EDI 送信契約と EDI 受信契約をデプロイしました。 これらの契約では、標準の EDIFACT メッセージ スキーマに準拠するメッセージのみを処理できます。 ただし、このソリューションのシナリオでは、Contoso は社内独自のスキーマで Northwind に請求書を送信します。 そのため、メッセージは、EDI 送信ブリッジに送信される前に、社内スキーマから標準の EDIFACT 請求書スキーマに変換される必要があります。 これを実行するのが BizTalk Services EAI プロジェクトです。

BizTalk サービス プロジェクト **InvoiceProcessingBridge**, 、変換、メッセージがあるもダウンロードしたサンプルの一部として含まれています。 プロジェクトには、次のアーティファクトが含まれています。

*   **INHOUSEINVOICE します。XSD** – Northwind に送信される社内請求書のスキーマです。

*   **EFACT_D93A_INVOIC します。XSD** – 標準 EDIFACT 請求書のスキーマです。

*   **EFACT_4.1_CONTRL します。XSD** – Northwind が Contoso に送信する EDIFACT 肯定応答のスキーマです。

*   **INHOUSEINVOICE_to_D93AINVOIC します。TRFM** – 社内請求書スキーマを標準 EDIFACT 請求書スキーマにマッピングする変換です。  

### BizTalk Services プロジェクトを作成する
1.  Visual Studio ソリューションで、InvoiceProcessingBridge プロジェクトを開き、 **MessageFlowItinerary.bcs** ファイルです。

2.  キャンバス上の任意の場所をクリックし、設定、 **BizTalk サービスの URL** プロパティ ボックスに、BizTalk サービス サブスクリプション名を指定します。 たとえば、「`https://contosowabs.biztalk.windows.net`」のように入力します。

    ![][7]  
3.  ツールボックスからドラッグして、 **Xml 一方向ブリッジ** をキャンバスにします。 設定、 **エンティティ名** と **相対アドレス** プロパティへのブリッジの **ProcessInvoiceBridge**します。 ダブルクリックして **ProcessInvoiceBridge** ブリッジ構成画面を開きます。

4.  内で、 **メッセージの種類** ボックスで、プラス記号をクリックして (**+**)、受信メッセージのスキーマを指定する] ボタンをクリックします。 EAI ブリッジの受信メッセージは、社内請求書では常にであるために設定 **INHOUSEINVOICE**します。

    ![][8]  
5.  をクリックして、 **Xml 変換** 図形、およびプロパティのボックスでの **マップ** プロパティには、省略記号ボタン (**...**)] ボタンをクリックします。  **マップの選択** ダイアログ ボックスで、 **INHOUSEINVOICE_to_D93AINVOIC** ファイルを変換し、クリックして **OK**します。

    ![][9]  
6.  戻り **MessageFlowItinerary.bcs**, 、ツールボックスからドラッグし、 **Two-Way External Service Endpoint** の右側に、 **ProcessInvoiceBridge**します。 設定の **エンティティ名** プロパティを **EDIBridge**します。

7.  ソリューション エクスプ ローラーで、 **MessageFlowItinerary.bcs** をダブルクリックし、 **EDIBridge.config** ファイルです。 コンテンツを置き換える、 **EDIBridge.config** 次です。

    > [AZURE.NOTE] .Config ファイルを編集するはなぜですか。 ブリッジ デザイナー キャンバスに追加した外部サービス エンドポイントは、前にデプロイした EDI ブリッジを表しています。 EDI ブリッジは送信側と受信側がある双方向ブリッジです。 一方、ブリッジ デザイナーに追加した EAI ブリッジは一方向ブリッジです。 そこで、2 つのブリッジそれぞれのメッセージ交換パターンを処理するために、.config ファイルに設定を追加して、カスタムのブリッジ動作を使用しています。 さらに、カスタムの動作では、EDI 送信ブリッジ エンドポイントへの認証も処理します。このカスタム動作をで別個のサンプルとして使用できる [BizTalk のサービス ブリッジ チェーン サンプル - EAI EDI から](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104)します。 このソリューションでは、このサンプルを再利用しています。  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  EDIBridge.config ファイルを更新して、構成の詳細を追加します。

    *    _<behaviors>_, 、ACS 名前空間と BizTalk サービス サブスクリプションに関連付けられたキーを指定します。

    *    _<client>_, 、EDI 送信アグリーメントが展開されているエンドポイントを提供します。

    変更を保存し、構成ファイルを閉じます。

9.  [ツールボックス] からをクリックして、 **コネクタ** に参加し、 **ProcessInvoiceBridge** と **EDIBridge** コンポーネントです。 コネクタを選択し、プロパティ ボックスで、次のように設定します。 **フィルター条件** に **すべてに一致**します。 これにより、EAI ブリッジで処理されたすべてのメッセージが EDI ブリッジに送信されます。

    ![][10]  
10.  ソリューションの変更を保存します。  

### プロジェクトをデプロイする

1.  BizTalk Services プロジェクトを作成したコンピューターに、BizTalk Services サブスクリプションの SSL 証明書をダウンロードし、インストールします。 [BizTalk サービス] をクリックして **ダッシュ ボード**, 、クリックして **SSL 証明書のダウンロード**します。 証明書をダブルクリックし、画面の指示に従ってインストールを完了します。 [証明書をインストールするかどうかを確認 **信頼されたルート証明機関** 証明書ストア。

2.  Visual Studio ソリューション エクスプ ローラーで右クリックし、 **InvoiceProcessingBridge** [プロジェクト] をクリックして **展開**します。

3.  クリックして、図のように、値を指定 **展開**します。 クリックして、BizTalk サービスの ACS 資格情報を取得できる **接続情報** 上の BizTalk サービス ダッシュ ボードから、します。

    ![][11]  

    出力ペインで、EAI ブリッジがデプロイされているエンドポイントをコピーします (例: `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`)。 このエンドポイント URL は後で必要になります。  

## 手順 4. ソリューションをテストする


このトピックで使用して、ソリューションをテストする方法に注目、 **チュートリアル クライアント** アプリケーション、サンプルの一部として提供されています。  

1.  Visual Studio で f5 キーを押してスタート、 **チュートリアル クライアント**です。

2.  この画面には、Service Bus キューを作成する手順で用いた値が事前入力されています。 クリックして **次**します。

3.  次のウィンドウで、BizTalk Services サブスクリプションの ACS 資格情報と、EAI ブリッジおよび EDI (受信) ブリッジがデプロイされているエンドポイントを入力します。

    EAI ブリッジのエンドポイントは前の手順でコピーしています。 EDI 受信ブリッジのエンドポイントは、BizTalk Services ポータルで、[契約]、[受信の設定]、[トランスポート]、[エンドポイント] の順に進んで確認できます。

    ![][12]  
4.  [次へ] ウィンドウの Contoso の下で、 **社内請求書の送信** ] ボタンをクリックします。 [ファイルを開く] ダイアログ ボックスで、INHOUSEINVOICE.txt ファイルを開きます。 ファイルの内容を検証し、クリックして **OK** 請求書を送信します。

    ![][13]  
5.  数秒後に、Northwind に請求書が届きます。 クリックして、 **メッセージを表示する** Northwind が受信した請求書を表示するリンク。 Northwind が受信した請求書が標準 EDIFACT スキーマであり、Contoso が送信した請求書が社内スキーマである点に注目してください。

    ![][14]  
6.  請求書を選択し、クリックして **肯定応答の送信**します。 ダイアログ ボックスが表示されたら、受信された請求書と送信される受信確認でインターチェンジ ID が同じであることに注目してください。 [Ok] をクリックして、 **肯定応答の送信** ] ダイアログ ボックス。

    ![][15]  
7.  数秒後に、Contoso に受信確認が届きます。

    ![][16]  

## 手順 5 (任意). EDIFACT 請求書を一括送信する 
BizTalk Services EDI ブリッジでは、メッセージを一括送信することもできます。 この機能は、受信側のパートナーがメッセージを個別に受信するのではなく、(特定の基準を満たす) メッセージを一括で受信したい場合に役に立ちます。

このバッチ処理で最も重要な要素は、バッチの実際のリリースです。これはリリース条件とも呼ばれます。 リリース条件は、受信パートナーが希望するメッセージの受信方法に基づいて作成できます。 バッチ処理が有効になっている場合、EDI ブリッジはリリース条件が満たされるまで送信メッセージを受信パートナーに送信しません。 たとえば、メッセージ サイズを基準にするバッチ条件の場合、"n" 通のメッセージがたまってからバッチが送信されます。 バッチ条件は時間を基準にして作成することもできます。毎日指定した時間にバッチを送信できます。 このソリューションでは、メッセージ サイズを基準にした条件を試します。

1.  BizTalk Services ポータルで、前に作成した契約をクリックします。 [送信の設定]、[バッチ処理]、[バッチの追加] の順にクリックします。

2.  バッチ名を入力してください。 **InvoiceBatch**, 、説明を入力し、[クリック **次**します。

3.  一括処理するメッセージを定義するバッチ条件を指定します。 このソリューションでは、すべてのメッセージを一括処理します。 そのため、定義のオプションを高度な使用を選択し、入力 **1 = 1**します。 これは常に真になる条件であるため、すべてのメッセージが一括処理されます。 クリックして **次**します。

    ![][17]  
4.  バッチ リリース条件を指定します。 ドロップ ボックスから [ **MessageCountBased**, 、および **カウント**, 、指定 **3**します。 これで、3 つのメッセージをまとめたバッチが Northwind に送信されます。 クリックして **次**します。

    ![][18]  
5.  概要を確認し、クリックして **保存**します。 クリックして **展開** 、アグリーメントを再デプロイします。

6.  戻り、 **チュートリアル クライアント**, 、] をクリックして **社内請求書の送信**, 、請求書を送信する指示に従います。 バッチ サイズに到達していないため、請求書は Northwind に届きません。 3 つの請求書メッセージが Northwind に届くように、この手順をさらに 2 回繰り返します。 これで 3 通のメッセージというバッチ リリース条件が満たされたため、Northwind に請求書が届きます。


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG


