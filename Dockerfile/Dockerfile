FROM solr:8.11.3
RUN set -ex; #디버깅용도 
USER root #solr 이미지에서 user가 solr로 세팅되있어서 바꿔줌    
RUN mkdir -p /data
RUN chmod -R 0755 /data
RUN chown -R 8983:8983 /data # solr uid:gid 로 custom 컬렉션 업로드할 디렉토리 설정
COPY lib /opt/solr-8.11.3/server/lib #필요 라이브러리 업로드
COPY lib /opt/solr-8.11.3/server/solr-webapp/webapp/WEB-INF/lib
USER solr
COPY --chown=8983:8983 ksearch_collection_configs /data/ksearch_collection_configs
