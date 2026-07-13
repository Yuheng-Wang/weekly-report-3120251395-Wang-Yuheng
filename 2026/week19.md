# 周报
## 一、当前任务
1）项目：基于具身空间的架空输电线路运检关键技术及装备研究

## 二、本周工作
1）WeKnora本地windows部署

在win10上手动部署WeKnora

环境：win10 专业版

依赖：Docker  WSL  GIT  

步骤：

1.开启Windows虚拟化，并通过PowerShell启动WSL，安装WSL 2 Linux内核更新包

2.安装Docker windows 64位版

3.安装WeKnora

```
git clone https://github.com/Tencent/WeKnora.git
cd WeKnora
cp .env.example .env   # 按需编辑 .env，详见文件内注释,windows中直接复制粘贴重命名为.env文件即可
docker compose up -d   # 启动核心服务
```

三、下周任务

打通实验室模型+WeKnora连接

