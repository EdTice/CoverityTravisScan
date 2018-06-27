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
     
        def file1 = new File('files.txt')
        println file1.name
        println file1.path
     
        println new File(".").getCanonicalPath()
        file1 << myFiles
    }
}
