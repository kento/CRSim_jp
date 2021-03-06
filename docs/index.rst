概要
~~~~

(ア) simulate_cr 関数：

マルチレベルC/R をシミュレーションし、その結果を出力します。

(イ) optimize_cr 関数：

simulate_cr 関数の efficiency を最大化する、interval、L2_freq を見つけ出し、その時のシミュレーション結果を出力します。

開発詳細
~~~~~~~~

simulate_cr 関数
~~~~~~~~~~~~~~~~

tuple simulate_cr(interval, L2ckpt_freq, L1ckpt_overhead, L2ckpt_latency, ckptRestartTimes, failRates, N, SN, G, g, alpha, check_interval, n_check_ok, n_failure_max, efficiency_log)

・引数（入力）

+----------------------+-----------------------------+---------+--------------+
| **引数名**           | **説明**                    | **型**  |              |
+======================+=============================+=========+==============+
| **Interval**         | L1 チェックポイントの間隔   | int     | 必須         |
+----------------------+-----------------------------+---------+--------------+
| **L2ckpt_freq**      | L2 チェックポイントの頻度   | int     | 必須         |
+----------------------+-----------------------------+---------+--------------+
| **L1ckpt_overhead**  | 同期 L1                     | int     | 必須         |
|  | チェックポイントの時間      |         |              |
+----------------------+-----------------------------+---------+--------------+
| **L2ckpt_latency**   | 非同期L2                    | int     | 必須         |
|                      | チェックポイントの時間      |         |              |
+----------------------+-----------------------------+---------+--------------+
| **ckptRestartTimes** | L1,L2                       | List    | 必須         |
|                      | リカバリ                    |         |              |
|                      | に要する時間を格納した長さ  | [i      |              |
|                      | 2 の配列                    | nt,int] |              |
|                      |                             |         |              |
|                      | = [L1 リカバリ時間,L2       |         |              |
|                      | リカバリ時間]               |         |              |
+----------------------+-----------------------------+---------+--------------+
| **failRates**        | L1,L2                       | List    | 必須         |
|                      | リカバリを                  | [float  |              |
|                      | 必要とする障害の単位時間に  | ,float] |              |
|                      | 発生する回数を格納した長さ  |         |              |
|                      | 2 の配列 = [L1 障害回       |         |              |
|                      |                             |         |              |
|                      | 数,L2 障害回数]             |         |              |
+----------------------+-----------------------------+---------+--------------+
| **N**                | 全計算ノード数              | int     | 必須         |
+----------------------+-----------------------------+---------+--------------+
| **SN**               | 予備ノード数                | int     | 必須         |
|                      | ※仕様追加されたパラメータ   |         |              |
+----------------------+-----------------------------+---------+--------------+
| **G**                | L1                          | int     | 必須         |
|                      | チェッ                      |         |              |
|                      | クポイントのグループサイズ  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **g**                | L1                          | int     | 必須         |
|                      | チェックポイントの障害耐性  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **alpha**            | シミュレーション終了とする  | float   | 必須         |
|                      | Efficiency の変化量の閾値   |         |              |
+----------------------+-----------------------------+---------+--------------+
| **check_interval**   | Efficiency                  | int     | 省略可       |
|                      | の変化量チェックの頻度      |         | 、Default=1  |
|                      | ※仕様追加されたパラメータ   |         |              |
+----------------------+-----------------------------+---------+--------------+
| **n_check_ok**       | Efficiency                  | int     | 省略可       |
|                      | の変化量チェ                |         | 、Default=1  |
|                      | ックで終了と判定されるため  |         |              |
|                      | の連続回数                  |         |              |
|                      | ※仕様追加されたパラメータ  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **n_failure_max**    | 最大障害発生回数            | int     | 省略可、De   |
|                      | ※仕様追加されたパラメータ   |         | fault=500000 |
+----------------------+-----------------------------+---------+--------------+
| **efficiency_log**   | Efficiency                  | bool    | 省略可、D    |
|                      | 変化量チェ                  |         | efault=False |
|                      | ックの履歴出力のオン・オフ  |         |              |
|                      |                             |         |              |
|                      | ※仕様追加されたパラメータ  |         |              |
+----------------------+-----------------------------+---------+--------------+

返り値（出力）：tuple 型データ=(X,A,B,C,D,E,F)

+------------+-------------------------------------------+----------------+
| **引数名** | **説明**                                  |    **型**      |
+============+===========================================+================+
| **X**      | Efficiency = A/(B+C+D+F)                  |    float       |
+------------+-------------------------------------------+----------------+
| **A**      | 実質計算時間                              |    float       |
+------------+-------------------------------------------+----------------+
| **B**      | 計算状態に費やした時間                    |    float       |
+------------+-------------------------------------------+----------------+
| **C**      | L1 チェックポイントに費やした時間         |    float       |
+------------+-------------------------------------------+----------------+
| **D**      | L1 リカバリに費やした時間                 |    float       |
+------------+-------------------------------------------+----------------+
| **E**      | L2 チェックポイントに費やした時間         |    float       |
+------------+-------------------------------------------+----------------+
| **F**      | L2 リカバリに費やした時間                 |    float       |
+------------+-------------------------------------------+----------------+

optimize_cr 関数
~~~~~~~~~~~~~~~~

tuple optimize_cr (L1ckpt_overhead, L2ckpt_latency, ckptRestartTimes, failRates, N, SN, G, g, alpha, check_interval, n_check_ok, n_failure_max, n_steps, log_interval)

.. _引数入力-1:

・引数（入力）

+----------------------+-----------------------------+---------+--------------+
| **引数名**           | **説明**                    | **型**  |              |
+======================+=============================+=========+==============+
| **L1ckpt_overhead**  | 同期 L1                     | int     | 必須         |
|                      | チェックポイントの時間      |         |              |
+----------------------+-----------------------------+---------+--------------+
| **L2ckpt_latency**   | 非同期L2                    | int     | 必須         |
|                      | チェックポイントの時間      |         |              |
+----------------------+-----------------------------+---------+--------------+
| **ckptRestartTimes** | L1,L2                       | List    | 必須         |
|                      | リカバリ                    |         |              |
|                      | に要する時間を格納した長さ  | [i      |              |
|                      | 2 の配列                    | nt,int] |              |
|                      |                             |         |              |
|                      | = [L1 リカバリ時間,L2       |         |              |
|                      | リカバリ時間]               |         |              |
+----------------------+-----------------------------+---------+--------------+
| **failRates**        | L1,L2                       | List    | 必須         |
|                      | リカバリを                  | [float  |              |
|                      | 必要とする障害の単位時間に  | ,float] |              |
|                      | 発生する回数を格納した長さ  |         |              |
|                      | 2 の配列 = [L1 障害回       |         |              |
|                      |                             |         |              |
|                      | 数,L2 障害回数]             |         |              |
+----------------------+-----------------------------+---------+--------------+
| **N**                | 全計算ノード数              | int     | 必須         |
+----------------------+-----------------------------+---------+--------------+
| **SN**               | 予備ノード数                | int     | 必須         |
|                      | ※仕様追加されたパラメータ  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **G**                | L1                          | int     | 必須         |
|                      | チェッ                      |         |              |
|                      | クポイントのグループサイズ  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **g**                | L1                          | int     | 必須         |
|                      | チェックポイントの障害耐性  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **alpha**            | シミュレーション終了とする  | float   | 必須         |
|                      | Efficiency の変化量の閾値   |         |              |
+----------------------+-----------------------------+---------+--------------+
| **check_interval**   | Efficiency                  | int     | 省略可       |
|                      | の変化量チェックの頻度      |         | 、Default=1  |
|                      | ※仕様追加されたパラメータ  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **n_check_ok**       | Efficiency                  | int     | 省略可       |
|                      | の変化量チェ                |         | 、Default=1  |
|                      | ックで終了と判定されるため  |         |              |
|                      |                             |         |              |
|                      | の連続回数                  |         |              |
|                      | ※仕様追加されたパラメータ  |         |              |
+----------------------+-----------------------------+---------+--------------+
| **n_failure_max**    | 最大障害発生回数            | int     | 省略可、De   |
|                      | ※仕様追加されたパラメータ  |         | fault=500000 |
+----------------------+-----------------------------+---------+--------------+
| **n_steps**          | 最適化の反復回数            | int     | 省略可、     |
|                      | ※仕様追加されたパラメータ  |         | Default=5000 |
+----------------------+-----------------------------+---------+--------------+
| **log_interval**     | 最適化のログ出力間隔、0     | int     | 省略可、     |
|                      | とすると出力なし            |         | Default=100  |
|                      | ※仕様追加されたパラメータ  |         |              |
+----------------------+-----------------------------+---------+--------------+

・返り値（出力）：tuple 型データ=(X,A,B,C,D,E,F, interval, L2ckpt_freq)

+-----------------+------------------------------------------------+--------+
| **引数名**      | **説明**                                       | **型** |
+=================+================================================+========+
| **X**           | 最適化結果の interval, L2ckpt_freq 時の        |  float |
|                 | Efficiency = A/(B+C+D+F)                       |        |
+-----------------+------------------------------------------------+--------+
| **A**           | 最適化結果の interval, L2ckpt_freq             |  float |
|                 | 時の実質計算時間                               |        |
+-----------------+------------------------------------------------+--------+
| **B**           | 最適化結果の interval, L2ckpt_freq             |  float |
|                 | 時の計算状態に費やした時間                     |        |
+-----------------+------------------------------------------------+--------+
| **C**           | 最適化結果の interval, L2ckpt_freq 時の L1     |  float |
|                 | チェックポイントに費やした時間                 |        |
+-----------------+------------------------------------------------+--------+
| **D**           | 最適化結果の interval, L2ckpt_freq 時の L1     |  float |
|                 | リカバリに費やした時間                         |        |
+-----------------+------------------------------------------------+--------+
| **E**           | 最適化結果の interval, L2ckpt_freq 時の L2     |  float |
|                 | チェックポイントに費やした時間                 |        |
+-----------------+------------------------------------------------+--------+
| **F**           | 最適化結果の interval, L2ckpt_freq 時の L2     |  float |
|                 | リカバリに費やした時間                         |        |
+-----------------+------------------------------------------------+--------+
| **interval**    | 最適化結果のL1 チェックポイントの間隔          |  int   |
+-----------------+------------------------------------------------+--------+
| **L2ckpt_freq** | 最適化結果のL2 チェックポイントの頻度          |  int   |
+-----------------+------------------------------------------------+--------+

最適化手法について
~~~~~~~~~~~~~~~~~~

最適化手法には、焼きなまし法を採用しました。

初期状態
========

下記のinterval、L2_freq_freq 組み合わせ（24 通り）の内、最も Efficiency の高いものを初期状態とするよう実装しました。

interval = 1000, 2500, 5000, 8000, 12000, 24000

L2_freq_freq = 1, 2, 5, 10

状態遷移
========

状態遷移については、下記の４つの方法を検討しました。

方法 1：
１．interval と L2ckpt_freq のどちらの数値を変えるかをランダムに選択２．選択されたパラメータを 2％増減

方法 2：
１．interval と L2ckpt_freq のどちらの数値を変えるかをランダムに選択２．選択されたパラメータを 5％以内のランダムな値で増減

方法 3：
１．interval と L2ckpt_freq の両方を 0～5％以内のランダムな値で増減

方法 4：
１．interval と L2ckpt_freq のどちらの数値を変えるかをランダムに選択２．選択されたパラメータを固定値で増減

検討の結果、方法 4（※）以外は、どれもあまり差が見られなかったため、方法 1 を採用。

※interval は範囲が広いため、固定値で増減する場合、小さい値にすると範囲内の移動に回数が掛かりすぎ、大きい値にすると小さい側で変化量が大きくなりすぎる問題が発生しました。

上記の状態遷移の方法は、簡単なソースコード修正で、上記いずれの方法にも変更できるようにしていますの で、必要に応じて修正してご利用ください。また、2％や 5％の数字もソースコードの対応箇所の変更のみで変更可能です。
