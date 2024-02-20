pipeline {
   stage('打包') {
      steps {
        // 执行环境，nodejs-12.16是我们刚配置的别名，还有一种方式是在agent中配置执行环境，在tools中配置使用的包，感兴趣的可以自行研究
        nodejs('nodejs-18.18') {
          echo '开始安装依赖'
          sh 'yarn'
          echo '开始打包'
          sh 'yarn run build'
        }
      }
    }
}
