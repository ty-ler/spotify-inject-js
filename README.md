# spotify-inject-js

## Background

Spotify used to have an official feature for installing custom third-party applications such as lyric or radio apps. After a large UI rework, these third-party apps were removed. 

Spotify's Desktop client is essentially a large chromium window. Each "app" (page, usually) in Spotify is an `.spa` file located within the Apps folder in your Spotify installation directory. These .spa files are really just zipped up folders containing the apps resources and minified JavaScript code. According to [a former Spotfiy Engineer](https://www.quora.com/How-is-JavaScript-used-within-the-Spotify-desktop-application-Is-it-packaged-up-and-run-locally-only-retrieving-the-assets-as-and-when-needed-What-JavaScript-VM-is-used), these .spa files are rendered into iframes. This is so that engineers from different teams can use any framework or version they choose to. I am interested in the effectivness of development approach and its toll on overall performance.

This project is mostly for research into how Spotify is developed and also the state of security of Chromium-based hybrid applications. 

## Method

Since Spotify's .spa files can be extracted for the minified and obfuscated source, it may be possible to inject JS through directly adding custom JS, and then rebundle. There are several issues with this method though. To begin, the source is heavily obfuscated and it can be very difficult to modify or even analyze. On top of this, when Spotify sends out a new update, any changes made to an .spa file will be overwritten with the stock .spa files from the update.

Running the Spotify executable with the argument `--remote-debugging-port=9222` opens a WebSocket connection for Spotify on port 9222. You can access this WebSocket either through Chrome at `chrome://inspect/#devices` or programatically connect to the WebSocket to send payloads containing JavaScript to the Spotify client.

This method of injecting JavaScript into the desktop client was well documented by Danny Shemesh [here](https://medium.com/@dany74q/injecting-js-into-electron-apps-and-adding-rtl-support-for-microsoft-teams-d315dfb212a6).
