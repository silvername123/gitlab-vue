// 前端项目JenkinsFile配置，后端项目配置稍有不同，后面会区分说明
pipeline {
  agent any
  environment {
    HOST_TEST = 'github.com'
    HOST_ONLINE = 'jenkins@40.101.219.110'
    SOURCE_DIR = 'dist/*'
    TARGET_DIR = '/data/www/kuaimen-yunying-front'
  }
  parameters {
    choice(
      description: '你需要选择哪个环境进行部署 ?',
      name: 'env',
      choices: ['测试环境', '线上环境']
    )    
    string(name: 'update', defaultValue: '', description: '本次更新内容?')      
  }
  triggers {
    GenericTrigger(
     genericVariables: [
      [key: 'ref', value: '$.ref']
     ],
     causeString: 'Triggered on $ref',
     token: 'runcenter-front-q1w2e3r4t5',
     tokenCredentialId: '',
     printContributedVariables: true,
     printPostContent: true,
     silentResponse: false,
     regexpFilterText: '$ref',
     regexpFilterExpression: 'refs/heads/' + BRANCH_NAME
    )
  } 
  stages {
    stage('获取git commit message') {
     steps {
       script {
         env.GIT_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
       }
     }
  }
    
    stage('打包') {
      steps {
        nodejs('nodejs-12.16') {
          echo '开始安装依赖'
          sh 'yarn'
          echo '开始打包'
          sh 'yarn run build'
        }
      }
    }

    stage('部署') {
      when {
        expression {
          params.env == '测试环境'
        }
      }
      steps {
        sshagent(credentials: ['km-test2']) {
          sh "ssh -o StrictHostKeyChecking=no ${HOST_TEST} uname -a"
          sh "scp -r ${SOURCE_DIR} ${HOST_TEST}:${TARGET_DIR}"
          sh 'echo "部署成功~"'
        }
      }
    }

    stage('发布') {
      when {
        expression {
          params.env == '线上环境'
        }
      }
      steps {
        sshagent(credentials: ['km-online']) {
          sh "ssh -o StrictHostKeyChecking=no ${HOST_ONLINE} uname -a"
          sh "scp -r ${SOURCE_DIR} ${HOST_ONLINE}:${TARGET_DIR}"
          sh 'echo "发布成功~"'
        }
      }
    }
  }

}
