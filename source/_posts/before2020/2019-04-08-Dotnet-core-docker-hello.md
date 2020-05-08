---
title: '.Net Core Docker Hello'
date: 2019-04-08 19:51:05
tags: c#
---

## 创建 Console App

创建文件夹 DockerHello ,在 term 执行命令

``` bash
dotnet new console
dotnet run
```

## 容器化 .Net Core 程序

### 第一个 Dockerfile

创建没有后缀的的 Dockerfile

```bash
FROM microsoft/dotnet:2.1-sdk
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# copy and build everything else
COPY . ./
RUN dotnet publish -c Release -o out
ENTRYPOINT ["dotnet", "out/DockerHello.dll"]
````

### build and run

分别执行以下两个命令，分别是创建 image 和运行 image

```bash
docker build -t dotnetapp-dev .
docker run --rm dotnetapp-dev Hello from Docker
```