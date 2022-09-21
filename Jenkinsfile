//node {
//  stage ('dns update') {
//    aws ec2 describe-instances --instance-ids i-0d8a49ba11b61a358 | jq .Reservations[].Instances[].PublicIpAddress
//    }
//  }

pipeline {
  agent any
  stages {
    stage ( 'dns update' ) {
      steps {
        sh '''
          PUBLIC_IP=$(aws ec2 describe-instances --instance-ids i-0d8a49ba11b61a358 | jq .Reservations[].Instances[].PublicIpAddress)
          echo $PUBLIC_IP
          echo "{
            "Comment": "CREATE/DELETE/UPSERT a record ",
            "Changes": [{
              "Action": "UPSERT",
              "ResourceRecordSet": {
                "Name": "COMPONENT.roboshop.internal",
                "Type": "A",
                "TTL": 300,
                "ResourceRecords": [{ "Value": "IPADDRESS"}]
              }}]
          }" | sed -e "s/IPADDRESS/${PUBLIC_IP}/"
        '''
        }
      }
    }
  }


