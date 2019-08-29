#!/usr/bin/env groovy

import java.util.Properties;

void thigModulesProcessor() {
	
	String projectName = "${PROJECT_NAME}"
	String moduleNameListStr="";
	println("******************************************************************************************************************************")
	println("Option Selected: "+projectName)
    println("******************************************************************************************************************************")
	if (projectName.equals("default")){
		sh "cat nextgenyamlfile.conf"
		sh mvn -version

		moduleNameListStr = readFile "nextgenyamlfile.conf"
	}else{
		moduleNameListStr = projectName
	}

	def bufferModulesList = moduleNameListStr.split()
    def properties = readProperties file: 'nextgenyaml.properties'
	String artifactVersion = "${ARTIFACT_VERSION}"
	
	
	for (String fileModule: bufferModulesList) {
		String tempProject = fileModule.replace("./", "").replace(".yaml", "")
		
		String packageName = properties."${tempProject}.packageName"
		println("******************************************************************************************************************************")
		println("Project artifactName : "+tempProject)
		println("Project artifactVersion :" + artifactVersion)
		println("Project packageName :"+ packageName)
		println("******************************************************************************************************************************")
		
		if (!packageName.equals("") && packageName!=null) {
			sh "mvn generate-sources -DyamlFileName=${tempProject}.yaml -DprojectName=${tempProject} -DpackageName=${packageName} -f pom.xml"
			sh "echo DEBUG maven source generated"
			sh "sed -i s/1.0.0-SNAPSHOT/${artifactVersion}/g yaml-to-java/${tempProject}-javacode/pom.xml"
			sh "mvn clean package -f yaml-to-java/${tempProject}-javacode/pom.xml -DartifactVersion=${artifactVersion} -DmissedCount=100 -DskipTests=true"
			println("******************************************************************************************************************************")
			println("Build Successfull for "+tempProject)
			println("******************************************************************************************************************************")
		} else{
			println("******************************************************************************************************************************")
			println("Build  for "+tempProject)
			println("******************************************************************************************************************************")
		}
	}
}



pipeline {
	agent any
		
	parameters {
		string(name: 'ARTIFACT_VERSION', defaultValue: '1.0.0-SNAPSHOT',  description: 'ARTIFACT_VERSION to Build')
		string(name: 'PROJECT_NAME', defaultValue: 'default',  description: 'PROJECT_NAME to Build')
		}
	
		stages {
		stage('Process') {
			steps { thigModulesProcessor() }
		}
	}
	

}
