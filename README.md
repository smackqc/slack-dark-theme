# slack-dark-theme

Slack Dark Theme

---

## Mac Installation

```bash
cd /Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/
echo "document.addEventListener('DOMContentLoaded', function() {" >> ssb-interop.js
echo " $.ajax({" >> ssb-interop.js
echo "   url: 'https://cdn.rawgit.com/laCour/slack-night-mode/master/css/raw/black.css'," >> ssb-interop.js
echo "   success: function(css) {" >> ssb-interop.js
echo "     let overrides = \`" >> ssb-interop.js
echo "     code { background-color: #535353; color: #85c5ff; } /* Change color: to whatever font color you want */" >> ssb-interop.js
echo "     .c-mrkdwn__pre, .c-mrkdwn__quote { background: #535353 !important; background-color: #535353 !important; }" >> ssb-interop.js
echo "     \`" >> ssb-interop.js
echo "     \$(\"<style></style>\").appendTo('head').html(css + overrides);" >> ssb-interop.js
echo "   }" >> ssb-interop.js
echo " });" >> ssb-interop.js
echo "});" >> ssb-interop.js
```

---

## Windows Installation

Replace

`C:\Users\<UserFolder>\AppData\Local\slack\<SlackVersion>\resources\app.asar.unpacked\src\ssb-interop.js`

with ssb-interop.js in this repo.

Example

`C:\Users\JohnDoe\AppData\Local\slack\app-3.3.7\resources\app.asar.unpacked\src\ssb-interop.js`

***Slack app may be cached so it takes a few reloads for changes to take place***

---

## Customize Css

`Update/Add css overrides to customize your own slack styles`

***Check out [CSS File](https://cdn.rawgit.com/laCour/slack-night-mode/master/css/raw/black.css) to see what to override***

```js
document.addEventListener('DOMContentLoaded', function() {
 $.ajax({
   url: 'https://cdn.rawgit.com/laCour/slack-night-mode/master/css/raw/black.css',
   success: function(css) {
     // Change CSS Here
     let overrides = `
     code { background-color: #535353; color: #85c5ff; }
     .c-mrkdwn__pre, .c-mrkdwn__quote { background: #535353 !important; background-color: #535353 !important; }
     `
     $("<style></style>").appendTo('head').html(css + overrides);
   }
 });
});
```

## Customization Example

```js
// First make sure the wrapper app is loaded
document.addEventListener("DOMContentLoaded", function () {

  // Then get its webviews
  let webviews = document.querySelectorAll(".TeamView webview");

  // Fetch our CSS in parallel ahead of time
  const cssPath = 'https://raw.githubusercontent.com/mallowigi/slack-one-dark-theme/master/custom.css';
  let cssPromise = fetch(cssPath).then(response => response.text());

  let customCustomCSS = `
   :root {
      /* Modify these to change your theme colors: */
     --primary: #E751E1;
     --accent: #282C34;
     --text: #ABB2BF;
     --background: #282C34;
     --background-elevated: #282C34;
     
     /* These should be less important: */
     --background-hover: lighten(#3B4048, 10%);
     --background-light: #AAA;
     --background-bright: #FFF;

     --border-dim: #666;
     --border-bright: #21252B;

     --text-bright: #FFF;
     --text-dim: black;
     --text-special: #A5A8AD;
     --text-accent: var(--primary);

     --scrollbar-background: #000;
     --scrollbar-border: var(--primary);

     --yellow: #fc0;
     --green: #98C379;
     --cyan: #61AFEF;
     --blue: #61AFEF;
     --purple: #C678DD;
     --red: #E06C75;
     --red2: #BE5046;
     --red2: var(--primary);
     --orange: #D19A66;
     --orange2: #E5707B;
     --gray: #3E4451;
     --silver: #9da5b4;
     --black: #21252b;
      }
   `

  // Insert a style tag into the wrapper view
  cssPromise.then(css => {
    let s = document.createElement('style');
    s.type = 'text/css';
    s.innerHTML = css + customCustomCSS;
    document.head.appendChild(s);
  });

  // Wait for each webview to load
  webviews.forEach(webview => {
    webview.addEventListener('ipc-message', message => {
      if (message.channel == 'didFinishLoading')
        // Finally add the CSS into the webview
        cssPromise.then(css => {
          let script = `
                     let s = document.createElement('style');
                     s.type = 'text/css';
                     s.id = 'slack-custom-css';
                     s.innerHTML = \`${css + customCustomCSS}\`;
                     document.head.appendChild(s);
                     `
          webview.executeJavaScript(script);
        })
    });
  });
});
```