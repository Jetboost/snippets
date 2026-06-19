# Show Filter Results on a Separate Page

With this snippet you can allow users to select and choose filters from one page, and then view the filtered results on a separate page. For example, on a landing page of a real-estate website it's common to see a set of filters that allow users to select number of bedrooms, number of bathrooms, desired property type, price range, location, etc., and then view the filtered results on a separate page.

This snippet takes advantage of the Jetboost query string functionality (URL query/search params) to pass the selected filter values from the one page to a results/collection list page.

The nice thing is that this single snippet works for 2 possible filter setups, either filters within Webflow Form element (how Jetboost typically has you set them up) as well as standalone filters (not in a form) using Webflow's Custom Elements and a custom filter/search button. You can watch the video below to see it in action with step-by-step guidance or read the instructions.

<br>

## Video Tutorial

Coming soon!

<br>

### **Instructions:**

1. Copy and paste this script in the second code block before the `</body>` tag in page settings.
2. Update the `resultsPagePath` variable to match the path of your results page.
3. Not using a Webflow Form element? Update the `filterButton` variable (specifically the querySelector value) to match the ID of your filter button.
4. Using a Webflow Form element? Update the `filterForm` and `filterFormSubmitButton` querySelector values for the form and submit button to match your form and submit button class/id names.

<br>

Once you have updated the configuration variables, the script will automatically update the filter button and/or form action to point to your results page with the selected filter values. Simple, and extremely easy to set up.

<br>

> [!NOTE]
> Make sure the "Add selected filters to the URL?" step (#4) on the Settings page in the Jetboost Booster Editor for each Filter Booster is set to "Yes."

<br>

```html
<!-- FILTER RESULTS ON SEPARATE PAGE -->
<script>
	// CONFIG
	const resultsPagePath = '/results'

	// Update querySelector value if you're NOT using a Webflow Form element
	const filterButton = document.querySelector('#filter-query-button')

	// Update querySelector values if you ARE using a Webflow Form element
	const filterForm = document.querySelector('.filter-form')
	const filterFormSubmitButton = document.querySelector(
		'#filter-form-submit-button',
	)

	// END CONFIG

	function setDisabled(element, shouldDisable) {
		if (!element) return

		if (shouldDisable) {
			element.setAttribute('disabled', '')
			element.setAttribute('aria-disabled', 'true')
		} else {
			element.removeAttribute('disabled')
			element.setAttribute('aria-disabled', 'false')
		}
	}

	function updateFilterDestination() {
		const queryString = window.location.search
		const hasQueryString = Boolean(queryString)
		const destinationUrl = hasQueryString
			? `${resultsPagePath}${queryString}`
			: '#'

		if (filterButton) {
			filterButton.href = destinationUrl
			setDisabled(filterButton, !hasQueryString)
		}

		if (filterFormSubmitButton) {
			setDisabled(filterFormSubmitButton, !hasQueryString)
		}

		if (filterForm) {
			filterForm.setAttribute('action', destinationUrl)
		}
	}

	function watchUrlChanges(callback) {
		const originalPushState = history.pushState
		const originalReplaceState = history.replaceState

		history.pushState = function (...args) {
			const result = originalPushState.apply(this, args)
			callback()
			return result
		}

		history.replaceState = function (...args) {
			const result = originalReplaceState.apply(this, args)
			callback()
			return result
		}

		window.addEventListener('popstate', callback)
	}

	if (filterButton) {
		filterButton.href = '#'
		setDisabled(filterButton, true)

		filterButton.addEventListener('click', function (event) {
			if (filterButton.hasAttribute('disabled')) {
				event.preventDefault()
			}
		})
	}

	if (filterFormSubmitButton) {
		setDisabled(filterFormSubmitButton, true)
	}

	if (filterForm) {
		filterForm.setAttribute('action', '#')

		filterForm.addEventListener('submit', function (event) {
			event.preventDefault()
			event.stopPropagation()

			const queryString = window.location.search

			if (!queryString) return

			window.location.href = `${resultsPagePath}${queryString}`
		})
	}

	updateFilterDestination()
	watchUrlChanges(updateFilterDestination)
</script>
```
