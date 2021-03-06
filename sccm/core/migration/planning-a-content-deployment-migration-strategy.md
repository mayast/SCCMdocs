---
title: "Migrate content | Microsoft Docs"
description: "Use distribution points to manage content while you migrate data to a System Center Configuration Manager destination hierarchy."
ms.custom: na
ms.date: 10/06/2016
ms.prod: configuration-manager
ms.reviewer: na
ms.suite: na
ms.technology:
  - configmgr-other
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66f7759c-6272-4116-aad7-0d05db1d46cd
caps.latest.revision: 8
caps.handback.revision: 0
author: Brendunsms.author: brendunsmanager: angrobe

---
# Planning a content deployment migration strategy in System Center Configuration Manager*Applies to: System Center Configuration Manager (Current Branch)*
While you are actively migrating data to a System Center Configuration Manager destination hierarchy, Configuration Manager clients in both the source and destination hierarchies can maintain access to content that you deployed in the source hierarchy. Additionally, you can use migration to upgrade or reassign distribution points from the source hierarchy to become distribution points in the destination hierarchy. When you share and upgrade or reassign distribution points, this strategy can help you avoid having to redeploy content to new servers in the destination hierarchy for the clients that you migrate.  

Although you can recreate and distribute content in the destination hierarchy, you can also use the following options to manage this content:  

-   Share distribution points in the source hierarchy with clients in the destination hierarchy.  

-   Upgrade stand-alone Configuration Manager 2007 distribution points or Configuration Manager 2007 secondary sites in the source hierarchy to become distribution points in the destination hierarchy.  

-   Reassign distribution points from a System Center Configuration Manager source hierarchy to a site in the destination hierarchy.  

Use the following sections to help you plan for content deployment during migration:  

-   [Share distribution points between source and destination hierarchies](#About_Shared_DPs_in_Migration)  

-   [Planning to upgrade Configuration Manager 2007 shared distribution points](#Planning_to_Upgrade_DPs)  

    -   [Distribution point upgrade process](#BKIMK_UpgradeProcess)  

    -   [Planning to upgrade Configuration Manager 2007 secondary sites](#BKMK_UpgradeSS)  

-   [Planning to Reassign System Center Configuration Manager Distribution Points](#BKMK_ReassignDistPoint)  

    -   [Distribution point reassignment process](#BKMK_ReassignProcess)  

-   [Content ownership when migrating content](#About_Migrating_Content)  

##  <a name="About_Shared_DPs_in_Migration"></a> Share distribution points between source and destination hierarchies  
During migration, you can share distribution points from a source hierarchy with the destination hierarchy. You can use shared distribution points to make content that you have migrated from a source hierarchy immediately available to clients in the destination hierarchy without having to recreate that content, and then distribute it to new distribution points in the destination hierarchy. When clients in the destination hierarchy request content that is deployed to distribution points that you have shared, the shared distribution points can be offered to the clients as valid content locations.  

 In addition to being a valid content location for clients in the destination hierarchy as long as migration from the source hierarchy remains active, it is possible to upgrade or reassign a distribution point to the destination hierarchy. You can upgrade Configuration Manager 2007 shared distribution points and reassign System Center 2012 Configuration Manager shared distribution points. When you upgrade or reassign a shared distribution point, the distribution point is removed from the source hierarchy and becomes a distribution point in the destination hierarchy. After you upgrade or reassign a shared distribution point, you can continue to use the distribution point in the destination hierarchy after migration from the source hierarchy is finished. For more information about how to upgrade a shared distribution point, see [Planning to upgrade Configuration Manager 2007 shared distribution points](#Planning_to_Upgrade_DPs). For information about how to reassign a shared distribution point, see [Planning to Reassign System Center Configuration Manager Distribution Points](#BKMK_ReassignDistPoint).  

 You can choose to share distribution points from any source site in your source hierarchy. When you share distribution points for a source site, each qualifying distribution point at that primary site and at each of the primary sites child secondary sites are shared. To qualify to be a shared distribution point, the site system server that hosts the distribution point must be configured with a fully qualified domain name (FQDN). Any distribution points that are configured with a NetBIOS name are disregarded.  

> [!TIP]  
>  Configuration Manager 2007 does not require you to configure an FQDN for site system servers.  

Use the following information to help you plan for shared distribution points:  

-   Distribution points that you share must meet the prerequisites for shared distribution points. For information about these prerequisites, see the [Required configurations for migration](../../core/migration/prerequisites-for-migration.md#BKMK_Required_Configurations) section in the [Prerequisites for migration in System Center Configuration Manager](../../core/migration/prerequisites-for-migration.md) topic.  

-   The share distribution point action is a site-wide setting that shares all qualifying distribution points at a source site and at any direct child secondary sites. You cannot select individual distribution points to share when you enable distribution point sharing.  

-   Clients in the destination hierarchy can receive content location information for packages that are distributed to distribution points that are shared from the source hierarchy. For distribution points from a Configuration Manager 2007 source hierarchy, this includes branch distribution points, distribution points on server shares, and standard distribution points.  

    > [!WARNING]  
    >  If you change the source hierarchy, shared distribution points from the original source hierarchy are no longer available and cannot be offered as content locations to clients in the destination hierarchy. If you reconfigure migration to use the original source hierarchy, the previously shared distribution points are restored as valid content location servers.  

-   When you migrate a package that is hosted on a shared distribution point, the package version must remain the same in the source and destination hierarchies. When a package version is not the same in the source and destination hierarchy, clients in the destination hierarchy cannot retrieve that content from the shared distribution point. Therefore, if you update a package in the source hierarchy, you must re-migrate the package data before clients in the destination hierarchy can retrieve that content from a shared distribution point.  

    > [!NOTE]  
    >  When you view details for a package that is hosted on a shared distribution point, the number of packages that display as **Hosted Migrated Packages** on the source sites **Shared Distribution Points** tab is not updated until the next data gathering cycle is finished.  

-   You can view shared distribution points and their properties in the **Source Hierarchy** node of the **Administration** workspace in the Configuration Manager console that connects to the destination hierarchy.  

-   You cannot use a shared distribution point from a Configuration Manager 2007 source hierarchy to host packages for Microsoft Application Virtualization (App-V). App-V packages must migrate and be converted for use by clients in the destination hierarchy. However, you can use a shared distribution point from a System Center 2012 Configuration Manager or System Center Configuration Manager source hierarchy to host App-V packages for clients in a destination hierarchy.  

-   When you share a protected distribution point from a Configuration Manager 2007 source hierarchy, the destination hierarchy creates a boundary group that includes the protected network locations of that distribution point. You cannot modify this boundary group in the destination hierarchy. However, if you change the protected boundary information for the distribution point in the Configuration Manager 2007 source hierarchy, that change is reflected in the destination hierarchy after the next data gathering cycle finishes.  

    > [!NOTE]  
    >  System Center 2012 Configuration Manager and System Center Configuration Manager sites use the concept of preferred distribution points instead of protected distribution points, this condition only applies to distribution points that are shared from Configuration Manager 2007 source sites.  

Before you share distribution points from a source site, the eligible distribution points are not visible in the Configuration Manager console. After you share distribution points, only the distribution points that are successfully shared are listed.  

After you have shared distribution points, you can change the configuration of any shared distribution point in the source hierarchy. Changes that you make to the configuration of a distribution point are reflected in the destination hierarchy after the next data gathering cycle. Distribution points that you updated to qualify for sharing are shared automatically, while those that no longer qualify stop sharing distribution points. For example, you might have a distribution point that is not configured with an intranet FQDN and was not initially shared with the destination hierarchy. After you configure the FQDN for that distribution point, the next data gathering cycle identifies this configuration, and the distribution point is then shared with the destination hierarchy.  

##  <a name="Planning_to_Upgrade_DPs"></a> Planning to upgrade Configuration Manager 2007 shared distribution points  
When you migrate from a Configuration Manager 2007 source hierarchy, you can upgrade a shared distribution point to make it a System Center Configuration Manager distribution point. You can upgrade distribution points at both primary sites and secondary sites. The upgrade process removes the distribution point from the Configuration Manager 2007 hierarchy and makes it a site system server in the destination hierarchy. This process also copies the existing content that is on the distributing point to a new location on the distribution point computer. The upgrade process then modifies the copy of the content to create the single instance store for use with content deployment in the destination hierarchy. Therefore, when you upgrade a distribution point, you do not have to redistribute migrated content that was hosted on the Configuration Manager 2007 distribution point.  

After Configuration Manager converts the content to the single instance store, Configuration Manager deletes the original source content on the distribution point computer to free up disk space and does not use the original source content location.  

Not all Configuration Manager 2007 distribution points that you can share are eligible for upgrade to System Center Configuration Manager. To be eligible for upgrade, a Configuration Manager 2007 distribution point must meet the conditions for upgrade that include the site system server on which the distribution point is installed, and the type of Configuration Manager 2007 distribution point that is installed. For example, you cannot upgrade any type of distribution point that is installed on the site server computer at a primary site, but you can upgrade a standard distribution point that is installed on the site server computer at a secondary site.  

> [!NOTE]  
>  You can upgrade only those Configuration Manager 2007 shared distribution points that are on a computer that runs an operating system version that is supported for distribution points in the destination hierarchy. For example, although you can share a Configuration Manager 2007 distribution point that is on a computer that runs Windows Vista, you cannot upgrade this shared distribution point because the operating system is not supported by System Center Configuration Manager for use as a distribution point.  

The following table lists the supported locations for each type of Configuration Manager 2007 distribution point that can you can upgrade.  

|Type of distribution point|Distribution point on a site system computer other than the site server|Distribution point on a site system computer other than the site server and hosting other site system roles|Distribution point on a secondary site server|  
|--------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|---------------------------------------------------|  
|Standard distribution point|Yes|No|Yes|  
|Distribution point on server shares<sup>1</sup>|Yes|No|No|  
|Branch distribution point|Yes|No|No|  

 <sup>1</sup> System Center Configuration Manager does not support server shares for site systems but does support the upgrade of a Configuration Manager 2007 distribution point that is on a server share. When you upgrade a Configuration Manager 2007 distribution point that is on a server share, the distribution point type is automatically converted to a server, and you must select the drive on the distribution point computer that will store the single instance content store.  

> [!WARNING]  
>  Before you upgrade a branch distribution point, uninstall the Configuration Manager 2007 client software. When you upgrade a branch distribution point that has the Configuration Manager 2007 client software installed, the content that was previously deployed to the computer is removed from the computer, and the upgrade of the distribution point fails.  

To identify distribution points that are eligible for upgrade in the Configuration Manager console in the **Source Hierarchy** node, select a source site, and then select the **Shared Distribution Points** tab. Eligible distribution points display **Yes** in the **Eligible for Upgrade** column.  

When you upgrade a distribution point that is installed on a Configuration Manager 2007 secondary site server, the secondary site is uninstalled from the source hierarchy. Although this scenario is called a secondary site upgrade, this applies only to the distribution point site system role. The result is that the secondary site is not upgraded and instead is uninstalled. This leaves only a distribution point from the destination hierarchy on the computer that was the secondary site server. Because the secondary site is removed from the source hierarchy, if you plan to upgrade the distribution point on a secondary site, see the [Planning to upgrade Configuration Manager 2007 secondary sites](#BKMK_UpgradeSS) section in this topic.  

###  <a name="BKIMK_UpgradeProcess"></a> Distribution point upgrade process  
You can use the Configuration Manager console to upgrade Configuration Manager 2007 distribution points that you have shared with the destination hierarchy. When you upgrade a shared distribution point, the distribution point is uninstalled from the Configuration Manager 2007 site, and then installed as a distribution point that is attached to a primary or secondary site that you specify in the destination hierarchy. The upgrade process creates a copy of the migrated content that is stored on the distribution point, and then converts this copy to the single instance content store. When Configuration Manager converts a package to the single instance content store, it deletes that package from the SMSPKG share on the distribution point computer unless the package has one or more advertisements that are configured to **Run program from distribution point**.  

To upgrade the distribution point, Configuration Manager uses the **Source Site Access Account** that is configured to gather data from the SMS Provider of the source site. Although this account requires only **Read** permission for site objects to gather data from the source site, it must also have **Delete** and **Modify** permission to the **Site** class to successfully remove the distribution point from the Configuration Manager 2007 site during the upgrade.  

> [!NOTE]  
>  Configuration Manager can convert content to the single instance store on only one distribution point at a time. When you configure multiple distribution point upgrades, the distribution points are queued for upgrade and processed one at a time.  

Before you upgrade a shared distribution point, ensure that all content that is deployed to the distribution point is migrated. Content that you do not migrate before you upgrade the distribution point is not available in the destination hierarchy after the upgrade. When you upgrade a distribution point, the content in the migrated packages is converted into a format that is compatible with the single instance store of the destination hierarchy.  

To upgrade a distribution point from within the Configuration Manager console, the Configuration Manager 2007 site system server must meet the following conditions:  

-   The distribution point configuration and location must be eligible for upgrade.  

-   The distribution point computer must have sufficient disk space for the content to be converted from the Configuration Manager 2007 content storage format to the single instance store format. This conversion requires available free disk space equal to the size of the largest package that is stored on the distribution point.  

-   The distribution point computer must run an operating system version that is supported as a distribution point in the destination hierarchy.  

    > [!NOTE]  
    >  When Configuration Manager checks for the eligibility of a distribution point for upgrade, it does not validate the operating system version of the distribution point computer.  

To upgrade a distribution point, in the **Administration** workspace, expand **Migration**, expand the **Source Hierarchy** node, and then select the site that contains the distribution point that you want to upgrade. Next, in the details pane, on the **Shared Distribution Points** tab, select the distribution point that you want to upgrade.  

You can confirm that the distribution point is ready for upgrade by viewing the status in the **Eligible for Reassignment** column.  Next, in the Configuration Manager console ribbon, on the **Distribution Points** tab, in the **Distribution Point** group, select **Reassign**. This opens a wizard that you use to complete the upgrade of the distribution point.  

When you upgrade a shared distribution point, you must assign the distribution point to a primary or secondary site of your choice in the destination hierarchy. After the distribution point is upgraded, you manage the distribution point as a distribution point in the destination hierarchy, as you would any other distribution point.  

You can monitor the progress of a distribution point upgrade in the Configuration Manager console by selecting the **Distribution Point Migration** node under the **Migration** node of the **Administration** workspace. You can also view information in the **Migmctrl.log** on the central administration site server of the destination hierarchy, or in the **distmgr.log** on the site server in the destination hierarchy that manages the upgraded distribution point.  

> [!NOTE]  
>  When you upgrade a distribution point to the destination hierarchy, the distribution point site system role is removed from the Configuration Manager 2007 source site; however, packages that were sent to the distribution point are not updated in the Configuration Manager 2007 hierarchy. In the Configuration Manager 2007 console, packages that had been sent to the distribution point continue to list the site system computer as a distribution point with a **Type** of **Unknown**. Subsequent updates to the package in Configuration Manager 2007 result in Distribution Manager reporting errors in the distmgr.log for that site when the site attempts to update the package on the unknown site system.  

If you decide not to upgrade a shared distribution point, you can still install a distribution point from the destination hierarchy on a former Configuration Manager 2007 distribution point. Before you can install the new distribution point, you must first uninstall all Configuration Manager 2007 site system roles from the distribution point computer. This includes the Configuration Manager 2007 site if it is the site server computer. When you uninstall a Configuration Manager 2007 distribution point, content that was deployed to the distribution point is not deleted from the computer.  

###  <a name="BKMK_UpgradeSS"></a> Planning to upgrade Configuration Manager 2007 secondary sites  
 When you use migration to upgrade a shared distribution point that is hosted on a Configuration Manager 2007 secondary site server, Configuration Manager not only upgrades the distribution point site system role to be a distribution point in the destination hierarchy, but also uninstalls the secondary site from the source hierarchy. The result is a System Center Configuration Manager distribution point, but no secondary site.  

 For a distribution point on the site server computer to be eligible for upgrade, Configuration Manager must be able to uninstall the secondary site including each of the site system roles on that computer. Typically, a shared distribution point on a Configuration Manager 2007 server share is eligible for upgrade. However, when a server share exists on the secondary site server, the secondary site and any shared distribution points on that computer are not eligible for upgrade. This is because when the process attempts to uninstall the secondary site, the server share is treated as an additional site system object, and this process cannot uninstall this object. In this scenario, you can enable a standard distribution point on the secondary site server and then redistribute the content to that standard distribution point. This does not use network bandwidth, and when completed, you can uninstall the distribution point on the server share, remove the server share, and then upgrade the distribution point and secondary site.  

 Before you upgrade a shared distribution point, review the distribution point configuration in Configuration Manager 2007 to avoid upgrading a distribution point on a secondary site that you still want to use with Configuration Manager 2007. This is because after you upgrade a shared distribution point that is on a secondary site server, the site system server is removed from the Configuration Manager 2007 hierarchy and is no longer available for use with that hierarchy. When the secondary site is removed, any remaining distribution points at that secondary site are orphaned. This means they become unmanaged from Configuration Manager 2007 and are no longer shared or eligible for upgrade.  

> [! WARNING]  
>  When you view shared distribution points in the Configuration Manager console, there is no visible indication that a shared distribution point is on a remote site system server or whether it is located on the secondary site server.  

 Consider upgrading secondary sites that have a shared distribution point when you have a secondary site in a remote network location that is used primarily to control the deployment of content to that remote location. Because you can configure bandwidth control for when you distribute content to a System Center Configuration Manager distribution point, you can often upgrade a secondary site to a distribution point, configure the distribution point for bandwidth controls, and avoid installing a secondary site in that network location in the destination hierarchy.  

 The process to upgrade a shared distribution point on a secondary site server operates the same as any other shared distribution point upgrade. Content is copied and converted to the  single instance store in use by the destination hierarchy. However, when you upgrade a shared distribution point that is on a secondary site server the upgrade process also uninstalls the management point, if it is present, and then uninstalls the secondary site from the server. The result is that the secondary site is removed from the Configuration Manager 2007 hierarchy. To uninstall the secondary site, Configuration Manager uses the account that is configured to gather data from the source site.  

 During the upgrade, there is a delay between when the Configuration Manager 2007 secondary site is uninstalled and the when the installation of the distribution point in the destination hierarchy begins. The data gathering cycle determines this delay of up to four hours. The delay is intended to provide time for the secondary site to uninstall before the new  distribution point installation begins.  

 For more information about how to upgrade a shared distribution point, see [Planning to upgrade Configuration Manager 2007 shared distribution points](#Planning_to_Upgrade_DPs).  

##  <a name="BKMK_ReassignDistPoint"></a> Planning to Reassign System Center Configuration Manager Distribution Points  
 When you migrate from a supported version of System Center 2012 Configuration Manager to a hierarchy of the same version, you can reassign a shared distribution point from the source hierarchy to a site in the destination hierarchy. This is like the concept of upgrading a Configuration Manager 2007 distribution point to become a distribution point in the destination hierarchy. You can reassign distribution points from both primary sites and secondary sites. The action to reassign a distribution point removes the distribution point from the source hierarchy and makes the computer, and its distribution point, a site system server of the site that you select in the destination hierarchy.  

 When you reassign a distribution point, you do not have to redistribute migrated content that was hosted on the source site distribution point. Additionally, unlike the upgrade of a Configuration Manager 2007 distribution point, reassignment of a distribution point does not require additional disk space on the distribution point computer. This is because beginning with System Center 2012 Configuration Manager, distribution points use the single instance store format for content and the content on the distribution point computer does not need to be converted when the distribution point is reassigned between hierarchies.  

 For a System Center 2012 Configuration Manager distribution point to be eligible for reassignment, it must meet the following criteria:  

-   A shared distribution point must be installed on a computer other than the site server.  

-   A shared distribution point cannot be co-located with any additional site system roles.  

To identify distribution points that are eligible for reassignment in the Configuration Manager console in the **Source Hierarchy** node, select a source site, and then select the **Shared Distribution Points** tab. Eligible distribution points display **Yes** in the **Eligible for Reassignment** column (Prior to System Center 2012 R2 Configuration Manager, this column is named **Eligible for Upgrade**).  

###  <a name="BKMK_ReassignProcess"></a> Distribution point reassignment process  
 You can use the Configuration Manager console to reassign distribution points that you have shared from an active source hierarchy. When you reassign a shared distribution point, the distribution point is uninstalled from its source site, and then installed as a distribution point that is attached to a primary or secondary site that you specify in the destination hierarchy.  

 To reassign the distribution point, the destination hierarchy uses the Source Site Access Account that is configured to gather data from the SMS Provider of the source site. For information about required permissions and additional prerequisites, see [Prerequisites for migration in System Center Configuration Manager](../../core/migration/prerequisites-for-migration.md).  

## Migrate multiple shared distribution points at the same time
Beginning with version 1610, you can use the option to **Reassign Distribution point** to have Configuration Manager process in parllel the reassignment of up to 50 shared distribution points at the same time. This includes shared distribution points from supported source sites that run:  
- Configuration Manager 2007
- System Center 2012 Configuration Manager
- System Center 2012 R2 Configuration Manager
- System Center Configuration Manager Current Branch site

When you reassign distribution points, each distribution point must qualify to be either upgraded or reassigned. The name of the action and process involved, upgrade or reassign, depends on which version of Configuration Manager the source site runs. However, upgraded or reassigned, the end results are the same: The distribution point is assigned to one of your Current Branch sites with its content in-place.

Prior to version 1610, Configuration Manager could process only one distribution point at a time. Now, you can reassign as many distribution points as you want with the following caveats:  
- Although you  cannot multi-select distribution points to be reassigned, when you have queued up more than one, Configuration Manager will process them in parallel instead of waiting to complete one before starting the next.  
- By default, up to 50 distribution points are processed in parallel at a time. After the reassignment of the first distribution point is complete, Configuration Manager will begin to process the 51st, and so on.  
- When you use the Configuration Manager SDK, you can modify the property **SharedDPImportThreadLimit** to adjust the number of reassigned distribution points that Configuration Manager can process in parallel.


##  <a name="About_Migrating_Content"></a> Content ownership when migrating content  
 When you migrate content for deployments, you must assign the content object to a site in the destination hierarchy. This site then becomes the owner for that content in the destination hierarchy. Although the top-level site of your destination hierarchy is the site that migrates the metadata for content, it is the assigned site that accesses the original source files for the content across the network.  

 To minimize the network bandwidth that is used when you migrate content, consider transferring ownership of content to a site in the destination hierarchy that is close on the network to the content location in the source hierarchy. Because information about the content in the destination hierarchy is shared globally, it will be available at every site.  

 Although information about content is shared to all sites by using database replication, any content that you assign to a primary site and then deploy to distribution points at other primary sites, transfers by using file-based replication. This transfer is routed through the central administration site and then to the additional primary site. By centralizing packages that you plan to distribute to multiple primary sites before or during migration when you assign a site as the content owner, you can reduce data transfers across low bandwidth networks.  
