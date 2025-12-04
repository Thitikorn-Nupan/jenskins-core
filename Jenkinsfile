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
    }

    // stages work as working flow it tells Pipeline what gonna do
    stages {

        // step 1
        stage('Before init reads the environment') {
            steps {
                echo '******************************'
                // Note call env you have to use " " not ' '
                echo "DOMAIN_URI : ${env.DOMAIN}"
                echo "PATH_APP_ECOMMERCE : ${env.PATH_APP_ECOMMERCE}"
                echo "PATH_APP_REVIEWS_BOOK : ${env.PATH_APP_REVIEWS_BOOK}"
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

        }


        // step 4
        stage('Init') {
            steps {
                echo '******************************'
            }
            //  the post section allows the definition of actions to be executed after the main Pipeline or a specific stage completes.
            post {
                success {
                    echo 'Init passed successfully!'
                }
                failure {
                    echo 'Init failed.'
                }
            }
        }

        // step 5
        stage('Build') {
            steps {
                echo '******************************'
            }
        }

        // step 6
        stage('Deploy') {
            steps{
                echo 'Deploy'
                echo '******************************'
            }
        }

        //  the post section allows the definition of actions to be executed after the main Pipeline or a specific stage completes.

    }
}