# SecBrowser


SecBrowser is a security focused browser that provides better protection from exploits, thereby reducing the risk of a virus infection. Enhanced usability is achieved with a built-in security slider that can be used to easily disable web site features that increase attack surface such as JavaScript. Since many of the features that are commonly exploited in browsers are disabled by default, SecBrowser's attack surface is greatly reduced. In the default configuration, SecBrowser offers better security than Firefox, Google Chrome or Microsoft Edge without any customizations necessary.<sup>[[3]](https://2019.www.torproject.org/projects/torbrowser/design/)</sup> It also has better protections from [online tracking](https://www.whonix.org/wiki/Data_Collection_Techniques), [fingerprinting](http://www.whonix.org/wiki/Data_Collection_Techniques#Fingerprinting_of_Browser_.28HTTP.29_Header) and reduces users linkability across websites.

SecBrowser is based on Tor Browser without [Tor](https://www.whonix.org/wiki/Tor). This means unlike Tor Browser, SecBrowser does _not_ route traffic over the Tor network, which in common parlance is referred to as "clearnet" traffic. Even without the aid of the Tor network, SecBrowser still benefits from the numerous [patches](https://gitweb.torproject.org/tor-browser.git) that Tor developers merged into the code base. Even with developer skills, these enhancements would be arduous and time consuming to duplicate in other browsers, with the outcome unlikely to match SecBrowser's many security benefits. While users can install browser extensions to mitigate specific attack vectors. Its unlikely to compare to SecBrowser which leverages the experience and know how of the Tor Project devs and the battle tested Tor Browser. 

## Security Enhancements:

* **WebRTC disabled by default**: WebRTC can compromise the security of VPN tunnels, by exposing the external (real) IP address of a user.<sup>[[6]](https://en.wikipedia.org/wiki/WebRTC#Concerns)</sup><sup>[[7]](https://torrentfreak.com/huge-security-flaw-leaks-vpn-users-real-ip-addresses-150130/)</sup>
* **Security Slider**: Lets you increase your security by disabling certain web features that could be used to attack your security.<sup>[[8]](https://tb-manual.torproject.org/security-slider/)</sup>
* **NoScript installed by default**: NoScript can provide significant protection with the correct configuration.<sup>[[9]](https://en.wikipedia.org/wiki/NoScript)</sup> NoScripts blocks active (executable) web content and protects against [cross-site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting) (XSS). "The add-on also offers specific countermeasures against security exploits".
* **HTTPS Everywhere installed by default**: HTTPS Everywhere is a browser extension that encrypts your communications with many major websites, making your browsing more secure.<sup>[[10]](https://www.eff.org/https-everywhere)</sup>
* **Reproducible builds**: Tor Browser build security is achieved through a reproducible build process that enables anyone to produce byte-for-byte identical binaries to the ones Tor Project releases.<sup>[[11]](https://2019.www.torproject.org/projects/torbrowser/design/#BuildSecurity)</sup><sup>[[12]](https://blog.torproject.org/deterministic-builds-part-two-technical-details)</sup>
* **DNS and proxy configuration obedience**: Proxy obedience is achieved through custom patches, Firefox proxy settings, and build flags. Plugins which can bypass proxy setting are disabled.<sup>[[13]](https://2019.www.torproject.org/projects/torbrowser/design/#proxy-obedience)</sup>

## Install and Configure SecBrowser.

**Note: Debian platforms only!**

Tor Browser can be installed using [tb-updater](https://github.com/Whonix/tb-updater) which is a package developed and maintained by Whonix developers. When run, `tb-updater` seamlessly automates the download and verification of Tor Browser (from The Tor Project's website). Moreover, for users that have a requirement for a security focused clearnet browser (SecBrowser), `tb-updater` comes with the functionality to disable Tor prebaked into the source. To disable Tor, users need only configure the `tb_clearnet=true` option in the initial set up.<sup>[[14]](https://forums.whonix.org/t/todo-research-and-document-how-to-use-tor-browser-for-security-not-anonymity-how-to-use-tbb-using-clearnet/3822/54)</sup> Unlike other methods that require users to manually disable Tor, this greatly simplifies configuration and lessons the chances that a configuration error will be made.

## Install tb-updater

The first step to install `tb-updater` is to download the Whonix signing key using APT package manager. However, as outlined in this [Qubes issue](https://github.com/QubesOS/qubes-issues/issues/4291) downloading GPG keys with APT will fail in TemplateVMs. To work around this issue the signing key can be downloaded in an AppVM and copied over to the Debian TemplateVM in a text file. Then `tb-updater` can be downloaded and verified in the TemplateVM.

1. Download the Whonix signing key.

   In the AppVM, run.

       sudo apt-key --keyring /etc/apt/trusted.gpg.d/whonix.gpg adv --keyserver hkp://ipv4.pool.sks-keyservers.net:80 --recv-keys 916B8D99C38EAF5E8ADC7A2A8D66066A2EEACCDA

2. Display the key's fingerprint.

   In the AppVM, run.
                  
       sudo apt-key adv --fingerprint 916B8D99C38EAF5E8ADC7A2A8D66066A2EEACCDA

3. Compare the fingerprint displayed in the terminal to the one listed at the following link; https://www.whonix.org/wiki/Patrick_Schleizer.

4. Copy the Whonix signing key to a new text file named `whonix.key`.

    In the AppVM, run.

       sudo apt-key export 916B8D99C38EAF5E8ADC7A2A8D66066A2EEACCDA > /tmp/whonix.key

    If the following error is encountered it can safely be ignored.

    >Warning: apt-key output should not be parsed (stdout is not a terminal)
    gpg: WARNING: nothing exported

5. Copy the `whonix.key` text file to the Debian TemplateVM.

       qvm-copy /tmp/whonix.key <templatevm_name>

     If the following error appears, it can be safely ignored (hit "OK" when prompted).
     
     >qfile-agent: Fatal error: stat TemplateVM (error type: No such file or directory)
     
6. In the Debian TemplateVM, import the Whonix signing key.

   cd to the Qubes Incoming directory.

       cd ~/QubesIncoming/appvm_name

   Next, import the Whonix signing key.

       gpg --import whonix.key

7. In the Debian TemplateVM, add the Whonix APT repository to the sources.list.

       echo "deb https://deb.whonix.org buster main contrib non-free" | sudo tee /etc/apt/sources.list.d/whonix.list

8. In the Debian TemplateVM, update the packages lists.

       sudo apt-get update

9. Install both `tb-updater` and `tb-starter`. (tb-updater `Recommends:` package `tb-starter`)

       sudo apt-get install tb-updater tb-starter

## Install Tor Browser

Tor Browser can be installed simply by running `tb-updater` in the Debian TemplateVM. Since running applications in a TemplateVM is strongly recommended against, the process will exit after Tor Browser is extracted and installed.

In the Debian TemplateVM, run.

    update-torbrowser

## Configure SecBrowser

Any newly created AppVM based on the above TemplateVM will inherit the Tor Browser package that was downloaded. To configure SecBrowser to start when `torbrowser` is run, users can choose to create either of the following configuration files in `/rw/config`

**Note:** The configuration option `tb_clearnet=true` disables Tor in all SecBrowser and Tor Browser instances run in the AppVM. Disabling Tor means traffic will not be routed through the Tor network. Similar to other browsers, your IP address will be visible to the recipients of any communications. This configuration is not anonymous.

(Option 1) In the AppVM terminal, open a newly created `torbrowser.d` folder in a text editor with root rights.

    sudo mkdir -p /rw/config/torbrowser.d
    
Next, add the following text.

    tb_clearnet=true

(Option 2) In the AppVM terminal, open a newly created `50_user.conf` folder in a text editor with root rights.

    sudo mkdir -p /rw/config/torbrowser.d/50_user.conf
    
 Next, add the following text.

    tb_clearnet=true   

## Start SecBrowser

To start SecBrowser, in a dom0 terminal, run.

    qvm-run <appvm_name> torbrowser

SecBrowser will have a red background with a message stating _"Something Went Wrong!" Tor is not working in this browser._ Which is what you want when using the `tb_clearnet=true` option. 

## Normalizing SecBrowser behaviour (Security vs. Usability trade-off)

While SecBrowser has numerous security enhancements they can come at a cost of decreased usability. Since it is also highly configurable, security settings and behavior can be customized according to the requirements of the user.

Note: If users edit the TemplateVM to modify SecBrowser behavior, all AppVMs created thereafter will inherit those changes. However, AppVMs created prior to the aforementioned edits will not benefit from any changes to the SecBrowser configuration file in the TemplateVM. 
   
**Security Slider**: SecBrowser has a “Security Slider” in the shield menu that allows you to [increase security](https://tb-manual.torproject.org/security-settings/) by disabling certain web features that can be used to attack your security. By default, the Security Slider is set to "Standard" which is the lowest security level. Increasing SecBrowser's security level will prevent some web pages from functioning properly, so you should weigh your security needs against the degree of usability you require.

**Private Browsing Mode**: In the default configuration SecBrowser has private browsing mode enabled. This setting prevents browsing and download history as well as cookies from remaining persistent across SecBrowser restarts. However, `tb-updater` includes a custom `user_pref` that disables private browsing mode when the `tb_clearnet=true` option is used. 

When private browsing mode is disabled SecBrowser's built-in "long-term linkability" protections are deactivated. The user loses protection which aims to prevent for example, "activities from an earlier browser session from being linkable to a later session". If security is paramount users can enable private browsing mode by commenting out the corresponding user preference.
   
   In the AppVM, open the `user.js` configuration file in an editor.
   
   `gedit ~/.tb/Browser/TorBrowser/Data/Browser/profile.default/user.js`
   
   Next, comment out "//" `user_pref("browser.privatebrowsing.autostart", false);`.

  When completed, the corresponding line should look like the following text block. 
  ```
  // Normalize Tor Browser behavior
  user_pref("extensions.torbutton.noscript_persist", true);
  //user_pref("browser.privatebrowsing.autostart", false);
  ```
   
If you prefer to keep private browsing mode disabled, it may be advantageous to install one or more anti-tracking browser extensions. The extensions [Disconnect](https://addons.mozilla.org/en-US/firefox/addon/disconnect/), [Privacy Badger](https://www.eff.org/privacybadger/faq#How-is-Privacy-Badger-different-from-Disconnect,-Adblock-Plus,-Ghostery,-and-other-blocking-extensions) and [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/) are all open-source and are generally recommended. Research which one(s) may be most suitable in the circumstances; their use cases are different.

**Persistent NoScript Settings**: `tb-updater` includes a `user_pref` that allows custom NoScript settings to persist across browser sessions. This is also a security vs usability trade-off.<sup>[[17]](https://www.whonix.org/wiki/Tor_Browser#NoScript_Custom_Setting_Persistence)</sup> Keep in mind that all NoScript preference will be overridden and all custom per-site settings lost, if the SecBrowser "Security Slider" setting is changed afterwards. This holds true regardless if the security setting was increased or decreased.

  If you prefer to disable persistent NoScript setting this can easily be done by commenting out the corresponding `user_pref`.

  In the AppVM, open the `user,js` configuration file in an editor.

  `gedit ~/.tb/Browser/TorBrowser/Data/Browser/profile.default/user.js`

  Next, comment out "//" `user_pref("extensions.torbutton.noscript_persist", true);`

  When completed, the corresponding line should look like the following text block.
  ```
  // Normalize Tor Browser behavior
  //user_pref("extensions.torbutton.noscript_persist", true);
  user_pref("browser.privatebrowsing.autostart", false);
  ```
**Remember logins and passwords for sites**: To increase usability, by default SecBrowser does save site login information such as user names or password. To implement this, `signon.rememberSignons` is set to `true` in which allows this information to be saved across browser sessions. 
  
  If you prefer to disable this feature open `user.js` in an editor and comment out the corresponding `user_pref`. 
  
  In the AppVM, open the `user.js` configuration file in an editor.

  `gedit ~/.tb/Browser/TorBrowser/Data/Browser/profile.default/user.js`
  
  Next, comment out "//" `user_pref("signon.rememberSignons", true);`
 
  When completed, the corresponding line should look like the following text block.
  ```
  // Save passwords.
  //user_pref("signon.rememberSignons", true);
  ```
## FAQ

**Whonix developers focus their efforts on advanced anonymity with Tor being a core component. Why develop a package that disables Tor?**

Package `tb-upater` was developed with design goals focused on securely downloading and verifying Tor Browser. However, requirements for a new operating system under development -- a security focused OS [based on Hardened Debian](https://forums.whonix.org/t/hardened-debian-security-focused-linux-distribution-based-on-debian-in-development-feedback-wanted/5943) -- called for a security hardened clearnet browser. Tor Browser without Tor (SecBrowser) met those requirements. Hence, the patch that disables Tor was integrated into `tb-updater`.

**What is Clearnet?**

This term has two meaning.<sup>[[15]](https://www.whonix.org/wiki/FAQ)</sup>

* Connecting to the regular Internet without the use of Tor or other anonymity networks; and/or
* Connecting to regular servers which are not onion services, irrespective of whether Tor is used or not.

**How does the `tb_clearnet=true` option disable Tor?**

Tor Browser supports custom user preferences `"user_pref"` which can be used to change browser configuration and behavior. In `tb-updater` the user preferences that disable Tor are located in /usr/share/tb-updater/tb_without_tor_settings.js. When the `tb_clearnet=true` option is added to /rw/config/torbrowser.d/, this file is copied over to the corresponding Tor Browser profile were the custom `user_pref(s)` are parsed.<sup>[[16]](https://github.com/Whonix/tb-starter/blob/28102df140f3f0f8a9b1bd5bc7dc19336420ccce/usr/bin/torbrowser#L354-L365)</sup> 

Tor is disabled by setting these three preferences to false.
```
user_pref("extensions.torbutton.startup", false);
user_pref("extensions.torlauncher.start_tor", false);
user_pref("network.proxy.socks_remote_dns", false);
```
**Can I use the `tb_clearnet=true` option in a Whonix-Workstation VM (`anon-whonix`)?**

VMs behind a `sys-whonix` are always routed through Tor, traffic would still be torified. However, this is strongly recommended against because using the `tb_clearnet` option will break Tor Browser's per tab stream isolation.

**Can I use the `tb_clearnet=true` option in a VM torified by something other than Whonix to avoid Tor over Tor?**

This is strongly recommended against because using the `tb_clearnet=true` option will break Tor Browser's per tab stream isolation. A [complete implementation](https://www.whonix.org/wiki/Dev/anon-ws-disable-stacked-tor) compatible with Tor Browser's per tab stream isolation would be much better.

**Does the `tb_clearnet=true` option alter any other SecBrowser behavior?**

No, the only changes to SecBrowser are to the preferences previously shown. 

**Can I add my own custom preferences to change SecBrowser behavior?**

Yes, but this could degrade security and privacy. see: Normalizing SecBrowser behavior.

**I have an idea to improve SecBrowser's security in Qubes. Can is submit patch?**

_Most definitely!_ Patches are always welcome!
