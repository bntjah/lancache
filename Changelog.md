* 1-8-2017 fh
    * General
        * Templated hosts file (as it was in "installer" branch)
        * Updated configuration to be compatible with fhibler/lc-installer (forked from bntjah/lc-installer)
    * DNS
        * Replaced bind9 with unbound
        * Improvements and fixes to unbound configuration
        * Added additional domains for various CDNs
    * HTTPS
        * Incorporated sniproxy
* 3-2-2017 bn_ & raz3r83
    * General
        * Updated the caches for Uplay CDN
    * DNS
        * Added additional domains for Uplay CDN
* 3-7-2017 bn_ & Nexusofdoom
    * DNS
        * Added additional domain for Blizzard CDN	
* 3-27-2017 Travispk
    * Cleaned up Readme
    * Cleaned up interfaces
    * Made some tweaks to unbound.conf to make it faster and lower the TTL if changes are necessary during event
* 3-30-2017 Travispk
    * Cleaned up hosts to be in a more logical order
* 6-19-2017 Nexusofdoom & bn_
    * Nexus: Made some changes to Windows updates
    * Nexus: Provided working solution for OSX updates
    * bn_: Added them to installer
    * bn_: Added some minor tweaks to nginx as suggested in issues to counter bursty performance
* 6-21-2017 bn_
    * bn_: Added the necessary things as remarked by It0w in issues. Thanks for letting us know we forgot something!
* 6-30-2017 saambd
    * Added missing } on line 56 of Microsoft conf* 8-03-2017 bn_
* 8-03-2017 bn_    
    * Added 2 Steam URLs to Unbound.Conf as posted in Multiplay Github
* 10-17-2017 bn_    
    * Added Sony DNS to Unbound.Conf as posted by Muffeee in issues
    * Added Glyph Support (Still needs testing)
* 10-25-2017 Nagilum99 & Nexusofdoom
    * Merged the pull request from nagilum99 to correct / standardize the layout (Issue #65)
    * Changed Steamconfig as problem and solution posted by Nexusofdoom (Issue #61)
* 10-28-2017 nexusofdoom
    * Added 2 Battlenet URLs to Unbound.Conf
    * Added New Origins URLs to Unbound.Conf
    * Did some magic in lancache-origin.conf to make it work :-)
* 12-4-2017 nexusofdoom
    * Added Warframe and ESO/Elder Scrolls Online to Config.
* 9-19-2018 bn_
    * Added the two CDN wich were posted by @Billthecat in Issue #111
* 9-26-2018 bn_
    * Adding CDN's wich are posted in Issues by @Billthecat & @Chong601
    * Adding Gaijin Support as requested by @Chong601 in Issue #94
    * Added some CDN's listed by Apple within https://support.apple.com/en-us/HT201999
* 10-20-2018 Nagilum99
    * Rewrote Readme
    * Fixed an issue where bn_ missed some Gaijin hosts    
* 08-18-2019 bn_
    * Added EPIC Games CDN's and config as Lancache.net & UKLANS have been in contact with EPIC Games
 * 10-24-2019 Nagilum99
    * Added MS DNS Entries that can be cached (See Issue #146)
