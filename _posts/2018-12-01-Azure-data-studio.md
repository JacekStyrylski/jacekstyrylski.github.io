---
layout: post
title: Azure Data Studio
subtitle: Microsoft new editor for SQL operations.
comments: true
tags: Azure, SQL, MSSQL, Azure Data Studio
---
For years if one would like to manage MS SQL database, or even view its data, the natural choice was to use for it Microsoft SQL Server Management Studio. This still is the most powerful MS SQL Server management tool, with vast capabilities.

![ManagementStudioPic](/images/ssms.png)

Unfortunately, it is not as developer friendly as it could be. For several years developers around the globe started to use Visual Studio Code - very powerful open source IDE. Thousands of plugins, full integration with GIT and many, many more useful features makes it one of the most favorite IDE for many developers.

Sensing this popularity  Microsoft branched VS Code into **Azure Data Studio**.

![Azure Data Studio](/images/azure-data-query.png)

This tool allows us to connect to any MS SQL server, with very nice integration with Azure.

Summary of most useful features:
- Query tool
- Data editor
- Explain
- GIT integration
- Extensions
- Azure integration
- Themes

Unfortunately, for now, Azure Data Studio does not allow us to connect to Azure Cosmos DB, what could be expected from it. Also, it would be useful to have Azure Data Studio features brought by extension to Visual Studio Code, what is not the case.