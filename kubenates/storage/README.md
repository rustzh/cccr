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
   지정된 이미지의 index.html은 3초마다 변경되기 때문에 3초 안에 두 명령어를 모두 사용해서 같은 내용이 뜨는지 확인해보자!

2. **초기화 컨테이너**

   myapp-rs-git.yml 레플리카셋 배포

   ```
   kubectl get pod --watch
   ```
   초기화 컨테이너 동작 확인

   ```
   kubectl exec <파드 이름> -- ls /usr/share/nginx/html
   ```
   디렉토리에 깃 디렉토리가 내용이 제대로 마운트되었는지 확인

   myapp--svc-git.yml 서비스 배포

   external IP로 접속해보기
   
3. **hostPath 볼륨**

   1번 노드 /srv/web-content/index.html 파일 생성

   2번 노드 /srv/web-content 디렉토리만 생성

   3번 노드 아무것도 생성하지 않음

   myapp-ds-hp.yml 데몬셋 배포

   데몬셋 확인 / 파드 -o wide 옵션으로 확인
  
   myapp-rs-hp.yml 레플리카셋 생성

   레플리카셋 확이 / 파드 확인
