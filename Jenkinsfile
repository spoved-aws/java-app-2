node{

    
    stage("Git checkout"){
        git branch: 'main', url: 'https://github.com/spoved-aws/java-app-2.git'
        
    }

    stage("Building Shopfront Artifacts"){
        stage("Build Shopfront App"){
            sh 'mvn clean install -f shopfront/pom.xml'
            }
        stage("Build Shopfront image from Dockerfile"){
            sh 'pwd'
            sh 'docker image build -t kanukhosla10/shopfront:v1.$BUILD_ID shopfront/.'
        }

    }

    stage("Building Stockmanager Artifacts"){
        stage("Build Stockmanager App"){
            sh 'mvn clean install -f stockmanager/pom.xml'
            }
        stage("Build Stockmanager image from Dockerfile"){
            sh 'pwd'
            sh 'docker image build -t kanukhosla10/stockmanager:v1.$BUILD_ID stockmanager/.'
        }

    }

    stage("Building Product Catalogue Artifacts"){
        stage("Build Product Catalogue App"){
            sh 'mvn clean install -f productcatalogue/pom.xml'
            }
        stage("Build Product Catalogue image from Dockerfile"){
            sh 'pwd'
            sh 'docker image build -t kanukhosla10/productcatalogue:v1.$BUILD_ID productcatalogue/.'
        }

    }

    stage("Docker images tagging to LATEST tag"){
        ///Shopfront
        ///sh 'docker image tag shopfront:v1.$BUILD_ID kanukhosla10/shopfront:v1.$BUILD_ID'
        sh 'docker image tag kanukhosla10/shopfront:v1.$BUILD_ID kanukhosla10/shopfront:latest'
        ///Stockmanager
        ///sh 'docker image tag stockmanager:v1.$BUILD_ID kanukhosla10/stockmanager:v1.$BUILD_ID'
        sh 'docker image tag kanukhosla10/stockmanager:v1.$BUILD_ID kanukhosla10/stockmanager:latest'
        ///Product Catalogue
        ///sh 'docker image tag productcatalogue:v1.$BUILD_ID kanukhosla10/productcatalogue:v1.$BUILD_ID'
        sh 'docker image tag kanukhosla10/productcatalogue:v1.$BUILD_ID kanukhosla10/productcatalogue:latest'


    }

    stage("Push the images to dockerhub"){
        withCredentials([string(credentialsId: '0985b54a-7246-4e3c-bdb4-5e7bacd6a814', variable: 'dockerhub_password')]) {
            /// use double quotes = "" when using {} in sh command 
            sh "docker login -u kanukhosla10 -p ${dockerhub_password}"
            sh 'docker image push kanukhosla10/stockmanager:v1.$BUILD_ID'
            sh 'docker image push kanukhosla10/productcatalogue:v1.$BUILD_ID'
            sh 'docker image push kanukhosla10/shopfront:v1.$BUILD_ID'
            sh 'docker image push kanukhosla10/stockmanager:latest'
            sh 'docker image push kanukhosla10/stockmanager:latest'
            sh 'docker image push kanukhosla10/productcatalogue:latest'
            sh 'docker image push kanukhosla10/shopfront:latest'
        }
    }
    
    stage("Delete the docker images from local"){
        sh 'docker image rm  kanukhosla10/stockmanager:v1.$BUILD_ID kanukhosla10/productcatalogue:v1.$BUILD_ID kanukhosla10/shopfront:v1.$BUILD_ID kanukhosla10/stockmanager:latest kanukhosla10/productcatalogue:latest kanukhosla10/shopfront:latest'
    }

    stage("Ansible deploy the app on nodes"){
        sh 'ansible-playbook kubernetes/ansible/ansible-playbook.yml -K -i inventory.yml'
    }    
}
