Here using the docker folder we storing the logs inside the kibana
step-1:
  Go to the docker folder.Make sure the docker container is installed and running.
step-2:
  -> docker-copmose up -d
  check running containers using
  -> docker ps
  check weather the filebeat is getting the log messages or not
  -> docker logs -f filebeat
  check weather the logs are sending to the kibana or not
  -> docker logs -f logstash

step-3:
  Open kibana with localhost:5601
step-4:
  Go to stack management and create new index pattern wiht "spring-boot-logs-*" and select @timestamp
step-5:
  You can check your logs by clicking selecting "Discovery".
