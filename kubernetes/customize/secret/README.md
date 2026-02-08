# 실습 내용
**(1) 시크릿 TLS 인증서를 사용해 nginx HTTPS 웹 서버 구성**
   
   1. 키와 인증서를 저장할 nginx-tls 디렉토리 생성
      ```
      mkdir nginx-tls
      ```
   3. 암호화된 TLS 키 생성
      ```
      openssl genrsa -out nginx-tls/nginx-tls.key 2048
      ```
   4. 인증서 생성
      ```
      openssl req --new -x509 -key nginx-tls/nginx-tls.key --out nginx-tls/nginx-tls.crt -days 3650 -subj /CN=myapp.example.com
      ```
   5. 인증서와 키를 저장하기 위해 시크릿(nginx-tls-secret) 생성
      ```
      kubectl create secret tls nginx-tls-secret --cert=nginx-tls/nginx-tls.crt --key=nginx-tls/nginx-tls.key
      ```
   6. nginx-cm-https.yml 컨피그맵 배포
      ```
      kubectl create -f nginx-cm-https.yml
      ```
   8. nginx-pod-https.yml 파드 배포
      ```
      kubectl create -f nginx-pod-https.yml
      ```
   10. 파드에 접속해 마운트 확인
       
       파드 접속
       ```
       kubectl exec -it nginx-pod-https -- bash
       ```
       파일 확인 (컨피그맵과 시크릿으로 마운트된 파일)
       ```
       ls /etc/nginx/conf.d
       ls /etc/nginx/ssl
       exit
       ```
   12. 접속 확인

       파드 IP 주소 확인
       ```
       kubectl get po -o wide
       ```
       nettool 파드 접속
       ```
       kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
       ```
       파드의 웹 서버 접속
       ```
       curl <nginx-pod-https IP 주소>
       curl https://<nginx-pod-https IP 주소> -k
       ```
       여기서 k 옵션은 자체 서명 인증서로 접속이 가능하도록 설정하는 것이다.

**(2) 인그레스를 이용한 TLS 종료 프록시 구현**
   1. 인증서와 키는 (1)에서 사용한 nginx-tls-secret 그대로 사용
   2. myapp-ing-tls-term.yml 인그레스 배포
      ```
      kubectl create -f myapp-ing-tls-term.yml
      ```
   4. myapp-svc-np 서비스 배포
      ```
      kubectl create -f myapp-svc-np.yml
      ```
   6. controller/myapp-rs-np 레플리카셋 배포
      ```
      kubectl create -f myapp-rs-np.yml
      ```
   8. 연결 확인

      클라이언트 -> 인그레스
      ```
      curl https://myapp.example.com -k
      ```
