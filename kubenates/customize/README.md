# 디렉토리 소개

**이미지 사용자화**
   
   
**환경 변수 사용자화**
 

### **/configmap - 컨피그맵**

   - 환경 변수로 사용
   
   - 볼륨으로 사용
   
   - 볼륨으로 사용 - nginx 웹 서버 구성


### **시크릿**

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
      kubectl create secret tls nginx-tls-secret --cer=nginx-tls/nginx-tls.crt --key=nginx-tls/nginx-tls.key
      ```
   6. nginx-cm-https.yml 컨피그맵 배포
   7. nginx-pod-https.yml 파드 배포
      
      nginx-cm-https.yml 컨피그맵과 nginx-tls-secret을 마운트하는 파드
   9. 연결 확인
       
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
      접속 확인
      ```
      kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
      ```
      ```
      curl <nginx-pod-https IP 주소>
      curl https://<nginx-pod-https IP 주소> -k
      ```
      여기서 k 옵션은 자체 서명 인증서로 접속이 가능하도록 설정하는 것이다.

**(2) 인그레스를 이용한 TLS 종료 프록시 구현**
   1. 인증서와 키는 (1)에서 사용한 nginx-tls-secret 그대로 사용
   2. myapp-ing-tls-term.yml 인그레스 배포
   3. service/myapp-svc-np 서비스 배포
   4. controller/myapp-rs-np 레플리카셋 배포
   5. 연겷 확인

      클라이언트 -> 인그레스
      ```
      curl https://myapp.example.com -k
      ```
      
