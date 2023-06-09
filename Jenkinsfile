pipeline {
    agent any
    parameters {
        string(name: 'USERNAME', description: 'Username of the user whose password needs to be reset')
        string(name: 'MYSQL_USER', description: 'MySQL user with privileges to access the WordPress database')
        string(name: 'MYSQL_PASSWORD', description: 'Password for the MySQL user')
        string(name: 'MYSQL_HOST', description: 'Hostname or IP address of the MySQL server')
        string(name: 'MYSQL_DATABASE', description: 'Name of the WordPress database')
        string(name: 'TABLE_PREFIX', defaultValue: 'wp_', description: 'Prefix of WordPress database tables')
    }
    stages {
        stage('Check MySQL client') {
            steps {
                script {
                    def osName = sh(script: "uname -s", returnStdout: true).trim()
                    if (osName == 'Linux') {
                        sh 'dpkg -s mysql-client || (sudo apt-get update && sudo apt-get install mysql-client -y)'
                    } else {
                        error "Unsupported operating system: ${osName}"
                    }
                }
            }
        }
        stage('Reset password') {
            steps {
                script {
                    def newPassword = generateRandomPassword()
                    def mysqlPassword = md5(newPassword)
                    def query = "UPDATE ${env.TABLE_PREFIX}users SET user_pass='${mysqlPassword}' WHERE user_login='${env.USERNAME}';"
                    sh "mysql -u ${MYSQL_USER} -p${MYSQL_PASSWORD} -h ${MYSQL_HOST} ${MYSQL_DATABASE} -e \"${query}\""
                    echo "New password for user ${env.USERNAME} is: ${newPassword}"
                }
            }
        }
    }
}

def generateRandomPassword() {
    def randomString = sh(script: "cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 8 | head -n 1", returnStdout: true).trim()
    return randomString
}

def md5(String password) {
    def command = "echo -n '${password}' | md5sum | awk '{print \$1}'"
    return sh(script: command, returnStdout: true).trim()
}
