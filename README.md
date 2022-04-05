# trafficcamnet-on-deepstream
trafficcamnet-on-deepstream は、DeepStream 上で TrafficCamNet の AIモデル を動作させるマイクロサービスです。  

## 動作環境
- NVIDIA 
    - DeepStream
- TrafficCamNet
- Docker
- TensorRT Runtime

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
