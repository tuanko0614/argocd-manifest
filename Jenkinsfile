pipeline {
    agent any

    environment {
        GIT_USERNAME = 'tuandt0614' // Thay bằng tên đăng nhập GitHub của bạn
        DOCKERTAG = 'latest' // Thay bằng tag Docker của bạn hoặc giá trị biến
    }

    stages {
        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        // Sử dụng thông tin đăng nhập loại Secret text
                        withCredentials([string(credentialsId: 'github-token', variable: 'GIT_TOKEN')]) {
                            // Cấu hình Git
                            sh "git config user.email tuanko0614@gmail.com"
                            sh "git config user.name tuandt0614"
                            // Hiển thị nội dung hiện tại của deployment.yaml
                            sh "cat deployment.yaml"
                            // Thay đổi tag hình ảnh trong deployment.yaml bằng lệnh sed
                            sh "sed -i 's+raj80dockerid/test.*+tuandt0614/test:${DOCKERTAG}+g' deployment.yaml"
                            // Hiển thị nội dung đã cập nhật của deployment.yaml
                            sh "cat deployment.yaml"
                            // Stage các thay đổi
                            sh "git add ."
                            // Commit các thay đổi
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            // Push các thay đổi lên GitHub repository sử dụng mã thông báo
                            sh "git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/${GIT_USERNAME}/argocd.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
