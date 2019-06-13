# Privacy-and-Security-Focused-Browser

Tor Browser without Tor[Tor Browser](https://www.torproject.org/projects/torbrowser) is a fork of the Mozilla Firefox web browser with patches that enhance both security and privacy. It routes all traffic through the [Tor network](https://en.wikipedia.org/wiki/Tor_%28anonymity_network%29) to conceal a users and usage from anyone conducting network surveillance or traffic analysis. Using Tor makes it very difficult to trace Internet activity to the user: this includes "visits to Web sites, online posts, instant messages, and other communication forms.<sup>[[1]](https://www.torproject.org/about/overview.html)</sup>

But it is also possible (and easy!) to use Tor Browser without [Tor](https://www.whonix.org/wiki/Tor) and take advantage of its excellent enhancements for reducing linkability, which is, "the ability for a user's activity on one site to be linked with their activity on another site without their knowledge or explicit consent."<sup>[[2]](https://www.torproject.org/projects/torbrowser/design/#privacy)</sup> Even without routing traffic over the Tor network, Tor Browser offers better protection from [online tracking](https://www.whonix.org/wiki/Data_Collection_Techniques) than Firefox, Google Chrome/Chromium or Microsoft Edge, especially against [fingerprinting](http://www.whonix.org/wiki/Data_Collection_Techniques#Fingerprinting_of_Browser_.28HTTP.29_Header), without any customization necessary.<sup>[[3]](https://2019.www.torproject.org/projects/torbrowser/design/)</sup>

## Security Enhancements:

* **Selfrando**: Randomizes Tor browser code to ensure that an attacker doesn't know where the code is on your computer. This makes it much harder for someone to construct a reliable attack.<sup>[[4]](https://blog.torproject.org/selfrando-q-and-georg-koppen)</sup><sup>[[5]](https://people.torproject.org/~gk/misc/Selfrando-Tor-Browser.pdf)</sup>
* **WebRTC disabled by default**: WebRTC can compromise the security of VPN tunnels, by exposing the external (real) IP address of a user.<sup>[[6]](https://en.wikipedia.org/wiki/WebRTC#Concerns)</sup><sup>[[7]](https://torrentfreak.com/huge-security-flaw-leaks-vpn-users-real-ip-addresses-150130/)</sup>
* **Security Slider**: Lets you increase your security by disabling certain web features that could be used to attack you security.<sup>[[8]](https://tb-manual.torproject.org/security-slider/)</sup>
* **NoScript installed by default**: NoScript can provide significant protection with the correct configuration.<sup>[[9]](https://en.wikipedia.org/wiki/NoScript)</sup> NoScripts blocks active (executable) web content and protects against [cross-site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting) (XSS). "The add-on also offers specific countermeasures against security exploits".
* **HTTPS Everywhere installed by default**: HTTPS Everywhere is a browser extension that encrypts your communications with many major websites, making your browsing more secure.<sup>[[10]](https://www.eff.org/https-everywhere)</sup>
* **Reproducible builds**: Tor Browser build security is achieved through a reproducible build process that enables anyone to produce byte-for-byte identical binaries to the ones Tor Project releases.<sup>[[11]](https://2019.www.torproject.org/projects/torbrowser/design/#BuildSecurity)</sup><sup>[[12]](https://blog.torproject.org/deterministic-builds-part-two-technical-details)</sup>
* **DNS and proxy configuration obedience**: Proxy obedience is achieved through custom patches, Firefox proxy settings, and build flags. Plugins which can bypass proxy setting are disabled.<sup>[[13]](https://2019.www.torproject.org/projects/torbrowser/design/#proxy-obedience)</sup>

## Install and Configure Tor Browser without Tor.

**Note: Debian platforms only!**

Tor Browser can be installed using [tb-updater](https://github.com/Whonix/tb-updater) which is a package developed and maintained by Whonix developers. When run, `tb-updater` seamlessly automates the download and verification of Tor Browser (from The Tor Project's website). Moreover, for users that have a requirement for a security and privacy focused clearnet browser (Tor Browser without Tor), `tb-updater` comes with the functionality to disable Tor prebaked into the source. To disable Tor, users need only append the `--clearnet` switch when starting Tor Browser.<sup>[[14]](https://forums.whonix.org/t/todo-research-and-document-how-to-use-tor-browser-for-security-not-anonymity-how-to-use-tbb-using-clearnet/3822/54)</sup> Unlike other methods that require users to manually disable Tor, this greatly simplifies configuration and lessons the chances that a configuration error will be made.


## Install tb-updater

The first step to install `tb-updater` is to download the Whonix signing key using APT package manager. However, as outlined in this [Qubes issue](https://github.com/QubesOS/qubes-issues/issues/4291) downloading GPG keys with APT will fail in TemplateVMs. To work around this issue the signing key can be downloaded in an AppVM and copied over to the Debian TemplateVM in a text file. Then `tb-updater` can be downloaded and verify in the TemplateVM.

1. Download the Whonix signing key

   In the AppVM, run

       sudo apt-key --keyring /etc/apt/trusted.gpg.d/whonix.gpg adv --keyserver hkp://ipv4.pool.sks-keyservers.net:80 --recv-keys 916B8D99C38EAF5E8ADC7A2A8D66066A2EEACCDA

2. Display the keys fingerprint

   In the AppVM, run
                  
       sudo apt-key adv --fingerprint 916B8D99C38EAF5E8ADC7A2A8D66066A2EEACCDA

3. Compare the fingerprint displayed in the terminal to the one listed at the following link; https://www.whonix.org/wiki/Patrick_Schleizer

4. Copy the Whonix signing key to a new text file named `whonix.key`

    In the AppVM, run.

       sudo apt-key export 916B8D99C38EAF5E8ADC7A2A8D66066A2EEACCDA > /tmp/whonix.key

    If the following error is encountered it can safely be ignored

    >Warning: apt-key output should not be parsed (stdout is not a terminal)
    gpg: WARNING: nothing exported

5. Copy the `whonix.key` text file to the Debian TemplateVM

       qvm-copy /tmp/whonix.key <templatevm_name>

     If the following error appears, it can be safely ignored (hit "OK" when prompted)

     >qfile-agent: Fatal error: stat TemplateVM (error type: No such file or directory)

6. In the Debian TemplateVM import the Whonix signing key

   cd to the Qubes Incoming directory

       cd ~/QubesIncoming/appvm_name

   Next, import the Whonix signing key

       gpg --import whonix.key

7. In the Debian TemplateVM add the Whonix APT repository to the sources.list

       echo "deb http://deb.whonix.org buster main contrib non-free" | sudo tee /etc/apt/sources.list.d/whonix.list

8. In the Debian TemplateVM update the packages lists

       sudo apt-get update

9. Finally, install `tb-updater`

       sudo apt-get install tb-updater

## Install Tor Browser

Tor Browser can be installed simply by running `tb-updater` in the Debian TemplateVM. Since running applications in a TemplateVM is strongly recommended against, the process will exit after Tor Browser is extracted and installed.

In the Debian TemplateVM, run

    update-torbrowser

## Starting Tor Browser

Any newly created AppVM based on the above TemplateVM will inherit the Tor Browser package that was downloaded. To disable Tor users need only run Tor Browser with the `--clearnet` switch.

**Note:** Disabling Tor means traffic will not be routed through the Tor network. Similar to other browsers, your IP address will be visible to the recipients of any communications. This configuration is not anonymous.

To start Tor Browser without Tor, in dom0 terminal, run

    qvm-run <appvm_name> "torbrowser --clearnet"

Tor Browser will have a red background with a message stating _"Something Went Wrong!" Tor is not working in this browser._ Which is what you want when using the `--clearnet` switch. 


## Normalizing Tor Browser behaviour (Security vs. Usability trade-off)

While Tor Browser has numerous security enhancements they can come at a cost of decreased usability. Since Tor Browser is highly configurable, security settings and behavior can be customized according to the requirements of the user.

**Security Slider**: Tor Browser has a “Security Slider” in the shield menu that allows you to [increase security](https://tb-manual.torproject.org/security-settings/) by disabling certain web features that can be used to attack your security and privacy. By default, the Security Slider is set to "Standard" which is the lowest security level. Increasing Tor Browser's security level will prevent some web pages from functioning properly, so you should weigh your security needs against the degree of usability you require.

**Private Browsing Mode**: In the default configuration Tor Browser has private browsing mode enabled. This setting prevents browsing and download history as well as cookies from remaining persistent across Tor Browser restarts. However, `tb-updater` includes a custom `user_pref` that disables private browsing mode when the `--clearnet` switch is used. 

  When private browsing mode is disabled Tor Browser's built-in "long-term linkability" protections are deactivated. The user loses protection which aims to prevent for example, "activities from an earlier browser session from being linkable to a later session". If security is paramount users can enable private browsing mode by commenting out the corresponding user preference.

  In Debian TemplateVM, open tb_without_tor_settings.js in a text editor with root rights.

  `sudo gedit /usr/share/tb-updater/tb_without_tor_settings.js`

  Next, comment out "//" `user_pref("browser.privatebrowsing.autostart", false);`

  When completed, the corresponding line should look like the following text block. 
  ```
  // Normalize Tor Browser behavior
  user_pref("extensions.torbutton.noscript_persist", true);
  //user_pref("browser.privatebrowsing.autostart", false);
  ```

  If you prefer to keep private browsing mode disabled, it may be advantageous to install one or more anti-tracking browser extensions. The extensions [Disconnect](https://addons.mozilla.org/en-US/firefox/addon/disconnect/), [Privacy Badger](https://www.eff.org/privacybadger/faq#How-is-Privacy-Badger-different-from-Disconnect,-Adblock-Plus,-Ghostery,-and-other-blocking-extensions) and [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/) are all open-source and are generally recommended. Research which one(s) may be most suitable in the circumstances; their use cases are different.

**Persistent NoScript Settings**: `tb-updater` includes a `user_pref` that allows custom NoScript settings to persist across browser sessions. This is also a security vs usability trade-off.<sup>[[17]](https://www.whonix.org/wiki/Tor_Browser#NoScript_Custom_Setting_Persistence)</sup> Keep in mind that all NoScript preference will be overridden and all custom per-site settings lost, if the Tor Browser "Security Slider" setting is changed afterwards. This holds true regardless if the security setting was increased or decreased.

  If you prefer to disable persistent NoScript setting this can easily be done by commenting out the corresponding `user_pref`.

  In Debian TemplateVM, open tb_without_tor_settings.js in an editor with root rights.

  `sudo gedit /usr/share/tb-updater/tb_without_tor_settings.js`

  Next, comment out "//" `user_pref("extensions.torbutton.noscript_persist", true);`

  When completed, the corresponding line should look like the following text block.
  ```
  // Normalize Tor Browser behavior
  //user_pref("extensions.torbutton.noscript_persist", true);
  user_pref("browser.privatebrowsing.autostart", false);
  ```
**Remember logins and passwords for sites**: By default Tor Browser does not save site login information such as user names or password. To increase usability, `signon.rememberSignons` is set to true in which allows this information to be saved across browser sessions. 
  
  If you prefer to disable this feature open tb_without_tor_settings.js in an editor as previously shown. 
  
  Next, comment out "//" `user_pref("signon.rememberSignons", true);`
 
  When completed, the corresponding line should look like the following text block.
  ```
  // Save passwords.
  //user_pref("signon.rememberSignons", true);
  ```
## FAQs

**Whonix developers focus their efforts on an advanced anonymity with Tor being a core component. Why develop a package that disables Tor?**

Package `tb-upater` was developed with design goals focused on securely downloading and verifying Tor Browser. However, requirements for a new operating system under development -- a security focused OS [based on Hardened Debian](https://forums.whonix.org/t/hardened-debian-security-focused-linux-distribution-based-on-debian-in-development-feedback-wanted/5943) -- called for a security hardened clearnet browser. Tor Browser without Tor met those requirements. Hence, the patch that disables Tor was integrated into tb-updater.

**What is Clearnet?**

This term has two meaning<sup>[[15]](https://www.whonix.org/wiki/FAQ)</sup>

* Connecting to the regular Internet without the use of Tor or other anonymity networks; and/or
* Connecting to regular servers which are not onion services, irrespective of whether Tor is used or not.

**How does the --clearnet switch disable Tor?**

Tor Browser supports custom user preferences `"user_pref"` which can be used to change browser configuration and behavior. In `tb-updater` the user preferences that disable Tor are located in /usr/share/tb-updater/tb_without_tor_settings.js. When the `--clearnet` switch is appended to /usr/bin/torbrowser, this file is copied over to the corresponding Tor Browser profile were the custom `user_pref(s)` are parsed.<sup>[[16]](https://github.com/Whonix/tb-starter/blob/28102df140f3f0f8a9b1bd5bc7dc19336420ccce/usr/bin/torbrowser#L354-L365)</sup> 

Tor is disabled by setting these three preferences to false.
```
user_pref("extensions.torbutton.startup", false);
user_pref("extensions.torlauncher.start_tor", false);
user_pref("network.proxy.socks_remote_dns", false);
```
**Can is use the `--clearnet` switch in a torified VM?**

This is strongly recommended against because using the `--clearnet` switch will break Tor Browser's per tab stream isolation.

**Does the `--clearnet` switch alter any other Tor Browser behavior?**

 No, the only cahnges are to the preference prevously shown. They include;

 * Disables private browsing mode by setting `browser.privatebrowsing.autostart` to false. 

 * Enables password storage by setting `signon.rememberSignons` to true

 * Enables persistent NoScript per-site settings by setting `extensions.torbutton.noscript_persist` to true

**Can I add my own custom preferences to change Tor Browser behavior?**

 Yes, but this could degrade security and privacy. See; Normalizing Tor Browser behavior.

**I have an idea to improve Tor Browser without Tor's security in Qubes. Can is submit patch?**

_Most definitely!_ Patches are always welcome!


