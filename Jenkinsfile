pipeline {
    agent any
    
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

        stage ('tomcat workspace') {
            
            steps {
                 sh ''' cd /usr/local && rm -rf /usr/local/apache-tomcat-9.0.52*
                wget https://mirrors.estointernet.in/apache/tomcat/tomcat-9/v9.0.52/bin/apache-tomcat-9.0.52.tar.gz
                     tar -xvf apache-tomcat-9.0.52.tar.gz
                     echo "export CATALINA_HOME="/usr/local/apache-tomcat-9.0.52"" >> ~/.bashrc
                     source ~/.bashrc
                     cd /usr/local/apache-tomcat-9.0.52/webapps
		             cd /usr/local/apache-tomcat-9.0.52/bin '''
		             

            }
        }
            
        stage ('deployment') {
            
            steps {
                
             sh '''  cd /root/.m2/repository/in/ezeon/SpringContactApp/1.0-SNAPSHOT && cp -r *.war  /usr/local/apache-tomcat-9.0.52/webapps 
                     cd /usr/local/apache-tomcat-9.0.52/bin && sh startup.sh '''
                     
            }
        }

        }
        
        post {
        always {
           
            cleanWs()
        }
    }
        
        
        
}
