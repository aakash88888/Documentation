# How to Implement rrweb on Our Website?

## Overview

This guide provides instructions for integrating `rrweb` into your website to enable session recording. Follow the steps below to set up `rrweb` and configure the necessary JavaScript for recording and saving session events to your backend server.

## Prerequisites

- Basic knowledge of HTML, JavaScript, and API interactions.
- A backend service configured to handle API requests for saving and deleting session data.

## Integration Steps

### 1. Include `rrweb` Script in HTML

Add the following script tag to your HTML file to include the `rrweb` library:

```html
<script src="https://cdn.jsdelivr.net/npm/rrweb@latest/dist/rrweb-all.min.js"></script>
```
#Place this script tag within the <head> section of your HTML document.

### 2. Get `rrweb_impl.js` Code

To obtain the `rrweb_impl.js` code, follow these steps:

1. **Clone the Repository**: Start by cloning the GitHub repository where `rrweb_impl.js` is located. Use the following command:

   ```bash
   git clone https://github.com/aakash88888/Cloud_Express/blob/main/frontend/js/rrweb_impl.js
   ```
2. **Navigate to the Directory** : 

   Once you have cloned the repository, navigate to the directory containing `rrweb_impl.js`. You can do this using the command line or by browsing through the repository structure. For example:

   ```bash
   cd YourRepositoryName/path/to/directory
   ```
Alternatively, you can view or download the file directly from the GitHub web interface:

1. **Obtain the File:**

   You can view or download the file directly from the GitHub web interface:

   - Navigate to the file in the GitHub repository.
   - Click on the file to view it.
   - Click the "Raw" button to download it to your local machine.

2. **Include `rrweb_impl.js` in Your Project:**

   Ensure that the `rrweb_impl.js` file is placed in your project’s directory where your HTML file can access it. You may need to update your HTML file to include this script. For example:

   ```html
   <script src="path/to/rrweb_impl.js"></script>
   ```

   Replace path/to/rrweb_impl.js with the relative path to where you placed the file in your project directory.
### 3. Backend Setup

  To set up the backend for handling rrweb data, download and configure the backend code from the following repository:
    
  [Backend Repository link](https://github.com/aakash88888/Cloud_Express/tree/main/backend)
  go to that directory and run buid command: npm install

### 4. Testing

#### Deploy the HTML File

Ensure your HTML file with the `rrweb` script is hosted on your server.

#### Verify JavaScript Integration

Check that `rrweb_impl.js` is correctly linked and loaded by examining network requests in your browser's developer tools.

#### Backend Integration

Test the integration by interacting with your website and verifying that session events are recorded and sent to the backend.

### 5. Troubleshooting

#### Events Not Being Sent

- Ensure the backend server is running and accessible at the specified `serverURL`.
- Check for network errors or issues with API endpoints.

#### Errors in Console

- Check error messages in the browser console for issues.
- Common problems include incorrect API URLs or server errors.


### JavaScript Functions in `rrweb_impl.js`

The `rrweb_impl.js` file includes several key functions and configurations for handling session recording and interacting with the backend. Here’s a breakdown of each function and its purpose:

### Recording Events with `rrweb`

**Purpose**: To capture user interactions and record them for later use.

**Functionality**:
- **`var events = [];`**: Initializes an empty array named `events` to store recorded user interactions.
- **`rrweb.record({ ... });`**: Configures `rrweb` to start recording user interactions on the page.
- **`emit(event)`**: A callback function provided to `rrweb.record` that is called every time an event is recorded.
  - **`console.log(event);`**: Logs the recorded event to the console for debugging purposes.
  - **`events.push(event);`**: Adds the recorded event to the `events` array for later processing or sending to a backend server.

**Code**:
```javascript
var events = [];

rrweb.record({
  emit(event) {
    console.log(event);
    events.push(event);
  },
});
```

### Saving Recorded Events with `saveEvents()`

**Purpose**: To periodically send recorded user interaction events to the backend server for storage or further processing.

**Functionality**:
- **`async function saveEvents()`**: An asynchronous function that handles the process of sending recorded events to the backend server.
- **`const body = JSON.stringify(events);`**: Converts the `events` array into a JSON string to be sent in the HTTP request body.
- **`events = [];`**: Clears the `events` array after converting it to JSON, ensuring that only new events are recorded in subsequent cycles.
- **`await fetch(${serverURL}/api/last-record, { ... });`**: Sends a `POST` request to `${serverURL}/api/last-record` with the events data.
  - **`method: 'POST'`**: Specifies that the request method is `POST`.
  - **`headers: { 'Content-Type': 'application/json' }`**: Sets the `Content-Type` header to `application/json`, indicating that the request body contains JSON data.
  - **`body: body`**: Includes the JSON string of events in the request body.
- **`if (response.ok) { ... }`**: Checks if the response status indicates success (status code in the range 200-299).
  - **`console.log('Event sent successfully', response);`**: Logs a success message if the events were sent successfully.
  - **`console.error('Failed to send event:', response.statusText);`**: Logs an error message if the request failed.
- **`catch (error) { ... }`**: Catches and logs any errors that occur during the `fetch` request.

**Periodic Execution**:
- **`setInterval(saveEvents, 10 * 1000);`**: Calls the `saveEvents` function every 10 seconds to periodically send recorded events to the backend server.

**Code**:
```javascript
async function saveEvents() {
  try {
    const body = JSON.stringify(events);
    events = []; // Clear events after sending

    const response = await fetch(`${serverURL}/api/last-record`, 
      {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: body,
    });

    if (response.ok) {
      console.log('Event sent successfully', response);
    } else {
      console.error('Failed to send event:', response.statusText);
    }
  } catch (error) {
    console.error('Error sending event:', error);
  }
}

// Save events periodically
setInterval(saveEvents, 10 * 1000);
```


# How to Implement RL Algorithm on Our Website?

## Overview

This document provides a step-by-step guide on implementing a Reinforcement Learning (RL) algorithm to dynamically apply styles to your website. The RL algorithm fetches style configurations from the backend server and applies them to the page elements when the page is refreshed.

## Prerequisites

1. **Backend Server**: Ensure you have a backend server that provides a `/random-style` endpoint returning JSON with style properties. For example:
   ```json
   {
     "backgroundColor": "#ffffff",
     "backgroundImage": "url('image.jpg')",
     "backgroundPosition": "center",
     "backgroundRepeat": "no-repeat",
     "backgroundSize": "cover",
     "fontSize": "16px",
     "color": "#000000",
     "fontFamily": "Arial, sans-serif",
     "fontWeight": "bold",
     "fontVariant": "small-caps",
     "lineHeight": "1.5",
     "letterSpacing": "0.5px",
     "wordSpacing": "1px",
     "textAlign": "center",
     "textDecoration": "underline",
     "textTransform": "uppercase"
   }

2. **Implementation Steps**

   #### 1. Create or Update JavaScript File

   Ensure you have a JavaScript file that will handle the fetching and applying of styles. The script should be included in your HTML file or linked externally.

   ```javascript
   window.onload = function () {
     fetchAndApplyStyles(); // Function to fetch and apply styles
   };

   async function fetchAndApplyStyles() {

    const PORT = 3001;
    // Use your backend URL
    const serverURL = 'https://cloudexpress-backend.onrender.com';

    try {
        const response = await fetch(`${serverURL}/random-style`);
        const data = await response.json();
        console.log(data);

        // Select page elements
        const elements = document.querySelectorAll('.menu__item-title');
        const body = document.querySelector('body');

        // Apply styles to body
        body.style.backgroundColor = data['backgroundColor'];
        body.style.backgroundImage = data['backgroundImage'];
        body.style.backgroundPosition = data['backgroundPosition'];
        body.style.backgroundRepeat = data['backgroundRepeat'];
        body.style.backgroundSize = data['backgroundSize'];

        // Apply styles to selected elements
        elements.forEach(element => {
            element.style.color = data['color'];
            element.style.fontFamily = data['fontFamily'];
            element.style.fontWeight = data['fontWeight'];
            element.style.fontVariant = data['fontVariant'];
            element.style.letterSpacing = data['letterSpacing'];
            element.style.wordSpacing = data['wordSpacing'];
            element.style.textAlign = data['textAlign'];
            element.style.textDecoration = data['textDecoration'];
            element.style.textTransform = data['textTransform'];
        });

    } catch (error) {
        console.error('Error fetching or applying styles:', error);
    }
   }

  #### 2. Integrate JavaScript with HTML

  Ensure the JavaScript file is included in your HTML file, preferably at the end of the `<body>` tag for better performance.

  #### 3. Testing

  - **Start Backend Server**: Ensure your backend server is running and accessible.
  - **Open Website**: Refresh the page and verify that styles are applied as expected.

  #### Troubleshooting

  - **Fetch Errors**: Ensure your backend server is running and the URL is correct.
  - **Style Application Issues**: Verify that the JSON structure matches the expected format and that CSS properties are applied correctly.

### Explanation of the RL Algorithm

- **`window.onload` Event:**
  - Triggered when the entire page has fully loaded. It calls the `fetchAndApplyStyles()` function to fetch style data from the backend and apply it to the webpage.

- **`fetchAndApplyStyles` Function:**
  - An asynchronous function that:
  
  - **Sets Up the Server URL:**
    - Defines the `serverURL` pointing to the backend (`https://cloudexpress-backend.onrender.com`) that provides style properties as a JSON object.

  - **Fetches Data from the Server:**
    - Sends a GET request to the `/random-style` endpoint. The response, containing CSS properties, is parsed into a `data` object.

  - **Applies Styles to Webpage Elements:**
    - Selects elements like `.menu__item-title`, `a` tags, and `body`. Styles from `data` are applied, such as background properties to the body and various text styles to the `.menu__item-title` elements. Some 
      properties like `fontSize`, `height`, and `width` are optional and can be uncommented if needed.








