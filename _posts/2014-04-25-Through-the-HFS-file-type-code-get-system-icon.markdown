---
layout: post
title: "通过HFS文件类型码获取图标"
date: 2014-04-25 10:51:00 
comments: true
categories: [cocoa]
tags: [cocoa]
description: 有时发现系统的图标不错，但不知怎么弄出来，后面发现可以通过HFS文件类型码获取Mac OSX系统图标。
keywords: cocoa HFS icon

---

有时发现系统的图标不错，但不知怎么弄出来，后面发现可以通过HFS文件类型码获取Mac OSX系统图标，不过应该还有其他的方法。

{% highlight objective-c %} 

/* Get the icon for a given file type.  The file type may be a filename extension, or a HFS code encoded via NSFileTypeForHFSTypeCode, or a Universal Type Identifier (UTI).   Returns a default icon if the operation fails.  */
- (NSImage *)iconForFileType:(NSString *)fileType;

// Given an HFS file type code, return a string that represents the file type.  The string will have been autoreleased.  The format of the string is a private implementation detail, but such strings are suitable for inclusion in arrays that also contain file name extension strings.  Several Cocoa API methods take such arrays.
FOUNDATION_EXPORT NSString *NSFileTypeForHFSTypeCode(OSType hfsFileTypeCode);

/*
   Type of the predefined/generic icons. For example, the call:
      err = GetIconRef(kOnSystemDisk, kSystemIconsCreator, kHelpIcon, &iconRef);
   will retun in iconRef the IconRef for the standard help icon.
*/

/* Generic Finder icons */

enum {
  kClipboardIcon                = 'CLIP',
  kClippingUnknownTypeIcon      = 'clpu',
  kClippingPictureTypeIcon      = 'clpp',
  kClippingTextTypeIcon         = 'clpt',
  kClippingSoundTypeIcon        = 'clps',
  kDesktopIcon                  = 'desk',
  kFinderIcon                   = 'FNDR',
  kComputerIcon                 = 'root',
  kFontSuitcaseIcon             = 'FFIL',
  kFullTrashIcon                = 'ftrh',
  kGenericApplicationIcon       = 'APPL',
  kGenericCDROMIcon             = 'cddr',
  kGenericControlPanelIcon      = 'APPC',
  kGenericControlStripModuleIcon = 'sdev',
  kGenericComponentIcon         = 'thng',
  kGenericDeskAccessoryIcon     = 'APPD',
  kGenericDocumentIcon          = 'docu',
  kGenericEditionFileIcon       = 'edtf',
  kGenericExtensionIcon         = 'INIT',
  kGenericFileServerIcon        = 'srvr',
  kGenericFontIcon              = 'ffil',
  kGenericFontScalerIcon        = 'sclr',
  kGenericFloppyIcon            = 'flpy',
  kGenericHardDiskIcon          = 'hdsk',
  kGenericIDiskIcon             = 'idsk',
  kGenericRemovableMediaIcon    = 'rmov',
  kGenericMoverObjectIcon       = 'movr',
  kGenericPCCardIcon            = 'pcmc',
  kGenericPreferencesIcon       = 'pref',
  kGenericQueryDocumentIcon     = 'qery',
  kGenericRAMDiskIcon           = 'ramd',
  kGenericSharedLibaryIcon      = 'shlb',
  kGenericStationeryIcon        = 'sdoc',
  kGenericSuitcaseIcon          = 'suit',
  kGenericURLIcon               = 'gurl',
  kGenericWORMIcon              = 'worm',
  kInternationalResourcesIcon   = 'ifil',
  kKeyboardLayoutIcon           = 'kfil',
  kSoundFileIcon                = 'sfil',
  kSystemSuitcaseIcon           = 'zsys',
  kTrashIcon                    = 'trsh',
  kTrueTypeFontIcon             = 'tfil',
  kTrueTypeFlatFontIcon         = 'sfnt',
  kTrueTypeMultiFlatFontIcon    = 'ttcf',
  kUserIDiskIcon                = 'udsk',
  kUnknownFSObjectIcon          = 'unfs',
  kInternationResourcesIcon     = kInternationalResourcesIcon /* old name*/
};

{% endhighlight %} 

例如：获取应用程序的图标

	[[NSWorkspace sharedWorkspace] iconForFileType:NSFileTypeForHFSTypeCode(kGenericApplicationIcon)];
 
 


