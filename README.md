# 使用说明

```bash
git clone https://github.com/youyve/clash-linux-cli-backup.git
mv clash-linux-cli-backup clash
cd clash

chmod +x bin/clash*

# 使用默认配置文件config.yaml（注意替换为自己的节点配置）
./bin/clash-linux-arm64 -d .
or
# 指定特定的yaml配置文件
./bin/clash-linux-arm64 -f path/to/the/yaml
```

运行成功后输出一下内容

![image-20241128175902413](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241128175902413.png)

打开一个新终端

```bash
# 若自行更改了port号，就将7890改为对应端口号
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890

# 测试
curl google.com
curl huggingface.co
# 除了可以用curl测试外还可以用wget进行测试
# 注意⚠️不要用ping来测试，因为ping走的是icmp协议，而代理走的是tcp或者udp
```

测试成功后，在运行clash的终端中会有显示：

![image-20241128175528541](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241128175528541.png)



# FQA

Question1：配置文件的格式问题

![image-20241129151923920](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241129151923920.png)

若运行后没有出现且代理无法正常运行，则有可能是config.yaml文件格式不正确。

假设当前yaml配置文件中已经包含了节点订阅的信息

尝试复制该项目中的config.yaml文件中位于关键词 `proxies` 之前的配置，并粘贴到自己的yaml配置文件中替换掉原本位于关键词 `proxies` 之前的内容，这样应该就能解决了。

![image-20241129153551063](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241129153551063.png)

Question2：如何手动选择节点的问题

![image-20241129153905179](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241129153905179.png)

在配置文件中找到 `proxy-groups` 关键词，更改 `proxies` 关键词下的节点顺序即可完成指定节点的选择，具体来说就是哪个节点排在第一就默认连接哪个节点，默认情况下使用的机场设置的自动选择的节点。

Question3：端口被占用的问题

![image-20241128174753085](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241128174753085.png)

若出现上述端口已经被占用的情况

先确认端口是否真的被占用

```bash
netstat -tln | grep -E '9090|789.'
```

![image-20241128175033887](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241128175033887.png)

找出占用端口的程序并解除占用

```bash 
netstat -tlnp | grep -E '7890|7891|7892|9090'

kill -9 6516
```

![image-20241128175129650](/Users/youlz/Library/Mobile Documents/com~apple~CloudDocs/GML/Projs/clash/assets/image-20241128175129650.png)





参考项目：

https://github.com/yhdxtn/clash-linux-arm64

https://www.clash.la/releases/

https://github.com/Elegycloud/clash-for-linux-backup