## Digital Forensics , Part 5 (Windows Registry Forensics)

### What Is the Registry? [^1]

### Hives [^2]

- `HKEY_USERS`：包含所有已加载的用户配置文件
- `HKEY_CURRENT_USER`：当前登录用户的个人资料
- `HKEY_CLASSES_ROOT`：用于打开文件的应用程序的配置信息
- `HKEY_CURRENT_CONFIG`：启动时系统的硬件配置文件
- `HKEY_LOCAL_MACHINE`：配置信息，包括硬件和软件设置

### Registry Structure [^3]

### Accessing the Registry

`regedit`

### Information in the Registry with Forensic Value

- 用户和他们上次使用系统的时间
- 最近使用的软件
- 安装到系统的任何设备，包括闪存驱动器，硬盘驱动器，电话，平板电脑等可由唯一标识符标识的设备。
- 系统连接到特定的无线接入点的时间
- 访问文件的内容和时间
- 系统上执行所有搜索的列表
- ...

### Wireless Evidence in the Registry

`HKEY_LOCAL_MACHINE \ SOFTWARE \ Microsoft \ Windows NT \ CurrentVersion \ NetworkList \ Profiles`

---

### 示例：DateLastConnected字段REG_BINARY值转换成日期

- 位置： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles`
- 值： `DA 07 03 00 03 00 18 00 19 00 18 00 21 00 2d 01`

### 转换步骤

1. Little Endian 转换成 Big Endian

   `07 DA 00 03 00 03 00 18 00 19 00 18 00 21 01 2d`

2. The **year** value = 07 DA = 2010 

   (Ignore last hex value which is [A] in the first two bytes)

3. The **month** = 00 03 = March  [^4]

  (1= January, 2= February ...etc)

4. The **weekday** = 00 03 = Wednesday
  (0 = Sunday, 1 = Monday ...etc)

5. The **date** = 00 18 = 18th

6. The **hour** = 00 19 = 19:00 or 7:00 pm

7. The **minutes** = 00 18 = 18 minutes

8. The **second** = 00 21= 21 seconds 

Ref: [Forensic Analysis of the Windows 7 Registry](https://ro.ecu.edu.au/adf/72/)



[原文](https://null-byte.wonderhowto.com/how-to/hack-like-pro-digital-forensics-for-aspiring-hacker-part-5-windows-registry-forensics-0160561/)

---

[^1]: 注册表是存储有关Windows系统上的用户，硬件和软件的配置信息的数据库。尽管注册表是为了配置系统而设计的，但它会跟踪有关用户活动，连接到系统的设备，使用的软件以及何时使用的大量信息等等。所有这些对于取证人员来说都很有用，即可以跟踪调查人员，内容，地点和时间。
[^2]: 在注册表中，有根文件夹。这些根文件夹称为配置单元。有五个注册表配置单元。
[^3]: 注册表的结构与Windows目录/子目录结构非常相似。它有五个根键或配置单元，然后是子键。在某些情况下，您有子项。然后，这些子项具有显示在内容窗格中的描述和值。通常，值只是0或1，表示打开或关闭，但也可以包含通常以十六进制显示的更复杂信息
[^4]: Each two bytes has a corresponding value for the year, month, date, hour, minute and second.