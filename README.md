QGISPluginMQTT是一个QGIS插件，用于实现在QGIS里编辑的矢量文件可以实时通过MQTT协议发布，从而让接收MQTT的终端如Cesium得以实时显示编辑效果。
以下为实现上述功能的安装调试指南：

# 步骤1：配置MQTT broker （约需10分钟完成）
1. 从https://mosquitto.org/download/下载mosquitto安装程序并完成安装
2. 在C:\Program Files\mosquitto找到mosquitto.conf文件
3. 用管理员身份启动任意文字处理器如notepad++ （可以鼠标右键点击该文字处理器图标,然后选择更多>管理员权限运行）
4. 在文字处理器中打开mosquitto.conf文件。在文件末尾添加如下命令行
    ```
    listener 1883
    protocol mqtt
    
    listener 9001
    protocol websockets
    
    allow_anonymous true
    ```
5. 保存mosquitto.conf文件。
6. 从Windows里运行command prompt，键入`cd C:\Program Files\mosquitto`
7. 键入`mosquitto -c "C:\Program Files\mosquitto\mosquitto.conf" -v`。注意到mosquitto在端口1883及websocket端口9001聆听讯息
8. (可选）如果想手动测试mosquitto:
   - 在command prompt里键入`mosquitto_sub -h localhost -t qgis/updates`
   - 另开一个command prompt窗口，键入`mosquitto_pub -h localhost -t qgis/updates -m ‘Good luck 2025!’`
   - 如Mosquitto sub的窗口里即时显示”Good luck 2025“则broker建立成功。 
# 步骤2：安装PublishMQTT插件 （QGIS 3.40 以上版本；约需5分钟完成）
1. 在GitHub界面点击`Code`下拉菜单的Local标签组最底部选择Download ZIP下载压缩文件包。
2. 解压文件至任意一个临时文件夹。
3. 在QGIS插件所在文件夹里创建一个命名为PublishMQTT的新文件夹。如果是Windows系统，QGIS插件文件夹通常为： C:/Users/<你的用户名>/AppData/Roaming/QGIS/QGIS3/profiles/default/python/plugins。
4. 拷贝或移动解压后的文件从临时文件夹至C:/Users/<你的用户名>/AppData/Roaming/QGIS/QGIS3/profiles/default/python/plugins/PublishMQTT文件夹。
5. 启动QGIS程序。
6. 在菜单项中点击插件>管理及安装插件
7. 在搜索框里键入”plugin reloader“，点击安装。
8. 重新回到插件>管理及安装插件，在对话框里选择已安装组，可以看到PublishMQTT，勾选该插件。
9. 在工具条中找到Reload plugin按钮，在 reload plugin下拉菜单中选择PublishMQTT。 可以看到QGIS窗口里显示该插件已启动待命。<br/> 
**请注意每次使用该插件要先运行mosquitto, 否则端口未开启，插件启动后会报错。遇到这种情况可以在运行mosquitto后重启QGIS,再运行插件即可。**
# 步骤3：设置 Cesium (约需5分钟完成)
1. 从cesium.com/downloads/下载Cesium JS最新版本并完成安装
2. 在C:/Users/<你的用户名>/AppData/Roaming/QGIS/QGIS3/profiles/default/python/plugins/PublishMQTT文件夹中找到index2.html
3. 移动该文件至CesiumJS安装的文件夹中，通常为C:\Cesium-1.xxx (1.xxx为当前版本号）
4. 在Visual Studio Coder或其他程序编译器中打开C:\Cesium-1.xxx文件目录找到index2.html，右键点击选择open with live server. (如果live server选项不存在，则需在程序编译器的扩展菜单里选择安装live server，因为不同程序编译器设置不同，在此不展开）。Cesium浏览器会在网页打开，当前地址是北加州，可根据所在研究区自行调整。

## 完成以上安装及设置后，在QGIS打开一副矢量图，开始编辑模式，可以发现每次改动都同步显示到Cesium浏览器中。
