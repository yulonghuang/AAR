# AAR


## library Module的gradle中添加以下代码：
```
apply plugin: 'maven'
ext {
    GITHUB_REPO_PATH = "C/AAR"
    PUBLISH_GROUP_ID = 'zhousf.lib'
    PUBLISH_ARTIFACT_ID = 'aar'
    PUBLISH_VERSION = '1.0.1'
}

uploadArchives {
    repositories {
        mavenDeployer {
            //本地maven仓库地址,也可以使用远程maven仓库
            def deployPath = file(project.GITHUB_REPO_PATH)
            repository(url: "file://${deployPath.absolutePath}")
            pom.project {
                groupId project.PUBLISH_GROUP_ID
                artifactId project.PUBLISH_ARTIFACT_ID
                version project.PUBLISH_VERSION
            }
        }
    }
}

// 源代码一起打包
task androidSourcesJar(type: Jar) {
    classifier = 'sources'
    from android.sourceSets.main.java.sourceFiles
}

artifacts {
    archives androidSourcesJar
}
```
## 运行以下命令生成aar文件
```
gradlew uploadArchives
```
![](https://github.com/MrZhousf/AAR/blob/master/pic/1.jpg?raw=true)

## 上传aar文件至GitHub仓库中
![](https://github.com/MrZhousf/AAR/blob/master/pic/2.jpg?raw=true)

## 采用以下方式进行远程aar引用
在项目的gradle中添加：
```
allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://raw.githubusercontent.com/MrZhousf/AAR/master" }
    }
}
```
在需要引用项目的gradle中添加：
```
compile 'zhousf.lib:aar:1.0.1'
```