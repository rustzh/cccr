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
