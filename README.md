#JSSAPIScripts

JSS Utility scripts featuring the JAMF API by Jeffrey Compton

Table of Contents
=================

  * [Table of Contents](#table-of-contents)
  * [jssMigrationUtility\.bash](#jssmigrationutilitybash)
  * [getSelfServicePolicyIcons\.bash](#getselfservicepolicyiconsbash)
  * [generateSaticGroupMembershipList\.bash](#generatesaticgroupmembershiplistbash)
  * [addMacToStaticGroupOnEnroll\.bash](#addmactostaticgrouponenrollbash)
  * [purgeAllPoliciesInCategory\.bash](#purgeallpoliciesincategorybash)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc.go)
  
[jssMigrationUtility.bash](https://github.com/igeekjsc/JSSAPIScripts/blob/master/jssMigrationUtility.bash)
==================

Version 1.1

The JSS Migration Utility uses the JAMF API to download resources from a source JSS and 
upload those resources to a destination JSS.  The utiltiy does NOT migrate computers.  

The primary goal and use-case for this utiltiy is to provide a mechanism where a JAMF 
admin can set up a barebones, clean JSS and import management resources (categories, 
scripts, extension attributes, computer groups, etc.) from another JSS instance.  This is 
perhaps most helpful when circumstances (e.g. a crufty database) prevent JSS migration via
the usual process - a database restore.

Basic Process:

1. Run this utility from your local Mac
2. XML files are downloaded from source JSS to local system 
3. XML files are parsed, depending on the current resource 
4. XML files are then uploaded to destination JSS

WARNINGS:

For this to work correctly, some data must be stripped from the downloaded XML before
uploading to new server.  This occurs during the parsing process.  Before each group of 
resource files are parsed, a warning will display explaining what data, if any, will be 
stripped.  For example, before the ldapservers resource files are parsed, you will see
this message -- 

*Passwords for authenticating to LDAP will NOT be included!
You must enter passwords for LDAP in web app*

Local File System:

By default, XML files are stored to ~/Desktop/JSS_Migration  Before work begins on each
resource, the utility looks for the presence of any previously downloaded resources and
archives to ~/Desktop/JSS_Migration/archives if necessary


[getSelfServicePolicyIcons.bash](https://github.com/igeekjsc/JSSAPIScripts/blob/master/getSelfServicePolicyicons.bash)
==================

Casper Admins aren't perfect.  Sometimes we forget to save all the icons we use for 
Self Service policies.  After all - they are in the JSS.  But if you have to stand up
a separate JSS or a new one from scratch - you may need to get those.

This script finds all Self Service policies in your JSS and downloads the icons for each.

But what if we have uploaded the same icon more than once? You then have multiple versions
of the same icon, each slightly different - different resolution, new style, etc.  

No worries - this script will ensure that all flavors of the same icon are downloaded.  
The file name will be appended with a "-" and the policy ID, ensuring all versions of 
same icon are downloaded.

But wait!  There's more!  At the end of the download process, you will be asked if you
want to rename each file with a resolution indicator.  That way you can more easily find
the right version of the icon you are looking for.

[generateSaticGroupMembershipList.bash](https://github.com/igeekjsc/JSSAPIScripts/blob/master/generateSaticGroupMembershipList.bash)
==================

This tool simply generates a long comma-separated value list of computer serial numbers 
and static group memberships.  (Run locally from your Mac)

You must manually enter which static groups you want to query.  See comments in script
for more details.

Each line in the output will look like --

**FAK3APLS3RL1,StaticGroup01**

**Important NOTE:** If your static group names have spaces or other special characters,
you must manually replace with appropriate URL encoding handlers.  (e.g. "%20" for space)
Your static groups can **NOT** have commas in them.

This is designed as a use-only-once script.  The idea is to run this against your OLD JSS
to generate a list you can paste into the **addComputerToStaticGroupOnEnroll.bash** script,
which will be run once per computer on enrollment.  See below for more info.

[addMacToStaticGroupOnEnroll.bash](https://github.com/igeekjsc/JSSAPIScripts/blob/master/addMacToStaticGroupOnEnroll.bash)
==================

This script is designed to be uploaded to your new JSS and run as a policy with
enrollment event.

As computers enroll in your new JSS, they will put themselves back into appropriate 
static groups.

For security reasons - API credentials are not stored in the script.  They are set as
parameters to be defined in your policy.

For a version of this script that you can download straight into your JSS, download 
addMacToStaticGroupOnEnroll.bash.xml instead.

[purgeAllPoliciesInCategory.bash](https://github.com/igeekjsc/JSSAPIScripts/blob/master/purgeAllPoliciesInCategory.bash)
==================

Casper Admins are sometimes known to be pack rats.  We disable policies but keep them 
in the JSS for ages "just in case."  Sometimes we even create a category like 
"zzzzz_Disabled_Policies" so that we can keep old, inactive policies at the bottom of
the policy list out of the way.

But there may come a time when you want to clean all those up and just delete them.  If 
you are like me though, finding the time to click on each one and clicking "Delete" is
just not feasible.  This script allows you to purge all policies from a single category.

