pipeline {
    agent { label 'tomcat2' }
    
    stages {
        /*stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }*/
        
    stage('mvn workspace'){
        steps{
            sh '''#!/bin/bash
            
           
            wget https://downloads.apache.org/maven/maven-3/3.8.1/binaries/apache-maven-3.8.1-bin.tar.gz -P /opt
            cd /opt && tar -zxvf apache-maven-3.8.1-bin.tar.gz
            export M2_HOME=/opt/apache-maven-3.8.1
            export PATH=${M2_HOME}/bin:${PATH}
            mvn --version
            which mvn
            '''
        }
    }
    
    /*stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
    }*/
    
    
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/VikramThakur8/SpringContactApp.git"
            }
        }

        stage ('Build') {
            
            steps {
               
                 sh '''#!/bin/bash
                    sh /opt/apache-maven-3.8.1/bin/mvn clean install'''
            }
        }
        
        stage ('docker service status check') {
            
            steps {
               
                 sh '''#!/bin/bash
                        value1=myjavaapp
                        value=$(docker service ls | head -4 |  tail -1 | awk \'{ print $2 }\')
                        if [ $value == myjavaapp ]
                        then
                           echo -e "\\e[1;31m =======stopping service $value===============  \\e[0m"
                           sudo docker service rm myjavaapp && echo -e "\\e[1;31m =====stopped and removed the service $value1========= \\e[0m"
                        else
                           echo "============= no service found $value1  ====================="
                        fi'''
            }
        }

        stage ('docker deployment') {
            
            steps {
                 sh ''' cd /root/.m2/repository/in/ezeon/SpringContactApp/1.0-SNAPSHOT && cp -r *.war /home/ec2-user/docker
                 docker login -u mansiju03 -p Mansi@12345
                 docker tag testimage:latest mansiju03/study_12:$BUILD_NUMBER
                 docker push mansiju03/study_12:$BUILD_NUMBER
                 cd /home/ec2-user/docker && docker build -t testimage .
                 docker service create --replicas 4 --name myjavaapp -p 8080:8080 testimage:latest
                
                 '''
		             

            }
        }
    
            
        

        }
        
        post {
        always {
           
            cleanWs()
        }
    }
        
        
        
}
