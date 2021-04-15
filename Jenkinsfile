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
          sshCommand remote: remote, command: "docker exec --interactive ${swarm_container} bash -c "cd root && python test.py > output.csv""  
     }
     
     /*

     stage ('Copy output.csv to desired directory') {
           dir ('C:\\Users\\z0048yrk\\Desktop') {
           sshCommand remote: remote, command: "docker cp ${swarm_container}:/root/output.csv output.csv"
        }   
     }

     stage ('Stop Swarm-Container') {
           sshCommand remote: remote, command: "docker stop ${swarm_container}"
     }  */
}  
