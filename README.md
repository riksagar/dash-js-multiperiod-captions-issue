# Test page to demostrate issue with captions

This project contains sample content and a dash-js based player to demostrate an issue handling captions in multi-period dash with versions 4 and 5 of DASH-JS.

## Steps to reproduce

 - Run the nginx webserver from the `./nginx/` directory using the `start-nginx.sh` script
 - Navigate in browser to `http://localhost:8888/player.html`
 - start playback
   - Observe that the captions start almost immediately.
 - Allow to play for 30 seconds
    - Observe that captions continue ot be displayed, and change, during the _BBB_ content
 - After approx 30 seconds seek back to start
 - Allow to play again
    - Observe that captions stop displaying once the content transitions into the second `<Period ... />`

## Video of observed behavior

 - Video on [YouTube](https://youtu.be/Yzwk1ob0TbA)

## Expected behavior

 - The captions should replay when the user seeks back to the start of the content
 - The captions should start playing during the second period
    - The VTT file is in an `<AdapationSet />` in that period

## Observed behavior
 - After seeking back to the start of the presentation captions are not rendered during the second period
    - Issue occurs when seeking into first period from second period, then allowing content to transition into second period a subsequent time
 - The captions are displayed relative to the start of the presentation, not the start of the period in which they reside
    - Seems like DASH-JS does not take into account the `@start` of the period to adjust the presentation time of the VTT track


## Details

### Server setup

There's an `nginx.conf` file that can be use to run a local Nginx webserver.  You'll need to change the config to point the `root` and `alias` to the directory where you clone the repo.

### Dash Periods

The manifest file `./packaged/manifest-multiperiod.mpd` ( [here](https://github.com/riksagar/dash-js-multiperiod-captions-issue/blob/main/packaged/manifest-mutiperiod.mpd) ) is a multiperiod DASH manifest.

There are three DASH periods in the manifest:
 - 6 seconds of "pre-roll"
 - 40 seconds of "main content" (_Big Buck Bunny_)
 - 10 seconds of "post-roll"

The second period has captions (the first and third period do not).

### DASH-JS version

Version 5.0.3 of DASH-JS is bundled and included in the repo ( [here](https://github.com/riksagar/dash-js-multiperiod-captions-issue/tree/main/nginx/www/dashjs) ).  It was build with `npm run build-legancy` using the tagged `v5.0.3` code from https://github.com/Dash-Industry-Forum/dash.js.git
