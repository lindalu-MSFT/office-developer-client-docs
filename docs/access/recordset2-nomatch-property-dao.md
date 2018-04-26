---
title: "Recordset2.NoMatch Property (DAO)"
 
 
manager: soliver
ms.date: 11/16/2014
ms.audience: Developer
ms.topic: reference
  
localization_priority: Normal
ms.assetid: 2d7a02ff-a2bf-5f0e-bd71-a6d42c25b13a
description: "Indicates whether a particular record was found by using the Seek method or one of the Find methods (Microsoft Access workspaces only)."
---

# Recordset2.NoMatch Property (DAO)

Indicates whether a particular record was found by using the **[Seek](recordset2-seek-method-dao.md)** method or one of the **[Find](recordset2-findfirst-method-dao.md)** methods (Microsoft Access workspaces only). 
  
## Syntax

 *expression*  . **NoMatch**
  
 *expression*  A variable that represents a **Recordset2** object. 
  
## Remarks

When you open or create a **[Recordset](recordset-object-dao.md)** object, its **NoMatch** property is set to **False**. 
  
To locate a record, use the **Seek** method on a table-type **Recordset** object or one of the **Find** methods on a dynaset- or snapshot-type **Recordset** object. Check the **NoMatch** property setting to see whether the record was found. 
  
If the **Seek** or **Find** method is unsuccessful and the **NoMatch** property is **True**, the current record will no longer be valid. Be sure to obtain the current record's bookmark before using the **Seek** method or a **Find** method if you'll need to return to that record. 
  
> [!NOTE]
> Using any of the **[Move](recordset-movefirst-method-dao.md)** methods on a **Recordset** object won't affect its **NoMatch** property setting. 
  
## Example

This example uses the **NoMatch** property to determine whether a **Seek** and a **FindFirst** were successful, and if not, to give appropriate feedback. The SeekMatch and FindMatch procedures are required for this procedure to run. 
  
```
Sub NoMatchX() 
 
 Dim dbsNorthwind As Database 
 Dim rstProducts As Recordset2 
 Dim rstCustomers As Recordset2 
 Dim strMessage As String 
 Dim strSeek As String 
 Dim strCountry As String 
 Dim varBookmark As Variant 
 
 Set dbsNorthwind = OpenDatabase("Northwind.mdb") 
 ' Default is dbOpenTable; required if Index property will 
 ' be used. 
 Set rstProducts = dbsNorthwind.OpenRecordset("Products") 
 
 With rstProducts 
 .Index = "PrimaryKey" 
 
 Do While True 
 ' Show current record information; ask user for 
 ' input. 
 strMessage = "NoMatch with Seek method" &amp; vbCr &amp; _ 
 "Product ID: " &amp; !ProductID &amp; vbCr &amp; _ 
 "Product Name: " &amp; !ProductName &amp; vbCr &amp; _ 
 "NoMatch = " &amp; .NoMatch &amp; vbCr &amp; vbCr &amp; _ 
 "Enter a product ID." 
 strSeek = InputBox(strMessage) 
 If strSeek = "" Then Exit Do 
 
 ' Call procedure that seeks for a record based on 
 ' the ID number supplied by the user. 
 SeekMatch rstProducts, Val(strSeek) 
 Loop 
 
 .Close 
 End With 
 
 Set rstCustomers = dbsNorthwind.OpenRecordset( _ 
 "SELECT CompanyName, Country FROM Customers " &amp; _ 
 "ORDER BY CompanyName", dbOpenSnapshot) 
 
 With rstCustomers 
 
 Do While True 
 ' Show current record information; ask user for 
 ' input. 
 strMessage = "NoMatch with FindFirst method" &amp; _ 
 vbCr &amp; "Customer Name: " &amp; !CompanyName &amp; _ 
 vbCr &amp; "Country: " &amp; !Country &amp; vbCr &amp; _ 
 "NoMatch = " &amp; .NoMatch &amp; vbCr &amp; vbCr &amp; _ 
 "Enter country on which to search." 
 strCountry = Trim(InputBox(strMessage)) 
 If strCountry = "" Then Exit Do 
 
 ' Call procedure that finds a record based on 
 ' the country name supplied by the user. 
 FindMatch rstCustomers, _ 
 "Country = '" &amp; strCountry &amp; "'" 
 Loop 
 
 .Close 
 End With 
 
 dbsNorthwind.Close 
 
End Sub 
 
Sub SeekMatch(rstTemp As Recordset2, _ 
 intSeek As Integer) 
 
 Dim varBookmark As Variant 
 Dim strMessage As String 
 
 With rstTemp 
 ' Store current record location. 
 varBookmark = .Bookmark 
 .Seek "=", intSeek 
 
 ' If Seek method fails, notify user and return to the 
 ' last current record. 
 If .NoMatch Then 
 strMessage = _ 
 "Not found! Returning to current record." &amp; _ 
 vbCr &amp; vbCr &amp; "NoMatch = " &amp; .NoMatch 
 MsgBox strMessage 
 .Bookmark = varBookmark 
 End If 
 
 End With 
 
End Sub 
 
Sub FindMatch(rstTemp As Recordset, _ 
 strFind As String) 
 
 Dim varBookmark As Variant 
 Dim strMessage As String 
 
 With rstTemp 
 ' Store current record location. 
 varBookmark = .Bookmark 
 .FindFirst strFind 
 
 ' If Find method fails, notify user and return to the 
 ' last current record. 
 If .NoMatch Then 
 strMessage = _ 
 "Not found! Returning to current record." &amp; _ 
 vbCr &amp; vbCr &amp; "NoMatch = " &amp; .NoMatch 
 MsgBox strMessage 
 .Bookmark = varBookmark 
 End If 
 
 End With 
 
End Sub 

```

