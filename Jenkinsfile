
def runCommand = { strList ->
  assert ( strList instanceof String ||
           ( strList instanceof List && strList.each{ it instanceof String } ) \
)
  def proc = strList.execute()
  proc.in.eachLine { line -> println line }
  proc.out.close()
  proc.waitFor()

  print "[INFO] ( "
  if(strList instanceof List) {
    strList.each { print "${it} " }
  } else {
    print strList
  }
  println " )"

  if (proc.exitValue()) {
    println "gave the following error: "
    println "[ERROR] ${proc.getErrorStream()}"
  }
  assert !proc.exitValue()
}



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
     
        def covint = workspace+"\\covint"
     
        def fileList = file1.getCanonicalPath()
        println "Using File List {fileList}
        file1 << myFiles
     
        buildCommand = ["C:\\Coverity\\cov-analysis-win64-2018.06\\bin\\cov-build", "--dir", covint, "--fs-capture-list", fileList, "--no-command"]
        runCommand(buildCommand)

        analysisCommand = ["C:\\Coverity\\cov-analysis-win64-2018.06\\bin\\cov-run-desktop", "--dir", covint, "--host", "localhost", "--user", "admin", "--password", "SIGpass8!", "--stream", "Empty", fileList]
        runCommand(analysisCommand)
    }
}
