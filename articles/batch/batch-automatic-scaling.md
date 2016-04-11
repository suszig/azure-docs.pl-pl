
<properties
    pageTitle="Azure Batch プール内のコンピューティング ノードの自動スケール | Microsoft Azure"
    description="クラウド プールで自動スケールを有効にして、プール内のコンピューティング ノードの数を動的に調整します。"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="12/04/2015"
    ms.author="marsma"/>

# Azure Batch プール内のコンピューティング ノードの自動スケール

Azure Batch プール内のコンピューティング ノードの自動スケールは、アプリケーションによって使用される処理能力を動的に調整します。 この調整のしやすさによって、時間と費用を節約できます。 詳細については計算ノードとプールについては、次を参照してください。 [Azure Batch の基本](batch-technical-overview.md)します。

自動スケールがプールで有効になっていて、プールに数式が関連付けられている場合に自動スケールが行われます。 数式は、アプリケーションの処理に必要なコンピューティング ノードの数を決定するために使用されます。 定期的に収集されるサンプル上で動作する場合、プール内の利用可能なコンピューティング ノードの数は、関連付けられている数式に基づいて 15 分ごとに調整されます。

自動スケールは、プールの作成時に設定したり、後で既存のプールで有効化したりすることができます。 自動スケールが以前に有効になっていたプールで、数式を更新することもできます。 プールに割り当てる前には必ず数式を評価し、自動スケールが実行している状態を監視することが重要です。それぞれの方法について以下で説明します。

> [AZURE.NOTE] 各 Azure Batch アカウントは、処理するために使用できるコンピューティング ノードの最大数に制限されます。 システムはその制限までのみノードを作成するため、数式で指定された目標数に到達しない場合があります。

## コンピューティング リソースを自動的にスケール

定義したスケーリングの数式は、次の処理間隔にプール内で使用可能なコンピューティング ノードの数を決定します。 自動スケーリングの数式は単に文字列値をプールに割り当てる [autoScaleFormula](https://msdn.microsoft.com/library/azure/dn820173.aspx) (REST API) の要求の本文内の要素または [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) プロパティ (.NET API)。 この数式はサイズが 8 KB 以下の文字列で、最大 100 のステートメントをセミコロンで区切って含めることができます。また、改行やコメントを含めることもできます。

数式内のステートメントの式は自由形式です。 システム定義の変数、ユーザー定義の変数、定数値、これらの変数や定数でサポートされる操作を含めることができます。

    VAR = Expression(system-defined variables, user-defined variables);

複雑な数式は、複数のステートメントや変数を使用して作成されます。

    VAR₀ = Expression₀(system-defined variables);
    VAR₁ = Expression₁(system-defined variables, VAR₀);

> [AZURE.NOTE] 自動スケーリングの数式が成り立って [バッチ REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API 変数、型、操作、および関数。 これらは、操作中にも式の文字列で使用されて、 [バッチ .NET](https://msdn.microsoft.com/library/azure/mt348682.aspx) ライブラリです。

### 変数

システム定義の変数とユーザー定義の変数の両方を数式で使用できます。

*取得* と *設定* これらの値 **システム定義の変数** プール内の計算ノード数を管理します。

<table>
  <tr>
    <th>変数</th>
    <th>説明</th>
  </tr>
  <tr>
    <td>$TargetDedicated</td>
    <td>プールの専用コンピューティング ノードの目標数。 タスクの実際の使用状況に基づいて値を変更できます。</td>
  </tr>
  <tr>
    <td>$NodeDeallocationOption</td>
    <td>コンピューティング ノードがプールから削除されるときに発生するアクション。 次のいずれかの値になります。
      <br/>
      <ul>
        <li><p><b>再キュー</b> – タスクをすぐに終了し、再スケジュールされるようにジョブ キューに再配置します。</p></li>
        <li><p><b>終了</b> – タスクをすぐに終了し、ジョブ キューから削除します。</p></li>
        <li><p><b>taskcompletion</b> – 現在実行中のタスクを完了し、プールからノードを削除するまで待機します。</p></li>
        <li><p><b>retaineddata</b> - をプールからノードを削除する前にクリーンアップするノード上ですべてのローカル タスクで保持されたデータを待機します。</p></li>
      </ul></td>
   </tr>
</table>

*取得* これらの値 **システム定義の変数** サンプル内の計算ノードのメトリックに基づいて調整を行います。 次の変数は、読み取り専用です。

<table>
  <tr>
    <th>変数</th>
    <th>説明</th>
  </tr>
  <tr>
    <td>$CPUPercent</td>
    <td>平均 CPU 使用率</td>
  </tr>
  <tr>
    <td>$WallClockSeconds</td>
    <td>使用された秒数</td>
  </tr>
  <tr>
    <td>$MemoryBytes</td>
    <td>使用された平均メガバイト数</td>
  <tr>
    <td>$DiskBytes</td>
    <td>ローカル ディスクで使用された平均ギガバイト数</td>
  </tr>
  <tr>
    <td>$DiskReadBytes</td>
    <td>読み取られたバイト数</td>
  </tr>
  <tr>
    <td>$DiskWriteBytes</td>
    <td>書き込まれたバイト数</td>
  </tr>
  <tr>
    <td>$DiskReadOps</td>
    <td>実行された読み取りディスク操作の数</td>
  </tr>
  <tr>
    <td>$DiskWriteOps</td>
    <td>実行された書き込みディスク操作の数</td>
  </tr>
  <tr>
    <td>$NetworkInBytes</td>
    <td>受信バイト数</td>
  </tr>
  <tr>
    <td>$NetworkOutBytes</td>
    <td>送信バイト数</td>
  </tr>
  <tr>
    <td>$SampleNodeCount</td>
    <td>コンピューティング ノードの数</td>
  </tr>
  <tr>
    <td>$ActiveTasks</td>
    <td>アクティブ状態のタスクの数</td>
  </tr>
  <tr>
    <td>$RunningTasks</td>
    <td>実行状態のタスクの数</td>
  </tr>
  <tr>
    <td>$SucceededTasks</td>
    <td>正常に完了したタスクの数</td>
  </tr>
  <tr>
    <td>$FailedTasks</td>
    <td>失敗したタスクの数</td>
  </tr>
  <tr>
    <td>$CurrentDedicated</td>
    <td>専用コンピューティング ノードの現在の数</td>
  </tr>
</table>

### 型

これら **型** 数式ではサポートされています。

- double
- doubleVec
- string
- timestamp -- timestamp は次のメンバーを含む複合構造になっています。
    - year
    - month (1 ～ 12)
    - day (1 ～ 31)
    - weekday (数値の形式で表記します。月曜は 1 など)
    - hour (24 時間の数字で表記します。午後 1 時は 13 など)
    - minute (00 ～ 59)
    - second (00 ～ 59)
- timeinterval
    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

### 操作

これら **操作** は上記の型では許可されます。

<table>
  <tr>
    <th>操作</th>
    <th>使用可能な演算子</th>
  </tr>
  <tr>
    <td>二重 & lt; 演算子 & gt;倍精度浮動小数点 = & gt;二重</td>
    <td>+, -, *, /</td>
  </tr>
  <tr>
    <td>二重 & lt; 演算子 & gt;timeinterval = & gt;timeinterval</td>
    <td>*</td>
  </tr>
  <tr>
    <td>doubleVec & lt; 演算子 & gt;倍精度浮動小数点 = & gt;doubleVec</td>
    <td>+, -, *, /</td>
  </tr>
  <tr>
    <td>doubleVec & lt; 演算子 & gt;doubleVec = & gt;doubleVec</td>
    <td>+, -, *, /</td>
  </tr>
  <tr>
    <td>timeinterval & lt; 演算子 & gt;倍精度浮動小数点 = & gt;timeinterval</td>
    <td>*, /</td>
  </tr>
  <tr>
    <td>timeinterval & lt; 演算子 & gt;timeinterval = & gt;timeinterval</td>
    <td>+, -</td>
  </tr>
  <tr>
    <td>timeinterval & lt; 演算子 & gt;タイムスタンプ = & gt;タイムスタンプ</td>
    <td>+</td>
  </tr>
  <tr>
    <td>タイムスタンプ & lt; 演算子 & gt;timeinterval = & gt;タイムスタンプ</td>
    <td>+</td>
  </tr>
  <tr>
    <td>タイムスタンプ & lt; 演算子 & gt;タイムスタンプ = & gt;timeinterval</td>
    <td>-</td>
  </tr>
  <tr>
    <td>& lt; 演算子 & gt; 倍精度浮動小数点 = & gt;二重</td>
    <td>-, !</td>
  </tr>
  <tr>
    <td>& lt; 演算子 & gt; timeinterval = & gt;timeinterval</td>
    <td>-</td>
  </tr>
  <tr>
    <td>二重 & lt; 演算子 & gt;倍精度浮動小数点 = & gt;二重</td>
    <td>(& a) lt;、& lt; は、=、= =、& gt; =、& gt;、! =</td>
  </tr>
  <tr>
    <td>文字列 & lt; 演算子 & gt;文字列 = & gt;二重</td>
    <td>(& a) lt;、& lt; は、=、= =、& gt; =、& gt;、! =</td>
  </tr>
  <tr>
    <td>タイムスタンプ & lt; 演算子 & gt;タイムスタンプ = & gt;二重</td>
    <td>(& a) lt;、& lt; は、=、= =、& gt; =、& gt さん、! =</td>
  </tr>
  <tr>
    <td>timeinterval & lt; 演算子 & gt;timeinterval = & gt;二重</td>
    <td>(& a) lt;、& lt; は、=、= =、& gt; =、& gt;、! =</td>
  </tr>
  <tr>
    <td>二重 & lt; 演算子 & gt;倍精度浮動小数点 = & gt;二重</td>
    <td>&&, ||</td>
  </tr>
  <tr>
    <td>double 型のみテストする (ゼロ以外は true、ゼロは false)</td>
    <td>? :</td>
  </tr>
</table>

### 関数

この定義済み **関数** 自動スケーリングの数式を定義するために使用します。

<table>
  <tr>
    <th>関数</th>
    <th>説明</th>
  </tr>
  <tr>
    <td>二重 <b>avg</b>(doubleVecList)</td>
    <td>DoubleVecList のすべての値の平均値。</td>
  </tr>
  <tr>
    <td>二重 <b>len</b>(doubleVecList)</td>
    <td>DoubleVecList から作成されたベクター長。</td>
  <tr>
    <td>二重 <b>lg</b>(double)</td>
    <td>対数の底 2。</td>
  </tr>
  <tr>
    <td>doubleVec <b>lg</b>(doubleVecList)</td>
    <td>Componentwise の対数の底 2。 vec(double) は 1 つの double パラメーターに明示的に渡される必要があります。それ以外の場合は、double lg(double) バージョンと見なされます。</td>
  </tr>
  <tr>
    <td>二重 <b>ln</b>(double)</td>
    <td>自然対数。</td>
  </tr>
  <tr>
    <td>doubleVec <b>ln</b>(doubleVecList)</td>
    <td>Componentwise の対数の底 2。  vec(double) は 1 つの double パラメーターに明示的に渡される必要があります。それ以外の場合は、double lg(double) バージョンと見なされます。</td>
  </tr>
  <tr>
    <td>二重 <b>ログ</b>(double)</td>
    <td>対数の底 10。</td>
  </tr>
  <tr>
    <td>doubleVec <b>ログ</b>(doubleVecList)</td>
    <td>Componentwise の対数の底 10。 vec(double) は 1 つの double パラメーターに明示的に渡される必要があります。それ以外の場合は、double log(double) バージョンと見なされます。</td>
  </tr>
  <tr>
    <td>二重 <b>max</b>(doubleVecList)</td>
    <td>DoubleVecList の最大値。</td>
  </tr>
  <tr>
    <td>二重 <b>min</b>(doubleVecList)</td>
    <td>DoubleVecList の最小値。</td>
  </tr>
  <tr>
    <td>二重 <b>norm</b>(doubleVecList)</td>
    <td>DoubleVecList から作成された 2 つの標準ベクター。
  </tr>
  <tr>
    <td>二重 <b>百分位</b>(doubleVec v、二重 p)</td>
    <td>ベクター v の百分位要素。</td>
  </tr>
  <tr>
    <td>二重 <b>rand</b>)</td>
    <td>0.0 ～ 1.0 のランダムな値。</td>
  </tr>
  <tr>
    <td>二重 <b>範囲</b>(doubleVecList)</td>
    <td>DoubleVecList の最小値と最大値の違い。</td>
  </tr>
  <tr>
    <td>二重 <b>std</b>(doubleVecList)</td>
    <td>DoubleVecList の値のサンプルの標準偏差。</td>
  </tr>
  <tr>
    <td><b>停止</b>)</td>
    <td>自動スケールの式評価を停止します。</td>
  </tr>
  <tr>
    <td>二重 <b>合計</b>(doubleVecList)</td>
    <td>doubleVecList のすべてのコンポーネントの合計。</td>
  </tr>
  <tr>
    <td>タイムスタンプ <b>時間</b>(文字列の dateTime ="")</td>
    <td>パラメーターが渡されない場合は現在の時刻のタイムスタンプ、渡された場合は dateTime 文字列のタイムスタンプ。 サポートされている dateTime 形式は、W3CDTF と RFC1123 です。</td>
  </tr>
  <tr>
    <td>二重 <b>val</b>(doubleVec v、二重 i)</td>
    <td>開始インデックス 0 のベクター v の場所 i にある要素の値。</td>
  </tr>
</table>

上記の表に示した一部の関数は、リストを引数として受け入れることができます。 コンマ区切りのリストが任意に組み合わせた *二重* と *doubleVec*します。 次に例を示します。

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

 *DoubleVecList* 値は、1 つに変換 *doubleVec* 評価の前にします。 たとえば、`v = [1,2,3]` の場合、`avg(v)` の呼び出しは `avg(1,2,3)` の呼び出しに相当し、`avg(v, 7)` の呼び出しは `avg(1,2,3,7)` の呼び出しに相当します。

### サンプル データの取得

上記のシステム定義の変数は、関連付けられたデータにアクセスするメソッドを提供するオブジェクトです。 たとえば、次の式は、最後の 5 分間の CPU 使用率を取得する要求を示しています。

`$CPUPercent.GetSample(TimeInterval_Minute * 5)`

次のメソッドを使用して、サンプル データを取得できます。

<table>
  <tr>
    <th>メソッド</th>
    <th>説明</th>
  </tr>
  <tr>
    <td>Count()</td>
    <td>メトリック履歴のサンプルの合計数を返します。</td>
  </tr>
  <tr>
    <td>GetSample()</td>
    <td><p>データ サンプルのベクターを返します。
    <p>サンプルは、30 秒相当のメトリック データです。 つまり、サンプルは 30 秒ごとに取得されますが、後述のように、サンプルが収集される時間と数式に使用できるようになる時間には遅延があります。 そのため、特定の期間に取得されたすべてのサンプルを数式の評価に使用できない可能性があります。
        <ul>
          <li><p><b>doubleVec GetSample (double count)</b> - 収集された、最新のサンプルからを取得するためのサンプルの数を指定します。</p>
                  <p>GetSample(1) は、使用できる最新のサンプルを返します。 $CPUPercent などのメトリック、ただし、この指定しないで認識することであるため <em>と</em> サンプルの収集 - 最新のものである可能性がありますか、システムの問題があるためより古い場合があります。 このような場合は、次のように期間を使用することをお勧めします。</p></li>
          <li><p><b>doubleVec GetSample ((timestamp | timeinterval) startTime [、double samplePercent])</b> – サンプル データを収集するための期間を指定し、オプションで要求されたタイム フレームで使用する必要があるサンプルの割合を指定します。</p>
          <p><em>$CPUPercent.GetSample(TimeInterval_Minute * 10)</em> は過去 10 分間にすべてのサンプルが CPUPercent 履歴に存在する場合の 20 のサンプルを返します。 ただし、過去 1 分間の履歴を使用できない場合は、18 個のサンプルのみが返されます。この場合、<br/>
          &nbsp;&nbsp;&nbsp;&nbsp;<em>$Cpupercent.getsample(timeinterval (TimeInterval_Minute * 10, 95)</em> サンプルの 90% のみが、使用可能なために失敗し、<br/>
          &nbsp;&nbsp;&nbsp;&nbsp;<em>$Cpupercent.getsample(timeinterval (TimeInterval_Minute * 10, 80)</em> は成功します。</p></li>
          <li><p><b>doubleVec GetSample ((timestamp | timeinterval) startTime、(timestamp | timeinterval) endTime [、double samplePercent])</b> – 開始時刻と終了時刻の両方でデータを収集するための期間を指定します。</p></li></ul>
          <p>前述のように、サンプルが収集される時間と、数式に使用できるようになる時間には遅延があります。 使用するときにこれを考慮する必要があります、 <em>GetSample</em> メソッドを参照してください <em>GetSamplePercent</em> 以下です。</td>
  </tr>
  <tr>
    <td>GetSamplePeriod()</td>
    <td>履歴のサンプル データ セットで受け取ったサンプルの期間を返します。</td>
  </tr>
  <tr>
    <td>HistoryBeginTime()</td>
    <td>メトリックの最も古い使用可能なデータ サンプルのタイムスタンプを返します。</td>
  </tr>
  <tr>
    <td>GetSamplePercent()</td>
    <td><p>現在、履歴に一定期間あるサンプルの割合を返します。 次に例を示します。</p>
    <p><b>doubleVec GetSamplePercent ((timestamp | timeinterval) startTime [、(timestamp | timeinterval) endTime])</b>
    <p>返されたサンプルの割合が指定した samplePercent 未満の場合は、GetSample メソッドが失敗するため、GetSamplePercent メソッドを使用して最初に確認し、十分なサンプルが存在しない場合は、自動スケール評価を停止せずに代替の操作を実行します。</p></td>
  </tr>
</table>

### メトリック

リソースとタスクの両方を使用することができます **メトリック** いつ数式とこれらのメトリックを定義する使用できるか、プール内の計算ノードを管理します。

<table>
  <tr>
    <th>メトリック</th>
    <th>説明</th>
  </tr>
  <tr>
    <td>リソース</td>
    <td><p>リソース メトリックは、CPU 使用量、帯域幅使用量、メモリ使用量、およびコンピューティング ノードの数に基づきます。 これらのシステム定義変数 (」に記載 **変数** 上) の数式で、プール内のコンピューティング ノードを管理するために使用します。</p>
    <p><ul>
      <li>$TargetDedicated</li>
      <li>$NodeDeallocationOption</li>
    </ul></p>
    <p>次のシステム定義の変数は、ノードのリソース メトリックに基づいて調整を行うために使用されます。</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td>タスク</td>
    <td><p>タスクの状態 (アクティブ、保留中、完了) に基づきます。</p>
    <p>次のシステム定義の変数は、タスク メトリックに基づいて調整を行うために使用されます。</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$SucceededTasks</li>
      <li>$FailedTasks</li>
      <li>$CurrentDedicated</li></ul></p></td>
  </tr>
</table>

## 自動スケールの数式のビルド

自動スケールの数式の構築は、上記のコンポーネントを使用して、ステートメントと完全な数式を組み合わせて行われます。 たとえば、ここでは最初に次を行う数式の要件を定義して、数式を構築しています。

1. CPU 使用率が高い場合、プール内のコンピューティング ノードの目標数を増やす
2. CPU 使用率が低い場合、プール内のコンピューティング ノードの目標数を減らす
3. 常に最大ノード数を 400 までに制限する

 *増やす* CPU 使用率が高い中に、ノードの最後の 10 分間に平均最小の CPU 使用率が 70% を超えるがあった場合のノードの現在の目標数の 110% である値を持つユーザー定義の変数 ($TotalNodes) を設定するステートメントを定義します。

    $TotalNodes = (min($CPUPercent.GetSample(TimeInterval_Minute*10)) > 0.7) ? ($CurrentDedicated * 1.1) : $CurrentDedicated;

次のステートメントが過去 60 分間の平均 CPU 使用率があった場合に、ノードの現在の目標数の 90% に同じ変数を設定 *[* 20%、CPU 使用率が低い中に対象を減らすことです。 このステートメントにも、ユーザー定義変数が参照するメモ *$TotalNodes* ステートメントです。

    $TotalNodes = (avg($CPUPercent.GetSample(TimeInterval_Minute*60)) < 0.2) ? ($CurrentDedicated * 0.9) : $TotalNodes;

専用のコンピューティング ノードの目標数を制限するようになりました、 **最大** 400。

    $TargetDedicated = min(400, $TotalNodes)

完全な数式を次に示します。

    $TotalNodes = (min($CPUPercent.GetSample(TimeInterval_Minute*10)) > 0.7) ? ($CurrentDedicated * 1.1) : $CurrentDedicated;
    $TotalNodes = (avg($CPUPercent.GetSample(TimeInterval_Minute*60)) < 0.2) ? ($CurrentDedicated * 0.9) : $TotalNodes;
    $TargetDedicated = min(400, $TotalNodes)

## 自動スケールを有効にしてプールを作成する

プールの作成時に自動スケールを有効にするには、次のいずれかの手法を使用します。

- [New-azurebatchpool](https://msdn.microsoft.com/library/azure/mt125936.aspx) – この Azure PowerShell コマンドレットでは、AutoScaleFormula パラメーターを使用して、自動スケーリングの数式を指定します。
- [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx) – プールの設定し、プールを作成するこの .NET メソッドが呼び出された場合、 [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) と [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) 自動スケーリングを有効にするプロパティです。
- [プールのアカウントを追加する](https://msdn.microsoft.com/library/azure/dn820174.aspx) – enableAutoScale と autoScaleFormula 要素が作成されると、プールの自動スケーリングを設定する REST API 要求で使用されます。

> [AZURE.NOTE] 上記の手法の 1 つを使用して、プールの作成時に自動スケーリングを設定した場合、 *targetDedicated* プールのパラメーターはありません (してはいけません) の作成時に指定します。 なお、自動スケールが有効なプールのサイズを手動で変更する場合 (たとえば [BatchClient.PoolOperations.ResizePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx))、最初に、プールの自動スケーリングを無効にし、プールのサイズを変更する必要があります。

次のコード スニペットは、自動スケール対応の作成を示しています。 [CloudPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx) を使用して、 [バッチ .NET](https://msdn.microsoft.com/library/azure/mt348682.aspx) ライブラリが数式の月曜日、に対象のノード数を 5 に設定すると、週のその他のすべての曜日に 1 です。 スニペットでは、"myBatchClient"は、適切に初期化されたインスタンスの [BatchClient](http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx))。

        CloudPool pool myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
        pool.AutoScaleEnabled = true;
        pool.AutoScaleFormula = "$TargetDedicated = (time().weekday==1?5:1);";
        pool.Commit();

## プールを作成した後に自動スケールを有効にする

既に指定した数を使用してコンピューティング ノードで、プールを設定した場合、 *targetDedicated* パラメーターを自動的に規模の設定は後で既存のプールを更新することができます。 次のいずれかの方法で行います。

- [BatchClient.PoolOperations.EnableAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx) – この .NET メソッドには、プールに適用する既存のプールと自動スケーリングの数式の ID が必要です。
- [プールの自動スケーリングを有効にする](https://msdn.microsoft.com/library/azure/dn820173.aspx) – この REST API 要求 URI に既存のプールと、要求本文に自動スケーリングの数式の ID が必要です。

> [AZURE.NOTE] 値を指定した場合、 *targetDedicated* パラメーター、プールの作成時に自動スケーリングの数式が評価されるときに無視されます。

このコード スニペットは、プールを使用して既存の自動スケールを有効にすると、 [バッチ .NET](https://msdn.microsoft.com/library/azure/mt348682.aspx) ライブラリです。 既存のプールでの数式の有効化と更新の両方に、同じメソッドを使用することに注意してください。 そのため、この手法は *更新* 自動スケールが有効になっている場合は、指定されたプールでの式。 このスニペットは、"myBatchClient"がの適切に初期化されたインスタンスであると想定しています。 [BatchClient](http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx), 、"mypool"は、既存の ID と [CloudPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx)します。

         // Define the autoscaling formula. In this snippet, the  formula sets the target number of nodes to 5 on
         // Mondays, and 1 on every other day of the week
         string myAutoScaleFormula = "$TargetDedicated = (time().weekday==1?5:1);";

         // Update the existing pool's autoscaling formula by calling the BatchClient.PoolOperations.EnableAutoScale
         // method, passing in both the pool's ID and the new formula.
         myBatchClient.PoolOperations.EnableAutoScale("mypool", myAutoScaleFormula);

## 自動スケールの数式を評価する

アプリケーションで使用する前には必ず数式を評価することをお勧めします。 既存のプールで数式の "テストを実行" することで、数式を評価します。 次を使用して行います。

- [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) または [BatchClient.PoolOperations.EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx) – これらの .NET メソッドは、既存のプールと自動スケーリングの数式を格納する文字列の ID を必要とします。 インスタンスに、呼び出しの結果が含まれる、 [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) クラスです。
- [自動スケーリングの数式を評価](https://msdn.microsoft.com/library/azure/dn820183.aspx) – この REST API 要求にプール ID は、URI で指定し、自動スケーリングの数式がで指定された、 *autoScaleFormula* 要求本文の要素。 操作の応答には、数式に関連する可能性があるすべてのエラー情報が含まれています。

> [AZURE.NOTE] 自動スケーリング式を評価するには、必要があります最初が有効にする有効な式を使用して、プールのオートスケー リング。

このコード スニペットを使用して、 [バッチ .NET](https://msdn.microsoft.com/library/azure/mt348682.aspx) に適用する前に式を評価してライブラリ、 [CloudPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx)します。

        // First obtain a reference to the existing pool
        CloudPool pool = myBatchClient.PoolOperations.GetPool("mypool");

        // We must ensure that autoscaling is enabled on the pool prior to evaluating a formula
        if (pool.AutoScaleEnabled.HasValue && pool.AutoScaleEnabled.Value)
        {
            // The formula to evaluate - adjusts target number of nodes based on day of week and time of day
            string myFormula = @"
                $CurTime=time();
                $WorkHours=$CurTime.hour>=8 && $CurTime.hour<18;
                $IsWeekday=$CurTime.weekday>=1 && $CurTime.weekday<=5;
                $IsWorkingWeekdayHour=$WorkHours && $IsWeekday;
                $TargetDedicated=$IsWorkingWeekdayHour?20:10;
            ";

            // Perform the autoscale formula evaluation. Note that this does not actually apply the formula to
            // the pool.
            AutoScaleEvaluation eval = client.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

            if (eval.AutoScaleRun.Error == null)
            {
                // Evaluation success - print the results of the AutoScaleRun. This will display the values of each
                // variable as evaluated by the the autoscaling formula.
                Console.WriteLine("AutoScaleRun.Results: " + eval.AutoScaleRun.Results);

                // Apply the formula to the pool since it evaluated successfully
                client.PoolOperations.EnableAutoScale(pool.Id, myFormula);
            }
            else
            {
                // Evaluation failed, output the message associated with the error
                Console.WriteLine("AutoScaleRun.Error.Message: " + eval.AutoScaleRun.Error.Message);
            }
        }

このスニペットで数式の評価に成功すると、次のような出力になります。

        AutoScaleRun.Results: $TargetDedicated = 10;$NodeDeallocationOption = requeue;$CurTime = 2015 - 08 - 25T20: 08:42.271Z;$IsWeekday = 1;$IsWorkingWeekdayHour = 0;$WorkHours = 0

## 自動スケールの実行に関する情報を取得する

数式が適切に実行されるように、自動スケールの実行の結果を定期的に確認する必要があります。 次のいずれかの方法で行います。

- [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) – のインスタンスを提供する .NET ライブラリのプールの特定のプロパティを使用すると、 [AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.aspx) 最新の自動スケーリングの実行の次のプロパティを提供するクラス。
  - [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)
  - [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
  - [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
- [プールに関する情報を取得](https://msdn.microsoft.com/library/dn820165.aspx) – この REST API 要求には、最新の自動スケーリングの実行を含むプールに関する情報が返されます。

## 数式の例

数式がプールのコンピューティング リソースの自動スケールを行うために使用できるいくつかの方法を示した例を見ていきましょう。

### 例 1

プール内のノードの数の増減に応じて、週の曜日と時刻に基づくプールのサイズを調整するとします。

```
$CurTime=time();
$WorkHours=$CurTime.hour>=8 && $CurTime.hour<18;
$IsWeekday=$CurTime.weekday>=1 && $CurTime.weekday<=5;
$IsWorkingWeekdayHour=$WorkHours && $IsWeekday;
$TargetDedicated=$IsWorkingWeekdayHour?20:10;
```

この数式は、最初に現在の時刻を取得します。 平日 (1 ～ 5) および勤務時間 (午前 8 時～午後 6 時) 内の場合、目標のプール サイズは 20 のノードに設定されます。 それ以外の場合、プール サイズの目標は 10 のノードに設定されます。

### 例 2

この例では、プールのサイズはキューのタスク数に基づいて調整されます。 コメントと改行の両方が数式の文字列に使用できることに注意してください。

```
// Get pending tasks for the past 15 minutes.
$Samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have less than 70% data points, we use the last sample point, otherwise we use the maximum of
// last sample point and the history average.
$Tasks = $Samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise half of current dedicated.
$TargetVMs = $Tasks > 0? $Tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it to 20. This value
// should be adjusted according to your use case.
$TargetDedicated = max(0,min($TargetVMs,20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### 例 3

タスクの数に基づいてプールのサイズを調整する別の例では、次の式も考慮、 [MaxTasksPerComputeNode](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx) がプールに設定されている値。 これは、特にコンピューティング ノードで並列タスクの実行が必要な場合に便利です。

```
// Determine whether 70% of the samples have been recorded in the past 15 minutes; if not, use last sample
$Samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$Tasks = $Samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the MaxTasksPerComputeNode
// property on this pool is set to 4, adjust this number for your use case)
$Cores = $TargetDedicated * 4;
$ExtraVMs = (($Tasks - $Cores) + 3) / 4;
$TargetVMs = ($TargetDedicated+$ExtraVMs);
// Attempt to grow the number of compute nodes to match the number of active tasks, with a maximum of 3
$TargetDedicated = max(0,min($TargetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### 例 4

次に自動スケールの数式例を示します。この例では、最初の期間はプール サイズを特定のノード数に設定し、最初の期間が経過した後は、実行中のアクティブなタスク数に基づいてプール サイズを調整します。

```
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"

    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

上記のコード スニペットの数式には、次の特徴があります。

- 最初のプール サイズを 4 ノードに設定しています
- プールのライフサイクルの最初の 10 分間は、プール サイズを調整しません
- 10 分間が経過した後は、過去 60 分間の実行中でアクティブなタスク数の最大値を取得します。
  - 両方の値が 0 の場合 (過去 60 分間に実行中またはアクティブなタスクがないことを示します)、プール サイズは 0 に設定されます
  - いずれかの値が 0 よりも大きい場合は、変更されません

## 次のステップ

1. アプリケーションの効率を完全に評価するには、コンピューティング ノードにアクセスする必要がある場合があります。 リモート アクセスを利用するには、アクセスするノードにユーザー アカウントを追加し、そのノードの RDP ファイルを取得する必要があります。
    - 次のいずれかの方法でユーザー アカウントを追加します。
        * [New-azurebatchvmuser](https://msdn.microsoft.com/library/mt149846.aspx) – この PowerShell コマンドレットは、プール名を受け取り、パラメーターとして、ノード名、アカウント名とパスワードを計算します。
        * [BatchClient.PoolOperations.CreateComputeNodeUser](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createcomputenodeuser.aspx) – この .NET メソッドのインスタンスを作成、 [ComputeNodeUser](https://msdn.microsoft.com/library/microsoft.azure.batch.computenodeuser.aspx) いるアカウント名とパスワードをコンピューティング ノードに対して設定できますクラスと [ComputeNodeUser.Commit](https://msdn.microsoft.com/library/microsoft.azure.batch.computenodeuser.commit.aspx) がそのノードで、ユーザーを作成するインスタンスで呼び出され、します。
        * [ユーザー アカウントのノードを追加](https://msdn.microsoft.com/library/dn820137.aspx) : プールと計算ノードの名前は URI で指定し、アカウント名とパスワードがこの REST API 要求の要求本文内のノードに送信されます。
    - RDP ファイルを取得する
        * [BatchClient.PoolOperations.GetRDPFile](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getrdpfile.aspx) – この .NET メソッドには、ノード ID、プールの ID が必要として、RDP の名前のファイルを作成します。
        * [ノードのリモート デスクトップ プロトコル ファイルを取得](https://msdn.microsoft.com/library/dn820120.aspx) – この REST API 要求には、プールの名前とコンピューティング ノードの名前が必要です。 応答に RDP ファイルの内容が含まれています。
        * [Get AzureBatchRDPFile](https://msdn.microsoft.com/library/mt149851.aspx) – この PowerShell コマンドレットは、指定した計算ノードから RDP ファイルを取得し、指定したファイルの場所またはストリームに保存されます。
2.  一部のアプリケーションは、処理するのに困難な大量のデータを生成します。 これは解決するために 1 つの方法で [効率的なリスト クエリ](batch-efficient-list-queries.md)します。


