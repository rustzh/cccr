# 실습 흐름

1. **exptyDir 볼륨**
  
   myapp-rs-emptydir.yml 레플리카셋 배포

   myapp-svc-emptydir.yml 서비스 배포

   레플리카셋 / 파드 확인

   생성된 파드 describe로 상세 정보 확인

   각 컨테이너의 마운트 경로 index.html 확인
   ```
   kubectl exec <파드 이름> -c html-generator -- cat /var/htdocs/index.html
   ```
   ```
   kubectl exec <파드 이름> -c web-server -- cat /usr/share/nginx/html/index.html
   ```
