pipeline{
    agent{
        node{
            label 'built-in'
            customWorkspace '/mnt/project'
        }
    }
    
    stages{
        stage('clone-repo'){
            steps{
                sh "rm -rf hello-world*"
                sh "git clone https://github.com/ghanashyam86/hello-world.git"
            }
        }
        
        stage('build-hello-world-project'){
            steps{
                dir ('/mnt/project/hello-world'){
                    sh "mvn clean install"
                }
            }
        }
        
       stage('copy war on slave'){
           steps{
               sh "chmod 777 /mnt/project/hello-world/webapp/target/webapp.war"
               sh "scp -i /mnt/shyam_mumbai_key.pem /mnt/project/hello-world/webapp/target/webapp.war ec2-user@172.31.41.214:/mnt"
               sh "scp -i /mnt/shyam_mumbai_key.pem /mnt/project/hello-world/docker-compose.yaml ec2-user@172.31.41.214:/mnt"
               sh "scp -i /mnt/shyam_mumbai_key.pem /mnt/project/hello-world/Dockerfile ec2-user@172.31.41.214:/mnt"
           }
       }
       
       stage('remove old container and image'){
           agent{
               node{
                   label'dev'
               }
           }
           steps{
               sh "docker stop ghana-container-1 ghana-container-2"
               sh "docker system prune -af"
           }
       }
       
       stage('build docker image and run container'){
           agent{
               node{
                   label 'dev'
               }
           }
           steps{
               
               sh "docker-compose up -d"
           }
       }
       
       
    }
}
