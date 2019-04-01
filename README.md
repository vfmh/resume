# 个人简历

:bookmark_tabs: 这是一份个人设计的简历模板

[Repo](https://github.com/FangMingHong/resume)
## 实现小功能

- :iphone: *响应式布局*
- :printer: *下载为PDF文档*

## 前提
安装了```nodeJS```

## 使用步骤
1. ```npm install```
2. 将信息填入```info.json```
3. ```npm run serve``` 搭建本地生产环境(非必要)
4. ```npm run build```
5. 导出 ```/dist``` 
6. 大功告成

## info.json文件说明
```
// 所有信息非必要填写
{
    "name":"", // 你的姓名
    "post":"", // 岗位
    "motto":"", // 一句话
    "content":[ // 简历模块  
        {
            "title":"", // 某个模块名称
            "items":[ 
                {
                    "time":"", // 日期
                    "name":"", // 名称
                    "position":"", // 职位
                    "description":"", // 描述
                    "list":[
                        "", // 显示列表信息
                        ""
                    ]
                }
            ]
        }
    ],
    "latesttime":"" // 最后更新日期
}
```
## 更改头像和.ico

更改图标 ```/public/myfavicon.ico```
更改头像 ```/src/assets/touxiang.jpg```
