pipeline
{
    agent any
    tools
    {
        
        maven 'maven_3.7.3'
    }
    	environment {
		        PROJECT_ID = 'golden-bonbon-311110'
                CLUSTER_NAME = 'k8s-cluster-37'
                LOCATION = 'us-central1-c'
                CREDENTIALS_ID = 'Kubernetes37'		
	}
    stages
    {
        stage('SCM')
        {
            steps
            {
                git branch: 'main', url: 'https://github.com/Venkatesh37/Minikube-springbootCICD.git'
            }
        }
        stage('MVN TEST BUILD')
        {
            steps
            {
            echo "Starting The Test..."
            sh 'mvn test'
            echo "Successfully Test Complteted... "
            }
        }
        stage('BUILDING THE CODE')
        {
            steps
            {
                
                echo "Starting the code build ..."
                sh 'mvn clean install'
                echo "Successfully the build code ..."
            }
        }
        stage('BUILDING THE DOCKER IMAGE')
        {
            steps
            {
                sh 'docker build -t venkatesh37/springboot-37-jun22:${BUILD_NUMBER} .'
            }
        }
        stage('LOGIN INTO THE DOCKERHUB AND PUSHING THE IMAGE')
        {
            steps
            {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-id-37', passwordVariable: 'password37', usernameVariable: 'username37')]) 
                {
                  sh "docker login -u ${username37} -p ${password37}"
                 }
                 sh 'docker push venkatesh37/springboot-37-jun22:${BUILD_NUMBER}'
            }
        }
        stage('DEPLOY TO KUBERNETES')
        {
            steps
            {
                echo "Start deployment of deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployement.yml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
			    echo "Deployment Finished ..."
			    echo "Start deployment of serviceLB.yaml"
			    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'service.yml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }
}
