# MitigationFlagsCliTool
Prints mitigation policy information for processes in a dump file.

## Usage
```
-d [dump file] - specify dump file
-l - query current machine (must run elevated)
-k [target machine information] - live kernel debugging (Example: -k net:port:50000,key:1.1.1.1,target:1.2.3.4)
-e - Only print enabled mitigations for process
-m [mitigation,mitigation,...] - Only print processes where any of the requested mitigations are enabled
-ma [mitigation,mitigation,...] - Only print processes where all of the requested mitigations are enabled
```

Usage example:
```
MitigationFlagsCliTool.exe -d c:\temp\live3.dmp -ma DisallowWin32kSystemCalls -e
```

Will show all processes that have the DisallowWin32kSystemCalls mitigation enabled:
```
Current process name: MsMpEngCP.exe, pid: 2352
        Mitigation Flags:
                ControlFlowGuardEnabled
                DisallowStrippedImages
                ForceRelocateImages
                HighEntropyASLREnabled
                ExtensionPointDisable
                DisallowWin32kSystemCalls
                AuditDisallowWin32kSystemCalls
                DisableNonSystemFonts
                PreferSystem32Images
                ProhibitRemoteImageMap
                ProhibitLowILImageMap
                SignatureMitigationOptIn
Current process name: vmwp.exe, pid: 4388
        Mitigation Flags:
                ControlFlowGuardEnabled
                ControlFlowGuardExportSuppressionEnabled
                ControlFlowGuardStrict
                DisallowStrippedImages
                ForceRelocateImages
                HighEntropyASLREnabled
                ExtensionPointDisable
                DisableDynamicCode
                AuditDisableDynamicCode
                DisallowWin32kSystemCalls
                AuditDisallowWin32kSystemCalls
                DisableNonSystemFonts
                PreferSystem32Images
                ProhibitRemoteImageMap
                ProhibitLowILImageMap
                SignatureMitigationOptIn
Current process name: vmmem, pid: 1948
        Mitigation Flags:
                HighEntropyASLREnabled
                DisallowWin32kSystemCalls
                AuditDisallowWin32kSystemCalls
                PreferSystem32Images
                ProhibitRemoteImageMap
                ProhibitLowILImageMap
```
  
## Mitigations documentation  
Most of the mitigations shown here are documented in some form. Information about a lot of them can be found here:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/exploit-protection-reference  
And information about how to enable and disable them is here:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/customize-exploit-protection  
A lot of those mitigations are also explained in detail in the Windows Internals book.  
  
Some sources give us more information about a specific mitigation class and the reasons it was implemeted:  
### Control Flow Guard (CFG)  
Mitigation FLags:
- ControlFlowGuardEnabled  
- ControlFlowGuardExportSuppressionEnabled  
- ControlFlowGuardStrict  

Sources:  
https://documents.trendmicro.com/assets/wp/exploring-control-flow-guard-in-windows10.pdf  
https://docs.microsoft.com/en-us/windows/win32/secbp/control-flow-guard  
https://docs.microsoft.com/en-us/windows/win32/secbp/pe-metadata#export-suppression  
  
### ASLR  
Mitigation FLags:  
- DisallowStrippedImages  
- ForceRelocateImages  
- HighEntropyASLREnabled  
- StackRandomizationDisabled  

Sources:  
https://insights.sei.cmu.edu/cert/2018/08/when-aslr-is-not-really-aslr---the-case-of-incorrect-assumptions-and-bad-defaults.html  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/exploit-protection-reference  
https://msrc-blog.microsoft.com/2013/12/11/software-defense-mitigating-common-exploitation-techniques/  
  
### Extension Points  
Mitigation Flags:  
- ExtensionPointDisable  

Sources:  
https://resources.infosecinstitute.com/common-malware-persistence-mechanisms/#gref  
  
### Dynamic Code Guard / Arbitrary Code Guard  
Mitigation Flags:  
- DisableDynamicCode  
- DisableDynamicCodeAllowOptOut  
- DisableDynamicCodeAllowRemoteDowngrade  
- AuditDisableDynamicCode  

Sources:  
https://blogs.windows.com/msedgedev/2017/02/23/mitigating-arbitrary-native-code-execution/  
https://www.crowdstrike.com/blog/state-of-exploit-development-part-2/  
  
### Win32k System Call Disable / Filter  
Mitigation Flags:  
- DisallowWin32kSystemCalls  
- AuditDisallowWin32kSystemCalls  
- EnableFilteredWin32kAPIs  
- AuditFilteredWin32kAPIs  

Sources:  
https://googleprojectzero.blogspot.com/2016/11/breaking-chain.html  
  
### Non System Fonts  
Mitigation Flags:  
- DisableNonSystemFonts  
- AuditNonSystemFontLoading  

Sources:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/block-untrusted-fonts-in-enterprise  
  
### Image Load Blocking 
Mitigation Flags:  
- PreferSystem32Images  
- ProhibitRemoteImageMap  
- AuditProhibitRemoteImageMap  
- ProhibitLowILImageMap  
- AuditProhibitLowILImageMap  

Sources:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/exploit-protection-reference#block-low-integrity-images
  
### Signatures  
Mitigation Flags:  
- SignatureMitigationOptIn  
- AuditBlockNonMicrosoftBinaries  
- AuditBlockNonMicrosoftBinariesAllowStore  
- LoaderIntegrityContinuityEnabled  
- AuditLoaderIntegrityContinuity  

Sources:  
https://www.sekoia.fr/blog/microsoft-edge-binary-injection-mitigation-overview/  
https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/exploit-protection-reference#validate-image-dependency-integrity (Here loader integrity continuity is named Image Dependency Integrity)  
  
### Module Tampering Protection  
Mitigation Flags:  
- EnableModuleTamperingProtection  
- EnableModuleTamperingProtectionNoInherit  

Sources:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-antivirus/prevent-changes-to-security-settings-with-tamper-protection  
  
### Side Channel
Mitigation Flags:  
- RestrictIndirectBranchPrediction  
- SpeculativeStoreBypassDisable  

Sources:  
https://software.intel.com/security-software-guidance/deep-dive/deep-dive-indirect-branch-restricted-speculation  
https://msrc-blog.microsoft.com/2018/05/21/analysis-and-mitigation-of-speculative-store-bypass-cve-2018-3639/  
  
### Security Domain Isolation  
Mitigation Flags:  
- IsolateSecurityDomain  

Sources:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/domain-isolation-policy-design  
  
### Export Address Filtering
Originally implemented by EMET and later added to Windows Defender.  
Mitigation Flags:  
- EnableExportAddressFilter  
- AuditExportAddressFilter  
- EnableExportAddressFilterPlus  
- AuditExportAddressFilterPlus  

Sources:  
https://adsecurity.org/?p=2579  
  
### Import Address Filtering  
Originally implemented by EMET and later added to Windows Defender.  
Mitigation Flags:  
- EnableImportAddressFilter  
- AuditImportAddressFilter  

Sources:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/exploit-protection-reference#import-address-filtering-iaf  
  
### ROP and Stack Pivot  
Originally implemented by EMET and later added to Windows Defender.  
Mitigation Flags:  
- EnableRopStackPivot  
- AuditRopStackPivot  
- EnableRopCallerCheck  
- AuditRopCallerCheck  
- EnableRopSimExec  
- AuditRopSimExec  

Sources:  
https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-atp/exploit-protection-reference#simulate-execution-simexec  
  
### Disable Page Combine  
Mitigation Flags:  
- DisablePageCombine  

Sources:  
https://vulners.com/nessus/SMB_NT_MS16-092.NASL  
https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-process_mitigation_side_channel_isolation_policy  
  
### CET  
Mitigation Flags:  
- CetUserShadowStacks  
- AuditCetUserShadowStacks  
- AuditCetUserShadowStacksLogged  
- UserCetSetContextIpValidation  
- AuditUserCetSetContextIpValidation  
- AuditUserCetSetContextIpValidationLogged  
- CetUserShadowStacksStrictMode  
- BlockNonCetBinaries  
- BlockNonCetBinariesNonEhcont  
- AuditBlockNonCetBinaries  
- AuditBlockNonCetBinariesLogged  
- PointerAuthUserIp  
- AuditPointerAuthUserIp  
- AuditPointerAuthUserIpLogged  
- CetDynamicApisOutOfProcOnly  
- UserCetSetContextIpValidationRelaxedMode  

Sources:  
https://windows-internals.com/cet-on-windows/ (Only documents some of those, as a lot were added very recently)  
  
### eXtended Control Flow Guard (XFG)  
Mitigation Flags:  
- XtendedControlFlowGuard  
- AuditXtendedControlFlowGuard  

Sources:  
https://connormcgarr.github.io/examining-xfg/  
https://www.crowdstrike.com/blog/state-of-exploit-development-part-2/  
