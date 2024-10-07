
VMware 有一个非常强大的命令行工具叫 [PowerCLI](https://github.com)，该工具是基于 PowerShell 开发的模块，主要用于在 Windows 环境中连接和管理传统虚拟化解决方案，比如 vSphere、vSAN 以及 NSX 等。之所以 PowerCLI 非常强大，是因为它几乎可以实现这些解决方案 WEB UI 中的所有管理操作，甚至更多。如果你安装使用过 PowerCLI，应该会发现在 PowerCLI 的安装包里包含了许多不同模块，根据不同模块的名字可以知道它用于不同的解决方案，并且这些模块之间具有互操作兼容性。


其实，在 PowerCLI 中也有用于连接和管理 VMware Cloud Foundation 环境的 [VMware.Sdk.Vcf.SddcManager](https://github.com) 模块，不过相对来说，对于 VCF 环境管理和操作的命令支持较少，而更多使用的是 **PowerVCF** 模块，除此之外还有 Power Validated Solutions 模块。这两个用于 VCF 环境的核心模块与 PowerCLI 不同的是，它们是开源的，不受 VMware 服务支持，你可以为该社区项目做出贡献，随时提 issue 来帮助产品完善和增强。当然，PowerVCF 和 Power Validated Solutions 模块中会包含不同的命令以解决 VCF 环境中不同的需求，比如，Power Validated Solutions 模块更专注用来加速 [VMware Validated Solutions](https://github.com) 解决方案的部署和管理。


PowerVCF 模块：


* [PowerShell Module for VMware Cloud Foundation](https://github.com) (Github)
* [PowerShell Module for VMware Cloud Foundation](https://github.com) (PowerShell Gallery)


Power Validated Solutions 模块：


* [PowerShell Module for VMware Validated Solutions](https://github.com) (Github)
* [PowerShell Module for VMware Validated Solutions](https://github.com) (PowerShell Gallery)


除上面所说的两个核心模块以外，还有其他可用于 VCF 环境的 PowerShell 模块，这些模块通常是专注于某一项具体的任务，可根据需要单独安装这些模块。关于这些模块，你可以在以下链接中找到它们： 


* [PowerShell Module for VMware Cloud Foundation Power Management](https://github.com) (Github)
* [PowerShell Module for VMware Cloud Foundation Instance Recovery](https://github.com) (Github)
* [PowerShell Module for VMware Cloud Foundation Password Management](https://github.com) (Github)
* [PowerShell Module for VMware Cloud Foundation Certificate Management](https://github.com) (Github)
* [PowerShell Module for VMware Cloud Foundation Logging Management](https://github.com) (Github)
* [PowerShell Module for VMware Cloud Foundation Reporting](https://github.com) (Github)


 



## 一、环境要求



安装并使用 PowerVCF 和 Power Validated Solutions 模块对于运行的环境有一定要求，你可以通过以下链接查看关于这两个模块的详细文档说明，本次以当前最新版本为例。


* [PowerShell Module for VMware Cloud Foundation Documentation](https://github.com)
* [PowerShell Module for VMware Validated Solutions Documentation](https://github.com)


PowerVCF 模块：


* 操作系统 Windows 10 和 11，Windows Server 2019 和 2022。
* PowerShell 版本 5\.1，PowerShell Core 版本 7\.x 及更高。
* 依赖模块 PowerCLI 版本 12\.3\.0 及更高。


Power Validated Solutions 模块：


* 操作系统 Windows 10 和 11，Windows Server 2019 和 2022。
* PowerShell Core 版本 7\.2\.0 及更高。
* 依赖模块 PowerCLI 版本 13\.2\.1 及更高，PowerVCF 2\.4\.1 及更高，VMware.vSphere.SsoAdmin 版本 1\.3\.9 及更高，ImportExcel 版本 7\.8\.5 及更高。


综合这两个模块对环境的要求，本次准备和安装以下环境，环境已连接互联网，如果需要在离线环境安装，请参考文档中的说明。


* 操作系统 Windows 11。
* PowerShell 版本 5\.1，PowerShell Core 版本 7\.4\.5。
* 依赖模块 PowerCLI 版本 13\.3\.0，PowerVCF 2\.4\.1，VMware.vSphere.SsoAdmin 版本 1\.3\.9，ImportExcel 版本 7\.8\.5。


 



## 二、PowerVCF 模块



1\.运行 PowerShell，查看 Windows 版本信息。



```
Get-ComputerInfo | Select OsName,OsVersion,WindowsVersion,OSDisplayVersion,OsBuildNumber
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005132412193-495700823.png)](https://github.com)


2\.查看 PowerShell 版本信息，默认由 Windows 操作系统自带。



```
$PSVersionTable
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005132723184-1446130506.png)](https://github.com)


3\.设置软件包存储库的策略，安装 PowerCLI 并查看版本信息。



```
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
Install-Module -Name VMware.PowerCLI -Scope CurrentUser
Get-Module -Name VMware.PowerCLI -ListAvailable
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005135938911-924476061.png)](https://github.com):[豆荚加速器](https://yirou.org)


4\.设置允许本地执行脚本，连接时忽略不信任的证书，关闭 CEIP。



```
 Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
 Set-PowerCLIConfiguration -InvalidCertificateAction Ignore
 Set-PowerCLIConfiguration -ParticipateInCeip $false
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005142043625-32913318.png)](https://github.com)


5\.安装 PowerVCF 并查看版本信息。



```
Install-Module -Name PowerVCF -Repository PSGallery -Scope CurrentUser
Get-Module -Name PowerVCF -ListAvailable
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005142708432-1464387045.png)](https://github.com)


 



## 三、Power Validated Solutions 模块



安装 Power Validated Solutions 模块之前，请确保已完成上述步骤。


1\.下载 [PowerShell Core 7\.4\.5](https://github.com) 并安装，查看 PowerShell Core 版本信息。



```
$PSVersionTable
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005133750844-1980887156.png)](https://github.com)


2\.安装 VMware.vSphere.SsoAdmin 并查看版本信息。



```
Install-Module -Name VMware.vSphere.SsoAdmin -MinimumVersion 1.3.9 -Scope CurrentUser
Get-Module -Name VMware.vSphere.SsoAdmin -ListAvailable
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005143101000-317213924.png)](https://github.com)


3\.安装 ImportExcel 并查看版本信息。



```
Install-Module -Name ImportExcel -MinimumVersion 7.8.5 -Scope CurrentUser
Get-Module -Name ImportExcel -ListAvailable
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005143307389-412514899.png)](https://github.com)


4\.安装 PowerValidatedSolutions 并查看版本信息。



```
Install-Module -Name PowerValidatedSolutions -MinimumVersion 2.11.0 -Scope CurrentUser
Get-Module -Name PowerValidatedSolutions -ListAvailable
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005143642583-260972227.png)](https://github.com)


 



## 四、命令选项



PowerVCF 命令选项：



```
PS C:\Users\JUNIOR_MU> Get-Command -Module PowerVCF

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Commission-VCFHost                                 2.4.1.1000 PowerVCF
Alias           Decommission-VCFHost                               2.4.1.1000 PowerVCF
Alias           Get-VCFAriaAutomation                              2.4.1.1000 PowerVCF
Alias           Get-VCFAriaLifecycle                               2.4.1.1000 PowerVCF
Alias           Get-VCFAriaOperations                              2.4.1.1000 PowerVCF
Alias           Get-VCFAriaOperationsConnection                    2.4.1.1000 PowerVCF
Alias           Get-VCFAriaOperationsLogs                          2.4.1.1000 PowerVCF
Alias           Get-VCFAriaOperationsLogsConnection                2.4.1.1000 PowerVCF
Alias           Get-VCFNsxEdgeCluster                              2.4.1.1000 PowerVCF
Alias           Get-VCFNsxManagerCluster                           2.4.1.1000 PowerVCF
Alias           New-VCFAriaLifecycle                               2.4.1.1000 PowerVCF
Alias           New-VCFNsxEdgeCluster                              2.4.1.1000 PowerVCF
Alias           Remove-VCFAriaLifecycle                            2.4.1.1000 PowerVCF
Alias           Reset-VCFAriaLifecycle                             2.4.1.1000 PowerVCF
Alias           Set-VCFAriaOperationsConnection                    2.4.1.1000 PowerVCF
Alias           Set-VCFAriaOperationsLogsConnection                2.4.1.1000 PowerVCF
Function        Add-VCFApplicationVirtualNetwork                   2.4.1.1000 PowerVCF
Function        Add-VCFNetworkIPPool                               2.4.1.1000 PowerVCF
Function        Connect-CloudBuilder                               2.4.1.1000 PowerVCF
Function        Debug-CatchWriter                                  2.4.1.1000 PowerVCF
Function        Get-CloudBuilderSDDC                               2.4.1.1000 PowerVCF
Function        Get-CloudBuilderSDDCValidation                     2.4.1.1000 PowerVCF
Function        Get-VCFApplicationVirtualNetwork                   2.4.1.1000 PowerVCF
Function        Get-VCFBackupConfiguration                         2.4.1.1000 PowerVCF
Function        Get-VCFBundle                                      2.4.1.1000 PowerVCF
Function        Get-VCFCeip                                        2.4.1.1000 PowerVCF
Function        Get-VCFCertificate                                 2.4.1.1000 PowerVCF
Function        Get-VCFCertificateAuthority                        2.4.1.1000 PowerVCF
Function        Get-VCFCertificateCsr                              2.4.1.1000 PowerVCF
Function        Get-VCFCluster                                     2.4.1.1000 PowerVCF
Function        Get-VCFClusterValidation                           2.4.1.1000 PowerVCF
Function        Get-VCFConfigurationDNS                            2.4.1.1000 PowerVCF
Function        Get-VCFConfigurationDNSValidation                  2.4.1.1000 PowerVCF
Function        Get-VCFConfigurationNTP                            2.4.1.1000 PowerVCF
Function        Get-VCFConfigurationNTPValidation                  2.4.1.1000 PowerVCF
Function        Get-VCFCredential                                  2.4.1.1000 PowerVCF
Function        Get-VCFCredentialExpiry                            2.4.1.1000 PowerVCF
Function        Get-VCFCredentialTask                              2.4.1.1000 PowerVCF
Function        Get-VCFDepotCredential                             2.4.1.1000 PowerVCF
Function        Get-VCFEdgeCluster                                 2.4.1.1000 PowerVCF
Function        Get-VCFFederation                                  2.4.1.1000 PowerVCF
Function        Get-VCFFederationMember                            2.4.1.1000 PowerVCF
Function        Get-VCFFederationTask                              2.4.1.1000 PowerVCF
Function        Get-VCFFipsMode                                    2.4.1.1000 PowerVCF
Function        Get-VCFHealthSummaryTask                           2.4.1.1000 PowerVCF
Function        Get-VCFHost                                        2.4.1.1000 PowerVCF
Function        Get-VCFIdentityProvider                            2.4.1.1000 PowerVCF
Function        Get-VCFLicenseKey                                  2.4.1.1000 PowerVCF
Function        Get-VCFLicenseMode                                 2.4.1.1000 PowerVCF
Function        Get-VCFManager                                     2.4.1.1000 PowerVCF
Function        Get-VCFNetworkIPPool                               2.4.1.1000 PowerVCF
Function        Get-VCFNetworkPool                                 2.4.1.1000 PowerVCF
Function        Get-VCFNsxtCluster                                 2.4.1.1000 PowerVCF
Function        Get-VCFPersonality                                 2.4.1.1000 PowerVCF
Function        Get-VCFProxy                                       2.4.1.1000 PowerVCF
Function        Get-VCFPSC                                         2.4.1.1000 PowerVCF
Function        Get-VCFRelease                                     2.4.1.1000 PowerVCF
Function        Get-VCFRestoreTask                                 2.4.1.1000 PowerVCF
Function        Get-VCFRole                                        2.4.1.1000 PowerVCF
Function        Get-VCFService                                     2.4.1.1000 PowerVCF
Function        Get-VCFSupportBundleTask                           2.4.1.1000 PowerVCF
Function        Get-VCFSystemPrecheckTask                          2.4.1.1000 PowerVCF
Function        Get-VCFTask                                        2.4.1.1000 PowerVCF
Function        Get-VCFUpgradable                                  2.4.1.1000 PowerVCF
Function        Get-VCFUpgrade                                     2.4.1.1000 PowerVCF
Function        Get-VCFUser                                        2.4.1.1000 PowerVCF
Function        Get-VCFvCenter                                     2.4.1.1000 PowerVCF
Function        Get-VCFVra                                         2.4.1.1000 PowerVCF
Function        Get-VCFVrli                                        2.4.1.1000 PowerVCF
Function        Get-VCFVrliConnection                              2.4.1.1000 PowerVCF
Function        Get-VCFVrops                                       2.4.1.1000 PowerVCF
Function        Get-VCFVropsConnection                             2.4.1.1000 PowerVCF
Function        Get-VCFVrslcm                                      2.4.1.1000 PowerVCF
Function        Get-VCFWorkloadDomain                              2.4.1.1000 PowerVCF
Function        Get-VCFWsa                                         2.4.1.1000 PowerVCF
Function        Invoke-VCFCommand                                  2.4.1.1000 PowerVCF
Function        Join-VCFFederation                                 2.4.1.1000 PowerVCF
Function        New-VCFCluster                                     2.4.1.1000 PowerVCF
Function        New-VCFCommissionedHost                            2.4.1.1000 PowerVCF
Function        New-VCFEdgeCluster                                 2.4.1.1000 PowerVCF
Function        New-VCFFederationInvite                            2.4.1.1000 PowerVCF
Function        New-VCFGroup                                       2.4.1.1000 PowerVCF
Function        New-VCFIdentityProvider                            2.4.1.1000 PowerVCF
Function        New-VCFLicenseKey                                  2.4.1.1000 PowerVCF
Function        New-VCFNetworkPool                                 2.4.1.1000 PowerVCF
Function        New-VCFPersonality                                 2.4.1.1000 PowerVCF
Function        New-VCFServiceUser                                 2.4.1.1000 PowerVCF
Function        New-VCFUser                                        2.4.1.1000 PowerVCF
Function        New-VCFVrslcm                                      2.4.1.1000 PowerVCF
Function        New-VCFWorkloadDomain                              2.4.1.1000 PowerVCF
Function        Remove-VCFCertificateAuthority                     2.4.1.1000 PowerVCF
Function        Remove-VCFCluster                                  2.4.1.1000 PowerVCF
Function        Remove-VCFCommissionedHost                         2.4.1.1000 PowerVCF
Function        Remove-VCFFederation                               2.4.1.1000 PowerVCF
Function        Remove-VCFIdentityProvider                         2.4.1.1000 PowerVCF
Function        Remove-VCFLicenseKey                               2.4.1.1000 PowerVCF
Function        Remove-VCFNetworkIPPool                            2.4.1.1000 PowerVCF
Function        Remove-VCFNetworkPool                              2.4.1.1000 PowerVCF
Function        Remove-VCFUser                                     2.4.1.1000 PowerVCF
Function        Remove-VCFVrslcm                                   2.4.1.1000 PowerVCF
Function        Remove-VCFWorkloadDomain                           2.4.1.1000 PowerVCF
Function        Request-VCFBundle                                  2.4.1.1000 PowerVCF
Function        Request-VCFCertificate                             2.4.1.1000 PowerVCF
Function        Request-VCFCertificateCsr                          2.4.1.1000 PowerVCF
Function        Request-VCFHealthSummaryBundle                     2.4.1.1000 PowerVCF
Function        Request-VCFSupportBundle                           2.4.1.1000 PowerVCF
Function        Request-VCFToken                                   2.4.1.1000 PowerVCF
Function        Reset-VCFVrslcm                                    2.4.1.1000 PowerVCF
Function        Restart-CloudBuilderSDDC                           2.4.1.1000 PowerVCF
Function        Restart-CloudBuilderSDDCValidation                 2.4.1.1000 PowerVCF
Function        Restart-VCFCredentialTask                          2.4.1.1000 PowerVCF
Function        Restart-VCFTask                                    2.4.1.1000 PowerVCF
Function        Set-VCFBackupConfiguration                         2.4.1.1000 PowerVCF
Function        Set-VCFCeip                                        2.4.1.1000 PowerVCF
Function        Set-VCFCertificate                                 2.4.1.1000 PowerVCF
Function        Set-VCFCluster                                     2.4.1.1000 PowerVCF
Function        Set-VCFConfigurationDNS                            2.4.1.1000 PowerVCF
Function        Set-VCFConfigurationNTP                            2.4.1.1000 PowerVCF
Function        Set-VCFCredential                                  2.4.1.1000 PowerVCF
Function        Set-VCFCredentialAutoRotatePolicy                  2.4.1.1000 PowerVCF
Function        Set-VCFDepotCredential                             2.4.1.1000 PowerVCF
Function        Set-VCFFederation                                  2.4.1.1000 PowerVCF
Function        Set-VCFMicrosoftCa                                 2.4.1.1000 PowerVCF
Function        Set-VCFOpensslCa                                   2.4.1.1000 PowerVCF
Function        Set-VCFProxy                                       2.4.1.1000 PowerVCF
Function        Set-VCFVrliConnection                              2.4.1.1000 PowerVCF
Function        Set-VCFVropsConnection                             2.4.1.1000 PowerVCF
Function        Set-VCFWorkloadDomain                              2.4.1.1000 PowerVCF
Function        Start-CloudBuilderSDDC                             2.4.1.1000 PowerVCF
Function        Start-CloudBuilderSDDCValidation                   2.4.1.1000 PowerVCF
Function        Start-SetupLogFile                                 2.4.1.1000 PowerVCF
Function        Start-VCFBackup                                    2.4.1.1000 PowerVCF
Function        Start-VCFBundleUpload                              2.4.1.1000 PowerVCF
Function        Start-VCFHealthSummary                             2.4.1.1000 PowerVCF
Function        Start-VCFRestore                                   2.4.1.1000 PowerVCF
Function        Start-VCFSupportBundle                             2.4.1.1000 PowerVCF
Function        Start-VCFSystemPrecheck                            2.4.1.1000 PowerVCF
Function        Start-VCFUpgrade                                   2.4.1.1000 PowerVCF
Function        Stop-CloudBuilderSDDCValidation                    2.4.1.1000 PowerVCF
Function        Stop-VCFCredentialTask                             2.4.1.1000 PowerVCF
Function        Update-VCFIdentityProvider                         2.4.1.1000 PowerVCF
Function        Write-LogMessage                                   2.4.1.1000 PowerVCF

PS C:\Users\JUNIOR_MU>
```

Power Validated Solutions 命令选项：



```
PS C:\Users\JUNIOR_MU> Get-Command -Module PowerValidatedSolutions

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Request-NsxToken                                   2.11.1.10… PowerValidatedSolutions
Function        Add-AntiAffinityRule                               2.11.1.10… PowerValidatedSolutions
Function        Add-AriaNetworksLdapConfiguration                  2.11.1.10… PowerValidatedSolutions
Function        Add-AriaNetworksNsxDataSource                      2.11.1.10… PowerValidatedSolutions
Function        Add-AriaNetworksVcenterDataSource                  2.11.1.10… PowerValidatedSolutions
Function        Add-CertToNsxCertificateStore                      2.11.1.10… PowerValidatedSolutions
Function        Add-ClusterGroup                                   2.11.1.10… PowerValidatedSolutions
Function        Add-ContentLibrary                                 2.11.1.10… PowerValidatedSolutions
Function        Add-DrsVmToVmGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Add-EsxiVrmsStaticRoute                            2.11.1.10… PowerValidatedSolutions
Function        Add-EsxiVrmsVMkernelPort                           2.11.1.10… PowerValidatedSolutions
Function        Add-GlobalPermission                               2.11.1.10… PowerValidatedSolutions
Function        Add-IdentitySource                                 2.11.1.10… PowerValidatedSolutions
Function        Add-Namespace                                      2.11.1.10… PowerValidatedSolutions
Function        Add-NamespacePermission                            2.11.1.10… PowerValidatedSolutions
Function        Add-NamespaceVmClass                               2.11.1.10… PowerValidatedSolutions
Function        Add-NetworkSegment                                 2.11.1.10… PowerValidatedSolutions
Function        Add-NsxtIdentitySource                             2.11.1.10… PowerValidatedSolutions
Function        Add-NsxtLdapRole                                   2.11.1.10… PowerValidatedSolutions
Function        Add-NsxtNodeProfileSyslogExporter                  2.11.1.10… PowerValidatedSolutions
Function        Add-NsxtPrefix                                     2.11.1.10… PowerValidatedSolutions
Function        Add-NsxtPrincipalIdentity                          2.11.1.10… PowerValidatedSolutions
Function        Add-NsxtVidmRole                                   2.11.1.10… PowerValidatedSolutions
Function        Add-PrefixList                                     2.11.1.10… PowerValidatedSolutions
Function        Add-ProtectionGroup                                2.11.1.10… PowerValidatedSolutions
Function        Add-RecoveryPlan                                   2.11.1.10… PowerValidatedSolutions
Function        Add-ResourcePool                                   2.11.1.10… PowerValidatedSolutions
Function        Add-RouteMap                                       2.11.1.10… PowerValidatedSolutions
Function        Add-SddcManagerRole                                2.11.1.10… PowerValidatedSolutions
Function        Add-SrmLicenseKey                                  2.11.1.10… PowerValidatedSolutions
Function        Add-SrmMapping                                     2.11.1.10… PowerValidatedSolutions
Function        Add-SrmProtectionGroup                             2.11.1.10… PowerValidatedSolutions
Function        Add-SrmRecoveryPlan                                2.11.1.10… PowerValidatedSolutions
Function        Add-SrmRecoveryPlanCalloutStep                     2.11.1.10… PowerValidatedSolutions
Function        Add-SsoPermission                                  2.11.1.10… PowerValidatedSolutions
Function        Add-SsoUser                                        2.11.1.10… PowerValidatedSolutions
Function        Add-StorageFolder                                  2.11.1.10… PowerValidatedSolutions
Function        Add-StoragePolicy                                  2.11.1.10… PowerValidatedSolutions
Function        Add-SupervisorClusterLicense                       2.11.1.10… PowerValidatedSolutions
Function        Add-SupervisorService                              2.11.1.10… PowerValidatedSolutions
Function        Add-TanzuKubernetesCluster                         2.11.1.10… PowerValidatedSolutions
Function        Add-vCenterGlobalPermission                        2.11.1.10… PowerValidatedSolutions
Function        Add-VdsPortGroup                                   2.11.1.10… PowerValidatedSolutions
Function        Add-VMClass                                        2.11.1.10… PowerValidatedSolutions
Function        Add-VMFolder                                       2.11.1.10… PowerValidatedSolutions
Function        Add-VmGroup                                        2.11.1.10… PowerValidatedSolutions
Function        Add-VmStartupRule                                  2.11.1.10… PowerValidatedSolutions
Function        Add-vRACloudAccount                                2.11.1.10… PowerValidatedSolutions
Function        Add-vRAGroup                                       2.11.1.10… PowerValidatedSolutions
Function        Add-vRAIntegrationItem                             2.11.1.10… PowerValidatedSolutions
Function        Add-vRANotification                                2.11.1.10… PowerValidatedSolutions
Function        Add-vRAResourceComputeTag                          2.11.1.10… PowerValidatedSolutions
Function        Add-vRAUser                                        2.11.1.10… PowerValidatedSolutions
Function        Add-vRLIAgentGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Add-vRLIAlertDatacenter                            2.11.1.10… PowerValidatedSolutions
Function        Add-vRLIAlertVirtualMachine                        2.11.1.10… PowerValidatedSolutions
Function        Add-vRLIAuthenticationAD                           2.11.1.10… PowerValidatedSolutions
Function        Add-vRLIAuthenticationGroup                        2.11.1.10… PowerValidatedSolutions
Function        Add-vRLIAuthenticationWSA                          2.11.1.10… PowerValidatedSolutions
Function        Add-vRLIGroup                                      2.11.1.10… PowerValidatedSolutions
Function        Add-vRLILogArchive                                 2.11.1.10… PowerValidatedSolutions
Function        Add-vRLILogForwarder                               2.11.1.10… PowerValidatedSolutions
Function        Add-vRLISmtpConfiguration                          2.11.1.10… PowerValidatedSolutions
Function        Add-VrmsNetworkAdapter                             2.11.1.10… PowerValidatedSolutions
Function        Add-VrmsReplication                                2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapter                                   2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapterIdentityManager                    2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapterNsxt                               2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapterPing                               2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapterSddcHealth                         2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapterSrm                                2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapterVcf                                2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAdapterVr                                 2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAlertPlugin                               2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSAlertPluginEmail                          2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSCollectorGroup                            2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSCredential                                2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSCurrency                                  2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSGroupRemoteCollectors                     2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSNsxCredential                             2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSNtpServer                                 2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSUserAccount                               2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSUserGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSVcenterCredential                         2.11.1.10… PowerValidatedSolutions
Function        Add-vROPSVcfCredential                             2.11.1.10… PowerValidatedSolutions
Function        Add-vROTrustedCertificate                          2.11.1.10… PowerValidatedSolutions
Function        Add-vROvCenterServer                               2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMApplianceNtpConfig                       2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMDatacenter                               2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMDatacenterVcenter                        2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMEnvironment                              2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMGroup                                    2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMGroupRole                                2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMLockerCertificate                        2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMLockerLicense                            2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMLockerPassword                           2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMMyVMwareAccount                          2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMNtpServer                                2.11.1.10… PowerValidatedSolutions
Function        Add-vRSLCMProductNtpServer                         2.11.1.10… PowerValidatedSolutions
Function        Add-vSphereReplication                             2.11.1.10… PowerValidatedSolutions
Function        Add-vSphereRole                                    2.11.1.10… PowerValidatedSolutions
Function        Add-WorkspaceOneDirectory                          2.11.1.10… PowerValidatedSolutions
Function        Add-WorkspaceOneDirectoryConnector                 2.11.1.10… PowerValidatedSolutions
Function        Add-WorkspaceOneDirectoryGroup                     2.11.1.10… PowerValidatedSolutions
Function        Add-WorkspaceOneRole                               2.11.1.10… PowerValidatedSolutions
Function        Add-WSAClient                                      2.11.1.10… PowerValidatedSolutions
Function        Add-WSAConnector                                   2.11.1.10… PowerValidatedSolutions
Function        Add-WSALdapDirectory                               2.11.1.10… PowerValidatedSolutions
Function        Add-WSARoleAssociation                             2.11.1.10… PowerValidatedSolutions
Function        Backup-VMOvfProperties                             2.11.1.10… PowerValidatedSolutions
Function        Connect-DRSolutionTovCenter                        2.11.1.10… PowerValidatedSolutions
Function        Connect-SrmRemoteSession                           2.11.1.10… PowerValidatedSolutions
Function        Connect-VrmsRemoteSession                          2.11.1.10… PowerValidatedSolutions
Function        Connect-vRSLCMUpgradeIso                           2.11.1.10… PowerValidatedSolutions
Function        Connect-vSphereMobServer                           2.11.1.10… PowerValidatedSolutions
Function        Connect-WMCluster                                  2.11.1.10… PowerValidatedSolutions
Function        Copy-vRealizeLoadBalancer                          2.11.1.10… PowerValidatedSolutions
Function        Copy-vSphereRole                                   2.11.1.10… PowerValidatedSolutions
Function        Debug-ExceptionWriter                              2.11.1.10… PowerValidatedSolutions
Function        Deploy-PhotonAppliance                             2.11.1.10… PowerValidatedSolutions
Function        Disable-vRLIAlert                                  2.11.1.10… PowerValidatedSolutions
Function        Disconnect-vRSLCMUpgradeIso                        2.11.1.10… PowerValidatedSolutions
Function        Disconnect-vSphereMobServer                        2.11.1.10… PowerValidatedSolutions
Function        Disconnect-WMCluster                               2.11.1.10… PowerValidatedSolutions
Function        Enable-Registry                                    2.11.1.10… PowerValidatedSolutions
Function        Enable-SupervisorCluster                           2.11.1.10… PowerValidatedSolutions
Function        Enable-vRLIAlert                                   2.11.1.10… PowerValidatedSolutions
Function        Enable-vRLIContentPack                             2.11.1.10… PowerValidatedSolutions
Function        Enable-vROPSManagementPack                         2.11.1.10… PowerValidatedSolutions
Function        Enable-WMRegistry                                  2.11.1.10… PowerValidatedSolutions
Function        Export-AriaNetworksJsonSpec                        2.11.1.10… PowerValidatedSolutions
Function        Export-CbrJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-CbwJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-CcmJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-DriJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-GlobalWsaJsonSpec                           2.11.1.10… PowerValidatedSolutions
Function        Export-HrmJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-IamJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-IlaJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-InvJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-IomJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-PcaJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-PdrJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-vRAJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Export-vRLIJsonSpec                                2.11.1.10… PowerValidatedSolutions
Function        Export-vROPsJsonSpec                               2.11.1.10… PowerValidatedSolutions
Function        Export-vRSLCMJsonSpec                              2.11.1.10… PowerValidatedSolutions
Function        Export-WsaJsonSpec                                 2.11.1.10… PowerValidatedSolutions
Function        Get-ADPrincipalGuid                                2.11.1.10… PowerValidatedSolutions
Function        Get-AriaNetworksDataSource                         2.11.1.10… PowerValidatedSolutions
Function        Get-AriaNetworksLdapConfiguration                  2.11.1.10… PowerValidatedSolutions
Function        Get-AriaNetworksNodes                              2.11.1.10… PowerValidatedSolutions
Function        Get-DrsVmToVmGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Get-ESXiAdminGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Get-EsxiAlert                                      2.11.1.10… PowerValidatedSolutions
Function        Get-GlobalPermission                               2.11.1.10… PowerValidatedSolutions
Function        Get-LocalAccountLockout                            2.11.1.10… PowerValidatedSolutions
Function        Get-LocalPasswordComplexity                        2.11.1.10… PowerValidatedSolutions
Function        Get-LocalUserPasswordExpiration                    2.11.1.10… PowerValidatedSolutions
Function        Get-MscaRootCertificate                            2.11.1.10… PowerValidatedSolutions
Function        Get-NsxEdgeCluster                                 2.11.1.10… PowerValidatedSolutions
Function        Get-NSXLBDetails                                   2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtAlarm                                      2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtApplianceUser                              2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtBackupConfiguration                        2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtBackupHistory                              2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtCertificate                                2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtComputeManager                             2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtComputeManagerStatus                       2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtEdgeCluster                                2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtEdgeNode                                   2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtEvent                                      2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtGlobalSegmentID                            2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtGroup                                      2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtLdap                                       2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtLdapStatus                                 2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtLocaleService                              2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtLogicalRouter                              2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtNodeProfile                                2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtPrefixList                                 2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtPrincipalIdentity                          2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtRole                                       2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtRouteMap                                   2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtRoutingConfigRedistribution                2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtRoutingConfigRedistributionRule            2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtRoutingConfigRouteMap                      2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtSecurityPolicy                             2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtSegment                                    2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtServerDetail                               2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtSyslogExporter                             2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtSyslogStatus                               2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTier0BgpStatus                             2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTier0Gateway                               2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTier0LocaleServiceBgp                      2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTier1Gateway                               2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTransportNode                              2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTransportNodeStatus                        2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTransportNodeTunnel                        2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTransportNodeTunnelStatus                  2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtTransportZone                              2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtUser                                       2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtVidm                                       2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtVidmGroup                                  2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtVidmStatus                                 2.11.1.10… PowerValidatedSolutions
Function        Get-NsxtVidmUser                                   2.11.1.10… PowerValidatedSolutions
Function        Get-SnapshotConsolidation                          2.11.1.10… PowerValidatedSolutions
Function        Get-SnapshotStatus                                 2.11.1.10… PowerValidatedSolutions
Function        Get-SrmApplianceDetail                             2.11.1.10… PowerValidatedSolutions
Function        Get-SrmConfiguration                               2.11.1.10… PowerValidatedSolutions
Function        Get-SrmNetworkAll                                  2.11.1.10… PowerValidatedSolutions
Function        Get-SrmNetworkDns                                  2.11.1.10… PowerValidatedSolutions
Function        Get-SrmNetworkInterface                            2.11.1.10… PowerValidatedSolutions
Function        Get-SrmProtectionGroup                             2.11.1.10… PowerValidatedSolutions
Function        Get-SrmRecoveryPlan                                2.11.1.10… PowerValidatedSolutions
Function        Get-SrmRecoveryPlanStep                            2.11.1.10… PowerValidatedSolutions
Function        Get-SrmRecoveryPlanVm                              2.11.1.10… PowerValidatedSolutions
Function        Get-SrmService                                     2.11.1.10… PowerValidatedSolutions
Function        Get-SrmSitePairing                                 2.11.1.10… PowerValidatedSolutions
Function        Get-SrmTask                                        2.11.1.10… PowerValidatedSolutions
Function        Get-SrmVamiCertificate                             2.11.1.10… PowerValidatedSolutions
Function        Get-SubscribedLibrary                              2.11.1.10… PowerValidatedSolutions
Function        Get-TanzuKubernetesCluster                         2.11.1.10… PowerValidatedSolutions
Function        Get-VCConfigurationDNS                             2.11.1.10… PowerValidatedSolutions
Function        Get-VCConfigurationNTP                             2.11.1.10… PowerValidatedSolutions
Function        Get-VcenterBackupStatus                            2.11.1.10… PowerValidatedSolutions
Function        Get-VCenterCEIP                                    2.11.1.10… PowerValidatedSolutions
Function        Get-VcenterPasswordExpiration                      2.11.1.10… PowerValidatedSolutions
Function        Get-VcenterRootPasswordExpiration                  2.11.1.10… PowerValidatedSolutions
Function        Get-vCenterServerDetail                            2.11.1.10… PowerValidatedSolutions
Function        Get-VcenterTriggeredAlarm                          2.11.1.10… PowerValidatedSolutions
Function        Get-VCFDnsSearchDomain                             2.11.1.10… PowerValidatedSolutions
Function        Get-VcLicense                                      2.11.1.10… PowerValidatedSolutions
Function        Get-VCVersion                                      2.11.1.10… PowerValidatedSolutions
Function        Get-VMClass                                        2.11.1.10… PowerValidatedSolutions
Function        Get-VMOvfProperty                                  2.11.1.10… PowerValidatedSolutions
Function        Get-VMvAppConfig                                   2.11.1.10… PowerValidatedSolutions
Function        Get-vRAAPIVersion                                  2.11.1.10… PowerValidatedSolutions
Function        Get-vRACloudAccount                                2.11.1.10… PowerValidatedSolutions
Function        Get-vRACloudZone                                   2.11.1.10… PowerValidatedSolutions
Function        Get-vRAGroup                                       2.11.1.10… PowerValidatedSolutions
Function        Get-vRAGroupRoles                                  2.11.1.10… PowerValidatedSolutions
Function        Get-vRAIntegrationDetail                           2.11.1.10… PowerValidatedSolutions
Function        Get-vRANotification                                2.11.1.10… PowerValidatedSolutions
Function        Get-vRAOrganizationDisplayName                     2.11.1.10… PowerValidatedSolutions
Function        Get-vRAOrganizationId                              2.11.1.10… PowerValidatedSolutions
Function        Get-vRAResourceCompute                             2.11.1.10… PowerValidatedSolutions
Function        Get-vRAServerDetail                                2.11.1.10… PowerValidatedSolutions
Function        Get-vRAServices                                    2.11.1.10… PowerValidatedSolutions
Function        Get-vRAUser                                        2.11.1.10… PowerValidatedSolutions
Function        Get-vRAUserRoles                                   2.11.1.10… PowerValidatedSolutions
Function        Get-vRAvRLIConfig                                  2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIAgentGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIAlert                                      2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIAuthenticationAD                           2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIAuthenticationWSA                          2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIContentPack                                2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIEmailNotification                          2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIGroup                                      2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIIndexPartition                             2.11.1.10… PowerValidatedSolutions
Function        Get-vRLILogForwarder                               2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIMarketplaceMetadata                        2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIRetentionThreshold                         2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIRole                                       2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIServerDetail                               2.11.1.10… PowerValidatedSolutions
Function        Get-vRLISmtpConfiguration                          2.11.1.10… PowerValidatedSolutions
Function        Get-vRLIVersion                                    2.11.1.10… PowerValidatedSolutions
Function        Get-VrmsConfiguration                              2.11.1.10… PowerValidatedSolutions
Function        Get-VrmsDatastore                                  2.11.1.10… PowerValidatedSolutions
Function        Get-VrmsNetworkInterface                           2.11.1.10… PowerValidatedSolutions
Function        Get-VrmsReplication                                2.11.1.10… PowerValidatedSolutions
Function        Get-VrmsSitePairing                                2.11.1.10… PowerValidatedSolutions
Function        Get-VrmsVamiCertificate                            2.11.1.10… PowerValidatedSolutions
Function        Get-VrmsVm                                         2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSAdapter                                   2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSAdapterKind                               2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSAlertDefinition                           2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSAlertPlugin                               2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSAuthRole                                  2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSAuthSource                                2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSCollector                                 2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSCollectorGroup                            2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSCredential                                2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSCurrency                                  2.11.1.10… PowerValidatedSolutions
Function        Get-vROpsLogForwarding                             2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSManagementPack                            2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSManagementPackActivity                    2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSManagementPackStatus                      2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSNotification                              2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSResourceDetail                            2.11.1.10… PowerValidatedSolutions
Function        Get-vROPsServerDetail                              2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSSolution                                  2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSUserAccount                               2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSUserGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Get-vROPSVersion                                   2.11.1.10… PowerValidatedSolutions
Function        Get-vROVersion                                     2.11.1.10… PowerValidatedSolutions
Function        Get-vROWorkflow                                    2.11.1.10… PowerValidatedSolutions
Function        Get-vROWorkflowExecution                           2.11.1.10… PowerValidatedSolutions
Function        Get-vROWorkflowExecutionResult                     2.11.1.10… PowerValidatedSolutions
Function        Get-vROWorkflowExecutionState                      2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMApplianceNtpConfig                       2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMDatacenter                               2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMDatacenterVcenter                        2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMEnvironment                              2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMEnvironmentVMs                           2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMGroup                                    2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMHealth                                   2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMLoadbalancer                             2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMLockerCertificate                        2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMLockerLicense                            2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMLockerPassword                           2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMMyVmwareAccount                          2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMProductBinaries                          2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMProductBinariesMapped                    2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMProductDetails                           2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMProductNode                              2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMProductNtpServer                         2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMProductPassword                          2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMProductVersion                           2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMPSPack                                   2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMRequest                                  2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMRole                                     2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMServerDetail                             2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMSshStatus                                2.11.1.10… PowerValidatedSolutions
Function        Get-vRSLCMUpgradeStatus                            2.11.1.10… PowerValidatedSolutions
Function        Get-VsanHealthTest                                 2.11.1.10… PowerValidatedSolutions
Function        Get-WMLicenseStatus                                2.11.1.10… PowerValidatedSolutions
Function        Get-WMRegistry                                     2.11.1.10… PowerValidatedSolutions
Function        Get-WMRegistryHealth                               2.11.1.10… PowerValidatedSolutions
Function        Get-WsaAccountLockout                              2.11.1.10… PowerValidatedSolutions
Function        Get-WSAActiveDirectoryGroupDetail                  2.11.1.10… PowerValidatedSolutions
Function        Get-WSAClient                                      2.11.1.10… PowerValidatedSolutions
Function        Get-WSAConnector                                   2.11.1.10… PowerValidatedSolutions
Function        Get-WSADirectory                                   2.11.1.10… PowerValidatedSolutions
Function        Get-WSADirectoryDomain                             2.11.1.10… PowerValidatedSolutions
Function        Get-WSAGroup                                       2.11.1.10… PowerValidatedSolutions
Function        Get-WSAIdentityProvider                            2.11.1.10… PowerValidatedSolutions
Function        Get-WSAOAuthToken                                  2.11.1.10… PowerValidatedSolutions
Function        Get-WsaPasswordPolicy                              2.11.1.10… PowerValidatedSolutions
Function        Get-WSARole                                        2.11.1.10… PowerValidatedSolutions
Function        Get-WSARoleAssociation                             2.11.1.10… PowerValidatedSolutions
Function        Get-WSARoleId                                      2.11.1.10… PowerValidatedSolutions
Function        Get-WSARuleSet                                     2.11.1.10… PowerValidatedSolutions
Function        Get-WSAServerDetail                                2.11.1.10… PowerValidatedSolutions
Function        Get-WSASmtpConfiguration                           2.11.1.10… PowerValidatedSolutions
Function        Get-WSAUser                                        2.11.1.10… PowerValidatedSolutions
Function        Import-ContentLibraryItem                          2.11.1.10… PowerValidatedSolutions
Function        Import-vROPSManagementPack                         2.11.1.10… PowerValidatedSolutions
Function        Import-vROPSNotification                           2.11.1.10… PowerValidatedSolutions
Function        Import-vROPSUserAccount                            2.11.1.10… PowerValidatedSolutions
Function        Import-vROPSUserGroup                              2.11.1.10… PowerValidatedSolutions
Function        Import-vRSLCMLockerCertificate                     2.11.1.10… PowerValidatedSolutions
Function        Initialize-WorkspaceOne                            2.11.1.10… PowerValidatedSolutions
Function        Install-SiteRecoveryManager                        2.11.1.10… PowerValidatedSolutions
Function        Install-SupervisorClusterCertificate               2.11.1.10… PowerValidatedSolutions
Function        Install-TanzuSignedCertificate                     2.11.1.10… PowerValidatedSolutions
Function        Install-VamiCertificate                            2.11.1.10… PowerValidatedSolutions
Function        Install-vRLIContentPack                            2.11.1.10… PowerValidatedSolutions
Function        Install-vRLIPhotonAgent                            2.11.1.10… PowerValidatedSolutions
Function        Install-vROPSManagementPack                        2.11.1.10… PowerValidatedSolutions
Function        Install-vRSLCMCertificate                          2.11.1.10… PowerValidatedSolutions
Function        Install-vRSLCMPSPack                               2.11.1.10… PowerValidatedSolutions
Function        Install-vSphereReplicationManager                  2.11.1.10… PowerValidatedSolutions
Function        Install-WMClusterCertificate                       2.11.1.10… PowerValidatedSolutions
Function        Install-WorkspaceOne                               2.11.1.10… PowerValidatedSolutions
Function        Install-WorkspaceOneCertificate                    2.11.1.10… PowerValidatedSolutions
Function        Invoke-CbrDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-CbrSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-CbwDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-CbwSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-CcmDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-CcmSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-DriDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-GenerateChainPem                            2.11.1.10… PowerValidatedSolutions
Function        Invoke-GeneratePKCS12                              2.11.1.10… PowerValidatedSolutions
Function        Invoke-GeneratePrivateKeyAndCsr                    2.11.1.10… PowerValidatedSolutions
Function        Invoke-GlobalWsaDeployment                         2.11.1.10… PowerValidatedSolutions
Function        Invoke-HrmDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-IamDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-IlaDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-IlaSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-InvDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-InvSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-IomDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-IomSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-PcaDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-PcaSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-PdrDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-PdrSolutionInterop                          2.11.1.10… PowerValidatedSolutions
Function        Invoke-RequestSignedCertificate                    2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoCbrDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoCbrSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoCbwDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoCbwSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoCcmDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoCcmSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoDriDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoGlobalWsaDeployment                     2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoHrmDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoIamDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoIlaDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoIlaSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoInvDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoInvSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoIomDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoIomSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoPcaDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoPcaSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoPdrDeployment                           2.11.1.10… PowerValidatedSolutions
Function        Invoke-UndoPdrSolutionInterop                      2.11.1.10… PowerValidatedSolutions
Function        Invoke-VcenterCommand                              2.11.1.10… PowerValidatedSolutions
Function        Invoke-vRORestMethod                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-vROWorkflow                                 2.11.1.10… PowerValidatedSolutions
Function        Invoke-vRSLCMDeployment                            2.11.1.10… PowerValidatedSolutions
Function        Invoke-VrslcmUndoDeployment                        2.11.1.10… PowerValidatedSolutions
Function        Invoke-vRSLCMUpgrade                               2.11.1.10… PowerValidatedSolutions
Function        Invoke-WsaDirectorySync                            2.11.1.10… PowerValidatedSolutions
Function        Move-VMtoFolder                                    2.11.1.10… PowerValidatedSolutions
Function        New-AriaNetworksDeployment                         2.11.1.10… PowerValidatedSolutions
Function        New-AriaNetworksLdapConfiguration                  2.11.1.10… PowerValidatedSolutions
Function        New-AriaNetworksNsxtDataSource                     2.11.1.10… PowerValidatedSolutions
Function        New-AriaNetworksvCenterDataSource                  2.11.1.10… PowerValidatedSolutions
Function        New-NsxtLBAppProfile                               2.11.1.10… PowerValidatedSolutions
Function        New-NsxtLBPersistenceAppProfile                    2.11.1.10… PowerValidatedSolutions
Function        New-NsxtLBPool                                     2.11.1.10… PowerValidatedSolutions
Function        New-NsxtLBServiceMonitor                           2.11.1.10… PowerValidatedSolutions
Function        New-NsxtLBVirtualServer                            2.11.1.10… PowerValidatedSolutions
Function        New-NsxtLdap                                       2.11.1.10… PowerValidatedSolutions
Function        New-NsxtLoadBalancer                               2.11.1.10… PowerValidatedSolutions
Function        New-NsxtPrefixList                                 2.11.1.10… PowerValidatedSolutions
Function        New-NsxtPrincipalIdentity                          2.11.1.10… PowerValidatedSolutions
Function        New-NsxtRouteMap                                   2.11.1.10… PowerValidatedSolutions
Function        New-NsxtSegment                                    2.11.1.10… PowerValidatedSolutions
Function        New-NsxtTier0BgpNeighborConfig                     2.11.1.10… PowerValidatedSolutions
Function        New-NsxtTier1                                      2.11.1.10… PowerValidatedSolutions
Function        New-NsxtTier1ServiceInterface                      2.11.1.10… PowerValidatedSolutions
Function        New-NsxtTier1StaticRoute                           2.11.1.10… PowerValidatedSolutions
Function        New-SrmSitePair                                    2.11.1.10… PowerValidatedSolutions
Function        New-SupervisorClusterCSR                           2.11.1.10… PowerValidatedSolutions
Function        New-TanzuKubernetesCluster                         2.11.1.10… PowerValidatedSolutions
Function        New-VcLicense                                      2.11.1.10… PowerValidatedSolutions
Function        New-VMOvfProduct                                   2.11.1.10… PowerValidatedSolutions
Function        New-VMOvfProperty                                  2.11.1.10… PowerValidatedSolutions
Function        New-vRACloudAccount                                2.11.1.10… PowerValidatedSolutions
Function        New-vRADeployment                                  2.11.1.10… PowerValidatedSolutions
Function        New-vRAGroup                                       2.11.1.10… PowerValidatedSolutions
Function        New-vRANotification                                2.11.1.10… PowerValidatedSolutions
Function        New-vRAUser                                        2.11.1.10… PowerValidatedSolutions
Function        New-vRAvROPSIntegrationItem                        2.11.1.10… PowerValidatedSolutions
Function        New-vRealizeLoadBalancerSpec                       2.11.1.10… PowerValidatedSolutions
Function        New-vRLIAgentGroup                                 2.11.1.10… PowerValidatedSolutions
Function        New-vRLIAlert                                      2.11.1.10… PowerValidatedSolutions
Function        New-vRLIDeployment                                 2.11.1.10… PowerValidatedSolutions
Function        New-vROParameterDefinition                         2.11.1.10… PowerValidatedSolutions
Function        New-vROPSDeployment                                2.11.1.10… PowerValidatedSolutions
Function        New-vROPSNotification                              2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMAdapterOperation                         2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMDatacenter                               2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMDatacenterVcenter                        2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMDeployment                               2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMLoadbalancer                             2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMLockerLicense                            2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMLockerPassword                           2.11.1.10… PowerValidatedSolutions
Function        New-vRSLCMMyVmwareAccount                          2.11.1.10… PowerValidatedSolutions
Function        New-WSADeployment                                  2.11.1.10… PowerValidatedSolutions
Function        Register-vRLIWorkloadDomain                        2.11.1.10… PowerValidatedSolutions
Function        Register-vROPSManagementPack                       2.11.1.10… PowerValidatedSolutions
Function        Register-vROPSWorkloadDomain                       2.11.1.10… PowerValidatedSolutions
Function        Register-vRSLCMProductBinary                       2.11.1.10… PowerValidatedSolutions
Function        Remove-AriaNetworksDataSource                      2.11.1.10… PowerValidatedSolutions
Function        Remove-DrsVmToVmGroup                              2.11.1.10… PowerValidatedSolutions
Function        Remove-GlobalPermission                            2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtGroup                                   2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtLdap                                    2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtNodeProfileSyslogExporter               2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtPrefixList                              2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtPrincipalIdentity                       2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtRole                                    2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtRouteMap                                2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtSecurityPolicy                          2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtSegment                                 2.11.1.10… PowerValidatedSolutions
Function        Remove-NsxtSyslogExporter                          2.11.1.10… PowerValidatedSolutions
Function        Remove-OperationsDefaultAdapter                    2.11.1.10… PowerValidatedSolutions
Function        Remove-PhotonAppliance                             2.11.1.10… PowerValidatedSolutions
Function        Remove-SrmConfiguration                            2.11.1.10… PowerValidatedSolutions
Function        Remove-SrmProtectionGroup                          2.11.1.10… PowerValidatedSolutions
Function        Remove-SrmRecoveryPlan                             2.11.1.10… PowerValidatedSolutions
Function        Remove-TanzuKubernetesCluster                      2.11.1.10… PowerValidatedSolutions
Function        Remove-VcLicense                                   2.11.1.10… PowerValidatedSolutions
Function        Remove-vRACloudAccount                             2.11.1.10… PowerValidatedSolutions
Function        Remove-vRACloudZone                                2.11.1.10… PowerValidatedSolutions
Function        Remove-vRAGroupOrgRole                             2.11.1.10… PowerValidatedSolutions
Function        Remove-vRAGroupRoles                               2.11.1.10… PowerValidatedSolutions
Function        Remove-vRAGroupServiceRole                         2.11.1.10… PowerValidatedSolutions
Function        Remove-vRAIntegrationItem                          2.11.1.10… PowerValidatedSolutions
Function        Remove-vRANotification                             2.11.1.10… PowerValidatedSolutions
Function        Remove-vRAUserOrgRole                              2.11.1.10… PowerValidatedSolutions
Function        Remove-vRAUserServiceRole                          2.11.1.10… PowerValidatedSolutions
Function        Remove-vRAvRLIConfig                               2.11.1.10… PowerValidatedSolutions
Function        Remove-vRLIAgentGroup                              2.11.1.10… PowerValidatedSolutions
Function        Remove-vRLIAlert                                   2.11.1.10… PowerValidatedSolutions
Function        Remove-vRLIAuthenticationAD                        2.11.1.10… PowerValidatedSolutions
Function        Remove-vRLIAuthenticationWSA                       2.11.1.10… PowerValidatedSolutions
Function        Remove-vRLIContentPack                             2.11.1.10… PowerValidatedSolutions
Function        Remove-vRLIGroup                                   2.11.1.10… PowerValidatedSolutions
Function        Remove-vRLILogForwarder                            2.11.1.10… PowerValidatedSolutions
Function        Remove-VrmsConfiguration                           2.11.1.10… PowerValidatedSolutions
Function        Remove-VrmsReplication                             2.11.1.10… PowerValidatedSolutions
Function        Remove-vROPSAdapter                                2.11.1.10… PowerValidatedSolutions
Function        Remove-vROPSAlertPlugin                            2.11.1.10… PowerValidatedSolutions
Function        Remove-vROPSCollectorGroup                         2.11.1.10… PowerValidatedSolutions
Function        Remove-vROPSCredential                             2.11.1.10… PowerValidatedSolutions
Function        Remove-vROPSNotification                           2.11.1.10… PowerValidatedSolutions
Function        Remove-vROPSUserAccount                            2.11.1.10… PowerValidatedSolutions
Function        Remove-vROPSUserGroup                              2.11.1.10… PowerValidatedSolutions
Function        Remove-vROvCenterServer                            2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMDatacenter                            2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMEnvironment                           2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMLoadbalancer                          2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMLockerCertificate                     2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMLockerLicense                         2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMLockerPassword                        2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMMyVmwareAccount                       2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMProductNtpServer                      2.11.1.10… PowerValidatedSolutions
Function        Remove-vRSLCMRequest                               2.11.1.10… PowerValidatedSolutions
Function        Remove-WMRegistry                                  2.11.1.10… PowerValidatedSolutions
Function        Request-AriaNetworksInternalApiToken               2.11.1.10… PowerValidatedSolutions
Function        Request-AriaNetworksToken                          2.11.1.10… PowerValidatedSolutions
Function        Request-IamMscaSignedCertificate                   2.11.1.10… PowerValidatedSolutions
Function        Request-IlaMscaSignedCertificate                   2.11.1.10… PowerValidatedSolutions
Function        Request-InvMscaSignedCertificate                   2.11.1.10… PowerValidatedSolutions
Function        Request-IomMscaSignedCertificate                   2.11.1.10… PowerValidatedSolutions
Function        Request-NsxtToken                                  2.11.1.10… PowerValidatedSolutions
Function        Request-PcaMscaSignedCertificate                   2.11.1.10… PowerValidatedSolutions
Function        Request-SignedCertificate                          2.11.1.10… PowerValidatedSolutions
Function        Request-SrmToken                                   2.11.1.10… PowerValidatedSolutions
Function        Request-SrmTokenREST                               2.11.1.10… PowerValidatedSolutions
Function        Request-VamiPKCS12Certificate                      2.11.1.10… PowerValidatedSolutions
Function        Request-VcenterApiToken                            2.11.1.10… PowerValidatedSolutions
Function        Request-vRAToken                                   2.11.1.10… PowerValidatedSolutions
Function        Request-vRLIToken                                  2.11.1.10… PowerValidatedSolutions
Function        Request-VrmsTokenREST                              2.11.1.10… PowerValidatedSolutions
Function        Request-vROpsLogForwardingConfig                   2.11.1.10… PowerValidatedSolutions
Function        Request-vROPSToken                                 2.11.1.10… PowerValidatedSolutions
Function        Request-vRSLCMBundle                               2.11.1.10… PowerValidatedSolutions
Function        Request-vRSLCMProductBinary                        2.11.1.10… PowerValidatedSolutions
Function        Request-vRSLCMToken                                2.11.1.10… PowerValidatedSolutions
Function        Request-vSphereApiToken                            2.11.1.10… PowerValidatedSolutions
Function        Request-WMClusterCSR                               2.11.1.10… PowerValidatedSolutions
Function        Request-WSAMscaSignedCertificate                   2.11.1.10… PowerValidatedSolutions
Function        Request-WSAToken                                   2.11.1.10… PowerValidatedSolutions
Function        Restore-VMOvfProperties                            2.11.1.10… PowerValidatedSolutions
Function        Resume-vRSLCMRequest                               2.11.1.10… PowerValidatedSolutions
Function        Search-vROPSUserAccount                            2.11.1.10… PowerValidatedSolutions
Function        Search-vROPSUserGroup                              2.11.1.10… PowerValidatedSolutions
Function        Set-DatastoreTag                                   2.11.1.10… PowerValidatedSolutions
Function        Set-ESXiAdminGroup                                 2.11.1.10… PowerValidatedSolutions
Function        Set-LocalAccountLockout                            2.11.1.10… PowerValidatedSolutions
Function        Set-LocalPasswordComplexity                        2.11.1.10… PowerValidatedSolutions
Function        Set-LocalUserPasswordExpiration                    2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtApplianceUserExpirationPolicy              2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtApplianceUserPassword                      2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtCertificate                                2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtComputeManager                             2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtNodeProfileSyslogExporter                  2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtRole                                       2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtRoutingConfigRedistributionRule            2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtSyslogExporter                             2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtTier1                                      2.11.1.10… PowerValidatedSolutions
Function        Set-NsxtVidm                                       2.11.1.10… PowerValidatedSolutions
Function        Set-RecoveryPlan                                   2.11.1.10… PowerValidatedSolutions
Function        Set-SrmApplianceState                              2.11.1.10… PowerValidatedSolutions
Function        Set-SrmConfiguration                               2.11.1.10… PowerValidatedSolutions
Function        Set-SrmNetworkDns                                  2.11.1.10… PowerValidatedSolutions
Function        Set-SrmNetworkInterface                            2.11.1.10… PowerValidatedSolutions
Function        Set-SrmRecoveryPlanVMPriority                      2.11.1.10… PowerValidatedSolutions
Function        Set-SrmService                                     2.11.1.10… PowerValidatedSolutions
Function        Set-SrmVamiCertificate                             2.11.1.10… PowerValidatedSolutions
Function        Set-VCenterCEIP                                    2.11.1.10… PowerValidatedSolutions
Function        Set-VcenterPasswordExpiration                      2.11.1.10… PowerValidatedSolutions
Function        Set-vCenterPermission                              2.11.1.10… PowerValidatedSolutions
Function        Set-VcenterRootPasswordExpiration                  2.11.1.10… PowerValidatedSolutions
Function        Set-VMOvfEnvTransport                              2.11.1.10… PowerValidatedSolutions
Function        Set-VMOvfEULA                                      2.11.1.10… PowerValidatedSolutions
Function        Set-VMOvfIPAssignment                              2.11.1.10… PowerValidatedSolutions
Function        Set-VMOvfProperty                                  2.11.1.10… PowerValidatedSolutions
Function        Set-vRADnsConfig                                   2.11.1.10… PowerValidatedSolutions
Function        Set-vRAGroupOrgRole                                2.11.1.10… PowerValidatedSolutions
Function        Set-vRAGroupServiceRole                            2.11.1.10… PowerValidatedSolutions
Function        Set-vRANtpConfig                                   2.11.1.10… PowerValidatedSolutions
Function        Set-vRAOrganizationDisplayName                     2.11.1.10… PowerValidatedSolutions
Function        Set-vRAUserOrgRole                                 2.11.1.10… PowerValidatedSolutions
Function        Set-vRAUserServiceRole                             2.11.1.10… PowerValidatedSolutions
Function        Set-vRAvRLIConfig                                  2.11.1.10… PowerValidatedSolutions
Function        Set-vRLIAlert                                      2.11.1.10… PowerValidatedSolutions
Function        Set-vRLIAuthenticationAD                           2.11.1.10… PowerValidatedSolutions
Function        Set-vRLIAuthenticationWSA                          2.11.1.10… PowerValidatedSolutions
Function        Set-vRLIEmailNotification                          2.11.1.10… PowerValidatedSolutions
Function        Set-vRLILogArchive                                 2.11.1.10… PowerValidatedSolutions
Function        Set-vRLILogForwarder                               2.11.1.10… PowerValidatedSolutions
Function        Set-vRLIRetentionThreshold                         2.11.1.10… PowerValidatedSolutions
Function        Set-vRLISmtpConfiguration                          2.11.1.10… PowerValidatedSolutions
Function        Set-vRLISyslogEdgeCluster                          2.11.1.10… PowerValidatedSolutions
Function        Set-VrmsConfiguration                              2.11.1.10… PowerValidatedSolutions
Function        Set-VrmsNetworkInterface                           2.11.1.10… PowerValidatedSolutions
Function        Set-VrmsReplication                                2.11.1.10… PowerValidatedSolutions
Function        Set-VrmsVamiCertificate                            2.11.1.10… PowerValidatedSolutions
Function        Set-vROPSAdapter                                   2.11.1.10… PowerValidatedSolutions
Function        Set-vROPSAlertPlugin                               2.11.1.10… PowerValidatedSolutions
Function        Set-vROPSAlertPluginStatus                         2.11.1.10… PowerValidatedSolutions
Function        Set-vROPSCurrency                                  2.11.1.10… PowerValidatedSolutions
Function        Set-vROPSDnsConfig                                 2.11.1.10… PowerValidatedSolutions
Function        Set-vROPSManagementPack                            2.11.1.10… PowerValidatedSolutions
Function        Set-vRSLCMApplianceNtpConfig                       2.11.1.10… PowerValidatedSolutions
Function        Set-vRSLCMDnsConfig                                2.11.1.10… PowerValidatedSolutions
Function        Set-vRSLCMSshStatus                                2.11.1.10… PowerValidatedSolutions
Function        Set-WorkspaceOneApplianceNtpConfig                 2.11.1.10… PowerValidatedSolutions
Function        Set-WorkspaceOneDnsConfig                          2.11.1.10… PowerValidatedSolutions
Function        Set-WorkspaceOneNsxtIntegration                    2.11.1.10… PowerValidatedSolutions
Function        Set-WorkspaceOneNtpConfig                          2.11.1.10… PowerValidatedSolutions
Function        Set-WorkspaceOneSmtpConfig                         2.11.1.10… PowerValidatedSolutions
Function        Set-WsaAccountLockout                              2.11.1.10… PowerValidatedSolutions
Function        Set-WSABindPassword                                2.11.1.10… PowerValidatedSolutions
Function        Set-WSADirectoryGroup                              2.11.1.10… PowerValidatedSolutions
Function        Set-WSADirectoryUser                               2.11.1.10… PowerValidatedSolutions
Function        Set-WsaPasswordPolicy                              2.11.1.10… PowerValidatedSolutions
Function        Set-WSARoleMember                                  2.11.1.10… PowerValidatedSolutions
Function        Set-WSASmtpConfiguration                           2.11.1.10… PowerValidatedSolutions
Function        Set-WSASyncSetting                                 2.11.1.10… PowerValidatedSolutions
Function        Show-PowerValidatedSolutionsOutput                 2.11.1.10… PowerValidatedSolutions
Function        Start-ValidatedSolutionMenu                        2.11.1.10… PowerValidatedSolutions
Function        Start-vROPSAdapter                                 2.11.1.10… PowerValidatedSolutions
Function        Start-vRSLCMProductNode                            2.11.1.10… PowerValidatedSolutions
Function        Start-vRSLCMSnapshot                               2.11.1.10… PowerValidatedSolutions
Function        Start-vRSLCMUpgrade                                2.11.1.10… PowerValidatedSolutions
Function        Start-WSADirectorySync                             2.11.1.10… PowerValidatedSolutions
Function        Stop-vROPSAdapter                                  2.11.1.10… PowerValidatedSolutions
Function        Stop-vRSLCMProductNode                             2.11.1.10… PowerValidatedSolutions
Function        Sync-vRSLCMDatacenterVcenter                       2.11.1.10… PowerValidatedSolutions
Function        Test-ADAuthentication                              2.11.1.10… PowerValidatedSolutions
Function        Test-AriaNetworksAuthentication                    2.11.1.10… PowerValidatedSolutions
Function        Test-AriaNetworksConnection                        2.11.1.10… PowerValidatedSolutions
Function        Test-AriaNetworksInternalAuthentication            2.11.1.10… PowerValidatedSolutions
Function        Test-CbrPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-CbwPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-CcmPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-DnsServers                                    2.11.1.10… PowerValidatedSolutions
Function        Test-DriPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-EndpointConnection                            2.11.1.10… PowerValidatedSolutions
Function        Test-EsxiAuthentication                            2.11.1.10… PowerValidatedSolutions
Function        Test-EsxiConnection                                2.11.1.10… PowerValidatedSolutions
Function        Test-GlobalWsaPrerequisite                         2.11.1.10… PowerValidatedSolutions
Function        Test-HrmPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-IamPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-IlaPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-InvPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-IomPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-IpAddress                                     2.11.1.10… PowerValidatedSolutions
Function        Test-IPaddressArray                                2.11.1.10… PowerValidatedSolutions
Function        Test-NSXTAuthentication                            2.11.1.10… PowerValidatedSolutions
Function        Test-NSXTConnection                                2.11.1.10… PowerValidatedSolutions
Function        Test-NtpServer                                     2.11.1.10… PowerValidatedSolutions
Function        Test-PcaPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-PdrPrerequisite                               2.11.1.10… PowerValidatedSolutions
Function        Test-PowerValidatedSolutionsPrereq                 2.11.1.10… PowerValidatedSolutions
Function        Test-PrereqDnsEntries                              2.11.1.10… PowerValidatedSolutions
Function        Test-SrmAuthentication                             2.11.1.10… PowerValidatedSolutions
Function        Test-SrmAuthenticationREST                         2.11.1.10… PowerValidatedSolutions
Function        Test-SrmConnection                                 2.11.1.10… PowerValidatedSolutions
Function        Test-SrmRegistration                               2.11.1.10… PowerValidatedSolutions
Function        Test-SrmSdkAuthentication                          2.11.1.10… PowerValidatedSolutions
Function        Test-SrmVamiAuthentication                         2.11.1.10… PowerValidatedSolutions
Function        Test-SrmVamiConnection                             2.11.1.10… PowerValidatedSolutions
Function        Test-SSOAuthentication                             2.11.1.10… PowerValidatedSolutions
Function        Test-SSOConnection                                 2.11.1.10… PowerValidatedSolutions
Function        Test-VCFAuthentication                             2.11.1.10… PowerValidatedSolutions
Function        Test-VCFConnection                                 2.11.1.10… PowerValidatedSolutions
Function        Test-vRAAuthentication                             2.11.1.10… PowerValidatedSolutions
Function        Test-vRAConnection                                 2.11.1.10… PowerValidatedSolutions
Function        Test-vRAIntegrationItem                            2.11.1.10… PowerValidatedSolutions
Function        Test-VrConnection                                  2.11.1.10… PowerValidatedSolutions
Function        Test-vRLIAuthentication                            2.11.1.10… PowerValidatedSolutions
Function        Test-vRLIConnection                                2.11.1.10… PowerValidatedSolutions
Function        Test-vRLILogForwarder                              2.11.1.10… PowerValidatedSolutions
Function        Test-VrmsAuthenticationREST                        2.11.1.10… PowerValidatedSolutions
Function        Test-VrmsVamiAuthentication                        2.11.1.10… PowerValidatedSolutions
Function        Test-VrmsVamiConnection                            2.11.1.10… PowerValidatedSolutions
Function        Test-vROPSAdapterConnection                        2.11.1.10… PowerValidatedSolutions
Function        Test-vROPsAdapterStatus                            2.11.1.10… PowerValidatedSolutions
Function        Test-vROPsAdapterStatusByType                      2.11.1.10… PowerValidatedSolutions
Function        Test-vROPSAuthentication                           2.11.1.10… PowerValidatedSolutions
Function        Test-vROPSConnection                               2.11.1.10… PowerValidatedSolutions
Function        Test-VrRegistration                                2.11.1.10… PowerValidatedSolutions
Function        Test-VrSdkAuthentication                           2.11.1.10… PowerValidatedSolutions
Function        Test-vRSLCMAuthentication                          2.11.1.10… PowerValidatedSolutions
Function        Test-vRSLCMConnection                              2.11.1.10… PowerValidatedSolutions
Function        Test-VrslcmPrerequisite                            2.11.1.10… PowerValidatedSolutions
Function        Test-vSphereApiAuthentication                      2.11.1.10… PowerValidatedSolutions
Function        Test-vSphereApiConnection                          2.11.1.10… PowerValidatedSolutions
Function        Test-VsphereAuthentication                         2.11.1.10… PowerValidatedSolutions
Function        Test-VsphereConnection                             2.11.1.10… PowerValidatedSolutions
Function        Test-WMSubnetInput                                 2.11.1.10… PowerValidatedSolutions
Function        Test-WSAAuthentication                             2.11.1.10… PowerValidatedSolutions
Function        Test-WSAConnection                                 2.11.1.10… PowerValidatedSolutions
Function        Undo-AntiAffinityRule                              2.11.1.10… PowerValidatedSolutions
Function        Undo-AriaNetworksDeployment                        2.11.1.10… PowerValidatedSolutions
Function        Undo-AriaNetworksLdapConfiguration                 2.11.1.10… PowerValidatedSolutions
Function        Undo-AriaNetworksNsxDataSource                     2.11.1.10… PowerValidatedSolutions
Function        Undo-AriaNetworksVcenterDataSource                 2.11.1.10… PowerValidatedSolutions
Function        Undo-ClusterGroup                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-ContentLibrary                                2.11.1.10… PowerValidatedSolutions
Function        Undo-DatastoreTag                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-DRSolutionTovCenter                           2.11.1.10… PowerValidatedSolutions
Function        Undo-EsxiVrmsStaticRoute                           2.11.1.10… PowerValidatedSolutions
Function        Undo-EsxiVrmsVMkernelPort                          2.11.1.10… PowerValidatedSolutions
Function        Undo-IdentitySource                                2.11.1.10… PowerValidatedSolutions
Function        Undo-Namespace                                     2.11.1.10… PowerValidatedSolutions
Function        Undo-NamespacePermission                           2.11.1.10… PowerValidatedSolutions
Function        Undo-NetworkSegment                                2.11.1.10… PowerValidatedSolutions
Function        Undo-NsxtIdentitySource                            2.11.1.10… PowerValidatedSolutions
Function        Undo-NsxtLdapRole                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-NsxtNodeProfileSyslogExporter                 2.11.1.10… PowerValidatedSolutions
Function        Undo-NsxtPrincipalIdentity                         2.11.1.10… PowerValidatedSolutions
Function        Undo-NsxtVidmRole                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-PrefixList                                    2.11.1.10… PowerValidatedSolutions
Function        Undo-ProtectionGroup                               2.11.1.10… PowerValidatedSolutions
Function        Undo-RecoveryPlan                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-Registry                                      2.11.1.10… PowerValidatedSolutions
Function        Undo-ResourcePool                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-RouteMap                                      2.11.1.10… PowerValidatedSolutions
Function        Undo-SddcManagerRole                               2.11.1.10… PowerValidatedSolutions
Function        Undo-SiteRecoveryManager                           2.11.1.10… PowerValidatedSolutions
Function        Undo-SrmLicenseKey                                 2.11.1.10… PowerValidatedSolutions
Function        Undo-SrmMapping                                    2.11.1.10… PowerValidatedSolutions
Function        Undo-SrmSitePair                                   2.11.1.10… PowerValidatedSolutions
Function        Undo-SsoPermission                                 2.11.1.10… PowerValidatedSolutions
Function        Undo-SsoUser                                       2.11.1.10… PowerValidatedSolutions
Function        Undo-StoragePolicy                                 2.11.1.10… PowerValidatedSolutions
Function        Undo-SupervisorCluster                             2.11.1.10… PowerValidatedSolutions
Function        Undo-SupervisorService                             2.11.1.10… PowerValidatedSolutions
Function        Undo-TanzuKubernetesCluster                        2.11.1.10… PowerValidatedSolutions
Function        Undo-vCenterGlobalPermission                       2.11.1.10… PowerValidatedSolutions
Function        Undo-VdsPortGroup                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-VMFolder                                      2.11.1.10… PowerValidatedSolutions
Function        Undo-VmStartupRule                                 2.11.1.10… PowerValidatedSolutions
Function        Undo-vRACloudAccount                               2.11.1.10… PowerValidatedSolutions
Function        Undo-vRADeployment                                 2.11.1.10… PowerValidatedSolutions
Function        Undo-vRADnsConfig                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-vRAGroup                                      2.11.1.10… PowerValidatedSolutions
Function        Undo-vRANtpConfig                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-vRAUser                                       2.11.1.10… PowerValidatedSolutions
Function        Undo-vRAvROPsIntegrationItem                       2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLIAgentGroup                                2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLIAlert                                     2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLIAuthenticationAD                          2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLIAuthenticationGroup                       2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLIAuthenticationWSA                         2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLIDeployment                                2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLILogForwarder                              2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLIPhotonAgent                               2.11.1.10… PowerValidatedSolutions
Function        Undo-vRLISyslogEdgeCluster                         2.11.1.10… PowerValidatedSolutions
Function        Undo-vROPSAdapter                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-vROPSCredential                               2.11.1.10… PowerValidatedSolutions
Function        Undo-vROPSDeployment                               2.11.1.10… PowerValidatedSolutions
Function        Undo-vROPSDnsConfig                                2.11.1.10… PowerValidatedSolutions
Function        Undo-vROPSNtpServer                                2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMDatacenter                              2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMDeployment                              2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMDnsConfig                               2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMGroupRole                               2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMLoadBalancer                            2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMLockerCertificate                       2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMLockerLicense                           2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMLockerPassword                          2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMMyVMwareAccount                         2.11.1.10… PowerValidatedSolutions
Function        Undo-vRSLCMNtpServer                               2.11.1.10… PowerValidatedSolutions
Function        Undo-vSphereReplication                            2.11.1.10… PowerValidatedSolutions
Function        Undo-vSphereReplicationManager                     2.11.1.10… PowerValidatedSolutions
Function        Undo-vSphereRole                                   2.11.1.10… PowerValidatedSolutions
Function        Undo-WorkspaceOne                                  2.11.1.10… PowerValidatedSolutions
Function        Undo-WorkspaceOneDirectoryGroup                    2.11.1.10… PowerValidatedSolutions
Function        Undo-WorkspaceOneDnsConfig                         2.11.1.10… PowerValidatedSolutions
Function        Undo-WorkspaceOneNsxtIntegration                   2.11.1.10… PowerValidatedSolutions
Function        Undo-WSADeployment                                 2.11.1.10… PowerValidatedSolutions
Function        Uninstall-vRLIContentPack                          2.11.1.10… PowerValidatedSolutions
Function        Update-AriaNetworksNsxtDataSourceCredentials       2.11.1.10… PowerValidatedSolutions
Function        Update-AriaNetworksvCenterDataSourceCredentials    2.11.1.10… PowerValidatedSolutions
Function        Update-SddcDeployedFlavor                          2.11.1.10… PowerValidatedSolutions
Function        Update-vRACloudAccountZone                         2.11.1.10… PowerValidatedSolutions
Function        Update-vRACloudZone                                2.11.1.10… PowerValidatedSolutions
Function        Update-vRAOrganizationDisplayName                  2.11.1.10… PowerValidatedSolutions
Function        Update-vRLIAlert                                   2.11.1.10… PowerValidatedSolutions
Function        Update-vRLIContentPack                             2.11.1.10… PowerValidatedSolutions
Function        Update-vRLILogForwarder                            2.11.1.10… PowerValidatedSolutions
Function        Update-vROPSAdapterCollecterGroup                  2.11.1.10… PowerValidatedSolutions
Function        Update-vROPSAdapterSddcHealth                      2.11.1.10… PowerValidatedSolutions
Function        Update-vROPSAdapterVcenter                         2.11.1.10… PowerValidatedSolutions
Function        Update-vROPSUserAccount                            2.11.1.10… PowerValidatedSolutions
Function        Update-vROPSvRAAdapterCredential                   2.11.1.10… PowerValidatedSolutions
Function        Update-vRSLCMPSPack                                2.11.1.10… PowerValidatedSolutions
Function        Watch-vRSLCMRequest                                2.11.1.10… PowerValidatedSolutions
Function        Watch-WmClusterConfigStatus                        2.11.1.10… PowerValidatedSolutions

PS C:\Users\JUNIOR_MU>
```

 



## 五、连接和管理 VCF 环境



使用 PowerVCF 命令 **Request\-VCFToken** 并搭配 vCenter Single Sign\-On 管理员帐户（administrator@vsphere.local）或者本地管理员帐户（admin@local）连接到 SDDC Manager，如果未添加用户名和密码选项，则会显示凭据窗口。



```
Request-VCFToken -fqdn vcf-mgmt01-sddc01.mulab.local
Request-VCFToken -fqdn vcf-mgmt01-sddc01.mulab.local -username administrator@vsphere.local -password Vcf520@password
```

[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005145027179-524208569.png)](https://github.com)


获取 VCF 工作负载域信息。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005145551000-131051961.png)](https://github.com)


获取 VCF 集群信息。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005145644520-2070514315.png)](https://github.com)


获取 VCF 主机信息。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005145940296-1042552318.png)](https://github.com)


获取 vCenter Server 信息。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005150109684-672290921.png)](https://github.com)


获取 NSX Manager 集群信息。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005150213385-6367212.png)](https://github.com)


获取 SDDC Manager 信息。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005151106030-643534241.png)](https://github.com)


获取 SDDC Manager 服务状态。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005151242767-875507935.png)](https://github.com)


获取 DNS、NTP、备份、网络池信息。


[![](https://img2024.cnblogs.com/blog/2313726/202410/2313726-20241005150610772-20211086.png)](https://github.com)


