https://upura.hatenablog.com/entry/2018/12/28/225234#notebook +
を参考にフォルダを構成 +
以下説明も上記サイトの説明を参考にしている +

== configs
jsonファイルで、諸設定を記載

記載している情報は「利用している特徴量」「学習器のパラメータ」など
....
{
    "features": [
        "age",
        "embarked",
        "family_size",
        "fare",
        "pclass",
        "sex"
    ],
    "lgbm_params": {
        "learning_rate": 0.1,
        "num_leaves": 8,
        "boosting_type": "gbdt",
        "colsample_bytree": 0.65,
        "reg_alpha": 1,
        "reg_lambda": 1,
        "objective": "multiclass",
        "num_class": 2
    },
    "loss": "multi_logloss",
    "target_name": "Survived",
    "ID_name": "PassengerId"
}
....
コンペのデータに依存するカラム名なども、このjsonファイルから読み取る

== data
dataフォルダは、input/outputに分けている

=== input
inputフォルダには、元データのcsvファイルや、feather形式に変換したファイルなどを配置

=== output
outputフォルダには、提出用のcsvファイルを出力 +
ファイル名は「sub_(year-month-day-hour-min)_(score)」のように設定し、後述するログと照合できるようにしている

== features
featuresフォルダには、train/testから作成した各特徴量を保存 +

base.pyなど、詳細については以下を参照 +
https://amalog.hateblo.jp/entry/kaggle-feature-management

=== importances
特徴量の重要度を出力

== logs
logsフォルダには、計算の実行ごとに下記の情報などを出力
ファイル名は「log_(year-month-day-hour-min).log」のように設定し、前述した通り提出用のcsvファイルと照合できるようにしている

* 利用した特徴量
* trainのshape
* 学習器のパラメータ
* cvのスコア
LightGBMのログ出力は以下を参照 +
https://amalog.hateblo.jp/entry/lightgbm-logging-callback +
https://icebee.hatenablog.com/entry/2018/12/16/221533

== models
modelフォルダには、学習器を用意 +
下記のように汎用的に作ることで、別のコンペでも使い回せることを意識

* 入力：pandas.DataFrame、パラメータ
* 出力：予測結果

== notebook
notebookフォルダには、探索的データ分析などで利用したJupyter Notebookを配置 +
ここで試行錯誤した結果を、適切なフォルダ内のpythonファイルに取り込む

== scripts
scriptsフォルダには、汎用的なpythonファイルを配置 +
例えば convert_to_feather.py ファイルは、csvファイルをfeather形式のファイルに変換

....
import pandas as pd

target = [
    'train',
    'test',
]

extension = 'csv'

for t in target:
    (pd.read_csv('./data/input/' + t + '.' + extension, encoding="utf-8"))\
        .to_feather('./data/input/' + t + '.feather')
....

== utils
utilsフォルダには、汎用的に使える関数を書く

== 計算の実行
リポジトリのルートで以下を実行 +

run.pyは、コンペに応じて自由に記述 +
このrun.pyから、汎用的に書いた各フォルダの要素を取り出してくる
....
python run.py
....
