def swarm_container

node {
     
     checkout scm
     def manager = [:]
     manager.name = 'Swarm-Manager'
     manager.host = '10.11.7.86'
     manager.user = 'docker'
     manager.password = 'tcuser'
     manager.allowAnyHosts = true
     
     def worker = [:]
     worker.name = 'Swarm-Worker'
     worker.host = '10.11.7.120'
     worker.user = 'docker'
     worker.password = 'tcuser'
     worker.allowAnyHosts = true

     stage ('Retrieve Swarm-Container ID') {
          //Stage -> Execute Docker ps command to define global variable
           swarm_container = sshCommand worker: worker, command: "docker ps --filter ancestor=swarm-demo -q"
     }

     stage ('Copy Source-Code into Swarm-Container') {
           //Stage -> Copies Source-Code from Root Directory of Swarm-Environment into Swarm-Container to run test.py
           echo "${swarm_container}"
           sshCommand worker: worker, command: "cd /root && docker cp test.py ${swarm_container}:/root"
     }

     stage ('Run Source-Code in Swarm-Container') {
          //Stage -> SSH command runs Source-Code and extracts output.csv to Swarm Environment
          sshCommand worker: worker, command: "docker exec -w /root ${swarm_container} python test.py > output.csv"
     }
     
     stage ('Transfer output.csv from Swarm-Environment into Desktop') {
          //Stage -> Move output.csv from Swarm-Environment into Desired Directory in Host Terminal
          sshGet worker: worker, from: 'output.csv' , into: 'C:\\Users\\z0048yrk\\Desktop\\COMPLETE POC\\Output-File' , override: true
        }

     stage ('Stop Swarm-Container') {
          //Stage -> Stop Swarm Container to keep Swarm Cluster light-weight
           sshCommand worker: worker, command: "docker stop ${swarm_container}"
     }
}
     

