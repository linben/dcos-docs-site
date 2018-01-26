---
layout: layout.pug
0: ""
title: Configuring browsers to trust your DC/OS CA
menuWeight: 200
excerpt: How to configure Chrome and Firefox to trust your DC/OS CA.
enterprise: true
---
# About configuring browsers to trust your DC/OS CA

**Prerequisite:** A local copy of the [root certificate of your DC/OS CA](/1.10/security/ent/tls-ssl/get-cert/).

The procedure for adding your DC/OS CA as a trusted root certificate authority varies by operating system and browser. Refer to the section that corresponds to your browser/operating system pair.

- [Google Chrome on OS X](#osx-chrome)

- [Google Chrome on Windows 10](#win-chrome)

- [Mozilla Firefox on OS X or Windows](#osx-win-firefox)

# <a name="osx-chrome"></a>Configuring Google Chrome on OS X to trust your DC/OS CA

**Notes:**

- Procedure works best with Chrome 56.

- At any time in this procedure, you may be prompted for your password to allow modifications to your keychain. Provide your password at the prompt.

1. Click the magnifying glass icon in the top right of your desktop to open Spotlight Search. Type **Keychain Access** in the box.
    
    ![Launch Keychain Access](/1.10/img/osx-chrome-launch-keychain.png)

2. In the **Keychain Access** dialog, select **System**.

3. Add the `dcos-ca.crt` file to the **System** keychain using one of the following methods:
    
    - Dragging and dropping the file
    - **File** -> **Import Items** 

4. Double-click the certificate in the keychain, expand the **Trust** section, and select **Always Trust** in **When using this certificate**.
    
    ![Always Trust Certificate](/1.10/img/osx-chrome-always-trust.png)

5. Close the dialog.

6. Open a new Incognito Chrome window and open the DC/OS web interface. The path to the DC/OS web interface in the address bar should be marked **Secure**. You can also try visiting the public IP address of each of your masters to confirm that all show up as **Secure**.

# <a name="win-chrome"></a>Configuring Google Chrome on Windows to trust your DC/OS CA

**Note:** Procedure works best with Chrome 56 and Windows 10.

1. Open your Chrome browser and type `chrome://settings` in the address bar.

2. Scroll down and click **Show advanced settings**.

3. Scroll down and click **Manage certificates**.

4. Click to open the **Trusted Root Certification Authorities** tab.

5. Click **Import**.

6. Click **Next** on the **Welcome** page of the Certificate Import Wizard.

7. Click **Browse**.

8. Navigate to your `dcos-ca.crt` file, select it, and click **Open**.

9. Click **Next**.

10. Make sure that the **Place all certificates in the following store** option is selected and that the **Certificate Store** is **Trusted Root Certification Authorities**.

11. Click **Next**.

12. Click **Finish**.
    
    ![Certificate Warning](/1.10/img/chrome-win-sec-wrning.png)

13. You should check that the thumbprint matches the thumbprint of your DC/OS CA root certificate, then click **Yes**.

14. Click **OK** on the confirmation message.

15. Click **Close**.

16. Close and restart Chrome, or open a new Incognito session. Visit your cluster URL and the public IP addresses of each master to confirm that the sites now show up as **Secure**.

# <a name="osx-win-firefox"></a>Configuring Mozilla Firefox on OS X or Windows to trust your DC/OS CA

1. Open your Mozilla Firefox browser and type `about:preferences#advanced` in the address bar.

2. Click **Certificates**.

3. Click **View Certificates**.

4. Click **Import**.

5. Locate and select the `dcos-ca.crt` file in the dialog and click **Open**.
    
    ![Firefox confirmation](/1.10/img/osx-ff-confirm.png)

6. We recommend clicking **View** to examine the certificate. Ideally, you should confirm that the fingerprints match those of your DC/OS CA's root certificate.

7. After verifying the certificate, select **Trust this CA to identify websites** and click **OK**.

8. Click **OK** again to close the **Certificates** dialog.

9. Type your cluster URL in the address bar and press ENTER. The path to the DC/OS web interface in the address bar should be marked **Secure**. You can also try visiting the public IP address of each of your masters to confirm that all show up as **Secure**.