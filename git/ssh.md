1. 生成密钥对 ssh-kengen -t rsa -C 'xxx@xxx.com'
2. github里添加一个ssh key
3. 复制~/.ssh/id_rsa.pub内容到2步生成的key里
4. 修改origin url为git协议, git remote set-url origin git@github.com/xxx