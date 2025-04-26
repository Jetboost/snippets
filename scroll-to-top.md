# Scroll To Top
A common (and justified) question we often receive, is how can you scroll the user back to the top of the of the list once they have paginated to a new page. This completely makes sense as we don't want the user to remain at the bottom of the list on a new page. 

Here we have a perfect code snippet from CodeCrumbs that will help you achieve this functionality. It's very simple to use, and works like a charm. 

### Video Tutorial
(*Coming soon!*)

<br>


We can start by copying and pasting the CodeCrumbs Proxy in the first custom code block (`<head>`) either in your page settings where you're using this functionality or in the `<head>` of your global Site Settings > Custom Code tab. The proxy just makes sure that if CodeCrumbs doesn't load for any reason, that it won't prohibit anything else from loading/running.
```html
<!-- CodeCrumbs Proxy -->
<script>!function(e,t){e[t]=new Proxy(e[t]||{},{get:(e,o)=>new Proxy(e[o]||function(){},{apply:(n,r,a)=>{const c=()=>e[o](...a);"complete"===document.readyState?c():document.addEventListener("readystatechange",(n=>{"complete"===n.target.readyState&&(e?.[o]?c():console.error(`${t}.${o} is not a function. Did it load correctly from the CDN? If not, did you use the correct name.`))}))}})})}(globalThis,"CodeCrumbs");</script>
```

<br>

Next you will want to copy and paste the code below into the second custom code block (`</body>`) within your page settings where your collection list is or in the `</body>` of your global Site Settings > Custom Code tab.
```html
<!-- SCROLL TO TOP ON CLICK -->
<script src="https://cdn.jsdelivr.net/gh/CodeCrumbsApp/scroll-top-on-click@latest/index.min.js"></script>
<script>
  window.CodeCrumbs.ScrollTopOnClick({
    scrollTriggerSelectors: ['.TRIGGER-BUTTON-CLASS'],
    windowScroll: true, // If false, will only scroll the scroll areas to top
    windowScrollToSelector: '.SCROLL-ANCHOR-CLASS', // Scroll to specific element rather than top of the page
    windowScrollOffset: 60, // Offset distance in pixels from element or window
    windowScrollDelay: 500, // Delay in milliseconds before starting scroll
    scrollAreas: [
      {
        selector: '.SCROLL-AREA-CLASS', // Class of element with "overflow: scroll"
        offset: 0, // Distance in pixels from the top of the scroll area
      },
    ],
    scrollAreaScrollDelay: 500, // Delay in milliseconds before starting scroll
  })
</script>
```

