ota-tools
======

A burgeoning collection of tools for Over-The-Air distribution of mobile applications.

OTA Tools started off as 2 disparate gists. One was a bash script for resigning
an iOS IPA file. The other was a sample plist and HTML file combo for serving an
iOS app for Over-The-Air distribution. When the later gist was updated with a
script for generating the combo, they were joined together to create a tools
package. These tools have saved me countless hours so they are being offered
under the MIT license.

## Contents:

    ipa_ota    - generate basic plist and html combo for ota hosting
    ipa_sign   - sign ipa with a keychain certificate and replace its provision
    app.plist  - simple plist example
    index.html - simple hosting flatfile example

## Usage:

**ipa_sign** takes an ipa file, provision file, and the name of the certificate in your keychain that you want to sign with.

    ~/Dropbox/Public/enterprise_builds rbronosky$ ./ipa_sign ~/Downloads/app.ipa ~/provisions/Huge_Enterprises.mobileprovision "iPhone Distribution: Huge Enterprises Inc."
    App has BundleIdentifier 'com.example.myapp' and BundleVersion 2.5
    App has provision   'iPhone Developer', which supports 'HDXJ2G3FGJ.com.example.*'
    Embedding provision 'provision for My App', which supports 'HDXJ2G3FGJ.com.example.myapp'
    Payload/myapp.app: replacing existing signature

Calling **ipa_ota** creates a basic plist in the current directory and outputs a single line for including in an existing HTML file.

    ~/Dropbox/Public/enterprise_builds rbronosky$ ./ipa_ota app.ipa http://dl.dropbox.com/u/310759/enterprise_builds/
        <p><a href="itms-services://?action=download-manifest&url=http://dl.dropbox.com/u/310759/enterprise_builds/app.plist">Install Enterprise App</a></p>

The script can detect when it is being redirected to a file. In this case it outputs an entire HTML file.

    ~/Dropbox/Public/enterprise_builds rbronosky$ ./ipa_ota app.ipa http://dl.dropbox.com/u/310759/enterprise_builds/ > download_app.html
    ~/Dropbox/Public/enterprise_builds rbronosky$ cat download_app.html
    <html>
      <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
      <head><title>Enterprise App Installer</title><style>*{text-align:center;}</style></head>
      <body>
        <p><a href="itms-services://?action=download-manifest&url=http://dl.dropbox.com/u/310759/enterprise_builds/app.plist">Install Enterprise App</a></p>
      </body>
    </html>

#### Note:
It has been reported that on Mountain Lion, you may see an error like this:

    Payload/MyApp.app: object file format unrecognized, invalid, or unsuitable

Googling suggests that you can resolve this by setting an env var like so:

    export CODESIGN_ALLOCATE="/Applications/Xcode.app/Contents/Developer/usr/bin/codesign_allocate"

I have never seen this, so I can't directly speak for it.
