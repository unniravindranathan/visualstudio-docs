---
title: "Walkthrough: Saving User Settings on a Start Page | Microsoft Docs"
ms.date: 11/15/2016
ms.prod: "visual-studio-dev14"
ms.technology: "vs-ide-sdk"
ms.topic: conceptual
ms.assetid: 754b9bf3-8681-4c77-b0a4-09146a4e1d2d
caps.latest.revision: 19
ms.author: gregvanl
manager: jillfra
---
# Walkthrough: Saving User Settings on a Start Page
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

You can persist user settings for your start page. By following this walkthrough, you can create a control that saves a setting to the registry when the user clicks a button, and then retrieves that setting every time the Start Page loads. Because the Start Page project template includes a customizable user control, and the default Start Page XAML calls that control, you do not have to modify the Start Page itself.  
  
 The settings store that is instantiated in this walkthrough is an instance of the <xref:Microsoft.VisualStudio.Shell.Interop.IVsWritableSettingsStore> interface, which reads and writes to the following registry location when it is called: HKCU\Software\Microsoft\VisualStudio\14.0\\*CollectionName*  
  
 When it is running in the experimental instance of Visual Studio, the settings store reads and writes to HKCU\Software\Microsoft\VisualStudio\14.0Exp\\*CollectionName.*  
  
 For more information about how to persist settings, see [Extending User Settings and Options](../extensibility/extending-user-settings-and-options.md).  
  
## Prerequisites  
  
> [!NOTE]
>  To follow this walkthrough, you must install the Visual Studio SDK. For more information, see [Visual Studio SDK](../extensibility/visual-studio-sdk.md).  
>   
>  You can download the Start Page project template by using **Extension Manager**.  
  
## Setting Up the Project  
  
#### To configure the project for this walkthrough  
  
1. Create a Start Page project by using the Start Page project template, as described in [Creating Your Own Start Page](../misc/creating-your-own-start-page.md). Name the project **SaveMySettings**.  
  
2. In **Solution Explorer**, add the following assembly references to the StartPageControl project:  
  
    - EnvDTE  
  
    - EnvDTE80  
  
    - Microsoft.VisualStudio.OLE.Interop  
  
    - Microsoft.VisualStudio.Shell.Interop.11.0  
  
3. Open MyControl.xaml.  
  
4. From the XAML pane, in the top-level <xref:System.Windows.Controls.UserControl> element definition, add the following event declaration after the namespace declarations.  
  
    ```  
    Loaded="OnLoaded"  
    ```  
  
5. In the design pane, click the main area of the control, and then press DELETE.  
  
     This removes the <xref:System.Windows.Controls.Border> element and everything in it, and leaves only the top-level <xref:System.Windows.Controls.Grid> element.  
  
6. From the **Toolbox**, drag a <xref:System.Windows.Controls.StackPanel> control to the grid.  
  
7. Now drag a <xref:System.Windows.Controls.TextBlock>, a <xref:System.Windows.Controls.TextBox>, and a Button to the <xref:System.Windows.Controls.StackPanel>.  
  
8. Add an **x:Name** attribute for the <xref:System.Windows.Controls.TextBox>, and a `Click` event for the <xref:System.Windows.Controls.Button>, as shown in the following example.  
  
    ```xml  
    <StackPanel Width="300" HorizontalAlignment="Center" VerticalAlignment="Center">  
        <TextBlock Width="140" FontSize="14">Enter your setting</TextBlock>  
        <TextBox x:Name="txtblk" Margin="0, 5, 0, 10" Width="140" />  
        <Button Click="Button_Click" Width="100">Save My Setting</Button>  
    </StackPanel>  
    ```  
  
## Implementing the User Control  
  
#### To implement the user control  
  
1. In the XAML pane, right-click the `Click` attribute of the <xref:System.Windows.Controls.Button> element, and then click **Navigate to Event Handler**.  
  
     This opens MyControl.xaml.cs, and creates a stub handler for the `Button_Click` event.  
  
2. Add the following `using` statements to the top of the file.  
  
     [!code-csharp[StartPageDTE#11](../snippets/csharp/VS_Snippets_VSSDK/startpagedte/cs/startpagecontrol/mycontrol.xaml.cs#11)]  
  
3. Add a private `SettingsStore` property, as shown in the following example.  
  
    ```csharp  
    private IVsWritableSettingsStore _settingsStore = null;  
    private IVsWritableSettingsStore SettingsStore  
    {  
        get  
        {  
            if (_settingsStore == null)  
            {  
                // Get a reference to the DTE from the DataContext.   
                var typeDescriptor = DataContext as ICustomTypeDescriptor;  
                var propertyCollection = typeDescriptor.GetProperties();  
                var dte = propertyCollection.Find("DTE", false).GetValue(  
                    DataContext) as DTE2;  
  
                // Get the settings manager from the DTE.   
                var serviceProvider = new ServiceProvider(  
                    (Microsoft.VisualStudio.OLE.Interop.IServiceProvider)dte);  
                var settingsManager = serviceProvider.GetService(  
                    typeof(SVsSettingsManager)) as IVsSettingsManager;  
  
                // Write the user settings to _settingsStore.  
                settingsManager.GetWritableSettingsStore(  
                    (uint)__VsSettingsScope.SettingsScope_UserSettings,  
                    out _settingsStore);  
            }  
            return _settingsStore;  
        }  
    }  
    ```  
  
     This property first gets a reference to the <xref:EnvDTE80.DTE2> interface, which contains the Automation object model, from the <xref:System.Windows.FrameworkElement.DataContext%2A> of the user control, and then uses the DTE to get an instance of the <xref:Microsoft.VisualStudio.Shell.Interop.IVsSettingsManager> interface. Then it uses that instance to return the current user settings.  
  
4. Fill in the `Button_Click` event as follows.  
  
    ```csharp  
    private void Button_Click(object sender, RoutedEventArgs e)  
    {  
        int exists = 0;  
        SettingsStore.CollectionExists("MySettings", out exists);  
        if (exists != 1)  
        {  
            SettingsStore.CreateCollection("MySettings");  
        }  
        SettingsStore.SetString("MySettings", "MySetting", txtblk.Text);  
    }  
    ```  
  
     This writes the content of the text box to a "MySetting" field in a "MySettings" collection in the registry. If the collection does not exist, it is created.  
  
5. Add the following handler for the `OnLoaded` event of the user control.  
  
    ```csharp  
    private void OnLoaded(Object sender, RoutedEventArgs e)  
    {  
        string value;  
        SettingsStore.GetStringOrDefault(  
            "MySettings", "MySetting", "", out value);  
        txtblk.Text = value;  
    }  
    ```  
  
     This sets the text of the text box to the current value of "MySetting".  
  
6. Build the user control.  
  
7. In **Solution Explorer**, open source.extension.vsixmanifest.  
  
8. In the manifest editor, set **Product Name** to **Save My Settings Start Page**.  
  
     This sets the name of the Start Page as it is to appear in the **Customize Start Page** list in the **Options** dialog box.  
  
9. Build StartPage.xaml.  
  
## Testing the Control  
  
#### To test the user control  
  
1. Press F5.  
  
     The experimental instance of Visual Studio opens.  
  
2. In the experimental instance, on the **Tools** menu, click **Options**.  
  
3. In the **Environment** node, click **Startup**, and then, in the **Customize Start Page** list, select **[Installed Extension] Save My Settings Start Page**.  
  
     Click **OK**.  
  
4. Close the Start Page if it is open, and then, on the **View** menu, click **Start Page**.  
  
5. On the Start Page, click the **MyControl** tab.  
  
6. In the text box, type **Cat**, and then click **Save My Setting**.  
  
7. Close the Start Page and then open it again.  
  
     The word "Cat" should be displayed in the text box.  
  
8. Replace the word "Cat" with the word "Dog". Do not click the button.  
  
9. Close the Start Page and then open it again.  
  
     The word "Dog" should be displayed in the text box, even though the setting was not saved. This happens because Visual Studio keeps tool windows in memory, even if they are closed, until Visual Studio itself is closed.  
  
10. Close the experimental instance of Visual Studio.  
  
11. Press F5 to re-open the experimental instance.  
  
12. The word "Cat" should be displayed in the text box.  
  
## Next Steps  
 You can modify this user control to save and retrieve any number of custom settings by using different values from different event handlers to get and set the `SettingsStore` property. As long as you use a different `propertyName` parameter for each call to <xref:Microsoft.VisualStudio.Shell.Interop.IVsWritableSettingsStore.SetString%2A>, the values will not overwrite one another in the registry.  
  
## See Also  
 <xref:EnvDTE80.DTE2?displayProperty=fullName>   
 [Creating Your Own Start Page](../misc/creating-your-own-start-page.md)   
 [Adding Visual Studio Commands to a Start Page](../extensibility/adding-visual-studio-commands-to-a-start-page.md)
