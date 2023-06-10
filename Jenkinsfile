pipeline {
    agent any

    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'Testing..'
                sh 'cd webapp && sudo docker run  --rm -e SONAR_HOST_URL="http://3.82.33.200:9000" -e SONAR_LOGIN="sqp_1eca3d30193b3bcf242f2c8502790b5bfced1e22"  -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
            }
        }

        stage('Build LMS') {
            steps {
                echo 'Building..'
               sh 'cd webapp && npm install && npm run build'
            }
        }

        stage('Release LMS') {
            steps {
                script {
                    echo "Releasing.."       
                  def packageJSON = readJSON file: 'webapp/package.json'
                  def packageJSONVersion = packageJSON.version
                  echo "${packageJSONVersion}"  
                  sh "zip webapp/dist-${packageJSONVersion}.zip -r webapp/dist"
                 sh "curl -v -u admin:PR@thibha123 --upload-file webapp/dist-${packageJSONVersion}.zip http://3.82.33.200:8081/repository/lms/"     
            }
            }
        }
        
        stage('Deploy LMS') {
            steps {
                script {
                    echo "Deploying.."       
                    //def packageJSON = readJSON file: 'webapp/package.json'
                    //def packageJSONVersion = packageJSON.version
                    //echo "${packageJSONVersion}"  
                   // sh "curl -u admin:PR@thibha123 -X GET \'http://3.82.33.200:8081/repository/lms/dist-${packageJSONVersion}.zip\' --output dist-'${packageJSONVersion}'.zip"
                    //sh 'sudo rm -rf /var/www/html/*'
                   // sh "sudo unzip -o dist-'${packageJSONVersion}'.zip"
                   // sh "sudo cp -r webapp/dist/* /var/www/html"
            }
            }
        }
    }
}

