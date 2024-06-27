pipeline {
    agent any

    environment {
        GIT_USERNAME = 'tuandt0614' // Tên đăng nhập GitHub của bạn
        DOCKERTAG = 'latest' // Thay bằng tag Docker của bạn hoặc giá trị biến
    }

    stages {
        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        // Sử dụng thông tin đăng nhập loại Secret text
                        withCredentials([string(credentialsId: 'github-token', variable: 'GIT_TOKEN')]) {
                            // Cấu hình Git sử dụng PowerShell
                            powershell """
                            git config user.email tuandt0614@gmail.com
                            git config user.name tuandt0614
                            Get-Content deployment.yaml
                            (Get-Content deployment.yaml) -replace 'raj80dockerid/test.*', 'tuandt0614/test:${DOCKERTAG}' | Set-Content deployment.yaml
                            Get-Content deployment.yaml
                            git add .
                            git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'
                            git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/tuanko0614/argocd.git HEAD:main
                            """
                        }
                    }
                }
            }
        }
    }
}
