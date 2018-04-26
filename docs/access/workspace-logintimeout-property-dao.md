---
title: "Workspace.LoginTimeout Property (DAO)"
 
 
manager: soliver
ms.date: 11/16/2014
ms.audience: Developer
ms.topic: reference
  
localization_priority: Normal
ms.assetid: 5f03b166-abbc-20de-1a01-3869a9f2907d
description: "Sets or returns the number of seconds before an error occurs when you attempt to log on to an ODBC database."
---

# Workspace.LoginTimeout Property (DAO)

Sets or returns the number of seconds before an error occurs when you attempt to log on to an ODBC database.
  
## Syntax

 *expression*  . **LoginTimeout**
  
 *expression*  A variable that represents a **Workspace** object. 
  
## Remarks

 The default **LoginTimeout** property setting is 20 seconds. When the **LoginTimeout** property is set to 0, no timeout occurs. 
  
When you're attempting to log on to an ODBC database, such as Microsoft SQL Server, the connection can fail as a result of network errors or because the server isn't running. Rather than waiting for the default 20 seconds to connect, you can specify how long to wait before raising an error. Logging on to the server happens implicitly as part of a number of different events, such as running a query on an external server database.
  
