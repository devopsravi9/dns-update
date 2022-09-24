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
        sh
          """
            PUBLIC_IP=$(aws ec2 describe-instances --instance-ids i-086869893435a86d5 | jq .Reservations[].Instances[].PublicIpAddress)
            echo '
            {
              "Comment": "CREATE/DELETE/UPSERT a record ",
              "Changes": [{
                "Action": "UPSERT",
                "ResourceRecordSet": {
                  "Name": "jenkins.devopsravi.online",
                  "Type": "A",
                  "TTL": 300,
                  "ResourceRecords": [{ "Value": IPADDRESS}]
                }}]
            }
            ' | sed -e "s/IPADDRESS/${PUBLIC_IP}/" > /tmp/record.json

            aws route53 change-resource-record-sets --hosted-zone-id Z0462442QH5T6H1KPDGO --change-batch file:///tmp/record.json
          """
        }
      }
    }
  }



