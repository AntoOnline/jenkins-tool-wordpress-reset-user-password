pipeline {
  parameters {
    string(name: 'USERNAME', description: 'Username of the user whose password needs to be reset')
    string(name: 'MYSQL_USER', description: 'MySQL user with privileges to access the WordPress database')
    password(name: 'MYSQL_PASSWORD', description: 'Password for the MySQL user')
    string(name: 'MYSQL_HOST', description: 'Hostname or IP address of the MySQL server')
    string(name: 'MYSQL_PORT', defaultValue: '3306', description: 'Port number on which the MySQL server is listening')
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
        withCredentials([usernamePassword(credentialsId: 'mysql-creds', usernameVariable: 'MYSQL_USER', passwordVariable: 'MYSQL_PASSWORD')]) {
          script {
            def newPassword = generateRandomPassword()
            def mysqlPassword = md5(newPassword)
            def query = "UPDATE ${env.TABLE_PREFIX}users SET user_pass='${mysqlPassword}' WHERE user_login='${env.USERNAME}';"
            sh "mysql -u ${MYSQL_USER} -p${MYSQL_PASSWORD} -h ${MYSQL_HOST} -P ${MYSQL_PORT} ${MYSQL_DATABASE} -e \"${query}\""
            echo "New password for user ${env.USERNAME} is: ${newPassword}"
          }
        }
      }
    }
  }
}

def generateRandomPassword() {
  def randomString = new Random().with {
    StringBuilder sb = new StringBuilder()
    (1..8).each {
      sb.append((nextInt(26) + 97) as Character)
    }
    return sb.toString()
  }
  return randomString
}

def md5(String password) {
  MessageDigest.getInstance("MD5").digest(password.getBytes()).collect { String.format("%02x", it) }.join()
}
