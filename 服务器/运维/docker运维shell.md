```bash
#!/bin/sh
clear;

dockerUID=1000      # 用于指定容器的用户
dockerGID=1000      # 用于指定容器的组

directories=();                # 项目映射的目录，如（日志/数据），脚本会自动创建这些目录
needCheckFilesPaths=();        # 需要检查的文件

# 需要创建的目录
directories+=('/projects/ziyifarm/log_app/logs_api');

# 需要检查的文件（如配置文件）
needCheckFilesPaths+=('/projects/ziyifarm/conf/redis.conf');  # Redis 配置文件


# 项目目录检查
workdir=$(cd $(dirname $0); pwd);
read -p "请确认项目目录是: ${workdir} 吗 (y/n)?" choice
if [ "$choice" != "y" ]; then
    echo "取消执行";
    echo -e "\033[31m脚本已结束.\033[0m";
    exit;
fi

# 检查 compose.yml 文件。
if [ ! -f ${workdir}/docker-compose.yml ];then
    echo "${workdir}/docker-compose.yml 文件不存在";
    echo -e "\033[31m脚本已结束.\033[0m";
    exit;
else
    echo "yes";
fi

# ==================================================================================  检查文件
echo "检查 conf 文件是否存在,${#needCheckFilesPaths[@]}个文件需要检查";
for(( i=0;i<${#needCheckFilesPaths[@]};i++)) do
    filePath=${needCheckFilesPaths[i]};
    echo -n "${filePath}:";
    if [ ! -f ${filePath} ];then
        echo "no";
        echo -e "\033[31m结束脚本.\033[0m"
        exit;
    else
        echo "yes";
    fi
done;


# ==================================================================================  创建文件夹
echo "=========================================";
echo "开始创建文件夹"
for(( i=0;i<${#directories[@]};i++)) do
    directory=${directories[i]};
    echo -n "${directory}:";
    if [ ! -d ${directory} ];then
        mkdir -p ${directory};
        echo -e "\033[31m创建\033[0m";
    else
        echo "已经存在";
    fi
done;


# ================================================================================== 启动项目
echo "启动前检查";
export dockerUID=$dockerUID;
export dockerGID=$dockerGID;
echo "当前用户:`id`";

echo "dockerUID:${dockerUID},dockerGID:${dockerUID}";

chown -R ${dockerUID}:${dockerGID} ${workdir}
chmod 770 -R ${workdir}

docker-compose -f ${workdir}/docker-compose.yml config
read -p "以上是 'docker-compose config' 的结果,要继续吗 (y/n)?" choice
if [ "$choice" != "y" ]; then
    echo "取消执行";
    echo -e "\033[31m脚本已结束.\033[0m";
    exit;
fi

echo -e "\033[31m开始启动项目 docker-compose up -d \033[0m";
docker-compose -f ${workdir}/docker-compose.yml up -d
docker-compose -f ${workdir}/docker-compose.yml top
```

