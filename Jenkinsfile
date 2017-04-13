//---maven project
node('master') {
    // some block
stage "SCM Checkout"
git credentialsId: 'c92b0958-af6f-4834-a103-f97a5c73d877', url: 'https://github.com/ankitapatil2/Ewallet.git'


stage "UnitTesting"
withMaven(jdk: 'JAVA_HOME_SYSTEM', maven: 'Maven_master') {
sh '''cd Ewallet_devops
unset JAVA_TOOL_OPTIONS
mvn clean test -Ptest org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true cobertura:cobertura -Dcobertura.report.format=xml\'
junit \'target/surefire-reports/*.xml'''
}

stage 'parallel SCA and Build'
//sh "/opt/sonar-runner-2.4/bin/sonar-runner -Dproject.settings=/home/1230154/Desktop/jenkins/workspace/Ewallet_Pipeline/src/sonar-project.properties"


parallel(SCA: {
//stage 'SCA'
sh "/Users/Shared/Jenkins/Downloads/sonar-runner-2.4/bin/sonar-runner -Dproject.settings=Ewallet_devops/src/sonar-project.properties"
}, Build: {
//stage 'Build'
withMaven(jdk: 'JAVA_HOME_SYSTEM', maven: 'Maven_master') {
sh '''cd Ewallet_devops
unset JAVA_TOOL_OPTIONS
mvn install -Ptest -Dmaven.test.skip=true'''
}})


stage "Nexus"
nexusArtifactUploader artifacts: [[artifactId: 'Ewallet_devops', classifier: '', file: 'Ewallet_devops/target/Ewallet_devops-0.0.1.war', type: 'war']], credentialsId: 'NexusUser', groupId: 'Ewallet_devops', nexusUrl: '10.136.53.84:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Ewallet_Development', version: '0.0.1'


stage "Deploy to tomcat"
sh 'curl --upload-file Ewallet_devops/target/Ewallet_devops-0.0.1.war "http://admin:admin@10.136.53.10:8080/manager/text/deploy?path=/Ewallet_devops-0.0.1&update=true"'
/*    
}
/*

node('master') {
    // some block

    
stage 'Functional Testing'
dir('FunctionalTesting') {
    // some block


git branch: 'development', credentialsId: '31609829-b9c1-48ef-aeb3-b79e2bdd6b70', url: 'http://10.136.53.84/root/ewallet_testing.git'


withMaven(jdk: 'JAVA_HOME', maven: 'Maven_master') {
    sh 'mvn install'

}
}

    
}

node('master') {
    // some block
    stage "Load Testing"
    dir('LoadTesting') {
    // some block

    git branch: 'development', credentialsId: '31609829-b9c1-48ef-aeb3-b79e2bdd6b70', url: 'http://10.136.53.84/root/ewallet_load_testing.git'
	sleep time: 1, unit: 'MINUTES'

	sh '/Users/Shared/Jenkins/Downloads/apache-jmeter-3.1/bin/jmeter.sh -Jjmeter.save.saveservice.output_format=xml -n -t Ewallet_LT.jmx -l Test.jtl'

performanceReport compareBuildPrevious: false, configType: 'ART', errorFailedThreshold: 0, errorUnstableResponseTimeThreshold: '', errorUnstableThreshold: 0, failBuildIfNoResultFile: false, modeOfThreshold: false, modePerformancePerTestCase: true, modeThroughput: true, nthBuildNumber: 0, parsers: [[$class: 'JMeterParser', glob: '/Users/Shared/Jenkins/Home/workspace/Ewallet_pipeline/LoadTesting/Test.jtl']], relativeFailedThresholdNegative: 0.0, relativeFailedThresholdPositive: 0.0, relativeUnstableThresholdNegative: 0.0, relativeUnstableThresholdPositive: 0.0

    }   
*/
    
}
