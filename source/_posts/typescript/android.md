

安装ndk

```sh
sdkmanager 'ndk;21.2.6472646'
```

二、安装android-sdk
1.下载sdk-tools-linux-4333796.zip

```sh
[root@CentOS-Master share]# wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
[root@CentOS-Master share]# unzip sdk-tools-linux-4333796.zip -d android-sdk
[root@CentOS-Master share]# cd android-sdk
[root@CentOS-Master android-sdk]# ls
 tools

```



2.配置android环境变量

```sh
[root@CentOS-Master android-sdk]# vim /etc/profile
```



输入然后保存

```sh
export ANDROID_HOME="/usr/share/android-sdk"
export PATH="$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:$PATH"

```

保存配置，重新刷新配置

```sh
[root@CentOS-Master android-sdk]# source profile
```

安装build-tools，platform-tools，platforms的25.0.2版本
首先运行：sdkmanager –list

```sh
[root@CentOS-Master android-sdk]# sdkmanager --list
```

```sh
sdkmanager "build-tools;29.0.3" "platforms;android-29" "ndk-bundle" "extras;android;m2repository" "extras;google;m2repository" "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.2" "tools"
```


三、安装JDK
下载压缩包并配置环境变量

保存配置，重新刷新配置

```profile
export GRADLE_USER_HOME=/data/.libs/gradle

export JAVA_HOME=/data/services/jdk/11.0.5
export MAVEN_HOME=/data/services/maven/3.6.3
export GRADLE_HOME=/data/services/gradle/6.3

export ANDROID_HOME=/data/services/android

export NODE_HOME=/data/services/nodejs/v12.16.2
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib

export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$MAVEN_HOME/bin:$GRADLE_HOME/bin:$NODE_HOME/bin:$PATH
export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:$PATH
```



[root@CentOS-Master android-sdk]# source profile
1
四、安装Gradle
1.下载Gradle-4.6.all.zip包

[root@CentOS-Master /]# cd /usr/share
[root@CentOS-Master share]# wget https://downloads.gradle.org/distributions/gradle-4.6-all.zip
1
2
2.解压

unzip -D gradle-4.6-all.zip 
1
3.配置环境变量

[root@CentOS-Master android-sdk]# vim /etc/profile
1
输入然后保存

export GRADLE_HOME=/usr/share/gradle-4.6
export PATH=$PATH:$GRADLE_HOME/bin
1
2
保存配置，重新刷新配置

[root@CentOS-Master android-sdk]# source profile
1
五、使用Jenkins配置打包Android工程
系统管理-系统设置， 添加ANDROID_HOME


系统设置-全局工具配置， 添加SDK，Gradle


2.创建任务, 输入任务名称，选择自由风格的软件项目


构建参数

参数化构建过程，我们可以增加一些参数来控制自己的项目的构建，这里对应的变量是工程下面的gradle.properties里面定义的，然后build.gradle脚本会采用这里配置的去动态构建工程，注意一定要与properties文件中一一对应。
如果没有安装Git Parameter 插件，先到系统管理–插件管理安装

源码管理
这个tab下面主要就是用来指定源码存放位置的

构建，选择use gradle wrapper ， 填写Tasks
clean
assemble$BUILD_TYPE
1
2
点开高级：Pass all job parameters as Project properties，一定要勾选这个选项，代表把你上面的那些参数用来当作gradle构建参数，否则的话是不会起作用的。
6.构建后操作，输出文件

————————————————
版权声明：本文为CSDN博主「朱建伟」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weijizhu1000/java/article/details/95752438