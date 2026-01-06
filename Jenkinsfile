pipeline {
    agent any 
    stages {
        stage('pre-parallel') {
            steps{
                echo 'Running pre-parallel stage'
            }
        }
        stage('Parallel Pipelines') {
            parallel {
                stage('macOS Pipeline') {
                    agent { label 'macos'}
                    stage('Build') { 
                        steps {
                            sh '/usr/bin/python3 -m py_compile sources/add2vals.py sources/calc.py' 
                            stash(name: 'compiled-results', includes: 'sources/*.py*') 
                        }
                    }
                    stage('Test') { 
                        steps {
                            sh '/Users/jenkins/Library/Python/3.9/bin/py.test --junit-xml test-reports/results.xml sources/test_calc.py' 
                        }
                        post {
                            always {
                                junit 'test-reports/results.xml' 
                            }
                        }
                    }
                    stage('Deliver') { 
                        steps {
                            sh "/Users/jenkins/Library/Python/3.9/bin/pyinstaller --onefile sources/add2vals.py" 
                        }
                        post {
                            success {
                                archiveArtifacts 'dist/add2vals' 
                            }
                        }
                    }
                }
                stage('Debian Pipeline') {
                    agent { label 'debian' }
                    stage('Build') { 
                        steps {
                            sh '/usr/bin/python3 -m py_compile sources/add2vals.py sources/calc.py' 
                            stash(name: 'compiled-results', includes: 'sources/*.py*') 
                        }
                    }
                    stage('Test') { 
                        steps {
                            sh '/Users/jenkins/Library/Python/3.9/bin/py.test --junit-xml test-reports/results.xml sources/test_calc.py' 
                        }
                        post {
                            always {
                                junit 'test-reports/results.xml' 
                            }
                        }
                    }
                    stage('Deliver') { 
                        steps {
                            sh "/Users/jenkins/Library/Python/3.9/bin/pyinstaller --onefile sources/add2vals.py" 
                        }
                        post {
                            success {
                                archiveArtifacts 'dist/add2vals' 
                            }
                        }
                    }
                }
            }
        }
        stage('post-parallel') {
            steps{
                echo 'Running post-parallel stage'
            }
        }
    }
}
