pipeline
{
    agent any
    stages
    {
        stage ('Build Backend')
        {
            steps 
            {
                bat 'mvn clean package -DskipTests=true'
            }
        }

        stage ('Unit Tests')
        {
            steps 
            {
                bat 'mvn test'
            }
        }

        stage ('Sonar Analysis')
        {
            environment 
            {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps
            {
              withSonarQubeEnv('SONAR_LOCAL')
              {
                bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=774c05fea9735bbbb373f7611dd5bdd4d0abcad6 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**.**Application.java"
              }
            }      
        }

        stage ('Quality Gate')
        {
            steps
            {
                sleep(10)
                timeout(time: 1, unit: 'MINUTES') 
                {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
        stage ('Deploy Backend')
        {
            steps
            {
                deploy adapters: [tomcat8(credentialsId: 'TomCatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

         stage ('API Test')
        {
            steps
            {
                dir('api-test')
                {
                    git credentialsId: 'GitHub', url: 'https://github.com/lucasfcorrea/tasks-api-test'
                    bat 'mvn test'
                }
            }
        }

          stage ('Deploy Frontend')
        {
            steps
            {
                 dir('frontend')
                {
                    git credentialsId: 'GitHub', url: 'https://github.com/lucasfcorrea/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomCatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
                
            }
        }

    }
}


