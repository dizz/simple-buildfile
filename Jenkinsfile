node('docker'){
    stage "Container Prep"
        
        echo("the node is up")
        def mycontainer = docker.image('gtunon/docker-in-docker:latest')
        mycontainer.pull() // make sure we have the latest available from Docker Hub
        
        mycontainer.inside("-u jenkins -v /var/run/docker.sock:/var/run/docker.sock:rw") {

            git 'https://github.com/dizz/simple-buildfile.git'
                    
            stage "Build image - Package"
                echo ("building..")
                //need to be corrected to the organization because at the moment elastestci can't create new repositories in the organization
                def myimage = docker.build "dizz/simple-buildfile"
                    
            stage "Run image"
                echo "run the image..."
            //    myimage.run()
            
            stage "Test"
                echo ("Starting tests")
                echo ("No tests yet, but these would be integration at least")

            stage "publish"
                //this is work arround as withDockerRegistry is not working properly 
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dizz-docker-hub',
                    usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                        sh 'docker login -u "$USERNAME" -p "$PASSWORD"'
                        myimage.push()
                }
        }
}