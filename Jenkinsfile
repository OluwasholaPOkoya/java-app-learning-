pipeline{
    agent any
    
    tools{
        maven "Maven-3.8.6"
    }
    
    stages{
        stage('github Clone'){
            steps{
                git branch: 'main', credentialsId: 'my-github-credentials', url: 'https://github.com/OluwasholaPOkoya/java-app-learning-.git'
            }
        }
        
        stage('build'){
            steps{
                echo "selected environment is : "
                //println params.Environment 
                sh "mvn clean install"
                
            }
        }
        
        
        stage ('Sonar scan'){
            environment{
               scanner = tool 'Sonar' 
            }
            steps{
              withSonarQubeEnv('SonarServer'){
                  sh "${scanner}/bin/sonar-scanner -Dsonar.projectKey=java_new_app -Dsonar.sources=. -Dsonar.java.binaries=target/classes/com/mycompany/app"
              }
            }
        } 
        
        Stage ('Quality Gate'){
            steps{
                waitForQualityGate: abortPipeline true
            }
        }
        
        stage('Deploy'){
            when{
                expression {params.Deploy}
            }
            steps{
                script{
                    if(params.Bucket=="june282022shola"){
                        s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'june282022shola', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: 'target/*jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'AWS-S3B', userMetadata: []
                    }
                    
                     if(params.Bucket=="shola-training-bucket"){
                        s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'shola-training-bucket', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-west-1', showDirectlyInBrowser: false, sourceFile: 'target/*jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'AWS-S3B', userMetadata: []
                    }
                    
                    if(params.Bucket=="olu-devops-artifact-bucket"){
                        s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'olu-devops-artifact-bucket', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: 'target/*jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'AWS-S3B', userMetadata: []
                    }
                }
                
               
            }
        }
    }
}
