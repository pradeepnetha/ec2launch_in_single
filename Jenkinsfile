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
           secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])  {    
           
   //        #git url: 'https://github.com/pradeepnetha/ec2launch.git'
     //      #echo "enter ami id"
sh '''
               
echo "
#read img_id
#echo "enter instance type"
#read instance_type 
#echo "select key-pair"
#read key_name
#echo "enter sg name"
#read sg_name
#echo "enter subnet id"
#read sub_id
#echo "enter region"
#read region_name

#img_id=ami-0b500ef59d8335eee
#instance_type=t2.micro
#key_name=asg-new
#sg_name=sg-04a6d324940433647
#sub_id=subnet-0e7e366cb34aca9b2
#region_name=us-east-2

img_id=$1
instance_type=$2
sub_id=$3
region_name=$4
sg_name=$5
key_name=$6
tag_name=$7
tag_value=$8
tag_instance=$9


#aws ec2 run-instances --image-id ami-abc12345 --count 1 --instance-type t2.micro --key-name MyKeyPair --subnet-id subnet-6e7f829e --tag-specifications 'ResourceType=instance,Tags=[{Key=webserver,Value=production}]' 'ResourceType=volume,Tags=[{Key=cost-center,Value=cc123}]' 

#aws ec2 run-instances --image-id $img_id --count 1 --instance-type $instance_type --key-name $key_name --security-group-ids $sg_name --subnet-id $sub_id --region $region_name > information.txt
#grep InstanceId information.txt | tr -d '", ":' > InstanceId
#sed -i 's/InstanceId//g' InstanceId
#InstanceId=$( cat InstanceId )
#echo $InstanceId
#aws ec2 create-tags --resources $InstanceId --tags Key=Name,Value=Web3

#instancelaunch() {
aws ec2 run-instances --image-id $img_id --count 1 --instance-type $instance_type --key-name $key_name --security-group-ids $sg_name --subnet-id $sub_id --region us-east-2 > information.txt
grep InstanceId information.txt | tr -d '", ":' > Instance_Id
#grep 'InstanceId' information.txt > Instance_Id
sed -i 's/InstanceId//g' Instance_Id
sed -i 's/"//g' Instance_Id 
#sed -i 's/KeyName//g' Instance_Id
Insta_Id=$( head -1 Instance_Id )
              
echo $Insta_Id
#aws ec2 create-tags --resources $Insta_Id --region $region_name  --tags Key=Name,Value=Web3
echo $tag_name
echo $tag_value
echo $tag_instance
aws ec2 create-tags --resources $Insta_Id --region $region_name --tags Key=$tag_name,Value=$tag_value Key=Name,Value=$tag_instance
#}

#instancelaunch 
 " > pradeepec2launch.sh
         
           
           
                chmod +x pradeepec2launch.sh
                ./pradeepec2launch.sh $img_id $instance_type $sub_id $region_name $sg_name $key_name $tag_name $tag_value $tag_instance

    '''       
                           
         // slackSend baseUrl: 'https://opstree.slack.com/services/hooks/jenkins-ci/', channel: 'testjenkins', color: '#439FE0', message: 'build info', teamDomain: 'opstree', tokenCredentialId: 'slack-jenkins'     
          //slackSend message: 'build is success', tokenCredentialId: 'slack-jenkins'
                         
            }

                }
        }         
  
        }
}
