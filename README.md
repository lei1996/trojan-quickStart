# trojan-quickStart
Just notes...   

# Env: 
  server: centos7 
  client: macos  ios


# 安装 acme.sh 
curl https://get.acme.sh | sh

# 安装 trojan-gfw
sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"

# 安装 nginx
sudo yum install -y epel-release
sudo yum –y install nginx


# 申请免费证书
cd ~/.acme.sh
acme.sh --issue -d example.com --standalone

# 启动 nginx
sudo systemctl start nginx

# 创建存放 key 目录
mkdir /usr/local/etc/trojan/v1.linairx.xyz

# 安装证书 
acme.sh --install-cert -d v1.linairx.xyz \
--key-file       /usr/local/etc/trojan/v1.linairx.xyz/key.pem  \
--fullchain-file /usr/local/etc/trojan/v1.linairx.xyz/cert.pem 
--reloadcmd     "service nginx force-reload"

# nginx 配置文件 将80的流量转发到443上
server {
    listen 80;

    server_name v1.linairx.xyz;
    return 301 https://$host$request_uri;
}

# trojan 配置文件
# 修改 password  cert key(绝对路径) 三项
/usr/local/etc/trojan/config.json

# 重启 trojan
sudo systemctl restart trojan

# 查看Trojan启动情况
sudo systemctl status trojan

# 开机自动启动 nginx 和 trojan-gfw
sudo systemctl enable trojan nginx

# 在docker 容器里面执行命令
docker exec -ti bce1eda9cfda sh -c "node -v"

# 修复linux 上node 监控文件受限制
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p


参考链接：
https://www.jianshu.com/p/c5dc7b5f5f0d
