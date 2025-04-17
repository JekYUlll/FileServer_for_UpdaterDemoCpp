/*
    2025/04/17
    张祝玙
*/

注意：当前文件均为测试用，Updates.xml与实际组件不存在对应关系，不同组件、不同版本间也不存在严格逻辑。

.sha1 校验和为真实生成，使用包为 golang 的"crypto/sha1"。

---

参考：QtIFW使用编译出的`repogen`工具来生成仓库，结构如下：

repository/
├── Updates.xml                 # 仓库主索引文件
├── [componentname]/            # 每个组件有独立目录
│   ├── [version]/              # 每个版本的目录
│   │   ├── [archivename].7z    # 组件内容存档
│   │   └── [archivename].7z.sha1 # 内容校验和
│   └── [version]-meta.7z       # 组件元数据存档
└── components.xml              # 组件清单文件(可选)

实际的结构示例：

repository/
├── Updates.xml
├── com.example.component1/
│   ├── 1.0.0/
│   │   ├── content.7z
│   │   └── content.7z.sha1
│   └── 1.0.0-meta.7z
├── com.example.component2/
│   ├── 2.1.0/
│   │   ├── content.7z
│   │   ├── content.7z.sha1
│   │   ├── resources.7z
│   │   └── resources.7z.sha1
│   └── 2.1.0-meta.7z
└── ...

每个`[version]-meta.7z`存档包含：

meta/
├── package.xml          # 组件描述文件
├── installscript.qs     # 安装脚本(如有)
├── *.qm                 # 翻译文件(如有)
├── *.ui                 # 自定义界面文件(如有)
├── license.txt          # 许可证文件(如有)
└── ...                  # 其他元数据

---

对每个需要更新的组件，先下载其meta包，读取其中的package.xml

package.xml(QtIFW中有很多隐含/默认行为，此处展示完整标签):

<?xml version="1.0" encoding="UTF-8"?>
<Package>
    <Name>com.example.component2</Name>
    <DisplayName>Component 2</DisplayName>
    <Description>Example component A</Description>
    <Version>1.0.1</Version>
    <ReleaseDate>2020-01-01</ReleaseDate>
    <Default>true</Default>
    <Script>installscript.qs</Script>
    <Dependencies>com.example.core</Dependencies>
    <DownloadableArchives>content.7z,resources.7z</DownloadableArchives>
</Package>

应用程序目录的components.xml:

<Packages>
    <ApplicationName>Online Installer Example</ApplicationName>
    <ApplicationVersion>1.0.0</ApplicationVersion>
    <Package>
        <Name>A</Name>
        <Title>A Title</Title>
        <Description>Example component A</Description>
        <Version>1.0.2-1</Version>
        <LastUpdateDate></LastUpdateDate>
        <InstallDate>2020-02-13</InstallDate>
        <Size>74</Size>
        <Checkable>true</Checkable>
    </Package>
    <Package>
        <Name>B</Name>
        <Title>B Title</Title>
        <Description>Example component B</Description>
        <Version>1.0.0-1</Version>
        <LastUpdateDate></LastUpdateDate>
        <InstallDate>2020-02-13</InstallDate>
        <Size>74</Size>
        <Checkable>true</Checkable>
    </Package>
</Packages>

