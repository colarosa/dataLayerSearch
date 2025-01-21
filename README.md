# DataLayer Property Search Script

This JavaScript function enables users to search for a specific property within the `dataLayer` object, a common use case in analytics and tagging setups. The script is recursive, making it capable of traversing deeply nested objects and arrays to locate the desired property.

---

## Features

- Searches for a specific property in the `dataLayer`.
- Handles deeply nested objects and arrays using recursion.
- Flexible configuration for specifying the property to search.
- Can be used in Google Tag Manager (GTM) as a **Custom JavaScript Variable** to dynamically return the value of the specified property.

---

## How to Use in Google Tag Manager (GTM)

1. **Create a Custom JavaScript Variable in GTM:**
   - Navigate to **Variables** in GTM.
   - Click **New** and select **Custom JavaScript**.
   - Paste the script into the editor.

2. **Customize the `searchProperty` Variable:**
   - Update the `searchProperty` variable in the script to the name of the property you want to search for. For example:
     ```javascript
     var searchProperty = 'value'; // Change this to 'items', 'id', etc.
     ```

3. **Usage in Tags and Triggers:**
   - Use the Custom JavaScript Variable in your tags to dynamically retrieve the value of the specified property from the `dataLayer`.

4. **Output:**
   - The variable will return the value of the specified property if found, or `null` if the property is not present in the `dataLayer`.

---

## Code Example

```javascript
function() {
  // User can change this variable to the property they want to search for
  var searchProperty = 'value'; // Change this to 'items', 'id', etc.

  // Recursive function to search for a specific property within the dataLayer
  function searchValue(obj) {
    if (obj.hasOwnProperty(searchProperty)) {
      return obj[searchProperty]; // Desired property found, return its value
    }

    // If the desired property is not found, check if the object has nested arrays and objects
    for (var prop in obj) {
      if (obj.hasOwnProperty(prop) && typeof obj[prop] === 'object' && obj[prop] !== null) {
        var result = searchValue(obj[prop]); // Recursive call to search inner objects/arrays
        if (result !== null) {
          return result; // Return the desired property value if found in nested object/array
        }
      }
    }

    // Return null if the desired property is not found in the current object
    return null;
  }

  var dataLayer = window.dataLayer || []; // Assuming dataLayer is a global variable

  // Start the search from the top-level objects in dataLayer
  for (var i = 0; i < dataLayer.length; i++) {
    var value = searchValue(dataLayer[i]);
    if (value !== null) {
      return value; // Return the desired property value if found
    }
  }

  // Return null if the desired property is not found in any object in the dataLayer
  return null;
}

// Example usage:
// Change the value of searchProperty variable to 'items', 'id', etc. to search for different properties.
