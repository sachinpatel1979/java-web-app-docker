node {
    def buildnumber= BUILD_NUMBER
    stage("Git Clone"){
        git url: 'https://github.com/sachinpatel1979/java-web-app-docker.git', branch: 'master'
    }
    
    stage("Maven Clean Package"){
        def mavenHome= tool name: "Maven",type: "maven"
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage("Build Docker Image"){
        
        sh "docker build -t sachinpatel1979/java-web-app-docker:${buildnumber} ."
    }
    stage("Docker Login and Push"){
        withCredentials([string(credentialsId: 'Docker_Hub_Credentials', variable: 'DockerHubPwd')]) {
            sh "docker login -u sachinpatel1979 -p ${DockerHubPwd}"
        }
        sh "docker push sachinpatel1979/java-web-app-docker:${buildnumber}"    
        
    }
   
    stage("Deploy application in K8s Cluster"){
        kubernetesDeploy(
            config: 'javawebapp-deployment.yml',
            kubeconfigId: 'KUBERNETES_CLUSTER_CONFIG',
            enableConfigSubstitution: true
        )
    }
    
}
