import java.nio.file.Paths
@NonCPS
def printParams() {
  env.getEnvironment().each { name, value -> println "Name: $name -> Value $value" }
}
printParams()
node {
    stage("configure conan"){
        client = Artifactory.newConanClient()
        source_path = Paths.get("$env.WORKSPACE", "tmp", "source").toString()
        build_path = Paths.get("$env.WORKSPACE", "tmp", "build").toString()   
        user = "fj_asap"
        channel = "testing"
    }
    stage('source') { // for display purposes
    def source_folder = new File(source_path)
    if (source_folder.exists()){
        println "${source_path} already exists -> Nothing to do in this stage"
    }
    else{
        bat 'conan source . --source-folder=${source_path}'
    }
   }
   stage('build'){
       println "source-pfad ist: ${source_path}"
       bat "conan install . --install-folder=tmp/build"
       bat "conan build . --source-folder=${source_path} --build-folder=${build_path}"
       bat "conan package . --source-folder=${source_path} --build-folder=${build_path} --package-folder=tmp/package"
       bat "conan export-pkg . ${user}/${channel} --source-folder=${source_path} --build-folder=${build_path} -f"
   }
   stage('test'){
       bat "conan test test_package Hello/1.1@${user}/${channel}"
   }

}