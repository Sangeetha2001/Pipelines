pipeline {
  agent any
  stages {
    stage('CI : Switch to docker dir') {
      steps {
        sh 'cd /opt/kickstart-docker/'
      }
    }

    stage('CI : Build Docker Image') {
      steps {
        sh 'sudo docker image build -t sloopstash/prometheus:v2.45.6 -f image/prometheus/2.45.6/prometheus.dockerfile image/prometheus/2.45.6/context'
      }
    }

    stage('CI : Create a network') {
      steps {
        sh 'sudo docker network create -d bridge --subnet=14.1.0.0/16 --ip-range=14.1.1.0/24 --gateway=14.1.1.1 sloopstash-Dev-prometheus'
      }
    }

    stage('CI : Container run') {
      steps {
        sh 'udo docker container run -d -i -t --rm --name prometheus_2.45.6 -h prometheus --network sloopstash-Dev-prometheus --ip 14.1.0.7 -p 7070:9090 --mount \'type=volume,src=prometheus_2.45.6-data,dst=/opt/prometheus/data\' --mount \'type=volume,src=prometheus_2.45.6-log,dst=/opt/prometheus/log\' -v /opt/kickstart-docker/workload/supervisor/conf/server.conf:/etc/supervisord.conf -v /opt/kickstart-docker/workload/prometheus/2.45.6/conf/supervisor.ini:/opt/prometheus/system/supervisor.ini -v /opt/kickstart-docker/workload/prometheus/2.45.6/conf/prometheus.yml:/opt/prometheus/conf/prometheus.yml --entrypoint /usr/bin/supervisord sloopstash/prometheus:v2.45.6 -c /etc/supervisord.conf'
      }
    }

    stage('Question time??') {
      steps {
        input 'Do u wanna proceed??'
      }
    }

    stage('Terminate the container ') {
      steps {
        sh 'sudo docker container stop prometheus_2.45.6'
      }
    }

  }
}