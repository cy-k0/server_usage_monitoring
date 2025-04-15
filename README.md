# run_monitor.sh
이 프로젝트는 server_status.py(서버 모니터링) 스크립트를 실행하는 Docker 컨테이너를 매일 한 시간마다 자동으로 실행합니다.

# run_report.sh
이 프로젝트는 server_status.py 실행 결과가 server_monitoring.db 에 저장되면, 해당 데이터를 csv파일로 export 하고 google sheet를 업데이트 합니다.


## 주의사항 
프로젝트에 포함된 로컬 경로는 사용 전 변경 바랍니다. (추후 env로 관리할 예정)



## Prerequisites

- Docker
- cron (Linux)



## Setup 


### 1. Repository 복제

`git clone https://chaeyeonko@bitbucket.org/innodep/inno_server_manager.git`



### 2. Docker 이미지 빌드

```
docker build -t server-monitor .
docker build -t server-monitoring-report -f Dockerfile.export .
```




### 3. run_monitor.sh, run_report.sh 실행 권한 부여
```
chmod +x run_monitor.sh
chmod +x run_report.sh
```



### cron 작업 설정
`crontab -e`



### 4. 스케줄링 설정 [editor 는 nano 선택]
```
0 * * * * /home/innonew_mam/inno_server_manager/server_monitor.sh
10 7 * * * /home/innonew_mam/inno_server_manager/run_report.sh
```



### 5. 로그 로테이션 설정 [필요 시 변경 혹은 삭제 가능]
``` 
/home/innonew_mam/inno_server_manager/logs/cron_*.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    create 644 innonew_mam innonew_mam
}
```
