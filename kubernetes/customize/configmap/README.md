# 실습 내용
**(1) 환경 변수로 사용**

   1. myapp-pod-cm.yml 파드 배포
      ```
      kubectl create -f myapp-pod-cm.yml
      ```
   3. 파드 상태 확인 (STATUS: CreateContainerConfigError)
      ```
      kubectl get po
      ```
   5. myapp-cm-message.yml 컨피그맵 배포
      ```
      kubectl create -f myapp-cm-message.yml
      ```
   7. 파드 상태 확인 (STATUS: Running)
      ```
      kubectl get po
      ```
   9. 파드 내 환경 변수 확인
      ```
      kubectl exec myapp-pod-cm -- env | grep MESSAGE
      ```

**(2) 볼륨으로 사용**

   1. 컨피그맵 배포
      ```
      kubectl create cm my-config1 --from-literal key1=value1
      ```
   2. myapp-pod-cm-vol.yml 파드 배포
      ```
      kubectl create -f myapp-pod-cm-vol.yml
      ```
   3. 볼륨 마운트 확인
      ```
      kubectl exec myapp-pod-cm-vol -- ls /mnt
      kubectl exec myapp-pod-cm-vol -- cat /mnt/key1
      ```
      
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
