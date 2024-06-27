pipeline {
    agent any

    environment {
        GIT_USERNAME = 'tuandt0614'
        DOCKERTAG = 'latest' // Thay bằng tag Docker của bạn hoặc giá trị biến
    }

    stages {
        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([string(credentialsId: 'github-token', variable: 'GIT_TOKEN')]) {
                            // Cấu hình Git
                            bat """
                            git config user.email tuandt0614@gmail.com
                            git config user.name tuandt0614
                            """
                            // Hiển thị nội dung hiện tại của deployment.yaml
                            bat "type deployment.yaml"
                            // Thay đổi tag hình ảnh trong deployment.yaml bằng lệnh sed
                            bat """
                            powershell -Command "(Get-Content deployment.yaml) -replace 'raj80dockerid/test.*', 'tuandt0614/test:${DOCKERTAG}' | Set-Content deployment.yaml"
                            """
                            // Hiển thị nội dung đã cập nhật của deployment.yaml
                            bat "type deployment.yaml"
                            // Stage các thay đổi
                            bat "git add ."
                            // Commit các thay đổi
                            bat "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            // Push các thay đổi lên GitHub repository sử dụng mã thông báo
                            bat "git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/${GIT_USERNAME}/argocd.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
