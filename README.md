# TwitchAdSolutions

This repo aims to provide multiple solutions for blocking Twitch ads.

## Recommendations

M3U8 proxies (or full proxies) are currently the most reliable way of avoiding ads (assuming you trust the third party). More proxy hosts would be ideal (see [#8](https://github.com/pixeltris/TwitchAdSolutions/issues/8)).

- `Twitch AdBlock` - [chrome](https://chrome.google.com/webstore/detail/twitch-adblock/mipdalemhlhfenbikcloloheedmmecme) / [firefox](https://addons.mozilla.org/de/firefox/addon/twitch-adblock/)
- `Twitch Video Ad Blocker` - [chrome](https://chrome.google.com/webstore/detail/ttv-intrusive-ad-blocker/gggloiaahekkkndacnpecpgafnehmlja) / [firefox](https://addons.mozilla.org/en-GB/firefox/addon/twitch-video-ad-blocker/) *(proxied url includes your user id / a unique id used for gql requests)*
- `proxy-m3u8` - see below (currently uses the proxy server used in `Twitch AdBlock`)

If you don't trust third parties then `dyn-skip` / `dyn-skip-midroll-alt` are decent. `ttv-tools` (firefox) also has nice features.

## Current solutions

*These solutions generally aren't compatible with other Twitch ad blockers. e.g. `ttv-ublock` will break `dyn-skip` (and others).*

- dyn-skip ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-skip/dyn-skip-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-skip/dyn-skip.user.js))
  - Notifies Twitch that ads were watched before requesting the main live stream.
  - Falls back to mute-black if this fails (use an alternative solution if it always fails for you, as it adds additional load).
  - *Midroll ads are muted/blacked out. See `dyn-skip-midroll-alt` for a an alternative solution.*
- dyn-skip-midroll-alt ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-skip-midroll-alt/dyn-skip-midroll-alt-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-skip-midroll-alt/dyn-skip-midroll-alt.user.js))
  - A mix of `dyn-skip` / `dyn`. During midrolls this plays a low resolution stream instead of nothing - might be a little glitchy but should always play *something*.
  - *If you see a `Waiting for ads to finish` banner without `midroll` in the banner text, you should be able to just refresh the page to get a regular stream.*
- dyn-skip-midroll ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-skip-midroll/dyn-skip-midroll-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-skip-midroll/dyn-skip-midroll.user.js))
  - The same as `dyn-skip`, but *attempts* to fully skip midroll ads (I'm not sure if this has ever actually worked).
  - **This requires the script to work perfectly, otherwise the player will hit a reload loop.**
- dyn-video-swap ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-video-swap/dyn-video-swap-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn-video-swap/dyn-video-swap.user.js))
  - Ads are replaced by a low resolution stream for the duration of the ad.
  - Similar to `dyn`, but skips closer to 20 seconds when switching to the live stream.
  - You might see tiny bits of the ad.
  - Audio controls wont work whilst the ad is playing.
- dyn ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn/dyn-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/dyn/dyn.user.js))
  - Ad segments are replaced by low resolution stream segments (on a m3u8 level).
  - Skips 2-3 seconds when switching to the live stream.
  - Stuttering and looping of segments often occur (during the ad segments).
  - **NOTE: Removing segments doesn't notify Twitch that ads were watched (aka more served ads).**
- low-res ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/low-res/low-res-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/low-res/low-res.user.js))
  - No ads.
  - The stream is 480p for the duration of the stream.
- mute-black ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/mute-black/mute-black-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/mute-black/mute-black.user.js))
  - Ads are muted / blacked out for the duration of the ad.
  - You might see tiny bits of the ad.
- proxy-m3u8 ([ublock](https://github.com/pixeltris/TwitchAdSolutions/raw/master/proxy-m3u8/proxy-m3u8-ublock-origin.js) / [userscript](https://github.com/pixeltris/TwitchAdSolutions/raw/master/proxy-m3u8/proxy-m3u8.user.js))
  - Uses a proxy server to fetch an ad-free stream.
  - Currently only the initial m3u8 is proxied, so there shouldn't be any additional latency.
  - **Assumes the proxy server acts in good faith and maintains a good uptime.**
  - **The current proxy owner doesn't like the url being used by this project, an alternative host would be ideal.** *(requires a web server hosted in a non-ad country, taking a channel name as the last url arg, and fetching the m3u8 - see [#8](https://github.com/pixeltris/TwitchAdSolutions/issues/8))*.

## Applying a solution (uBlock Origin)

uBlock Origin solutions are single files, suffixed by `ublock-origin.js` e.g. `low-res-ublock-origin.js`.

- Navigate to the uBlock Origin Dashboard (the extension options)
- Under the `My filters` tab add `twitch.tv##+js(twitch-videoad)`.
- Under the `Settings` tab, enable `I am an advanced user`, then click the cog that appears. Modify the value of `userResourcesLocation` from `unset` to the full url of the solution you wish to use (if a url is already in use, add a space after the existing url). e.g. `userResourcesLocation https://raw.githubusercontent.com/pixeltris/TwitchAdSolutions/master/dyn/dyn-ublock-origin.js` 

## Applying a solution (userscript)

Tampermonkey / Greasemonkey can be used on the files suffixed by `user.js` e.g. `low-res.user.js`

- Viewing one of the userscript files should prompt the given script to be added

## Other solutions / projects

- https://github.com/odensc/ttv-ublock (extension - purple screen may display every 10-15 mins)
- https://github.com/Nerixyz/ttv-tools (Firefox extension)
- https://github.com/LeonHeidelbach/ttv_adEraser (extension)
- https://github.com/instance01/Twitch-HLS-AdBlock (extension)
- https://github.com/Wilkolicious/twitchAdSkip (UserScript + FrankerFaceZ)
- https://gist.github.com/simple-hacker/ddd81964b3e8bca47e0aead5ad19a707 (UserScript + FrankerFaceZ(optional))
- https://greasyfork.org/en/scripts/415412-twitch-refresh-on-advert/code (UserScript + FrankerFaceZ(optional))
- [Alternate Player for Twitch.tv](https://chrome.google.com/webstore/detail/bhplkbgoehhhddaoolmakpocnenplmhf) - [code](https://robwu.nl/crxviewer/?crx=bhplkbgoehhhddaoolmakpocnenplmhf&qf=player.js) (extension)
- [Twitch AdBlock](https://chrome.google.com/webstore/detail/mipdalemhlhfenbikcloloheedmmecme) - [code](https://robwu.nl/crxviewer/?crx=mipdalemhlhfenbikcloloheedmmecme&qf=js/background.js) (extension - uses a proxy server to fetch an ad-free stream)
- [Twitch Video Ad Blocker](https://addons.mozilla.org/en-GB/firefox/addon/twitch-video-ad-blocker/) - [code](https://robwu.nl/crxviewer/?crx=https%3A%2F%2Faddons.mozilla.org%2Fen-GB%2Ffirefox%2Faddon%2Ftwitch-video-ad-blocker%2F&qf=js/background.js) (extension - uses a proxy server to fetch an ad-free stream *(proxied url includes your user id / a unique id used for gql requests)*)

---

- https://github.com/streamlink/streamlink (desktop application)
- https://github.com/nopbreak/TwitchMod (android app)
- https://twitchls.com/ (external site - purple screen may display every 10-15 mins)
- [Use a VPN targeting a region without ads](https://reddit.com/r/Twitch/comments/kisdsy/i_did_a_little_test_regarding_ads_on_twitch_and/)

## NOTE/TODO

NOTE: Many of these solutions could do with improvements.  
TODO: Test midroll ads.  
TODO: More testing in general.  
