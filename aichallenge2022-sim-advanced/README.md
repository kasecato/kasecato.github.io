# Japan Automotive AI Challenge
## 2022 (Simulation) Competition
## Advanced Course - 3rd Place - YutakaJCT

<img style="border-radius: 50% !important;" src="img/twitter.jpg" width="180px" alt="Keisuke KATO" />

### [Japan Automotive AI Challenge - Awards Ceremony](https://www.jsae.or.jp/jaaic/en/summary.php)
### @[Tokyo Culture Culture](https://tokyocultureculture.com/)

March 6, 2023

Note:
自動運転 AI Challenge アドバンスト コース 3 位に入賞しましたチーム YutakaJCT と申します。
本日は学びや交流を深めることができる貴重な機会をいただき，ありがとうございます。
よろしくおねがいします。



# About YutakaJCT

|                 |                                                   |
| --------------- | ------------------------------------------------- |
| Member          | (1) Keisuke Kato / 加藤 圭佑                      |
| Company         | LINE Fukuoka                                      |
| Work Experience | Server-side Engineer, Wallet                      |
| Education       | CS degree                                         |
| From            | Fukuoka                                           |
| Team Name       | Yutaka JCT / 豊JCT that don't curve well with ACC |
| My Car          | Peugeot e-208 GT Line FWD, 50kWh, 260Nm, 100kW    |
| Love Car        | AE86 H2 Concept, Honda e, Model 2, ID.2           |
| Best Car        | MIRAI 2nd Gen.                                    |

Note:
チーム YutakaJCT のメンバで「カトウ ケイスケ」と申します。チームメンバは私のみとなっています。

普段は LINE Fukuoka という会社でサーバサイドのエンジニアをしています。
LINE を開くと一番右下に「ウォレット」というタブがあります。
こちらに表示しているすべてのコンテンツをサーバ間の通信で高速に組み立てる仕事をしています。
スマートフォンからの大規模なアクセスを処理する Web サービスのサーバサイドを設計・開発しています。

このように普段は自動運転とは縁のない仕事をしています。
出身も情報の学士でいわゆる機械や電気などは素人です。情報は情報でも最近注目されている AI とはとても遠い職務で専門ではありません。

現在は会社名のとおり福岡に住んでいます。
YutakaJCT というチーム名も福岡の都市高速道路という，こちらの首都高速道路のような環状線にあるジャンクション名から取っています。
なぜチーム名がジャンクション名なのかですが，子どもが生まれて 2021 年に初めて購入した車の ACC を使って豊ジャンクション前の左カーブが曲がれなかったことに「なぜソフトウェアで曲がれなかったのか」という疑問から自動運転に興味を持ちました。

最近は特に電動車に興味があり，AE86 H2 や Honda e，Model 2 や ID.2 などコンパクトな車が好きです。大きくて私の家では乗れないのですが試乗して一番楽しかった車は MIRAI 2 代目です。



# What We Did?

1. No Code
   * Velocity Smoothing
   * Avoidance
   * Adoptive Cruise
   * Obstacle Stop
1. Off Topic
   * Buy Memory
   * Use GCP vGPU Instance

Note:
私のチームの一番の特徴はアドバンスト コースに参加したチームの中で Autoware に搭載された機能のみで参加している点と考えています。

意外に思われるかもしれませんが C++ や Python で ROS 2 のコーディングはしておらず，Autoware の既存機能を調整する No Code でアドバンスト コースに参加しました。
おそらくいくつかのモジュールは本来の意図とは違う使われ方をしてしまっていると思いますが，この分野は素人ですのでご了承ください。

それでは Autoware の既存機能をどのように使ったのか，順番に工夫した点を絞って解説します。



# (Our No Code) Architecture

![](img/autoware-architecture-overview.drawio.svg)

Input = LiDAR + Map + IMU (No Camera)

Note:
まず大会で使用した Autoware の概要ですが，LiDAR，GNSS やカメラなどの Sensing の入力と，高精度３次元地図データを組み合わせて環境を認識し，認識した情報から経路を Planning して，出力として車両を Control するようです。

私のチームでは初期設定の状態のままで入力は LiDAR，マップと IMU のみを使用し，自己位置推定 (Localization)，環境認識 (Perception) やセンサ フィージョンなども初期設定のままです。今回は Planning の中でも Lane Following のパラメータ調整のみ行っただけとなっています。



# Lane Following in Planning

|                                                   |                                          |                                                |                                              |
| ------------------------------------------------- | ---------------------------------------- | ---------------------------------------------- | -------------------------------------------- |
| <img src="img/planning/velocity_smoothing.svg" /> | <img src="img/planning/avoidance.svg" /> | <img src="img/planning/adoptive_cruise.svg" /> | <img src="img/planning/obstacle_stop.svg" /> |

Note:
具体的に今回のアドバンスト コースで調整した Planning の設定についてですが，主に 4 つの Lane Following モジュールのパラメータを調整しています。

その中でも今回は速度調整 (Velocity Smoothing) と障害物回避 (Avoidance) の 2 つのモジュールの設定に絞って解説したいと思います。



## Velocity Smoothing

|                                                                 | As-Is                                                            | To-Be                                                            |
| --------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| <img src="img/planning/velocity_smoothing.svg" height="420px"/> | <img src="img/velocity/velocity_limit_asis.png" width='420px' /> | <img src="img/velocity/velocity_limit_tobe.png" width='420px' /> |

| motion_velocity_smoother.param.yaml | As-Is    | To-Be       |
| ----------------------------------- | -------- | ----------- |
| max_velocity                        | 20.0 m/s | 12.9722 m/s |

Note:
まずは速度調整についてです。

12 月にチュートリアルが公開された当時の最初の大きな課題は，Autoware のパラメータの変更方法がわからず，どこから手を付けてよいのか全くわからないことでした。

Autoware を起動すると Rviz2 に最高速度が 72km/h と表示されたので，まずは「72」を書き換えることができる設定ファイルを探すことにしました。
公式のドキュメントを「72」で検索しても見つからず，Autoware のコンテナに接続して「72」で検索をするのですが設定ファイルは見つかりませんでした。

そこで「max_velocity」でドキュメントを検索してみると「Motion Velocity Smoother」にあることがわかりました。
max_velocity の設定は 20.0m/s で km/h に変換するには x3.6 が必要で，この単位の感覚が情報出身の自分にはないことに気づいたことが新鮮な体験でした。

Autoware のコンテナから Motion Velocity Smoother を含む launch/tier4_planning_launch をローカルにコピーして，max_velocity を書き換えてビルドし起動してみたところ画面上の最高速度が書き換わったためパラメータの変更方法を見つけることができました。

大会のルールに最高速度制限があるため，46.7km/h に調整しました。直線で速度超過しないようにマージンを入れました。



## Avoidance

|                                                        | As-Is                                                        | To-Be                                                        |
| ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="img/planning/avoidance.svg" height="420px"/> | <img src="img/avoidance/avoidance_asis.png" width='500px' /> | <img src="img/avoidance/avoidance_tobe.png" width='520px' /> |

Note:
次に障害物回避です。

アドバンスト コースでは複数車両が駐停車していて追い越せないシナリオがあります。
デフォルトの設定ではトラックの後ろで停車し続けてしまいます。

狭い車両の間を進むように経路計画を改善する必要がありました。



## Avoidance - Center

<img src="img/avoidance/target_vehicle_selection.drawio.svg" width="1200px">

| avoidance.param.yaml                   | As-Is | To-Be | Ideal  |
| -------------------------------------- | ----- | ----- | ------ |
| threshold_distance_object_is_on_center | 1.0 m | 0.0 m | Hazard |

Note:
Autoware の障害物回避モジュールのドキュメントを機械翻訳機を使って読んでみると，車線のセンタにある車両は回避の対象外となっています。

これは一般的に車線のセンタで停車している車両は信号待ちの可能性があり，信号停車中の車両を自車両が追い越しさせない設定のようです。
人が車を運転するときは信号待ちの車を回避したりはしないように，とても自然に判断していると思います。

今回の大会では信号の制約がなくシミュレータを使うため失敗しても大丈夫なので，試しにセンタ設定をオフにしてみます。
この設定で車線のセンタで停車中のトラックを追い越し対象のオブジェクトして計算するようになりました。

しかしこの設定だけでは，狭いスペースを進むように経路計画は作られませんでした。
さらにシナリオ次第では信号停車の車両を追い越し運転する副作用があるのであまり良い設定ではありません。

本来であればハザードを認識する新しいモジュールを実装することが応用的なアプローチと思われます。



## Avoidance - Debug

Echoing debug message to find out why the objects were ignored.

```bash
$ ros2 topic echo /planning/scenario_planning/lane_driving\
/behavior_planning/behavior_path_planner/debug/avoidance_debug_message_array
```

```bash
avoidance_info:
- object_id: e2cd8ce7a53efe3dbcfd545046c0e830
  allow_avoidance: false
  longitudinal_distance: 19.933871635857543
  lateral_distance_from_centerline: -0.021392588522659205
  to_furthest_linestring_distance: 2.8958805756534582
  max_shift_length: 1.4478805756534583
  required_jerk: 0.0
  maximum_jerk: 0.0
  failed_reason: InsufficientLateralMargin
```

Note:
ドキュメントを詳細に読むと，障害物が回避の対象外と判定された理由をコンソールに出力してデバッグできることがわかりました。
デバッグをしてみると「InsufficientLateralMargin」と表示され，シフトするための横方向の間隔が狭いことがわかりました。

このように if-then-else 式のプログラムでは計算結果の理由が明確に出せることが非常に良い点だと思いました。



## Avoidance - Collision Margin

![](img/avoidance/shift_length_parameters.drawio.svg)

| avoidance.param.yaml            | As-Is | To-Be | Ideal |
| ------------------------------- | ----- | ----- | ----- |
| lateral_collision_margin        | 1.0 m | 0.1 m | 0.5m  |
| lateral_collision_safety_buffer | 0.7 m | 0.1 m | 0.4m  |
| road_shoulder_safety_margin     | 0.5 m | 0.1 m | 0.25m |

Note:
Autoware のコンテナに接続して「InsufficientLateralMargin」を検索すると，ソースコードに図の計算式が書かれていました。
ドキュメントに記載された障害物回避のシフト計算に使用するパラメータとコードが一致していたため，試しに狭い道路でもシフト計算できるようように衝突判定の値を小さく設定しました。

10cm 級の設定で障害物を回避する車には実際には乗りたくはありませんが，あくまで大会用のシミュレータで設定を事前に確認できるのはとても良い体験でした。

ただしこの衝突間隔を小さくした障害物回避の設定は他のシナリオに副作用を与えるため，他のモジュールの設定も微調整が必要となります。

このように一つのモジュールの変更による出力の変化が，他のモジュールの入力に影響を与えてしまうためシミュレータを使ったリグレッション テスト（回帰テスト）は非常に重要だと感じました。
ソフトウェアでシナリオのテストを繰り返すことで，ソフトウェアが鍛えられて変更にも強くなると思いました。

以上が調整した点の解説でした。



## Off Topic - Buy Memory

|        | Required for 2022    | My Laptop                     |
| ------ | -------------------- | ----------------------------- |
| CPU    | Core i7 (8 cores)    | Ryzen 7 5800H (8 cores) (45W) |
| GPU    | RTX 3080 12GB (350W) | RTX 3080 8GB (150W)           |
| Memory | 32GB                 | 16GB (Crashes) -> 40GB        |

<img src="img/memory.jpeg" width="500px" />

Note:
脱線ですが，環境構築で苦労した点についてですが，最初は AWSIM と Autoware が安定して起動ができないことがありました。

これは私のノート PC はメモリが 16GB だったので大会用の AWSIM が 5GiB と Autoware が 10GiB を瞬間的に専有することで Linux の OOM (Out-of-Memory) Killer が発動して最悪 AWSIM が強制終了されていたからでした。

大会用に 32GB のメモリを購入してからは安定的に AWSIM と Autoware が起動するようになりました。



## Off Topic - Use GCP vGPU

|              |                       |
| ------------ | --------------------- |
| GCP Zone     | us-west1-a            |
| Machine type | n1-standard-16        |
| GPUs         | 1 x NVIDIA Tesla P100 |

<img src="img/gcp.png" width="800px" />

Note:
大会が終わったあとですが，高価な PC がなくてもクラウドでも AWSIM を動かせるのではと思い試してみました。

Google Cloud Platform で試したところ，この性能であれば十分に動かすことができました。
ただしグラフィックスをストレスなく扱うために PCoIP といったリモート デスクトップのプロトコルが個人で扱い難いライセンスでネックですが，PC を購入するより素早く試すことが可能だと思いました。

LINE Fukuoka では毎年社内でハッカソンを開催しているため，今年は 2022 のシミュレーション大会を参考にしてハッカソンに参加したいと思っています。



# Thank you

Note:
ありがとうございました。



# References

- aichallenge2022-sim, https://github.com/AutomotiveAIChallenge/aichallenge2022-sim/blob/main/README_en.md
- Architecture overview, https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-architecture/
- Planning, https://autowarefoundation.github.io/autoware-documentation/main/design/autoware-architecture/planning/
- Motion Velocity Smoother, https://autowarefoundation.github.io/autoware.universe/main/planning/motion_velocity_smoother/
- Avoidance Module, https://autowarefoundation.github.io/autoware.universe/main/planning/behavior_path_planner/behavior_path_planner_avoidance-design/
- GCP NVIDIA P100 GPUs , https://cloud.google.com/compute/docs/gpus?hl=en#nvidia_p100_gpus
- PCoIP, https://cloud.google.com/architecture/creating-a-virtual-gpu-accelerated-linux-workstation?hl=en
- LINE Fukuoka Hackathon, https://engineering.linecorp.com/ja/blog/line-fukuoka-developers-remote-hackathon
