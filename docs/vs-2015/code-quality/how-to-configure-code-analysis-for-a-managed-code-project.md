---
title: "How to: Configure Code Analysis for a Managed Code Project | Microsoft Docs"
ms.date: 11/15/2016
ms.prod: "visual-studio-dev14"
ms.technology: vs-ide-code-analysis
ms.topic: conceptual
f1_keywords: 
  - "vs.codeanalysis.propertypages.csvb"
helpviewer_keywords: 
  - "code analysis, selecting rule sets"
  - "code analysis, rule sets"
ms.assetid: 618f6ff3-db0e-46cb-b08d-dfa35e62c9e7
caps.latest.revision: 35
author: gewarren
ms.author: gewarren
manager: "wpickett"
---
# How to: Configure Code Analysis for a Managed Code Project
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

In [!INCLUDE[vsUltLong](../includes/vsultlong-md.md)], [!INCLUDE[vsPreLong](../includes/vsprelong-md.md)] and [!INCLUDE[vsPro](../includes/vspro-md.md)], you can choose from a list of code analysis *rule sets* to apply to a managed code project. The default rule set is the Microsoft Minimum Recommended Rules. You can apply another rule set to a project or to all projects in a solution.  
  
> [!NOTE]
>  For information about how to configure a rule set for ASP.NET Web applications, see [How to: Configure Code Analysis for an ASP.NET Web Application](../code-quality/how-to-configure-code-analysis-for-an-aspnet-web-application.md).  
  
### To configure a rule set for a .NET Framework project  
  
1. In **Solution Explorer**, click the project.  
  
2. On the **Analyze** menu, click **Configure Code Analysis for** *ProjectName*.  
  
3. In the **Configuration** and **Platform** lists, click the build configuration and target platform.  
  
4. To run code analysis every time the project is built using the selected configuration, select the **Enable Code Analysis on Build (defines CODE_ANALYSIS constant)** check box. You can also run code analysis manually by opening the **Analyze** menu and clicking **Run Code Analysis on** *ProjectName*.  
  
5. By default, code analysis does not report warnings from code that is automatically generated by external tools. To view warnings from generated code, clear the **Suppress results from generated code** check box.  
  
    > [!NOTE]
    >  This option does not suppress code analysis errors and warnings from generated code when the errors and warnings appear in forms and templates. You can both view and maintain the source code for a form or a template.  
  
6. In the **Run this rule set** list, do one of the following:  
  
    - Click the rule set that you want to use.  
  
    - Click **\<Browse...>** to specify an existing custom rule set that is not in the list.  
  
    - Define a custom rule set.  
  
         For more information, see [Creating Custom Rule Sets](../code-quality/creating-custom-code-analysis-rule-sets.md).  
  
## See Also  
 [Walkthrough: Configuring and Using a Custom Rule Set](../code-quality/walkthrough-configuring-and-using-a-custom-rule-set.md)
