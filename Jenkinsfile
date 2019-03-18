pipeline {
    agent any
        parameters {
        //string (description: 'enter ami_id', name: 'img_id')
        //string (description: 'enter Instance Type', name: 'instance_type')
        //string (description: 'enter subnet id', name: 'sub_id')
        //string (description: 'enter region name', name: 'region_name')
        //string (description: 'enter Security Group', name: 'sg_name')
        string (description: 'enter tag name', name: 'tag_name') 
        string (description: 'enter tag value', name: 'tag_value')
        string (description: 'enter instance visible name', name: 'tag_instance')
        choice (choices: ['ami-0b500ef59d8335eee'], description: 'choose key pair?', name: 'img_id')
        choice (choices: ['t2.micro'], name: 'instance_type')
        choice (choices: ['subnet-0e7e366cb34aca9b2'], name: 'sub_id')
        choice (choices: ['us-east-2'], name: 'region_name')
        choice (choices: ['sg-04a6d324940433647'], name: 'sg_name')    
        choice (choices: ['asg-new','CI_VPC', 'test1'], description: 'choose key pair?', name: 'key_name')
    }
    
    stages {
        stage ('ec2-launch') {
           steps {
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', 
                   accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                   credentialsId: 'aws key', 
                   secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {    
              sh """ echo '
              img_id=$1 
              instance_type=$2 
              sub_id=$3 
              region_name=$4 
              sg_name=$5 
              key_name=$6 
              tag_name=$7 
              tag_value=$8 
              tag_instance=$9 
              #instancelaunch() {
              aws ec2 run-instances --image-id $img_id --count 1 --instance-type $instance_type --key-name $key_name --security-group-ids $sg_name --subnet-id $sub_id --region us-east-2 > information.txt 
              #} 
              instancelaunch' > pradeepec2launch.sh """
              //sh("""chmod +x pradeepec2launch.sh 
                // ./pradeepec2launch.sh $img_id $instance_type $sub_id $region_name $sg_name $key_name $tag_name $tag_value $tag_instance""")
              sh('''echo ```grep \'InstanceId\' information.txt | tr -d \'", "\' > hai
grep \'KeyName\' information.txt | tr -d \'", "\' > keyname
sed -i \'s/InstanceId://g\' hai
sed -i \'s/KeyName://g\' keyname
Insta_Id=$(cat hai)
     
aws ec2 create-tags --resources $Insta_Id --region $region_name --tags Key=$tag_name,Value=$tag_value Key=Name,Value=$tag_instance``` > proper.sh chmod +x proper.sh ./proper.sh''')
                }
            }
        }
        stage ('tagging') {
            steps {
                script {
                    def insta_id = ''
                    def key_name = ''
                    dir ('/var/lib/jenkins/workspace/ec2insingle') {    
                        insta_id = sh(script:"head -1 hai", returnStdout: true)
                        key_name = sh(script:"head -1 keyname", returnStdout: true)
                        echo "${insta_id}"                 
                    }    
                    echo "${insta_id}"
                    slackSend message: 'build is success ' +insta_id +key_name, tokenCredentialId: 'slack-jenkins' 
               }
           }
        }
    }
}
