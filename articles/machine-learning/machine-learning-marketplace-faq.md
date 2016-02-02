<properties 
    pageTitle="Azure Marketplace に Machine Learning Web service の公開と使用に関する FAQ | Microsoft Azure" 
    description="よく寄せられる質問" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/08/2015" 
    ms.author="luisca"/> 


# Azure Marketplace での Machine Learning アプリケーションの公開と使用に関する FAQ

## Marketplace から使用する場合

**1. なぜ次のエラー メッセージ、web サービスの入力後: * *

* * 要求タイムアウトのバック エンドまたはバックエンド エラーが発生しました。 チームでは現在この問題を調査中です。 ご不便をおかけして申し訳ありません。 (500)**

入力パラメーターが特定の Web サービスの所定の形式に準拠していない可能性があります。 入力パラメーターの正しい形式およびこの Web サービスの制限事項については、対応するドキュメントのリンクを参照してください。


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. 参照は、「このデータセットの参照」ページが表示される web サービスとは、結果にアクセスする必要がありますが使用および方法でどの資格情報の別のブラウザー ウィンドウに貼り付けの API リンクをコピーする場合ですか? * *

ユーザー名として Marketplace アカウントを使用し、パスワードとしてプライマリ アカウント キーを使用する必要があります。 プライマリ アカウント キーは、**[このデータセットの参照]** ページの Web サービスの説明の下に表示されています (**[表示]** ボタンをクリック)。 結果がブラウザーに表示またはダウンロード可能な場合がありますが、使用しているブラウザーによってです。

**3. なぜ次のエラー メッセージ「このデータセットの参照」ページの web サービスの入力を入力した後に: * *

* * 要求の処理中に、、予期しないエラーが発生しました。 お試しください again.* *

Marketplace の **[このデータセットの参照]** ページで Web サービスを使用するときに、Web サービスの 1 つ以上の入力パラメーターが長さの制限を超過した可能性があります。 HTTP POST メソッドを使用して、より長い入力文字列でサービスを呼び出すことができます。 例については、次を参照してください。 [Machine Learning で R を使用してソリューションをサンプリングし、Marketplace に発行された](machine-learning-r-csharp-web-service-examples.md)します。

**4. なぜ表示されないの [API エクスプ ローラー] タブに何も Azure クラシック ポータルで、ストア? * *

これは、Azure クラシック ポータルの Marketplace の既知の問題です。 チームではこの問題を解決するために作業中です。


## Marketplace の Azure Machine Learning からの発行に関する質問

**1. なぜ、ロゴ、イメージのトランザクション数いない web サービスに対して更新? * *

ロゴとイメージは公開ポータルでキャッシュされ、ポータルで新しいロゴとイメージに更新されるまで最大 10 日間かかる場合があります。

**2. エラー メッセージを示す Marketplace で web サービスの [詳細] タブはなぜですか * *。

Marketplace では、サービスの詳細情報を取得するために Azure Machine Learning にアクセスする場合の既知の問題があります。 チームではこの問題を解決するために作業中です。

**3. なぜ Azure Machine Learning web サービスでの R サンプル コードはされていません Marketplace で web サービスを使用するためでしょうか * *。

Azure Machine Learning Web サービスに直接接続するときと、これらの Web サービスに Marketplace を通じて接続するときとでは、認証システムが異なります。 Marketplace のサービスは OData サービスであり、GET か POST メソッドで呼び出すことができます。

**4. サービスが提供しないいくつか正しく更新プランでの web のサポート リンクはなぜですか * *。

サポート リンクは、プランごとにではなく発行者ごとにグローバルです。

**5. Marketplace でバッチ入力モードを使用して web サービスを発行する方法ですか? * *

バッチ入力モードは、現在 Marketplace Web サービスでサポートされていません。

**6. データ発行者になるに関する質問がある場合、または発行時に問題がある場合は、ヘルプを表示するユーザーに連絡すればよいですか? * *

Azure Marketplace チームにお問い合わせください <datamarketbd@microsoft.com> の詳細。











