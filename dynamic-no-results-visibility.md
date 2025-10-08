# Dynamic No Results Message Visibility
Sometimes, you may need to filter multiple Collection Lists that share the same CMS source, but each list displays different items.

For example, imagine a Colors collection where you have two Collection Lists: one filtered to show Most Popular colors and another showing the rest. Since both lists use the same CMS source (Colors), you only need to create a single Jetboost Filter Booster to filter both.

However, when using the “No Results Found” Power Up, the message element isn’t dynamic. If one list returns results and the other doesn’t, Jetboost may still display the “no results” message for both lists.

To fix this, you can use the small custom script below that ensures the “no results” element only appears when that specific Collection List actually has no results.

<br>

### Video Tutorial
(*Coming soon!*)

<br>

### Script
```html
<!-- DYNAMIC NO RESULTS MESSAGE VISIBILITY -->
<script type="text/javascript">
  window.JetboostListUpdated = function (collectionList) {
    const noResultsClass = "no-results"
    const collectionContainer = collectionList?.parentElement?.parentElement;
    if (!collectionContainer) return;

    const noResultsElem = collectionContainer.querySelector(`.${noResultsClass}`);
    if (!noResultsElem) return;

    noResultsElem.style.display = (collectionList.children.length === 0) ? "block" : "none";
  };
</script>
```

<br>

### How to use:
1. In Webflow, add a unique custom class to your "No Results" element (e.g., `no-results-message`), and remove any existing `jetboost-list-wrapper-empty` class.
2. Copy the script from this page and paste it into the `</body>` custom code block in your Page Settings.
3. In the script, find the line `const noResultsClass = "no-results"` and replace `"no-results"` with the custom class you created in step 1.
4. Save your page settings.
5. Publish your site.
