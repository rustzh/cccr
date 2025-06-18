# 기본 명령어



# 실습 흐름

1. **이미지 사용자화**
   
2. **환경 변수 사용자화**

3. **컨피그맵**
   
   **환경 변수로 사용**
   
   myapp-pod-cm.yml 파드 배포

   파드 상태 확인 (에러)

   myapp-cm-message.yml 컨피그맵 배포

   파드 상태 확인

   파드 내 환경 변수 확인

   **볼륨으로 사용**

   ```
   kubectl create cm my-config1 --from-literal key1=value1
   ```

   myapp-pod-cm-vol.yml 파드 배포

   ```
   kubectl exec myapp-pod-cm-vol -- ls /mnt
   ```
   **볼륨으로 사용 - nginx 웹 서버 구성**

   nginx-cm-compress.yml 컨피그맵 배포

   nginx-pod-compress.yml 파드 배포

   kubectl exec로 bash를 실행해 /etc/nginx/conf.d/nginx-gzip.conf 파일 확인

4. **시크릿**

   
   
