pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    sudo docker images
                    sudo docker ps
            ''' 
      }
    }

    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'sudo docker run gesellix/trufflehog --json https://github.com/frovai/devsecops.git > trufflehog'
        sh 'cat trufflehog'
      }
    }

    stage ('build') {
      steps {
        sh '''
            mvn clean package
            sudo docker build -t webapp:1 -f Dockerfile-tomcat . 
            sudo docker stop webapp
            sudo docker rm webapp
            sudo docker run --name=webapp -p 8080:8080 -di webapp:1 
            ''' 
      }
    }
    
    //stage ('Source Composition Analysis') {
    //  steps {
    //     sh 'rm owasp* || true'
    //     sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
    //     sh 'chmod +x owasp-dependency-check.sh'
    //     sh 'bash owasp-dependency-check.sh'
    //     sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
    //    
    //  }
    //}
    //
    //stage ('SAST') {
    //  steps {
    //    withSonarQubeEnv('sonar') {
    //      sh 'mvn sonar:sonar'
    //      sh 'cat target/sonar/report-task.txt'
    //    }
    //  }
    //}
    //
    //stage ('Build') {
    //  steps {
    //  sh 'mvn clean package'
    //   }
    //}
    //
    //stage ('Deploy-To-Tomcat') {
    //        steps {
    //       sshagent(['tomcat']) {
    //            sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.232.202.25:/prod/apache-tomcat-8.5.39/webapps/webapp.war'
    //          }      
    //       }       
    //}
    //
    //
    //stage ('DAST') {
    //  steps {
    //    sshagent(['zap']) {
    //     sh 'ssh -o  StrictHostKeyChecking=no ubuntu@13.232.158.44 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://13.232.202.25:8080/webapp/" || true'
    //    }
    //  }
    //}
    //
  }
}
