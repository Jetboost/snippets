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
1. Copy the code
2. Paste this code in the second custom code block (`</body>` at bottom) in your Page Settings
3. Replace all the Jetboost no results class names with your own custom class. The class name you’re replacing should look something like this: jetboost-list-wrapper-empty-abcd
4. Change the `”.no-results”`  (line 7 in the code below) to your own custom class leaving the “.” to denote that it's a class name. For example: grandparent.querySelector(".no-results”); can be changed to grandparent.querySelector(".my-no-results-element”);
5. Save the page settings
6. Publish your site

