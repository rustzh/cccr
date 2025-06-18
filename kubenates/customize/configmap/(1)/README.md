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
