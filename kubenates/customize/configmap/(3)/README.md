    **(3) 볼륨으로 사용 - nginx 웹 서버 구성**

   1. nginx-cm-compress.yml 컨피그맵 배포
      ```
      kubectl create -f nginx-cm-compress.yml
      ```
      컨피그맵 이름: nginx-gzip-config
      
      키: nginx-gzip.conf
      
      값: nginx 설정 파일 내용
      
   3. nginx-pod-compress.yml 파드 배포
      ```
      kubectl create -f nginx-pod-compress.yml
      ```

   3. 파드에 접속해 nginx 설정 파일 확인
      ```
      kubectl exec nginx-pod-compress -- cat /etc/nginx/conf.d/nginx-gzip.conf
      ```
