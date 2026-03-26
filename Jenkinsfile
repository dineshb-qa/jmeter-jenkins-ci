pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('JMeter') {
      steps {
        bat '''
            if exist logs rmdir /s /q logs
            if exist html rmdir /s /q html
            mkdir logs
            mkdir html
            mkdir html\\report
            "D:\\JMeter\\apache-jmeter-5.5\\bin\\jmeter.bat" -n -t "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\jmeter-browser-ci\\scripts\\Test_Plan_for_API_Load.jmx" -l logs/results.jtl -e -o html/report
        '''
      }
    }
    stage('Reports') {
      steps {
        perfReport sourceDataFiles: 'logs/results.jtl'
        publishHTML(
          target: [
            reportDir: 'html/report',
            reportFiles: 'index.html',
            reportName: 'JMeter HTML Report',
            keepAll: true,
            alwaysLinkToLastBuild: true,
            allowMissing: false
          ]
        )
      }
    }
  }
  post {
    always {
      archiveArtifacts 'logs/results.jtl, html/report/**'
    }
  }
}
