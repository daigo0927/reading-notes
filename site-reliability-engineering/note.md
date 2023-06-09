# [Site Reliability Engineering](https://www.oreilly.co.jp/books/9784873117911/)

## 第１部：イントロダクション

### １章：イントロダクション

- Googleでは全てのSREに対して、チケット、おんコール、手作業といった運用業務の合計を一定以下にするよう、上限を設けた。
- SREの責任：サービスの可用性、レイテンシ、パフォーマンス、効率性、変更管理、モニタリング、緊急対応、キャパシティプランニング
- サービス障害は予測可能でコントロールするべきもの。SREとプロダクト開発者は、機能のリリース速度を最大化するためにエラーバジェットを使うことを目標にする

### ２章：SREの観点から見たGoogleのプロダクション環境

- N+2の冗長性：アップデートによってタスクが一つずつ使えなくなる＋アップデート中にマシン一台に障害が発生する

## 第２部：原則

### ３章：リスクの需要

- サイトリライアビリティエンジニアリングでは、単純に稼働時間を最大化するよりも、可用性におけるリスクと、イノベーションの速度およびサービス運用の効率性というゴールのバランスを取ろうとする。これによってユーザーの満足感を機能、サービス、パフォーマンスについて最適化する
- サービスのリスク許容度の評価要素：必要な可用性のレベル、障害の種類によるサービスへの影響の違い、リスク曲線上におけるサービスコストの変化、検討できるサービスメトリクスの種類
- 可用性のターゲットレベルの要素：ユーザーの期待、サービスが直接的に収入につながっているか、サービスが有料か無料か、競合のサービスレベル、コンシューマ向けか企業向けか
- エラーバジェット：一つの期間内でサービスの信頼性がどの程度損なわれても許容できるかを示す、明確で客観的なメトリクス。エラーバジェットが消費されてきたら、リリースを一時的に停止させ、システムの弾力性強化などを実施するきっかけにできる。

### ４章：サービスレベル目標

- SLAはユーザーとの間で結ぶ契約であり、ビジネスやプロダクトに関する判断に密接に関わる。SREはSLAの構築には関わらないことが多いが、SLOの遵守やSLIの定義の支援には関わる。
- ユーザーとやり取りするサーバー、ストレージ、ビッグデータシステムなど、サービスの内容によってSLIの傾向は変わる
- ほとんどのメトリクスは、平均よりも分布として見る方が状態への適切な理解が得られる。
- SLOを常に満たし続けることを求めるのは現実的ではない。それよりもエラーバジェットを認めて日毎や週ごとに追跡した方が良い。エラーバジェットは他のSLOを満たすためのSLOに過ぎない。
- SLOを考えるときは、まずユーザーが気にすることは何かを考える。
- SLOを設定するためのポイント
  - 現在のパフォーマンスに基づいてターゲットを選択してはならない
  - シンプルさを保つ
  - 「絶対」は避ける
  - SLOは最小限に留める
  - 最初から完璧でなくても良い

### ５章：トイルの撲滅

- トイルの傾向：手作業、繰り返される、自動化できる、（戦略的でなく）戦術的、長期的な価値を持たない、サービスの成長に対して線形
- トイルを減らし、サービスをスケールさせる作業が、SREにおける「エンジニアリング」である
- 典型的なSREの活動は以下。GoogleのSREは一定期間で平均した時、最低50パーセントをエンジニアリングの作業に充てると目標にしている。
  - ソフトウェアエンジニアリング
  - システムエンジニアリング
  - トイル
  - オーバーヘッド

### ６章：分散システムのモニタリング

- 人間へのアラートを発生させるルールは、理解しやすく、明確な障害を表現するものであるべき
- モニタリングシステムは、何が壊れたのか、なぜそれが壊れたのかという二つに答える必要がある
- ホワイトボックスモニタリングは、ログやHTTPエンドポイントなどの、システム内部を調査する機能に依存しており、リトライなどによってマスクされる障害や、近々生じそうな問題を検出できる。ブラックボックスモニタリングは、現在システムが正常に動作していない、というような症状を扱う。
- ユーザーが直接利用するシステムにおける代表的なシグナル：レイテンシ、トラフィック、エラー、サチュレーション。
- 計測の粒度を細かくしすぎると、収集、保存、分析のコストが大きくなるので注意が必要
- モニタリングの対象を選別する上でのガイドラインは以下
  - 本当のインシデントを最も頻繁に捉えるルールは、可能な限りシンプルであり、予想しやすく信頼できるものであるべき
  - ほとんど実施されない（例えばSREチームによっては、四半期に１回未満が目安）データの収集、集計、アラートの設定は削除すべき
  - 収集されていても、事前に作成されたダッシュボードのいずれにも表示されず、どのアラートにも使われていないシグナルは削除の対象
- モニタリングやアラートのルールを作成する際のガイドラインは以下
  - ルールが検出する状況は、そのルールなしでは検出されない状況であり、緊急で、対応が可能で、ユーザーに影響を及ぼすか？
  - そのアラートが無害なものだと判断して、対応せずにそのままにしてしまえるか？
  - そのアラートは間違いなく、ユーザーに悪影響が生じていることを示しているか？
  - そのアラートに対応してアクションを取れるか？アクションは自動化できないか？
  - その問題でページを受ける人は他にもいるか？

### ７章：Googleにおける自動化の進化

- 大規模なサービスほど、自動化による一貫性、素早さ、信頼性というメリットが大きくなる
- 特定のオペレーションを自動化するだけでなく、そのオペレーションがシステムによって自律的に解決されるようなソフトウェア開発も視野に入れる必要がある。
- 自律的な運用を大規模システムに対応させるのは難しいが、その過程におけるサブシステムの分離やAPIの導入、副作用の最小化などの優れたエンジニアリングプラクティスは非常に役に立つ
- 感想：Googleにおける自動化・自律化の振り返り。やや抽象的に感じた。

### ８章：リリースエンジニアリング

- リリースエンジニアリングにおける４つの哲学：セルフサービスモデル、高速性、密封ビルド、ポリシーと手順の強制
- 適切なツールや自動化、しっかり定義されたポリシーがあれば、開発者やSREはソフトウェアのリリースについて心配せずに済む。
- リリースエンジニアリングは初期の段階から始めた方が良い

### ９章：単純さ

- システム管理におけるSREのアプローチをまとめると、「システム内でのアジリティと安定性のバランスを取ること」と言える
- ソフトウェアに単純性を導入することは、開発における信頼性を増すことにつながり、開発者は作成するソフトウェアやシステムの機能やパフォーマンスという、自分達が本当に注目しなければならないことに集中できるようになる
- SREチームが実施すべきこと
  - 受け持っているシステムに想定外の複雑さが生じていたら差し戻す
  - 関わっているシステムや、運用を受け持つことになるシステムから複雑さを取り除く努力
- 不要なコードをコメントアウトやフラグ管理で対応しようとするのは悪い提案である
- プロジェクトにおいて変更や追加されたコードの1行1行は、潜在的に障害やバグの可能性もつ
- 利用者に提供するメソッドや引数が少ないほど、APIは理解しやすくなり、それらのメソッドを改善するための努力も容易になる

## 第３部：実践

- SREが行うのは、サービスを稼働させ、その健全性の責任を持つこと。そのためにはモニタリングシステムの開発やキャパシティプランニング、インシデント対応、サービス障害の根本原因解決の確認などがある。
- サービスの健全性は、システムがサービスとして機能するための基本的な必要条件から、サービスの方向性を能動的にコントロールするような高度な機能まで広いレベルがある。

### 10章：時系列データからの実践的なアラート

  * 必要なのは、高レベルのサービスの目的に関するアラートを発しながら、必要に応じて個々のコンポーネントの調査もできる粒度の情報を保つようなモニタリング
  * Borgを補完するモニタリングシステムとしてBorgmonが生まれた。Borgmonは共通のデータ公開フォーマットを利用することで、オーバーヘッドを抑えながら大量のデータを収集できる。OSSとしてはRiemannやPrometheusなどがある。
  * ホワイトボックスモニタリングだけでは、ユーザーの視点に気づけなくなるため、ブラックボックスモニタリングも合わせて必要
  * Borgmonは設定したルールに対するユニットテスト、回帰テストの構築方法も提供している

### 11章：オンコール対応

  * どんなインシデントも、一つの根本原因から生じる一連のイベントとアラートと位置づけ、同じポストモーテムの一部として議論するべき。
  * インシデント対応は拙速な対応よりも慎重な判断が必要。そのためには以下が特に重要
	* 明確なエスカレーションパス
	* しっかりと規定されたインシデント管理の手順
	* 避難を伴わないポストモーテム文化
  * Google内部では、Webベースのツールによって、インシデント発生時の役割の受け渡しや状況の更新の記録、通知などをサポートしている
  * 運用の負荷が大きすぎると判断される場合には、開発チームと交渉してシステム改善の目標を設定し協力するのも良い

### 12章：効果的なトラブルシューティング

  * システムの問題に対して効果的なレポートを作るには、期待される動作と実際の動作、可能なら再現手順を含める。
  * 大きなサービス障害に際してまず必要となるのは、根本原因の究明ではなくシステムをできる限りうまく動作するように手当てすることとなる。
  * 異常を起こしているシステムに対しては、「何が」「どこで」「なぜ」異常を起こしているのか考える
  * テストと対応：明らかなことから考え始める、テストが環境に与える影響を考える、再現が難しい問題は妥協が必要な時もある、など
  * 否定的な結果も有益となる。実験結果は決定的なものであり、ツールや方法論は実験結果よりもさらに長く残る。また結果を公開することで、組織のナレッジにもなる。
  * 問題を起こした要因を見つけたら、システムで生じた問題、問題を突き止めた過程、問題の解決方法、再発防止策を書いておく→ポストモーテムの作成
  * 特定しきれていないバグを残したままのローンチは理想的ではないが、既知のバグを無くし切るのが現実的でない場合には、エンジニアリング上の判断を下して次善の対応を行う場合もある。
  * トラブルシューティングを迅速に行うための基本的なアプローチ
	* 観察のための仕組みを構築する。ホワイトボックスのメトリクスと、構造化されたログの双方を、基盤の部分から上位に至るまでの各コンポーネントに組み込む
	* システムの設計時に、十分に知られている技術を使ってコンポーネント間に観察可能なインターフェースを持たせる

### 13章：緊急対応

- まずは落ち着いて、組織の対応手順があるならそれに従って対応する。
- 感想：インシデントの対応手順の整備、ロールバック方法の確認は大事
- 解決できない問題は存在しない。
- サービス障害の歴史を残し、繰り返さない
- 「もしこんな事態が起きたら、どんな対応が必要か？誰を呼ぶか？などを考えておくのも良い」


### 14章：インシデント対応

- Googleのインシデント管理システムは、[Incident Command System](https://ja.wikipedia.org/wiki/%E3%82%A4%E3%83%B3%E3%82%B7%E3%83%87%E3%83%B3%E3%83%88%E3%83%BB%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%83%BB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)に基づいている。うまく設計されたインシデント管理のプロセスは以下の特徴を持つ
  - 責任の再帰的な分離
  - 明確な司令所
  - ライブインシデント状況ドキュメント
  - はっきりとした引き継ぎ
- インシデントを宣言する条件ははっきりと設定しておくべき。例えば「問題解決のための他チームの関与が必要」、「サービス障害がユーザーに影響している」、「集中した1時間分析を行なっても解決しない」、など
- インシデント管理のためのベストプラクティス：優先順位、準備、信頼、自己観察、代案の検討、訓練、持ち回り

### 15章：ポストモーテムの文化、失敗からの学び

- ポストモーテムを書くことの主な目的は、インシデントがドキュメント化されること、影響を及ぼした全ての根本原因群が十分に理解されること、そして再発の可能性や影響を削減するための予防策が確実に導入されるようにすることと言える。
- ポストモーテムの文化を導入するために、SREとしては定期的にポストモーテムを組織で共有する、ポストモーテム読書会を開催する、不運の輪というトレーニングを実施するなどができる。
- ベストプラクティス
  - 非難を避け、建設的であり続ける
  - ポストモーテムにはレビューを行い、必ず次に活かす。
  - 正しいことをおこなっった人には目に見える報酬を
  - ポストモーテムの効果に対するフィードバックを求める。定期的に調査する。

### 16章：サービス障害の追跡

- Slackなどのメッセージングツールは、Outalatorのような連絡ツールを連携するのに役立つ
- アラートなどのイベントを管理する際は、緩い命名規則でもタグ付けが役に立つ

### 17章：信頼性のためのテスト

- テストとは、変更が生じたときにある領域における等価性を示すための仕組みと言える。変更前後の双方で同じテストが通っているなら、変更による不確実性が減ったと言える。
- ソフトウェアテストには、大別して伝統的なテスト（ユニットテスト、結合テスト、システムテスト）とプロダクションテスト（稼働中のプロダクション環境に関するテスト、設定テストやストレステストなど。ブラックボックステストとも言える）
- 既存のプロジェクトでテストを作っていく際には、一度に全てのテストを作ろうとするのではなく、ビジネスへの影響や他チームからの依存などに応じて優先度をつけ、最大のインパクトを最小の労力で生み出せるテストから始めるべき。
- SREのツール（メトリクス測定、取得や、サーバー上ファイルの変更など）や自動化ツール（DBのインデックス選択やデータセンター間のロードバランスなど）もテストするべき。
- SREのモデルでは、プロダクション環境の設定からテスト用のインフラを分離すれば、悪影響が生じる。プロダクションの記述とアプリケーションの記述を関連づけられなくなるため。（？）
- 感想：設定ファイルが具体的に何の設定ファイルでどういう課題につながるのか分からなかった。
- 感想：「モニタリングプローブの仕組みをデプロイする」という意味がわからなかった。サービスへのヘルスチェックのような処理をテストとして拡張して、プロダクション環境などに適用するということ？
- プローブによるプロダクションテストは、サイトに対して保護を提供するとともに、エンジニアに対して明確なフィードバックを与える。

### 18章：SREにおけるソフトウェアエンジニアリング

- GoogleはSREのスタッフが伝統的なソフトウェア開発の経験を持つエンジニアと、システムエンジニアリングの経験を持つエンジニアから構成されるように計らっている
- 内部的なソフトウェアツールを多くの対象者に普及させるには、「継続的で一貫性のあるアプローチ」「ユーザーによる支持」「シニアエンジニアや管理層による支援、プロダクトの有用性への理解」が必要となる
- プロダクトを開発する際には、最小限の成功の条件と、プロダクトとしての野心的な目標を区別することが大切
- SREによる組織的なソフトウェア開発を導入するためのGoogleのガイドライン
  - 明確なメッセージの作成と発信
  - 組織の機能の評価
  - ローンチ&イテレート
  - プロダクトとしての基準は下げない

### 19章：フロントエンドにおけるロードバランシング

- トラフィックを分散させる「最適」な方法は、扱うサービスや評価する技術的なレベル（HW・SW）などによって異なる。

### 20章：データセンターでのロードバランシング

- 感想：具体的な話多め。気になったらまた見る。

### 21章：過負荷への対応

- 大規模な環境において過負荷を扱う際は、リソースの制限に応じてうまく動作できるようにクライアントやバックエンドを構築することとなる。つまり、可能な際にはリダイレクトを行い、他に方法がなければ透過的にリソースエラーを扱うことになる。
- 多くの場合では、リソースプロビジョニングがうまくいっているかを示すシグナルとして、CPUの消費量がうまく使える。
- バックエンドに到達したリクエストに対して、どの程度重要かを割り当てることで、重要度別に対応を実施できる。
- 多くなバッチジョブで、一度に多くの接続要求が発生する際には、データセンター間のロードバランシングを適用したり、独立したバッチプロキシーだけ使うなどの対応によって過負荷を軽減できる。

### 22章：カスケード障害への対応

- カスケード障害の原因としては、サーバーの過負荷、リソースの枯渇（CPU、メモリ、スレッドなど）、サービスが利用できなくなることなどがある。
- サーバーの過負荷を回避するには、以下の戦略が考えられる。
  - サーバーのキャパシティの上限のロードテスト
  - 品質を落とした結果を返す
  - 過負荷時のリクエスト拒否の仕組みをサーバーに用意する
  - サーバーを過負荷に陥らせるよりは、より高レベルのシステムでリクエストを拒否する仕組みを用意する
  - キャパシティプランニングの実施
- 自動的なリトライを実施する際には、以下を考慮する。
  - バックエンド保護の戦略（22.2：サーバーの過負荷の回避）
  - リトライのスケジューリングには、必ずランダム化した指数バックオフを使用する。
  - リクエストごとのリトライ回数を制限する。
  - サーバー全体に対するリトライバジェットを持たせることを検討する。
  - サービス全体において、あるレベルでリトライを行う必要性を判断する。
  - 明確なレスポンスコードを使用し、さまざまな障害に対する適切な処理を考慮する。
- カスケード障害を引き起こす条件：プロセスの停止、プロセスのアップデート、ロールアウト、自然な利用の増大、計画済みの変更やドレインなど
- カスケード障害に備えるテスト：負荷テストによる障害の発生とその後の振る舞いを観察してみる。クライアント側では、ダウン中に処理をキューイングして置けるか、指数バックオフを利用するか、などをチェックする。
- カスケード障害に対応するための直ぐに行うべき手順
  - リソースの追加
  - ヘルスチェックが障害を引き起こさないようにする
  - サーバーの再起動
  - トラフィックのドロップ（比較的後にとっておくべき手段）
  - デグレーデッドモードへの移行
  - バッチの負荷の排除
  - 問題のあるトラフィックの排除
- 失敗時のリトライや、不健全なサーバーからの負荷の移行などの変更は、単純に一つの障害を他の障害と交換するだけにならないよう注意が必要

### 23章：クリティカルな状態の管理：信頼性のための分散合意

- データストアの特性としては、伝統的なACID（原子性、一貫性、独立性、永続性）と、分散データストアで増えつつあるBASE特性（Basically Available, Soft state, Eventual consistency）などがある
- 分散合意アルゴリズムをうまく利用しているシステムの多くは、それらのアルゴリズムを実装しているZooKeeper, Consul, etcdなどのサービスのクライアントとして動作する。
- 分散合意システムのパフォーマンスを議論する際のワークロードとしては、スループットやリクエストの種類、またデプロイメント戦略などによって多くの差異がある
- 分散合意システムをデプロイする際のレプリカ数は、信頼性への要求や計画済みメンテナンスの頻度、リスクやパフォーマンス、コストなどのトレードオフを考える必要がある。
- 感想：ChatGPTに聞いたところ、Amazon DynamoDBやGoogle Cloud Spanner、Azure CosmosDBなどはデータストアの分散合意システムを提供しており、ユーザーが利用しやすくなっているらしい。
- 分散合意システムのモニタリングの際には、各合意グループ内で動作するメンバー数とプロセスの状態、処理が遅れているレプリカ、リーダーの有無などをモニタリングすると良い。

### 24章：cronによる分散定期スケジューリング

- Googleでは、cronサービスの複数のレプリカをデプロイし、それらの状態の一貫性をを保証するために、分散合意アルゴリズムのPaxosを使っている。
- 設定によっては、大量のcronジョブのスケジューリングはデータセンターの利用状況にスパイクを作り得る。ジョブの設定で時間の指定に幅を持たせることで、ジョブの軌道を分散することができる

### 25章：データ処理のパイプライン

- 定期的なパイプラインのモデルは、自然な成長や変化がシステムにストレスをかけ始めるという点で脆く、リソースの枯渇や処理の進まないチャンクと運用負荷につながる。
- 分散環境で定期パイプラインを実行する際には、バッチスケジューリングのリソースコストと、プロダクションの優先度を持つリソースコストに差をつけることで、リソース取得に合理性を付与する必要がある
- データパイプラインを設計する際には、ビジネスの状況から予想される成長亜k亭、設計変更への要求、追加リソース、レイテンシへの要求などをよく調べておく必要がある。
- Google Workflowでは、設定タスクを通じた出力やユニークな出力ファイル名、サーバートークンのチェックなど、処理の正しさに対する４つの保証を提供している
- Workflowは各タスクがユニークでイミュータブルという点で、分散インフラ上でうまく動作してスケールすることができる。

### 26章：データの完全性：What You Read Is What You Wrote

- データの完全性を考えるにあたって重要なのは、クラウド上のサービスがユーザーから利用可能であり続けることと言える。ユーザーがデータへアクセスできることは特に重要となる。
- データの完全性を極めて高くするための秘密は、予防的な検出と素早い修復及び回復といえる。
- バックアップをしたい人はいない、本当に必要なのはリストアである。
- データの完全性におけるGoogle SREの目標
  - データの完全性は手段であり、目標はデータの可用性：データの完全性とは、保持する期間にわたってのデータの正確性と一貫性を指す。また、ユーザーの観点から見れば、期待通りにデータが利用できなければ、データの完全性が保たれていようと実質的にそのデータは存在していないも同然と捉えられてしまう。
  - バックアップよりもリカバリのシステムを提供しよう：バックアップは普段は感謝されづらい税金のようなものであるが、リストアによる耐障害性やSLOへの貢献に目を向けることで着手の動機づけとなる。
  - データの障害につながる障害の種類：根本原因（運用ミス、アプリケーションのバグなど）×範囲（広範囲、狭く直接的）×レート（ビッグバン、ゆっくり）の組み合わせがある。使おうとしているクラウドAPIが提供しているデータリカバリの選択肢をよく検討することが大事。
  - 深く、広くデータの完全性を管理するのは難しい。システムの多様なレイヤー保護を考える必要がある。
- データ消失に対しては多層防御が必要になる：論理・遅延削除、バックアップと関連するリカバリ、（レプリケーション）、早期検出
- バックアップに関する判断は、リカバリを成功するための要素に基づいて下す必要がある。例えば以下
  - 使用するバックアップ及びリカバリの方法
  - データの完全なバックアップ、あるいはインクリメンタルなバックアップを取ることによってリストアのポイントを作成する頻度
  - バックアップの保存場所、期間
- 莫大なデータをバックアップするのに用いられる、最も一般的かつ効率的な方法は、データ中に「保管ポイント」を確立する方法と言える。
- クラウドの分散ストレージAPIは一定の信頼はできるが、それでも検証は必要。
- データ検証パイプラインを導入することで、長期的には開発者は自信を持って速度を上げられる。
- データリカバリの問題を把握するためには、1.通常の運用の一部として継続的にリカバリプロセスをテストする。2.リカバリのプロセスが失敗した時に通知するアラートをセットし、リカバリが定期的に成功していることを示す。などが必要となる。可能であればそれらのテストは自動化する。
- 障害が起きることは避けられないと考え、実際にそれが起きる前に、テストによって問題を洗い出し、解決することが大事。
- クラウド環境におけるデータ削除レートに対するアラートは、全ユーザーに対する全社規模の集計を行うことで役立てることができる
- データの完全性に対するSREの一般原則の適用
  - システムに対して初心者の心構えを忘れない
  - 信頼しつつも検証を：データの最も重要な要素が正しいかどうかは、通常の処理外のデータ検証システムでチェックする必要がある
  - 願望は戦略にあらず：データのリカバリがきちんとできることは定期的な演習で確認する
  - 多層防御：複数のフォールバック戦略を用意し、広範囲のシナリオに対して妥当なコストでまとめて対応しよう
- 参考：[障害復旧計画ガイド](https://cloud.google.com/architecture/dr-scenarios-planning-guide?hl=ja)

### 27章：大規模なプロダクトのローンチにおける信頼性

- Googleでは、新しいプロダクトやサービスを再現可能な信頼性の下でローンチするためにローンチチェックリストを作成している。これにはアーキテクチャやその依存関係、キャパシティプランニング、予想される障害の形態などを記載している。ローンチチェックリストは会社の内部的なサービスやインフラと密接に関係するため、会社によって異なると考えられる。
- Googleではほとんどの開発をバージョン管理システムのメインラインのブランチで行っているが、リリースはリリースごとのブランチでビルドされる。
- 信頼性のあるローンチのためのテクニックとしては、逐次的かつ段階的なロールアウト、機能フラグの適用、攻撃的なクライアントへの挙動の対処、過負荷時の挙動確認と負荷テストなどが検討できる。
- 急速なスケーラビリティの変化や運用負荷の増大、インフラの変化はLCEだけでは対応しきれないため、プラットフォームAPIの洗練化や継続的ビルドやテスト自動化などを含む、全社的な努力が必要となる。

## 第４部：管理

### 28章：SREの成長を加速する方法：新人からオンコール担当、そしてその先へ

- 新たなSREのための教育アプローチとしては、順序立てたカリキュラムや既存のポストモーテムの復習、限定的かつ計画的な障害に対するリカバリの演習、オンコールローテーションへのシャドウな参加などが考えられる。
- SREとして活躍する上では、以下のような能力や行動が必要となる。
  - 見たことのないシステムに対応するためのリバースエンジニアリングのスキル
  - 大規模な環境における異常を解き明かすための統計的な思考
  - 標準的な運用が破綻した際の臨機応変な対応行動
- オンコール担当者としての能力を獲得するためには、過去のポストモーテムの復習やディザスタロールプレイング、実際のオンコールのシャドウイングなどが有効となる。
- オンコール学習チェックリストを整備しておくことで、新規メンバーの学習やメンター・マネージャーによるオンボーディングの把握の役に立つ。またオンコールのシャドウイングに臨むための準備にもなる。
  - オンコール学習チェックリストの内容：要素技術やアーキテクチャ、関係者、前提知識、理解ポイント、練習問題など

### 29章：割り込みへの対処

- システム管理の際の運用負荷には、ページ、チケット、運用業務の負荷（トイル）が上げられる
- 割り込みの処理方法に影響するメトリクスとしては、割り込みのSLOや期待されるレスポンスタイム、バックログとなる割り込み数、割り込みの重大度や頻度などが挙げられる。
- エンジニアに認知的フロー状態をもたらし、生産性を上げるには、プロジェクトの作業を行うのか割り込み対応を行うのかで時間を二極化することが挙げられる。
- 1人のエンジニアに対して、プライマリオンコールを担当しながら同時にプロジェクト（またはコンテキストスイッチのコストが高い別の仕事）を求めるべきではない

### 30章：SREの投入による運用過負荷からのリカバリ

- SREとしてチームのリカバリに参加するには、順を追ったアプローチがある。
  - 1. サービスの学習と状況の把握：プロジェクトにおいて自動化できるポイントや、それまでの運用におけるストレス発生源、将来的な発火点の特定など
  - 2. 状況の共有：ポストモーテムの記載や、インシデントの整理
  - 3. 変化の推進：常にSREの減速を念頭に置き、発火点の掃除を組織的に行い、説明や質問によって情報を開示する
- これによってSREチームに、変化に対する定量的な視点や具体的な経験、スケーラブルな開発のために必要な中核的減速をもたらすことができる。

### 31章：SREにおけるコミュニケーションとコラボレ－ション

- GoogleのSREの働く対象は大きく二つである。サービスやインフラのSREチームの場合はそれらの開発を行うプロダクトチームとの密接なコラボレーション。もう一つは一般的なSREの文脈での、サービスのパフォーマンスに対する取り組みである。
- GoogleのSREが重視しているコミュニケーションの一つに、プロダクションミーティングがある。プロダクションミーティングでは、SREチームが担当するサービスの状況（プロダクション向けに予定されている変更やメトリクス、障害、ページに関するイベントなど）を運用からのフィードバックとして説明する。これによって関係者のサービスに対する認識を高め、改善することにつながる。
- SREチームの適した構成は責任範囲の流動性に応じて変わるが、多様性は常にメリットになると言える。
- SREとして良い仕事をしようとするなら、他のチームとのコラボレーションのために優れたコミュニケーションスキルが必要になる。
- プロダクト開発チームとSREのコラボレーションはできるだけ早く（理想的には設計段階から）行った方がよく、後からの変更が難しい箇所に対する助言をできるのが望ましい。

### 32章：進化するSREのエンゲージメントモデル

- SREのサポートを幅広く提供するには二つの方法がある。一つは設計の早い段階でSREが関与するケースであり、もう一つはすでにSREが管理するプラットフォーム上にアプリケーションを構築することである。
- プロダクションレディネスレビュー（PRR：サービスに求められる信頼性を特定するためのプロセス）は、SREがプロダクトやサービスへの関与を始める際の初めのステップとして一般的である。SREはPRRを通じて、プロダクション環境の責任を引き受けられるかどうかを評価する。
- PRRにあたっては、エンゲージメント、分析、改善とリファクタリング、トレーニング、オンボーディングなどのステップを経て、サービスのプロダクション環境の管理をSREに移行していく。
- SREの早期エンゲージメントや、SREによるプラットフォーム・フレームワーク利用を進めることで、SREによるサポートをさらにスケーラブルに提供することができる。
- SREのサポートをフレームワーク化するためには、以下の原則が必要となる
  - ベストプラクティスのコード化
  - 再利用可能なソリューション
  - 共通の制御インターフェース（運用制御やロギング、モニタリングなど）を持つ共通プロダクションプラットフォーム
  - 容易な自動化とスマートなシステム

## 第５部：まとめ

### 33章：他の業界からの教訓

- SREの取り組みと他の業界のプラクティスを比較する際は、以下の観点が挙げられる。
  - 準備とディザスターテスト
  - ポストモーテムの文化
  - 自動化と運用のオーバーヘッドの低減
  - 構造化された合理的な判断
- GoogleのSREの活動では、サービスのユーザーの最善の利益を根底に置くこと、入手できるデータに基づいて物事を進めることを前提としている。
- 原子力、航空、医療業界など、サービス中断や障害によるリスクが大きい業界では、高い信頼性のために保守的なアプローチを優先するのが妥当となる。

### 34章：まとめ

- SREには、オンコールシフトによってシステムの課題を理解し、対処する仕事と、そういったシステムを管理しやすくするためのエンジニアリングの仕事がある。
- SREチームはできるだけコンパクトであり、高レベルの抽象化のもとで働き、フェールセーフとしての多くのバックアップシステムと、システムとやりとりするための考え抜かれたAPIを頼りにする。
- 同時に、SREチームは日々のシステム運用から得られるシステムの運用方法、障害の発生の様子や障害への対応方法といった、システムに関する包括的な知識も併せ持つ必要がある。

