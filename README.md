# memo2

# Azure ML用ライブラリのインポート
from azure.ai.ml import MLClient
from azure.identity import DefaultAzureCredential

# ワークスペースへの接続設定 (ご自身の情報に置き換え)
subscription_id = "あなたのAzureサブスクリプションID"
resource_group = "あなたのリソースグループ名"
workspace_name = "あなたのMLワークスペース名"

ml_client = MLClient(
    credential=DefaultAzureCredential(),
    subscription_id=subscription_id,
    resource_group_name=resource_group,
    workspace_name=workspace_name
)

# データストアへのアクセス
from azure.ai.ml.entities import Data
from azure.ai.ml.constants import AssetTypes

# パスは同僚が指定したもの
datastore_path = "azureml://datastores/synw-ccbji0003-com/paths/project/sakaya_corr/data/raw/"

# Azure ML上のデータセット（データ資産）として定義
my_data = Data(
    path=datastore_path,
    type=AssetTypes.URI_FOLDER,
    description="同僚が置いたデータ",
    name="my_raw_data"
)

# データセットをワークスペースに登録（任意ですが、よくやる操作）
ml_client.data.create_or_update(my_data)

# Notebookから直接ファイルにアクセスして確認する方法
# 以下はPandasなどを使った例（CSVの場合）
import pandas as pd
from azure.ai.ml import command

# CSVファイルのパス例（ファイル名を指定）
csv_file_path = f"{datastore_path}ファイル名.csv"

# 直接読み込み
df = pd.read_csv(csv_file_path)

# データ確認
df.head()
