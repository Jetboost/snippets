# Reinitialize Jetboost
Until now, Jetboost initialized only once on page load. But if you’re using a library that hijacks navigation—like Barba JS’s smooth page transitions—you’ll notice your Jetboost filters stop working after navigating. That’s because Barba JS prefetches the new page and intercepts the normal reload, so Jetboost never runs again.

With `Jetboost.ReInit()`, you can manually restart Jetboost whenever you need. Below is a full example with Barba JS. You will notice that we call the ReInit method on Barba's `enter()` hook. The `enter()` hook runs anything in it's body when "entering/loading" the next page. In this case, we're calling our `AnimationEnter()` function to run our GSAP animation, and then `Jetboost.ReInit()` to get Jetboost working.

```html
<!-- GSAP JS SCRIPT -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.7/dist/gsap.min.js"></script>

<!-- BARBA JS SCRIPT -->
<script src="https://cdn.jsdelivr.net/npm/@barba/core"></script>

<!-- GSAP & BARBA JS -->
<script>
  const animationEnter = (container) => {
    return gsap.from(container, {opacity: 0, duration: .35, ease: 'none'})
  }

  const animationLeave = (container) => {
    return gsap.to(container, {opacity: 0, duration: .35, ease: 'none'})
  }
  
  barba.init({
    transitions: [
      {
        once({next}) {
          animationEnter(next.container)
        },
        async leave({current}) {
          await animationLeave(current.container)
        },
        enter({next}) {
          animationEnter(next.container)
          Jetboost.ReInit()
        }
      }
    ]
  });
</script>
```


<br>

## Why & When to Use It
1. **Page Transitions** – Libraries like Barba JS, Swup, PJAX, etc., intercept link clicks and prevent full reloads—so Jetboost can’t auto‑initialize on the new content.
2. **Dynamic Content** – If you inject or replace Jetboost‑powered elements via AJAX, you’ll need to call ReInit() to bind filters, sorters, and search inputs to the fresh markup.

<br>

> [!TIP]
> If you have multiple re‑initializations (say, on every AJAX call), debounce your calls to avoid performance hits

```js
const debouncedReInit = debounce(() => Jetboost.ReInit(), 200);

someAjaxLibrary.on('content:updated', () => {
  debouncedReInit();
});
```
