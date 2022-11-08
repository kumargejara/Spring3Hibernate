node {
    stage("Cloning Spring3Hibernate Project") {
        git branch: 'main', credentialsId: 'GithubConnection', url: 'https://github.com/kumargejara/Spring3Hibernate'
    }
    stage("Code Stability") {
        sh "/opt/maven/bin/mvn clean install"
    }
    stage("Code Quality") {
        sh "/opt/maven/bin/mvn checkstyle:checkstyle"
        recordIssues(tools: [checkStyle(pattern: '**/checkstyle-result.xml')])
    }
    stage("Unit Testing") {
        sh "/opt/maven/bin/mvn test"
        recordIssues(tools: [junitParser(pattern: 'target/surefire-reports/*.xml')])
    }
    stage("Security Testing") {
        sh "/opt/maven/bin/mvn org.owasp:dependency-check-maven:check"
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target', reportFiles: 'dependency-check-report.html', reportName: 'Dependency Check Report', reportTitles: ''])
    }
    stage("Sonarqube Analysis") {
        sh "/opt/maven/bin/mvn sonar:sonar -Dsonar.host.url=${SONAR_URL} -Dsonar.login=${SONAR_USER} -Dsonar.password=${SONAR_PASSWORD} -Dsonar.java.binaries=."
    }
}
