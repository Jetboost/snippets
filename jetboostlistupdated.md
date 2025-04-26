# Collection List Has Changed/Updated
Sometimes you may need to run a function or perform an action when the collection list has changed or updated. By "changed" or "updated", we mean when a user has selected filters, searched or anything else that would change or re-render the collection list.

In this case, we have a function called `JetboostListUpdated`. Anytime the collection list has changed or updated, this function will be called. You can write your own logic or call any other function within the body of this function. See the examples below:

### Video Tutorial
(*Coming soon!*)
 
```html
<!-- JetboostListUpdated -->
<script type="text/javascript">
  // This function will get called each time Jetboost updates a 
  // collection list via filters or anything like that
  
  window.JetboostListUpdated = function ( collectionList ) {
    
    // <-- Code goes here -->
    // You can call your own functions in this function
    // You can also use or remove the "collectionList" parameter above
    
  };
</script>
```
> [!NOTE]
> You can paste this code in the second custom code block (`</body>`).

<br>

As mentioned in the code comments, the `JetboostListUpdated` function will get called each time the collection list is updated. For example, if a user has selected a filter and the list changes, this function will then be called, which will of course run any code inside of the function body.

If you have your own custom functions that need to run when this happens, you can simply move your code inside the body of this function where it says `<!— Code goes here —>`, or just call your functions inside of it.
<br>

## Example Usage
```html
<!-- JetboostListUpdated -->
<script type="text/javascript">
  /**
   * Jetboost calls this function each time the items in a collection list have changed
   * collectionList is the Webflow Collection List element (.w-dyn-items). It's child elements are the Collection Items.
   * @param {HTMLDivElement} collectionList
   */
  
  window.JetboostListUpdated = function (collectionList) {
    
    // Loop through all collection items in the list that are currently on the page
    for (var collectionItem of collectionList.children) {
      
      // You can check to see if this is the first time Jetboost has added this collection item to the page.
      // This is useful for code that should only be run once per item, like adding event handlers.
      if (collectionItem.jetboostData.firstDisplay) {
        
        // Example: Add a click handler to a button inside each collection item
        collectionItem
          .querySelector(".my-button")
          .addEventListener("click", function (e) {
            
            // Do something when the button is clicked
            console.log("Button clicked!");
            
          });
      }
      
      // If there are actions you want to perform every time the items in the collection list change,
      // then you do not need to worry about the "firstDisplay" property that Jetboost provides.
      
      // Example: Get the item's slug
      var slug = collectionItem.querySelector(".jetboost-list-item").value;
      
      // Do something with the slug
      console.log(slug);
    }
  };
</script>
```

