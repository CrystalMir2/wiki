# Crystal on Linux

install Crystal  mir server  on linux 

在Linux上发布Craystal Mir 服务器

## .Net7
#### install
install .net 7 sdK on ubuntu 20.04 lts x64

Installing with APT can be done with a few commands. Before you install .NET, 
run the following commands to add the Microsoft package signing key 
to your list of trusted keys and add the package repository.

使用 APT 安装可以通过几个命令完成。在安装 .NET 之前，
请运行以下命令，将 Microsoft 包签名密钥添加到受信任密钥列表并添加包存储库。

Open a terminal and run the following commands:

打开终端并运行以下命令：

```shell
#add signing key for 
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

#install sdk 
sudo apt-get update 
sudo apt-get install -y dotnet-sdk-7.0

```

#### check

Use the dotnet --list-sdks and dotnet --list-runtimes commands to see which versions are installed. 
For more information, see How to check that .NET is already installed.

使用 dotnet --list-sdks and dotnet --list-runtimes 命令查看安装了哪些版本。
有关详细信息，请参阅如何检查是否已安装 .NET。

```shell
dotnet --list-sdks
#out print: 7.0.404 [/usr/share/dotnet/sdk]
```

#### FAQ 
get help or install other versions
[Help](https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu-2004)
[Other Version](https://learn.microsoft.com/en-us/dotnet/core/install/linux)


## IDE(optional)
install `rider` or `vs code` for developer (optional)

#### rider download
rider 2022.3.3 
[download](https://download-cdn.jetbrains.com/rider/JetBrains.Rider-2022.3.3-aarch64.tar.gz?_gl=1*qxwgxl*_ga*MTI0Mzc3NjI2NC4xNzAxNDgzMDk1*_ga_9J976DJZ68*MTcwMTQ4MzA5NS4xLjEuMTcwMTQ4MzI1NC4xMy4wLjA.&_ga=2.139501811.1763692958.1701483095-1243776264.1701483095)

#### jdk 17
install openjdk-17-jdk 17 for rider

```shell
#install jdk 17
sudo apt install openjdk-17-jdk
#check java version
java -version
#openjdk version "17.0.9" 2023-10-17
#OpenJDK Runtime Environment (build 17.0.9+9-Ubuntu-120.04)
#OpenJDK 64-Bit Server VM (build 17.0.9+9-Ubuntu-120.04, mixed mode, sharing)
```
#### vscode 
[link](https://learn.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code?pivots=dotnet-7-0)

## build 

#### source code
git clone(download) source code

```shell
sudo apt install git
git clone https://github.com/Suprcode/mir2.git
cd mir2
```

#### console

We have to create console application, case by Winform Not supported on Linux.

我们必须创建控制台应用程序，在 Linux 上不支持 Winform。

The console project template creates a simple application that displays `"Hello, World"`
in the console window by calling the `Console.WriteLine(String)` method in Program.cs.

项目模板通过调用 Program.cs 中的 Console.WriteLine（String） 方法，
创建一个简单的应用程序，该应用程序在控制台窗口中显示“Hello， World”。

```shell
mkdir MirServerConsole
cd MirServerConsole
dotnet new console  -f net7.0
dotnet run --project  MirServerConsole.csproj 
#Hello, World!
```

#### publish and run 
```shell
dotnet publish --configuration Release
./bin/Release/net7.0/MirServerConsole
#Hello, World!
```

编辑 Console.csproj 文件, 关联Server Library porject 的源代码

Edit the `Console.csproj` file to associate the source code of the Server Library porject.

``` shell
gedit  MirServerConsole.csproj
```

```xml

<Project Sdk="Microsoft.NET.Sdk">

    <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>net7.0</TargetFramework>
        <Nullable>enable</Nullable>
        <SatelliteResourceLanguages>Chinese</SatelliteResourceLanguages> 
        <OutputPath>..\Build\Server</OutputPath>
        <AppendTargetFrameworkToOutputPath>false</AppendTargetFrameworkToOutputPath> 
        <AppendRuntimeIdentifierToOutputPath>false</AppendRuntimeIdentifierToOutputPath> 

        
    </PropertyGroup>
    <ItemGroup>
      <ProjectReference Include="..\Server\Server.Library.csproj" />
      <ProjectReference Include="..\Shared\Shared.csproj" />
    </ItemGroup>

    <ItemGroup>
      <PackageReference Include="log4net" Version="2.0.15" />
    </ItemGroup>

</Project>

```

#### nuget restore 

使用nuget 更新依赖包

```shell

dotnet restore --verbosity normal

```

如果遇到如下错误, 

```txt
 error NU1301: The local source '/home/xxxx/xxxx/mir2-source/.\Packages' doesn't exist.
```

修改./NuGet.config中的
``` xml
        <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
        <add key="NuGet Offline Repository" value=".\Packages" />
```
to (为)

``` xml
        <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
        <!-- <add key="NuGet Offline Repository" value="./Packages" /> -->
```

重建nuget配置文件:

```shell
dotnet new nugetconfig
dotnet restore
```

#### publish and run again

再一次编译,发布并运行二进制

``` shell
dotnet publish --configuration Release
./bin/Release/net7.0/MirServerConsole
```


Good luck 

----
> QQ Group: 714472073
