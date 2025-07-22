pipeline {
    agent {label 'AGENT-1'}
    environment{
        SONAR_HOME=tool "sonar"
    }

    stages {
        stage("clone from github") {
            steps {
                git url:"https://github.com/nirmalaMahadasyam/wanderlust.git", branch: "devops"
            }
        }
        stage("sonarQube Quality Analysis"){
            steps{
                withSonarQubeEnv("sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
                
            }
        }
        
        stage('OWASP Dependenct check'){
            steps{
            dependencyCheck additionalArguments: '--scan ./' , odcInstallation:'dc'
            dependencyCheckPublisher pattern:'**/dependency-check-report.xml'
            }
        }
        stage("sonar Quality Gate Scan")
        {
            steps{
                timeout(time:2 ,unit:"minitues")
                waitForQualityGate abortionpipeline:false
            }
        }
        stage("trivy file systemscan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        
    }
}
