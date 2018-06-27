def showChangeLogs() {
 def changedFiles = [];
 def fileChange = false
 def changeLogSets = currentBuild.changeSets
 for (int i = 0; i < changeLogSets.size(); i++) {
  def entries = changeLogSets[i].items
  for (int j = 0; j < entries.length; j++) {
   def entry = entries[j]
   echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
   def files = new ArrayList(entry.affectedFiles)
   for (int k = 0; k < files.size(); k++) {
    def file = files[k]
    echo "  ${file.editType.name} ..File Path - ${file.path}"
    if (file.path.endsWith(".java"))
    {
     fileChange = true
    }
    changedFiles += file.path
   }
  }
 }
 return changedFiles
}

node {
    stage('Checkout') {
        checkout scm
    }


    stage('Build') {
        myFiles = showChangeLogs()
        println "myFiles ${myFiles}"
     
        def workspace = pwd()
     
        def file1 = new File("${workspace}\\files.txt")
     
        println new File(".").getCanonicalPath()
        file1 << myFiles
    }
}
