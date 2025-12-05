// the first start with pipeline { set up agent and stages inside pipeline }
pipeline {
    /*
        agent เป็นคำสั่งที่เอาไว้บอก Jenkins ว่าจะให้ใช้ executor ตัวใดมา run คำสั่งใน stages ทั้งหมดนี้ ที่ใช้บ่อย ๆ จะเป็น
        none คือ ไม่ใช้ executor ใด ๆ สำหรับ stages ทั้งหมด (ต้องไปกำหนด executor แยกสำหรับแต่ stage เอง)
        any คือ ใช้ executor ใด ๆ ก็ได้
        docker คือ ใช้ docker executor มา run stages ทั้งหมดนี้
    */
    agent any

    // declare env as var for using on stages { ... }
    environment {
            // you have to call tru env.<var name> ex, env.DOMAIN
            DOMAIN = 'thitikorn-nupan.com'
            PATH_APP_ECOMMERCE = 'http://www.thitikorn-nupan.com/app/ecommerce/'
            PATH_APP_REVIEWS_BOOK = 'http://www.thitikorn-nupan.com/app/reviews-book/'
            PATH_PASSWORD_VPS = 'B:\\txt\\password_vps.txt'
    }


    // stages work as working flow it tells Pipeline what gonna do
    stages {
        // step 0
        stage('Before init get key from file') {
            steps {
                script {
                    if (fileExists(env.PATH_PASSWORD_VPS)) {
                        // Read the content of the file Keep this format you can't use / just use \\
                        // def fileContent = readFile(file: 'B:\\txts\\password_vps.txt').trim()
                        def fileContent = readFile(file: env.PATH_PASSWORD_VPS).trim()
                        // Store the content in an environment variable (Note , should not declare first)
                        env.PASSWORD_VPS = fileContent
                        echo "Dynamic environment variable as PASSWORD_VPS set to : ${env.PASSWORD_VPS}"
                    } else {
                        // the error step is generally preferred in Jenkins Pipelines as it integrates more cleanly with Jenkins's build status and reporting mechanisms. The error step also avoids printing a stack trace by default, which can make logs cleaner.
                        error("File did not exist! : ${env.PATH_PASSWORD_VPS}")
                    }
                }
            }
        }


        // step 1
        stage('Before init reads the environment') {
            steps {
                echo '******************************'
                // Note call env you have to use " " not ' '
                echo "DOMAIN_URI : ${env.DOMAIN}"
                echo "PATH_APP_ECOMMERCE : ${env.PATH_APP_ECOMMERCE}"
                echo "PATH_APP_REVIEWS_BOOK : ${env.PATH_APP_REVIEWS_BOOK}"
                echo "PASSWORD_VPS : ${env.PASSWORD_VPS}"
                echo '******************************'
            }

        }


        // step 2
        stage('Before init write some groovy language') {
            steps {
                /*
                    เราสามารถเขียน Pipeline Logic ที่ซับซ้อนด้วยภาษา Groovy ได้ โดยการใช้ script block
                    โดยการ กำหนด script { ... } ไว้ใน steps { ... }
                */
                echo '******************************'
                script {
                      def numbers = [10, 20, 30, 40, 50];
                      def sum = 0;
                      for(int index = 0; index < numbers.size(); index++) {
                         println("value of item : " + numbers[index] );
                         sum += numbers[index]
                      }
                      println("sum of item : " + sum );
                }
                echo '******************************'
            }

        }

        // step 3
        stage('Before init check software installed') {
            steps {
                  // Note , you do on local that meaning all software you version you have installed !!
                  // sh เป็นคำสั่งที่ใช้ในการ run Linux Command เช่น
                  echo '******************************'
                  sh 'java -version'
                  sh 'mvn -version'
                  sh 'git --version'
                  sh 'node --version'
                  sh 'nvm --version'
                  echo '******************************'
            }
            post {
                success {
                   echo 'showed version software installed'
                }
            }
        }


        // step 4
        stage('Init') {
            steps {
                echo '******************************'
            }
            //  the post section allows the definition of actions to be executed after the main Pipeline or a specific stage completes.
            post {
                always { // success: Steps execute only if the Pipeline or stage completes successfully.
                    echo 'Init stage gonna build!'
                }
                success { // success: Steps execute only if the Pipeline or stage completes successfully.
                    echo 'Init passed successfully!'
                }
                failure { // failure: Steps execute only if the Pipeline or stage fails.
                    echo 'Init failed.'
                }
            }
        }

        // step 5
        stage('Build') {
            steps {
                echo '******************************'
                sh 'pwd'
                // Go to target dir
                dir('jenkins_basic_env_linux_groovy') {
                    sh "ls -l"
                    sh "cat Jenkinsfile"
                }
                // Returns to the original working directory
                sh 'pwd'
            }
            post {
                success { // success: Steps execute only if the Pipeline or stage completes successfully.
                    echo 'Build passed successfully!'
                }
            }
        }

        // step 6
        stage('Deploy') {
            steps{
                echo 'Deploy'
                echo '******************************'
            }
        }


    }
    // The post section allows the definition of actions to be executed after the main Pipeline or a specific stage completes.
    // The post section can be defined at both the global Pipeline level and within individual stage blocks, allowing for granular control over post-execution actions.
    post {
          /*
            always: Steps within this block execute regardless of the Pipeline's or stage's final status (success, failure, unstable, aborted).
            success: Steps execute only if the Pipeline or stage completes successfully.
            failure: Steps execute only if the Pipeline or stage fails.
            unstable: Steps execute only if the Pipeline or stage completes with an "unstable" status.
            aborted: Steps execute only if the Pipeline or stage is aborted.
            changed: Steps execute if the current run's status differs from the previous run's status.
            fixed: Steps execute if the current run is successful and the previous run was either failed or unstable.
            regression: Steps execute if the current run's status is worse than the previous run's status (e.g., successful to unstable, unstable to failure).
            cleanup: This is a special condition within the global post section, primarily used for tasks like workspace cleanup, regardless of the build result.
         */
         always {
             echo 'Pipeline finished.'
         }
         success {
             // Send success notification
             echo 'Pipeline completed successfully.'
         }
         failure {
             // Send failure notification
             echo 'Pipeline failed.'
         }
         changed {
             echo 'Pipeline changed.'
         }
         cleanup {
             echo 'Pipeline cleanup successfully.'
         }
   }
}