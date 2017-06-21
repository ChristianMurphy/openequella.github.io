# Testing Guide for Equella 6.5-GA

*_In Progress_*

Please refer to the 6.5-GA release notes for more details.

### #104 Scripting pack
* Now in the master docs repo.
* self-creation of the javadoc in issues #105 and #106 (still pending)

### #98 / #102 Office Integration

* Equella has the ability integrate with Microsoft Office products to enable a smoother editing experience.  Due to licensing issues, and the inherit issues of using an older interop DLL on more modern installs of Office, the download of the Office Integration msi package has been removed from the Equella Web UI, and the DLLs in question have been removed from the git repo.  The functionality is still useful, so documentation has been provided for users to access this functionality.

### #101 / Documentation / example files moved to the master docs repo

### #100 HTML Editor Plugins

* Working pending...

### #96 (Make installer set executable bit for files that need it)

* After a Linux install, the sys admin no longer needs to chmod the jsvc, manager, and equellaserver scripts
* Functional testing:  N/A
* Performance testing: N/A
* Regression testing:  install Equella, and then ensure you can start / stop the equellaserver and manager with your typical 'Equella' user.

### #95 (Scrub equella-deps)
* Several dependencies had to be changed / removed
  * jnlp (admin console launcher) was included in a different way
  * reporting - an older 6.2 reporting-common dependency was removed.  
* Functional testing:  
  * Ensure the admin console launches and basic CRUD ops on entities (ie collections) work
  * Functional testing:  Confirm the reporting suite of tests to ensure all Data Source and Data Source types can be run, both from the BIRT Report Designer and directly from Equella.
* Performance testing: N/A
  * Confirm no lag in running reports 
* Regression testing: N/A

### #84 Re-implement file upload features
* In #80 the code was fixed to not require the special commons upload jar, which is working but it now lacks a couple of features which we need to be re-implemented, most likely in javascript.  Changes to functionality include:
  * File upload progress bars
  * Fast failure for invalid uploads (invalid mime type)
* Also the multipart parsing is now handled by tomcat which has some maximum sizes which need configuring somewhere. <<<< Need to review.

### #83 (Upgrade commons-beanutils)
Need to review for testing impact.  No UI / functionality changes

### #56 (Remove dhfjava dependency)
The functionality was changed to not rely on dhfjava.  Instead it uses a Tika open source library.  The results are not as clean as dhfjava, so enhancements are welcome!
* Functional testing
  * Confirm the conversion service can be disabled
  * Confirm the conversion service can be enabled, and is usable
* Regression testing - N/A
* Performance testing
  * Try converting a 10 MB file on 6.4-QA3, and the same file on 6.5-GA.  

### #72 (Allow configuration of the historically hardcoded donotreply@equella.com email address)
System admins now configure the 'do not reply' email address via the Server admin pages
* Functionality testing:
  * Confirm the email address can be changed, and when an email is sent from Equella, the 'do not reply' sender field is set to the custom / configured email address.
* Regression testing - N/A
* Performance testing - N/A

### #74 (Remove the UpgradeProxyWeb python server)
The functionality point the Equella Manager at a server that provides the latest Equella update has been removed
* Functional testing
  * Ensure the Equella Manager has no mention of an 'upgrade server'
* Regression testing
  * Ensure the Equella Manager can still perform updates from a local upgrade binary.
* Performance testing - N/A

### #73 / #60 General build changes / scrubbing of commercial terminology
Hard-coded words denoting the last commercial owner and last commercial website of Equella were removed.  Generally, resources (lang bundles and images) functionally haven't changed, but how they are bundled into Equella did change.  
* Functional testing
  * Ensure 'Pearson', and 'equella.com' don't show up, and instead have been replaced by meaningful alternatives for the open source community.
* Regression testing
  * Ideally, review all places strings and images are presented to the user, and ensure no missing language strings (???my.lang.string???) or errors finding images occur.
* Performance testing - N/A

### #71 ( Allow configuration of LTI external tool contact for Equella )
When an LTI attachment that added to an item, at times it's considered 'default', and historically, a default consumer contact email of support@equella.com was used.  Now, you can configure this behavior by adding external.tool.contact.email into optional-config.properties.
* Functional testing
  * Ensure absent, blank, malformed email, and good email configured values work as expected in this userflow.
* Regression testing
  * Ensure previously configured 'default' LTI attachments have the expected consumer  contact email when being edited.
* Performance testing - N/A

### #67 ( Guice recipes library changes )
Looks like a Jsr250Module was added.  ??? What impact does this have?
* Functional testing - N/A
* Regression testing - N/A
* Performance testing - N/A

### #64 ( Remove image processing from conversion service )
??? What impact, if any, does this have?
* Functional testing - N/A
* Regression testing - N/A
* Performance testing - N/A

### #62 ( Upgrade flamingo )
Flamingo was upgraded to streamline dependencies from Maven.
* Functional testing
  * CRUD operations in the File Manager applet
  * ??? Specific browsers or other test cases?
* Regression testing - N/A
* Performance testing - N/A

### #61 ( SBT build loose ends )
Work on jar signing, in place editor applet, BIRT, dev config setup, and ant to sbt build process.
* Functional testing
  * CRUD operations in the Admin Console
  * BIRT testing is handled via #95.  No other changes in functionality.
  * Ensure a new commit bumps the rXYZ version number and can upgrade to that version.
  * ??? other test cases?
* Regression testing - N/A
* Performance testing - N/A

### #59 ( Replace use of hibernate-beanlib )
We use beanlib-hibernate for cloning a tree of interconnected hibernate objects, in particular it's used in two particular places:
* Attachments and Navigation tree nodes
* Workflow nodes

The library was patched by us but we don't have the source for our patches + the it doesn't play nicely with the new flat classpath because it uses an incompatible version of cglib.
* Functional testing
  * CRUD operations in the navigation tree nodes
  * CRUD operations in the workflow editor
  * ??? other test cases for Navigation trees, workflow, others?
* Regression testing - N/A
* Performance testing - N/A

### #55 ( Research need for Kaltura and replace/remove )
The Kaltura Java client API is not Apache-license-friendly.  It was moved into it's own repo, but can easily be integrated back into Equella per client.  Additional effort is noted to turn this into truly a 'drop in' plugin.
* Functional testing
  * Follow the tests here for Equella with and without Kaltura:  https://equella.github.io/tests/migration/64QA3-to-OS/TestEnablingKaltura.html
* Regression testing
  * Start with Equella 6.4-QA3 (which has Kaltura enabled), upgrade to Equella without Kaltura, and then upgrade to Equella with Kaltura, following the guide here:  https://equella.github.io/tests/migration/64QA3-to-OS/TestEnablingKaltura.html
* Performance testing - N/A
