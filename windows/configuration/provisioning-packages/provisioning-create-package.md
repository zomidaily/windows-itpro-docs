---
title: Create a provisioning package (Windows 10/11)
description: Learn how to create a provisioning package for Windows 10/11, which lets you quickly configure a device without having to install a new image.
ms.prod: windows-client
author: lizgt2000
ms.author: lizlong
ms.topic: article
ms.localizationpriority: medium
ms.reviewer: gkomatsu
manager: aaroncz
ms.technology: itpro-configure
---

# Create a provisioning package


**Applies to**

-   Windows 10
-   Windows 11

You can use Windows Configuration Designer to create a provisioning package (`.ppkg`) that contains customization settings, and then apply the provisioning package to a device running Windows client.

>[Learn how to install Windows Configuration Designer.](provisioning-install-icd.md)

> [!TIP]
> We recommend creating a local admin account when you develop and test your provisioning package. We also recommend using a *least privileged* domain user account to join devices to the Active Directory domain.

## Start a new project

1. Open Windows Configuration Designer: From either the Start menu or Start menu search, type **Windows Configuration Designer**, and then select the **Windows Configuration Designer** shortcut.

2. Select your desired option on the **Start** page, which offers multiple options for creating a provisioning package, as shown in the following image:

    ![Configuration Designer wizards.](../images/icd-create-options-1703.png)

    - The following wizard options provide a simple interface for configuring common settings for desktop and kiosk devices: 

        - [Instructions for the desktop wizard](provision-pcs-for-initial-deployment.md)
        - [Instructions for the kiosk wizard](../kiosk-single-app.md#wizard)
        - [Instructions for HoloLens wizard](/hololens/hololens-provisioning)
        - [Instructions for Surface Hub wizard](/surface-hub/provisioning-packages-for-surface-hub)

      Wizards are also available for creating provisioning packages for Microsoft Surface Hub and Microsoft HoloLens devices. For a summary of the settings available in the desktop and kiosk devices, see [What you can configure using Configuration Designer wizards](provisioning-packages.md#configuration-designer-wizards).
      
      >[!NOTE]
      >To target devices running versions earlier than Windows 10, version 2004, ComputerName customization must be defined from the setting path: `Accounts/ComputerAccount/ComputerName` from the advanced editor. The default path from the simple editor uses a new CSP that isn't available on older systems.

    - The **Advanced provisioning** option opens a new project with all the runtime settings available. (The rest of this procedure uses advanced provisioning.)

        >[!TIP]
        > You can start a project in the simple wizard editor and then switch the project to the advanced editor.
        >
        > ![Switch to advanced editor.](../images/icd-switch.png)

3. Enter a name for your project, and then select **Next**.

4. Select the settings you want to configure, based on the type of device, and then select **Next**. The following table describes the options.


    | Windows edition | Settings available for customization | Provisioning package can apply to |
    |---|---|---|
    |  All Windows editions | Common settings  | All Windows client devices |
    | All Windows desktop editions    |    Common settings and settings specific to desktop devices |  All Windows client desktop editions (Home, Pro, Enterprise, Pro Education, Enterprise Education) |
    | Windows 10 IoT Core        |  Common settings and settings specific to Windows 10 IoT Core   | All Windows 10 IoT Core devices |
    | Windows 10 Holographic       | Common settings and settings specific to Windows 10 Holographic | [Microsoft HoloLens](/hololens/hololens-provisioning) |
    | Common to Windows 10 Team edition |    Common settings and settings specific to Windows 10 Team  | [Microsoft Surface Hub](/surface-hub/provisioning-packages-for-surface-hub) |


5. On the **Import a provisioning package (optional)** page, you can select **Finish** to create your project, or browse to and select an existing provisioning package to import to your project, and then select **Finish**.

    >[!TIP]
    >**Import a provisioning package** can make it easier to create different provisioning packages that all have certain settings in common. For example, you could create a provisioning package that includes the settings for your organization's network. Then, import that package into other packages that you create so you don't have to reconfigure those common settings repeatedly.

6. In the **Available customizations** pane, you can now configure settings for the package. 

## Configure settings

For an advanced provisioning project, Windows Configuration Designer opens the **Available customizations** pane. The example in the following image is based on **All Windows desktop editions** settings.

![What the ICD interface looks like.](../images/icd-runtime.png)

The settings in Windows Configuration Designer are based on Windows client configuration service providers (CSPs). To learn more about CSPs, see [Introduction to configuration service providers (CSPs) for IT pros](./how-it-pros-can-use-configuration-service-providers.md).

The process for configuring settings is similar for all settings. The following table shows an example.

1. Expand a category:

    :::image type="content" source="../images/icd-step1.png" alt-text="In Windows Configuration Designer, expand the Certificates category.":::

2. Select a setting:

    :::image type="content" source="../images/icd-step2.png" alt-text="In Windows Configuration Designer, select ClientCertificates.":::

3. Enter a value for the setting. Select **Add** if the button is displayed:

    :::image type="content" source="../images/icd-step3.png" alt-text="In Windows Configuration Designer, enter a name for the certificate.":::

4. Some settings, such as this example, require additional information. In **Available customizations**, select the value you just created, and more settings are displayed:

    :::image type="content" source="../images/icd-step4.png" alt-text="In Windows Configuration Designer, additional settings for client certificate are available.":::

5. When the setting is configured, it is displayed in the **Selected customizations** pane:

    :::image type="content" source="../images/icd-step5.png" alt-text="In Windows Configuration Designer, the selected customizations pane shows your settings.":::

For details on each specific setting, see [Windows Provisioning settings reference](../wcd/wcd.md). The reference article for a setting is also displayed in Windows Configuration Designer when you select the setting, as shown in the following image.

![Windows Configuration Designer opens the reference topic when you select a setting.](../images/icd-setting-help.png)


 ## Build package

1. After you're done configuring your customizations, select **Export**, and then select **Provisioning Package**.

    ![Export on top bar.](../images/icd-export-menu.png)

2. In the **Describe the provisioning package** window, enter the following information, and then select **Next**:
    - **Name** - This field is pre-populated with the project name. You can change this value by entering a different name in the **Name** field.
    - **Version (in Major.Minor format** - Optional. You can change the default package version by specifying a new value in the **Version** field. 
    - **Owner** - Select **IT Admin**. For more information, see [Precedence for provisioning packages](provisioning-how-it-works.md#precedence-for-provisioning-packages).
    - **Rank (between 0-99)** - Optional. You can select a value between 0 and 99, inclusive. The default package rank is 0.

3. In the **Select security details for the provisioning package** window, you can select to encrypt and/or sign a provisioning package with a selected certificate, and then select **Next**. Both selections are optional:

   - **Encrypt package** -  If you select this option, an autogenerated password will be shown on the screen.
   - **Sign package** - If you select this option, you must select a valid certificate to use for signing the package. You can specify the certificate by selecting **Select** and choosing the certificate you want to use to sign the package.

     >[!NOTE]
     >You should only configure provisioning package security when the package is used for device provisioning and when the package has content with sensitive security data, such as certificates or credentials that should be prevented from being compromised. When applying an encrypted and/or signed provisioning package, either during OOBE or through the setting UI, the package can be decrypted, and if signed, be trusted without explicit user consent. An IT administrator can set policy on a user device to restrict the removal of required packages from the device, or the provisioning of potentially harmful packages on the device. 
     >
     >If a provisioning package is signed by a trusted provisioner, it can be installed on a device without a prompt for user consent. In order to enable trusted provider certificates, you must set the **TrustedProvisioners** setting prior to installing the trusted provisioning package. This is the only way to install a package without user consent. To provide additional security, you can also set **RequireProvisioningPackageSignature**, which prevents users from installing provisioning packages that are not signed by a trusted provisioner.

4. In the **Select where to save the provisioning package** window, specify the output location where you want the provisioning package to go once it's built, and then select **Next**. By default, Windows Configuration Designer uses the project folder as the output location.

5. In the **Build the provisioning package** window, select **Build**. The provisioning package doesn't take long to build. The project information is displayed in the build page and the progress bar indicates the build status. 

    If you need to cancel the build, select **Cancel**. This cancels the current build process, closes the wizard, and takes you back to the **Customizations** page.

6. If your build fails, an error message will appear that includes a link to the project folder. You can scan the logs to determine what caused the error. Once you fix the issue, try building the package again.

    If your build is successful, the name of the provisioning package, output directory, and project directory will be shown. 

    If you choose, you can build the provisioning package again and pick a different path for the output package. To do this, select **Back** to change the output package name and path, and then select **Next** to start another build.

7. When you are done, select **Finish** to close the wizard and go back to the **Customizations** page.

**Next step**: [How to apply a provisioning package](provisioning-apply-package.md)

## Learn more

- [How to bulk-enroll devices with On-premises Mobile Device Management in Microsoft Configuration Manager](/configmgr/mdm/deploy-use/bulk-enroll-devices-on-premises-mdm)

## Related articles

- [Provisioning packages for Windows client](provisioning-packages.md)
- [How provisioning works in Windows client](provisioning-how-it-works.md)
- [Install Windows Configuration Designer](provisioning-install-icd.md)
- [Apply a provisioning package](provisioning-apply-package.md)
- [Settings changed when you uninstall a provisioning package](provisioning-uninstall-package.md)
- [Provision PCs with common settings for initial deployment (simple provisioning)](provision-pcs-for-initial-deployment.md)
- [Use a script to install a desktop app in provisioning packages](provisioning-script-to-install-app.md)
- [PowerShell cmdlets for provisioning Windows client (reference)](provisioning-powershell.md)
- [Windows Configuration Designer command-line interface (reference)](provisioning-command-line.md)
- [Create a provisioning package with multivariant settings](provisioning-multivariant.md)
