# docker

## Docker Hub：

		docker login 登录dockerhub的账号
		
        docker logout 退出账户
		
		docker push   
		
        docker pull 

## 镜像制作：
		
        基于容器制作
			
            在容器中完成操作后制作；
		
        基于镜像制作
			
            编辑一个Dockerfile，而后根据此文件制作；
			
		基于容器制作：
			
            docker commit 
				
                docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
					
                    --author, -a 提交的镜像作者；
					
                    --pause, -p 在commit时，将容器暂停
					
                    --message, -m 提交时的说明文字；
					
                    --change, -c 使用Dockerfile指令来创建镜像


* 制作新镜像并上传到自己的dockerhub上：

    docker pull centos：7 拉取一个centos镜像标签为7

    docker run -it centos:7 /bin/bash 这样就可以交互啦，进入容器执行命令

    在容器里以yum install -y vim 为例安装vim命令

    安装好之后exit退出

    容器打包

        docker commit 容器id centos：vim 

    还可以在里面加-a作者信息等


    为了上传还需要在刚刚的镜像上打一个标签

        docker tag centos:vim 注册用户名/centos:vim

        docker push 注册用户名/镜像名   上传到自己的dockerhub上
