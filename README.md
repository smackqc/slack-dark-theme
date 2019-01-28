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