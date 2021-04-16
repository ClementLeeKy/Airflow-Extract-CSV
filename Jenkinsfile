def swarm_container

node {
     
     checkout scm
     def remote = [:]
     remote.name = 'Swarm-Manager'
     remote.host = '10.11.7.63'
     remote.user = 'docker'
     remote.password = 'tcuser'
     remote.allowAnyHosts = true

     stage ('Retrieve Swarm-Container ID') {
           swarm_container = sshCommand remote: remote, command: "docker ps --filter ancestor=swarm-demo -q"
     }

     stage ('Copy Source-Code into Swarm-Container') {
           echo "${swarm_container}"
           sshCommand remote: remote, command: "cd /root && docker cp test.py ${swarm_container}:/root"
     }

     stage ('Run Source-Code in Swarm-Container') {
          //Stage -> SSH command runs Source-Code and extracts output.csv to Swarm Environment from Container
          sshCommand remote: remote, command: "docker exec -w /root ${swarm_container} python test.py > output.csv"
     }
     
     stage ('Transfer output.csv from Swarm-Environment into Desktop') {
          sshGet remote: remote, from: 'output.csv' , into: 'C:\\Users\\z0048yrk\\Desktop\\COMPLETE POC\\Output-File'
        }
     }

     stage ('Stop Swarm-Container') {
           sshCommand remote: remote, command: "docker stop ${swarm_container}"
     }
}
     

