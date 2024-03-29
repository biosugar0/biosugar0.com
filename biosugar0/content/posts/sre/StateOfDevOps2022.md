+++
title = "State of DevOps Report2022のメモ"
description = "State of DevOps Report2022のメモ"
date = "2022-10-13"
tags = ["SRE","DevOps"]
slug = "state-of-devops-2022"
+++

[State of DevOps Report](https://www.devops-research.com/research.html#reports)の2022年版を読んだのでメモ。
DevOps Research and Assessment（DORA）チームが毎年調査研究を発表しており、今年はセキュリティに焦点を当てている。

今年のレポートはいくつか驚くべき結果が出ていて原因はだいたいデータ不足ということなので来年のレポートが楽しみになる内容だった。

## ソフトウェアサプライチェーンの保護

今年の調査では、SLSAフレームワークによって技術的な実践方法、
NISTのSSDFを利用してセキュリティに関する姿勢、プロセス、および非技術的な実践を調査している。

> * SLSA: Supply-chain Levels for Software Artifacts。Googleがソフトウェアサプライチェーン攻撃の脅威拡大に対処する手段として提唱したフレームワーク。
> * SSDF: Secure Software Development Framework。米国立標準技術研究所（NIST）提唱のソフトウェアの脆弱性のリスクを軽減するための推奨事項。
> * ソフトウェアサプライチェーン: ソフトウェア開発ライフサイクル全体で関与するあらゆるもの

その結果、組織のアプリケーション開発におけるセキュリティ対策を決定づける最大の要因は、
パフォーマンスを重視する、信頼性が高く、非難されることが少ない文化は、権力や規律を重視する、信頼性が低く、非難されることが多い文化より、新しいセキュリティ対策が平均以上に採用される可能性が 1.6 倍も高かった。

また、このようなセキュリティ対策の確立に重点を置いているチームは、開発者の燃え尽き症候群が減少することがわかった。
セキュリティ対策の水準が低いチームは、水準が高いチームに比べて、燃え尽き症候群の発生確率が 1.4 倍になる。

<!--more-->

CIが導入されていない場合、セキュリティ対策はソフトウェアデリバリパフォーマンスに影響を及ぼさない。しかし、CIを導入した場合、セキュリティ対策はソフトウェアデリバリーパフォーマンスに強い正の影響を与える。
つまり、セキュリティ対策がソフトウェアデリバリパフォーマンスにプラスの影響を与えるためには、CIが必要である。
SLSA や SSDF のフレームワークのようなアプローチに従うだけでは、文化や業績の測定基準をすべて改善することはできないかもしれないが、セキュリティが他の開発の優先事項を犠牲にする必要はないことは明らかである。

脅威が発見されたときの無計画な作業や「消火訓練」ではなく、セキュリティ対策をシフトレフトし、
既存の開発ワークフローに取り入れるためのツールやプロセスは、セキュリティリスクを低減し、開発者体験を高める。

### SLSAプラクティスと調査定義  
 
* **CI/CDの一元化**:本番リリースは、CI/CDシステムを使用して構築され、開発者のマシンでは決して行われません。
* **履歴の保存**:リビジョンとその変更履歴は無期限で保存される
* **ビルドスクリプト**:ビルドは、ビルドスクリプトによって完全に定義され、それ以外のものはない
* **独立性**:ビルドは分離されており、同時または後続のビルドに干渉することはない
* **ビルドテキストファイル**:ビルドの定義や設定は、バージョン管理システムに格納されたテキストファイルで定義される
* **パラメータメタデータ**:アーティファクトに関するビルドメタデータ（依存関係、ビルドプロセス、ビルド環境など）には、すべてのビルドパラメータが含まれる
* **依存関係メタデータ**:アーティファクトに関するビルドメタデータ（依存関係、ビルドプロセス、ビルド環境など）は、すべての依存関係を文書化する
* **生成されるメタデータ**:ビルドメタデータ（依存関係、ビルドプロセス、ビルド環境など）は、ビルドサービスによって生成されるか、ビルドサービスを読み込んだビルドメタデータ生成ツールによって生成される
* **入力の防止**:ビルドの実行時に、ビルドステップがビルド入力を動的にロードしないようにした（つまり、必要なソースと依存関係はすべて前もって取得される）
* **ユーザーによる編集不可**:アーティファクトに関するビルドメタデータ（依存関係、ビルドプロセス、ビルド環境など）をビルドサービスのユーザーが編集できない
* **メタデータの利用**:ビルドのメタデータ（依存関係、ビルドプロセス、ビルド環境など）を、必要な人が利用でき（例:中央データベース経由）、彼らが受け入れるフォーマットで提供されること
* **二人によるレビュー**:リビジョン履歴のすべての変更は、提出前に信頼できる2人の人物によって個別にレビューされ、承認されなければならない
* **メタデータの署名**:アーティファクトがどのように生成されたかについてのビルドメタデータ（依存関係、ビルドプロセス、ビルド環境など）は、私のビルドサービスによって署名される

※ 各質問に対して、「全く確立していない」、「わずかに確立している」、「適度に確立している」、「非常に確立している」、「完全に確立している」の5つの回答が可能であった。


### SSDFプラクティスと調査定義

* **セキュリティレビュー**:担当するアプリケーションの主要な機能すべてについて、セキュリティレビューを実施している
* **継続的なコード解析／テスト**:DORAでは、サポートするすべてのリリースの自動または手動によるコード解析とテストを継続的に行い、これまで検出されなかった脆弱性の存在を特定または確認している
* **早期のセキュリティテスト**:セキュリティテストは、私または他のチームが、ソフトウェア開発プロセスの早い段階で実施する。
* **脅威への効果的な対処**:私の組織には、セキュリティ上の脅威に対処するための効果的な方法がある
* **開発チームとの統合**:セキュリティの役割は、ソフトウェア開発チームに統合されています
* **要求事項の文書化**:組織が開発または取得するソフトウエア（サードパーティ及びオープンソースを含む）のセキュリティ要件をすべて特定し、 文書化するためのプロセスが存在する
* **定期的な要件レビュー**:セキュリティ要件が定期的（年1回、または必要に応じて早め）に見直される
* **メタデータの作成**:ビルドメタデータ（依存関係、ビルドプロセス、ビルド環境など）が、ビルドサービスによって生成されるか、ビルドサービスを読み込むビルドメタデータ生成ツールによって生成されるかのいずれかである
* **開発サイクルに統合**:私の会社では、ソフトウェアセキュリティプロトコルが開発プロセスにシームレスに組み込まれている
* **プロジェクト全体の標準プロセス**:私の会社では、プロジェクト全体でソフトウエアセキュリティに対処するための標準的なプロセスがある
* **セキュリティレポートを監視する**:使用しているソフトウエア及びそのサードパーティ製コンポーネントに潜在する脆弱性について、公的な情報源からもたらされる情報を監視する努力を継続している
* **必要なツールがある**:セキュリティテストを実施するために必要なツールにアクセスできる

※ 回答者は各質問に対して、「強く反対」、「反対」、「やや反対」、「どちらともいえない」、「やや賛成」、「賛成」、「強く賛成」の7つの回答が可能であった。

## コンテキストの重要性

あるコンテキストでは有利な技術的能力が、別のコンテキストでは不利になる可能性がある。今年は、このような仮説に基づいた条件を相互作用という形で明示的にモデル化することに重点を置き、その結果、多くの仮説が今年のデータで裏付けられた。

- ソフトウェアデリバリーのパフォーマンスが高くても、運用のパフォーマンスが高くなければ、組織のパフォーマンスにはつながらない。ユーザーの期待する信頼性を満たせないようなサービスでは、迅速なデリバリーは意味をなさないかもしれない。
- SLSA フレームワークが推奨するようなソフトウェアサプライチェーンセキュリティ管理策の導入は、継続的インテグレーションがしっかりと確立されていれば、ソフトウェアデリバリーのパフォーマンスにプラスの効果をもたらす。継続的インテグレーションが確立されていない場合、ソフトウェアデリバリーのパフォーマンスとセキュリティ管理は相反する可能性がある。
- サイト信頼性エンジニアリング（SRE）の実践が、チームの信頼性目標達成能力に与える影響は非線形である。SRE を実施しても、チームがあるレベルのSRE 成熟度に達するまでは、信頼性にプラスの影響を与えない。SRE の成熟度に達するまでは、SRE と信頼性目標達成の間に関連性は見られない。しかし、チームの SRE 採用率が高まるにつれ、SRE の利用が信頼性を強く決定づけるようになる変曲点に到達する。そして、信頼性の向上が組織のパフォーマンスに影響を与えるようになる。
- 継続的デリバリーとバージョン管理は、互いの能力を増幅し、高いレベルのソフトウェアデリバリーパフォーマンスを促進する。継続的デリバリー、疎結合アーキテクチャ、バージョン管理、継続的インテグレーションを組み合わせることで、部分の総和を超えるソフトウェアデリバリーパフォーマンスが育まれる。


結論、

**継続的な改善の必要性を認識しているチームは、そうでないチームよりも組織としてのパフォーマンスが高い傾向にある。**

## DevOpsパフォーマンスのクラスタリング

毎年使われているデプロイの頻度、変更のリードタイム、復元の平均時間、変更失敗率のfour keysと、昨年導入された5つ目の指標である信頼性を使用したクラスタリング。

昨年とは異なり、2018年から存在したEliteクラスタは見出されず、昨年のHighクラスタとEliteクラスタの中間にHighクラスタが存在する形だ。
全体としては、デリバリーパフォーマンスは向上しており、天井が下がり、底が上がったことを示唆している。

![](https://storage.googleapis.com/gweb-cloudblog-publish/images/3_DORA_2022.1000061920001104.max-2800x2800.jpg)
[2022 年の Accelerate State of DevOps Report を発表: セキュリティに焦点](https://cloud.google.com/blog/ja/products/devops-sre/dora-2022-accelerate-state-of-devops-report-now-out)より

DORAでは、COVID-19のパンデミックの影響で、チームや組織間で知識や実践を共有することが難しくなったことが原因かもしれないと予想している。互いに学び合う機会が少なくなり、その結果、プラクティス、ツール、情報共有の面でイノベーションが鈍化した可能性がある。

とはいえ、HighとLowの差は依然として大きく、HighはLowの417倍のデプロイを行っている。

|2022                  |High                                   |Medium            |Low                |
|----------------------|---------------------------------------|------------------|-------------------|
|デプロイの頻度        |オンデマンド（1日に複数回デプロイする）|週1回から月1回の間|月1回から6ヶ月に1回|
|リードタイム          |1日以上1週間以内                       |1週間から1ヶ月の間|1ヶ月以上6ヶ月未満 |
|サービス復旧の所要時間|1日未満                                |1日以上1週間未満  |1週間から1ヶ月の間 |
|変更失敗率            |0%-15%                                 |16%-30%           |46%-60%            |

## デリバリーパフォーマンスと運用パフォーマンスのクラスタリング

いつものクラスタリングに加えて、運用パフォーマンスを絡めたクラスタリングも行われた。
スループット（コード変更のリードタイム とデプロイ頻度の合成），安定性（サービス復元時間と変更失敗率の合成），運用性能（信頼性） の 3 つのカテゴリについてのクラスタリングだ。
その結果、4つのクラスターが導き出され、Starting、Folowing、Slowing、Retiringと名付けられた。
これらのクラスターは、その実践と技術的能力に顕著な違いがある。

### Starting
良い結果も悪い結果も出していない。
このクラスタは、製品、機能、またはサービスの開発の初期段階にある可能性がある。
フィードバックを得たり、製品と市場の適合性を理解したり、より一般的に探求することに重点を置いているため、信頼性にはあまり関心がないのかもしれない。

### Folowing
高信頼性、高安定性、高スループットというすべての特性において良好な結果を示している。この状態にあるのは17%に過ぎない。
他のクラスターと比較して、以下のことを重視している。

例年でもパフォーマンスに重要だと言われているものだ。

- 疎結合アーキテクチャ：他のチームのシステム変更に依存することなく、自分たちのシステムの設計に大規模な変更を加えることができる
- 柔軟性の提供：従業員の勤務形態に関して、企業がどれだけ柔軟に対応しているか
- バージョン管理：アプリケーションコード、システム構成、アプリケーション構成などの変更を管理する方法
- 継続的インテグレーション（CI）：ブランチがトランクに統合される頻度
- 継続的デリバリー（CD）: 変更を安全、持続的、かつ効率的に本番環境に取り込むことに焦点を当てた機能

不思議なことに、Flowing クラスタは、ドキュメンテーションをあまり重視しない傾向があった。
これは、ドキュメンテーションの実践が、デリバリーパフォーマンスと運用パフォーマンス（SDO）の両方の中心であるとした昨年のレポートと矛盾している。
DORAでは、コードのリファクタリングを継続的に行い、より自己流のドキュメント化プロセスを構築しているのではないかと予想している。

### Slowing
あまり頻繁にはデプロイしないが、デプロイしたときには成功する可能性が高い。回答者の3分の1以上がこのクラスターに属し、サンプルの中で最も多く、最も代表的なクラスター。
このパターンは、漸進的な改良を行っているが、彼らもその顧客もアプリケーションや製品の現状にほぼ満足しているチームの典型と考えられる。

他のクラスタよりもクラウドネイティブ度が低い傾向にある大企業の回答者で構成されている傾向がある。

### Retiring
彼らやその顧客にとってまだ価値のあるサービスやアプリケーションに取り組んでいるが、もはや活発な開発が行われていないチームのように見える。

しかし驚くべきことに、組織的パフォーマンスにおいて他のクラスターを上回った。
このクラスターの特徴（安定性の低さと処理能力の低さ）を見ると、DORAのこれまでの調査結果の多くと一見矛盾している。
DORAでは、その仮説として組織の規模、信頼性への期待の組織間の差異、短期的なパフォーマンスと長期的なパフォーマンスの違いなどを挙げ、今後も調査するとしている。

## ハイブリッドクラウドやマルチクラウドの利用は信頼性が高くない限り、ソフトウェアデリバリーのパフォーマンス指標であるMTTR、リードタイム、デプロイ頻度にマイナスの影響を与える

ハイブリッドクラウドやマルチクラウドの利用では、信頼性が高い場合、組織のパフォーマンスが1.4倍高くなることが示された。
しかし、ハイブリッドクラウドやマルチクラウド（プライベートクラウドも含む）の利用は、信頼性が高くない限り、いくつかのソフトウェアデリバリーのパフォーマンス指標（MTTR、リードタイム、デプロイ頻度）にマイナスの影響を与えるようだ。この発見は、強固なSREの実践と、ソフトウェアデリバリーにおいて信頼性が果たす役割の重要性をさらに物語っている。

## SREとDevOps
SREの導入は調査対象のチームに広く浸透しており、回答者の過半数が、今回質問した手法のうち1つまたは複数を利用していた。
信頼性が低い場合、ソフトウェアデリバリパフォーマンスは組織の成功の予測因子にならない。しかし，信頼性が向上すると，ソフトウェアデリバリーがビジネスの成功にプラスの影響を与えることが分かってきた。

この現象は、SREの「エラーバジェット」フレームワークの使用と一致する。サービスが信頼できない場合、コードを高速にプッシュしても、ユーザーは利益を得られない。

SREが長い間主張してきたように、信頼性はあらゆる製品の最も重要な「機能」である。
DORAの研究は、ユーザーとの約束を守ることが、ソフトウェアデリバリーを改善し、組織に利益をもたらすための必要条件であるという観察を支持している。

### Jカーブを認識する

2018年のState of DevOps Reportで紹介された「Jカーブ」は、組織の変革が初期に成功を収め、その後リターンが減少する時期、あるいは後退する傾向がある現象である。しかし、このような試練を乗り越えた人々は、多くの場合、新たなレベルの高い成果を持続的に経験する。

信頼性エンジニアリングの実践が少ない場合（SRE導入の初期段階であることを示唆）、これらの実践は信頼性の成果につながらない。しかし、SREの導入が進むにつれて、SREの活用が信頼性、ひいては組織のパフォーマンスを強く決定づけるようになる変曲点に到達する。

### 人、プロセス、ツールへの投資

信頼性とは人間の努力によるものであり、SREのアプローチは多くの点でこれを例証している。
SREの基本原則の1つは、内部監視データとは対照的に、ユーザーの認識こそが信頼性を測る真の尺度であるということである。
また、DevOps全体と同様に、信頼性エンジニアリングの取り組みには、プロセスやツールによって人的努力を補強することが有効である。

## トランクベース開発は経験が重要

トランクベース開発とは、コードを継続的にトランクにマージし、長寿命のフィーチャーブランチを避けるという手法である。
この実践は、継続的インテグレーションを補完するものと考えられており、ソフトウェアのデリバリー速度を加速することが長年にわたって示されてきた。

今年は、職務経験年数に関する層が変化し経験が重要であることが示された。昨年は、回答者の40%が16年以上勤務していると答えたが、今年は13%にとどまった。全体的に経験の浅い人は、トランクベース開発に関してあまり良い結果を出していないことがわかった。

▼ ソフトウェアデリバリーの全体的なパフォーマンスの低下

▲ 予定外の作業の増加 

▲ エラーが発生しやすくなる

▲ 変更の失敗率の増加

トランクベースの開発を使用している16年以上の経験を持つ個人は、その実践のメリットを実感している。

▲ソフトウェアデリバリーの全体的なパフォーマンスの向上 

▼予定外の作業の減少

▼エラーの発生しやすさの減少

▼変更の失敗率の減少


DORAの見解は、トランクベースの開発を成功させるために必要な追加のプラクティスによるもの。
壊れたトランクから決して離れないというルールを厳格に実施しないチームや、ゲート付きのコードブランチやトランクを破壊するコードを自動ロールバックしないチームは、トランクで開発しようとすると必ず痛みを経験することになる。

## 高業績の組織内にはチーム構成があまり変化していない安定したチームが存在する

チームの離職率について調査し、過去12ヶ月間にチーム構成があまり変化していない安定したチームが、高業績の組織内に存在する可能性が高いことが発見された。チームの入れ替わりが激しいと、新しいメンバーが定着するまでに時間がかかるため、生産性や士気に影響を与える可能性がある。

## 業績の良い組織ほど、柔軟な勤務形態をとっている。

COVID-19の大流行以降、多くの組織が柔軟な勤務形態を採用していることから、従業員に遠隔、対面、ハイブリッドなどの選択肢を自由に与えることが、組織業績の向上と関連するかどうかを調査した。その結果、柔軟性が高い組織は、より厳格な勤務体系を持つ組織と比較して、より高い組織パフォーマンスを持つことが示された。
これらの結果は、従業員に必要に応じて勤務形態を変更する自由を与えることは、組織にとって具体的かつ直接的な利益をもたらすという証拠を示している。

## いくつかの驚くべき結果

今年の結果はいくつか驚くべきものがあるが、その要因としてDORAは
- 今年の調査対象者が、これまでの報告書よりもキャリアが浅い人を多く含んでいる
- マクロ経済の影響や、COVID-19の大流行などの世界の変化

をあげ、6つの結果を示している。

1. DORAの調査では一貫して、トランクベースの開発プラクティスがソフトウェアのデリバリーパフォーマンスにプラスの影響を与えることを観察してきた。実際、これは2014年以降の調査のすべての年で観察されている。しかし、今年はソフトウェアデリバリーパフォーマンスに(条件付き)マイナスの影響を与えた。今後の研究で再現されるかどうかに注目。
2. ソフトウェアデリバリーパフォーマンスが組織パフォーマンスに有益なのは、運用パフォーマンスも高い場合のみであり、多くの回答者は運用パフォーマンスが高くないということがわかった。これは、ソフトウェアデリバリーパフォーマンスと組織パフォーマンスの関係がより明確であった、DORAの研究の以前の反復と矛盾している。
3. ドキュメンテーションの実践は、ソフトウェアデリバリーのパフォーマンスにマイナスの影響を与えた。これは，これまでの報告とは異なるものである．1つの仮説は、特に高パフォーマンスのチームにおいて、ドキュメンテーションの自動化が進んでいることである。しかし，この仮説を支持または反証できるようなデータは，まだない。
4. いくつかの技術的能力（トランクベース開発、疎結合アーキテクチャ、CI、CDなど）は、燃え尽き症候群の予測因子になるようであった。 上記のように、今回の回答者の多くは、前年度の参加者に比べてキャリアが著しく浅かった。したがって、イニシアチブの策定や監督を担当する人ではなく、能力の実装を担当する人に話を聞いていたのかもしれない。実施プロセスは、その監督よりもはるかに困難である可能性がある。
5. 信頼性エンジニアリングの実践は、ソフトウェアのデリバリー性能にマイナスの影響を与えた。1つの説明として、これらは必ずしも因果関係があるわけではない。今年、新しいクラスタリング分析で、あるクラスタのサブセットが信頼性を重視し、ソフトウェアデリバリーパフォーマンスを無視しているように見えることに気づいた。DORAでは、これらは切り離されていると考えている。つまり、一方を実行しなくても他方を実行することはできるが、最終的には、ソフトウェアデリバリーパフォーマンスを組織のパフォーマンスとして評価するには、信頼性が確保されている必要がある。
6.  SLSA に関連する実施内容を追加し、安全なソフトウエアサプライチェーンを維持するためにチームがこれらのアプローチを採用しているかどうかを把握した。セキュリティ対策の実施とパフォーマンス（例えば、技術的能力の使用、より良いソフトウェアデリバリーパフォーマンス、より良い組織的パフォーマンス）の間には何らかの関連があると予想していたが、実際には、セキュリティ対策が技術的能力を通じてソフトウェアデリバリーパフォーマンスと組織的パフォーマンスに影響を与えるメカニズムであることがわかった。

## 参考文献

* [State of DevOps Report](https://www.devops-research.com/research.html#reports)
* [2022 年の Accelerate State of DevOps Report を発表: セキュリティに焦点](https://cloud.google.com/blog/ja/products/devops-sre/dora-2022-accelerate-state-of-devops-report-now-out)
