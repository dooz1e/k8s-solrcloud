# On-premise 환경에서의 k8s를 활용한 solrcloud 구축
1. 도커 이미지 빌드
 # docker build -t solrtest .

2. private docker registry harbor 에 도커 이미지 push
 docker tag [IMAGE ID] [harbor 도메인]/[project명]/[이미지명]:label 
ex) # docker tag 34d10ec56c9a harbor.kdotdev.cloud/kdot-cloud/kdot-search:1.0.master.105
    # docker push harbor.kdotdev.cloud/kdot-cloud/kdot-search:1.0.master.105

3. 쿠버네티스 마스터 노드에서 이미지 pulling 위한 secret 생성
 # kubectl create secret docker-registry harbor --docker-server=[harbor 도메인] --docker-username='ID' --docker-password='password'

imagePullSecrets:
        - name: harbor 추가
4. metalb를 활용한 Loadbalancer 서비스 사용(solr 페이지 외부 접속용)
 # k get svc 
NAME           TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                      AGE
solr-service   LoadBalancer   10.109.172.187   172.16.31.201   8983:30928/TCP               2d
zookeeper-hs   ClusterIP      None             <none>          2888/TCP,3888/TCP,2181/TCP   2d
http://172.16.31.201:8983/solr/index.html 로 접속가능

5. zookeeper에 custom collection upload
   # k exec -it solrcloud-0 -- /bin/bash
   $ cd /opt/solr/bin
   $ ./solr zk cp -r file:/data/ksearch_collection_configs zk:/configs
   $ Copying from 'file:/data/' to 'zk:/configs'. ZooKeeper at zookeeper-0.zookeeper-hs.solrtest.svc.cluster.local:2181,zookeeper-1.zookeeper-hs.solrtest.svc.cluster.local:2181,zookeeper-2.zookeeper-hs.solrtest.svc.cluster.local:2181





    ![image](https://github.com/dooz1e/k8s-solrcloud/assets/170922638/219fd9f5-5be6-4bea-aebd-9f27424495ab)

/configs 아래 경로로 컬렉션 업로드됨
7. 컬렉션 생성


![image](https://github.com/dooz1e/k8s-solrcloud/assets/170922638/7c19b915-5bdb-4b2b-8806-c5aa150b1b36)

