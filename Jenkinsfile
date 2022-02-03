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
    }
}
