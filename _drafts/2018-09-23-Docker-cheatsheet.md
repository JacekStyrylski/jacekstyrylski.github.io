---
layout: post
title: Docker cheatsheet
subtitle: These are most usefull docker commands with their explanations
comments: true
tags: Docker, Container
---
*I'd like to summarize some of the most important commands and ideas of Docker.*
## Dockerfile
Primary file deescibing your docker image build is a dockerfile. Here you need to define, which image you'd like to use. Where do you want to put your application. Which commands should be run to prepare enviornment.
### Examplary Dockerfile:

```
FROM microsoft/dotnet:2.1-sdk AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out
```

In above Dockerfile we can see following commands together with their parameters:

- `FROM`
- `WORKDIR`
- `RUN`
- `COPY`