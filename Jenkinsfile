pipeline {
    agent {
        label 'master'
    }
    environment {
        CONAN_STABLE_BRANCH_PATTERN = 'release/*'
        CONAN_BUILD_TYPES = 'Release'
        CONAN_ARCHS = 'x86_64'
    }
    stages {
        stage('Build') {
            failFast true
            parallel {
                stage('macOS') {
                    agent {
                        label 'macOS'
                    }
                    environment {
                        CONAN_APPLE_CLANG_VERSIONS = '11.0'
                        CONAN_OS_VERSIONS='10.13'
                        CONAN_USER_HOME = "${env.WORKSPACE}"
                    }
                    steps {
                        sh '''
                            conan remove -f --locks
                            conan remove -f "*"
                            python3 "$PWD/build.py"
                        '''
                    }
                    post {
                        always {
                            sh '''
                                conan remove -f "*"
                                rm -fr "$CONAN_USER_HOME/.conan"
                            '''
                        }
                    }
                }
                stage('Windows') {
                    agent {
                        label 'Windows'
                    }
                    environment {
                        CONAN_BASH_PATH = 'C:\\Program Files\\Git\\usr\\bin\\bash.exe'
                        CONAN_USER_HOME = "${env.WORKSPACE}"
                        CONAN_VISUAL_RUNTIMES = 'MD,MDd'
                        CONAN_VISUAL_VERSIONS = '16'
                    }
                    steps {
                        sh '''
                            conan remove -f --locks
                            conan remove -f "*"
                            python "$PWD/build.py"
                        '''
                    }
                    post {
                        always {
                            sh '''
                                conan remove -f "*"
                                rm -fr "$CONAN_USER_HOME/.conan"
                            '''
                        }
                    }
                }
            }
        }
    }
}
