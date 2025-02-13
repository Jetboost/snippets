# Share Favorites List

We often get asked by customers about how their users can share their favorites list. There are two main methods for how someone might want/need to share their favorites list.

1. Share with the site owners via a Webflow form
2. Share via email through their own email client with formatted text

Each script below will have a CONFIG object. To make things easier for you, the values for each of those options in the CONFIG is the only thing you will have to adjust. You won't need to change any of the code below it if you don't want to or don't have much Javascript experience.

<br>

## **Option 1: Share Favorites via Webflow Form**
You can place this script in the second code block before the `</body>` tag in page settings.
```html
<!-- FAVS FORM SUBMIT -->
<script>
  const CONFIG = {
    form: '.favs-form',
    collectionItem: '.COLLECTION-ITEM-CLASS', // Collection Item class
    favoritesField: '.FORM-TEXTAREA-CLASS', // Text area class where favorites list will go
    favoritesData: '.FAVORITE-ITEM-EMBED-CLASS', // Embed class where favorites data is
    successMessage: '.w-form-done', // Don't need to change this
    errorMessage: '.w-form-fail' // Don't need to change this
  };

  // Initialize form handler
  const initFormHandler = () => {
    const form = document.querySelector(CONFIG.form);
    if (!form) return;

    const originalSubmit = HTMLFormElement.prototype.submit;

    form.addEventListener('submit', function(e) {
      e.preventDefault();
      e.stopImmediatePropagation();

      const textArea = form.querySelector(CONFIG.favoritesField);
      const collectionItems = [...document.querySelectorAll(CONFIG.collectionItem)];
      const errorMessage = document.querySelector(CONFIG.errorMessage);
      const successMessage = document.querySelector(CONFIG.successMessage);

      // Clear the textarea
      textArea.value = '';

      // Flag to track if we successfully added any data
      let hasAddedData = false;

      // Collect all the favorite data strings
      collectionItems.forEach((item, index) => {
        const dataElement = item.querySelector(CONFIG.favoritesData);
        if (!dataElement?.innerText) return;

        hasAddedData = true;
        textArea.value = textArea.value + (
          index < collectionItems.length - 1 
          ? `${dataElement.innerText}, ` 
          : `${dataElement.innerText}.`
        );
      });

      // If no data was added or textarea is empty, show error
      if (!hasAddedData || !textArea.value.trim()) {
        console.log('Form submission prevented: No favorites data found or textarea empty');
        if (errorMessage) {
          errorMessage.style.display = 'block';
          successMessage.style.display = 'none';
        }
        return false;
      }

      // If we have favorites content, create a new submit event
      const submitEvent = new Event('submit', {
        bubbles: true,
        cancelable: true
      });

      // Remove the handler temporarily
      form.removeEventListener('submit', arguments.callee);

      // Dispatch the event
      form.dispatchEvent(submitEvent);

      // Re-add the handler
      form.addEventListener('submit', arguments.callee);
    });
  };

  // Initialize when DOM is ready
  document.addEventListener('DOMContentLoaded', initFormHandler);
</script>
```
<br>
<br>

## **Option 2: Share Favorites via email client**
You can place this script in the first code block in the `</head>` in page settings.
```html
<!-- EMAIL FAVS -->
<script defer>
  const EMAIL_CONFIG = {
    // Email Settings
    recipient: '', // Add default recipient email here
    subject: 'My Favorite Items',
    introText: 'Here are my favorite items:',
    
    // Display Settings
    format: 'list', // Options: 'list' or 'comma'
    
    // Selectors (Update these with your own classes)
    buttonSelector: '.FAVORITES-EMAIL-BUTTON-CLASS',
    itemSelector: '.COLLECTION-ITEM-CLASS',
    dataSelector: '.FAVORITE-ITEM-EMBED-CLASS'
  };

  document.addEventListener('DOMContentLoaded', () => {
    // GENERATE MAIL LINK
    function generateMailtoLink(data) {
      let formattedData = '';
      if (EMAIL_CONFIG.format === 'list') {
        formattedData = data.map(item => `- ${encodeURIComponent(item)}`).join('%0A');
      } else if (EMAIL_CONFIG.format === 'comma') {
        formattedData = data.map(encodeURIComponent).join(', ');
      }

      const mailtoLink = `mailto:${EMAIL_CONFIG.recipient}?subject=${encodeURIComponent(EMAIL_CONFIG.subject)}&body=${encodeURIComponent(EMAIL_CONFIG.introText)}%0A%0A${formattedData}`;
      return mailtoLink;
    }

    // CREATE EMAIL
    const emailButton = document.querySelector(EMAIL_CONFIG.buttonSelector);

    emailButton.addEventListener("click", () => {
      // TARGET CLASSES
      const favItems = Array.from(document.querySelectorAll(EMAIL_CONFIG.itemSelector))
        .map(item => item.querySelector(EMAIL_CONFIG.dataSelector).textContent);
      
      const mailtoLink = generateMailtoLink(favItems);
      emailButton.setAttribute("href", mailtoLink);
    });
  });
</script>
```
