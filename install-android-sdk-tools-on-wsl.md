# 在WSL下安装Android-Sdk-Tools

安装android studio必须有桌面环境，因此wsl（windows subsystem linux）无法安装linux版本的android studio。

尝试了去在wsl下安装linux桌面，结果还是不行。因此只能使用google提供的android-sdk-tools手动安装。

## 安装

### 1. 安装Node.js

最好使用LTS版本的node，如12.14.0避免出现各种诡异的问题。

使用nvm进行node版本管理。
```
nvm install 12.14.0
nvm use 12.14.0
```

### 2. 安装android-sdk-tools

```shell
cd ~
sudo apt-get install unzip zip
wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
unzip sdk-tools-linux-4333796.zip -d Android
rm sdk-tools-linux-4333796.zip
sudo apt-get install -y lib32z1 openjdk-8-jdk
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
printf "\n\nexport JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64\nexport PATH=\$PATH:\$JAVA_HOME/bin" >> ~/.bashrc
cd Android/tools/bin
# android-28对应android 9平台，android-26对应android 8平台，可以根据需要自行更改
./sdkmanager --install "platform-tools" "platforms;android-28" "build-tools;28.0.3"

export ANDROID_HOME=~/Android
export PATH=$ANDROID_HOME/tools:$PATH
export PATH=$ANDROID_HOME/tools/bin:$PATH
export PATH=$ANDROID_HOME/platform-tools:$PATH
printf "\n\nexport ANDROID_HOME=~/Android\nexport PATH=\$PATH:\$ANDROID_HOME/tools\nexport PATH=\$PATH:\$ANDROID_HOME/platform-tools" >> ~/.bashrc
sdkmanager --update
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install gradle 6.2
gradle -v

# run on windows in powershell
# adb start-server
```

### 3. 配置环境变量

2中的脚本已经有了，切记**不要在wsl中使用react-native文档里面的配置**

```
# 不要这样配置，会导致wsl中环境变量与windows原生环境的环境变量冲突
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
# export PATH=$PATH:$ANDROID_HOME/emulator  wsl中没有emulator

# 应该这样，先只用wsl中的配置，再使用系统环境变量
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$ANDROID_HOME/tools:$PATH
export PATH=$ANDROID_HOME/tools/bin:$PATH
export PATH=$ANDROID_HOME/platform-tools:$PATH
```

Reference:
1. https://gist.github.com/jjvillavicencio/18feb09f0e93e017a861678bc638dcb0
2. https://askubuntu.com/questions/1189343/can-i-run-android-studio-on-windows-10-subsystem-linux
