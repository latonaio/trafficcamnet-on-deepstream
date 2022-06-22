# trafficcamnet-on-deepstream
trafficcamnet-on-deepstream は、DeepStream 上で TrafficCamNet の AIモデル を動作させるマイクロサービスです。  

## 動作環境
- NVIDIA 
    - DeepStream
- TrafficCamNet
- Docker
- TensorRT Runtime

## TrafficCamNetについて
TrafficCamNet は、画像内の車、人、道路標識、および二輪車を検出し、カテゴリラベルを返すAIモデルです。
TrafficCamNet は、特徴抽出にResNet18を使用しており、混雑した場所でも正確に物体検出を行うことができます。

## 動作手順
### Dockerコンテナの起動
Makefile に記載された以下のコマンドにより、TrafficCamNet の Dockerコンテナ を起動します。
```
docker-run: 
	docker-compose -f docker-compose.yaml up -d
```
### ストリーミングの開始
Makefile に記載された以下のコマンドにより、DeepStream 上の TrafficCamNet でストリーミングを開始します。  
```
stream-start:
	xhost +
	docker exec -it deepstream-trafficcamnet deepstream-app -c /app/src/deepstream_app_source1_trafficcamnet.txt
```
## 相互依存関係にあるマイクロサービス  
本マイクロサービスを実行するために TrafficCamNet の AIモデルを最適化する手順は、[trafficcamnet-on-tao-toolkit](https://github.com/latonaio/trafficcamnet-on-tao-toolkit)を参照してください。  


## engineファイルについて
engineファイルである trafficcamnet.engine は、[trafficcamnet-on-tao-toolkit](https://github.com/latonaio/trafficcamnet-on-tao-toolkit)と共通のファイルであり、当該レポジトリで作成した engineファイルを、本リポジトリで使用しています。  

## 演算について
本レポジトリでは、ニューラルネットワークのモデルにおいて、エッジコンピューティング環境での演算スループット効率を高めるため、FP16(半精度浮動小数点)を使用しています。  
浮動小数点値の変更は、Makefileの以下の部分を変更し、engineファイルを生成してください。

```
tao-convert:
	docker exec -it trafficcamnet-tao-toolkit tao-converter -k tlt_encode -t fp16 -d 3,544,960 -e /app/src/trafficcamnet.engine /app/src/resnet18_trafficcamnet_pruned.etlt 

```