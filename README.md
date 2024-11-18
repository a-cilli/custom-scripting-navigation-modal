# `createModal` JavaScript Function

This repository provides a custom JavaScript function, `createModal`, which creates a modal for displaying miniature pages and by pressing them it navigates the Flipbook to the selected page. The modal can be triggered by a button and can be closed with another button. The modal uses the iPaperJS API to trigger the navigation elements, and it adjusts its styles based on the screen size to ensure a responsive design.

## Function Overview

The `createModal` function initializes a modal that:
- Displays a button (open) at a specified location on the screen (either left or right).
- Shows a modal window when the button is clicked, displaying pages or images from a source URL.
- Includes a header with customizable text and a close button.
- Adjusts its layout and styling depending on the device's screen size (mobile vs. desktop).

### Parameters
- **`text`** (string): The text to display in the header of the modal. If the text exceeds **21 characters**, it will be truncated to **18 characters** and ellipsized (`...`) to fit within the design.
- **`location`** (string): The location of the open button. Can be `'left'` or `'right'`. This determines where the button appears on the screen.

### Function Breakdown

The function performs the following steps:

1. **Initialization**:
   - It starts by creating the `iPaperJsApi` instance.
   - Logs the current URL and initializes modal components (open button, close button, and modal div).
   
2. **Modal Styles**:
   - Applies different styles based on the screen size. For mobile devices, the modal takes up the full screen, while on desktop, it has a fixed width and height.
   
3. **Button Creation**:
   - A floating button is created to trigger the modal. The button is placed either on the left or right side of the screen based on the `location` parameter.
   
4. **Modal Content**:
   - The modal displays images or pages (from a URL) with overlay effects. The images are displayed in a grid layout, and the user can navigate between them by clicking on the page number.
   
5. **Responsive Design**:
   - The modal adapts to different screen sizes (mobile and desktop) by adjusting the layout, text styles, and other visual aspects.
   
6. **Close Functionality**:
   - The modal can be closed by clicking the close button (the "X" icon) in the header or navigating to a page pressing the page miniature.

## Example Usage

To use this function in your project, call it with the desired parameters:

```javascript
createModal('Click here to view', 'right');
```

This will create a modal with the header text "Click here to view" and position the open button on the right side of the screen.

## Customization

### Change Button Location
The modal open button can be positioned on the **left** or **right** side of the screen. This is controlled by the `location` parameter passed to the function.

```javascript
createModal('Example Modal', 'left'); // Button will appear on the left side
```

## Mobile vs Desktop Styles
The modal will adjust its styling depending on the device:

- **Mobile**: Full-screen modal with a background color and no borders.
- **Desktop**: Fixed modal with a maximum height and width.

This ensures that the modal is usable across different screen sizes.

## Text Truncation
If the `text` parameter exceeds **21 characters**, the text will be truncated to **18 characters** with an ellipsis (`...`) appended to fit within the design.

```javascript
createModal('This is a long text that will be truncated', 'right');
// This will display "This is a long tex..." in the modal header.
```

## License
This code is provided as-is. Feel free to use and modify the function as needed, but remember that it is **not officially supported by iPaper** and may not work in future versions of their platform. Be sure to test thoroughly if you're planning to integrate this into a production environment.

## Important Notes

### Not Officially Supported by iPaper
This solution is **not officially supported by iPaper** and may stop working at any time. iPaper does not guarantee that this function will continue to work in future versions of their platform, and updates to iPaper may cause the modal functionality to break.

### Inspiration for Your Own Modal
This code is meant as an **inspiration point** for creating your own modal. While it offers a starting point, you may need to modify or extend it based on your specific requirements. If you're looking for more advanced functionality (e.g., custom transitions, animations, etc.), consider using additional libraries or frameworks for modals, such as **Bootstrap** or custom JavaScript solutions.

##How to use

There are two ways to use the modal:

###Import the Code

To use the createModal function in your project, you can include the following code:
```JavaScript
<script src="https://a-cilli.github.io/custom-scripting-navigation-modal/modal.js"></script>
<script>
  createModal("Hello World", "right");
</script>
```
* The first <script> tag imports the JavaScript file from the specified URL. This file contains the definition of the createModal function.
* The second <script> tag calls the createModal function. In this example, it creates a modal with the text "Hello World" positioned on the right side.


###Use the code

The following snippet includes the complete code to create the modal in the Flipbook. Keep in mind that this code must be wrapped in a <script> tag, and the function needs to be called with the appropriate parameters afterwards.


<details>
<summary>Click to see the code</summary>

```JavaScript
function createModal(text, location) {
  const sanitizedLocation = location.toLowerCase(); // Ensure lowercase for comparison
  
  // Initialize iPaperJsApi and show information when ready
  (function () {
    const instance = iPaperJsApi(3); // Your existing code to initialize iPaperJsApi
    console.log(instance);

    // Log the current URL
    const currentUrl = window.location.href.split('?')[0];
    console.log("Current URL:", currentUrl);

    // Create variables for modal elements
    let imageDiv, openButton, closeButton;

    // Function to apply styles based on screen size and orientation
    function applyModalStyles(imageDiv) {
      const isMobile = window.matchMedia("(max-width: 992px)").matches;

      // Base styles for modal
      imageDiv.style.position = 'fixed';
      imageDiv.style.zIndex = '1000';
      imageDiv.style.top = '0';
      imageDiv.style.left = '0';
      imageDiv.style.flexDirection = 'column';  // Ensures flex column direction

      if (isMobile) {
        // Mobile: height 100% and max height 100%
        imageDiv.style.backgroundColor = '#fcfcfc';
        imageDiv.style.borderRadius = '0';
        imageDiv.style.border = 'none';
        imageDiv.style.width = '100%';
        imageDiv.style.height = '100%';
        imageDiv.style.maxHeight = '100%';
        imageDiv.style.transform = 'none';
      } else {
        // Desktop: height auto and max height calc(100% - 90px)
        imageDiv.style.backgroundColor = '#fcfcfc';
        imageDiv.style.borderRadius = '4px';
        imageDiv.style.border = '1px solid #c7c4c4';
        imageDiv.style.width = '350px';
        imageDiv.style.height = 'auto';
        imageDiv.style.maxHeight = 'calc(100% - 90px)';
        
        // Set position based on location
        if (sanitizedLocation === 'left') {
          imageDiv.style.transform = 'translate(20px, calc(100vh - 100% - 20px))';
        } else {
          imageDiv.style.transform = 'translate(calc(100vw - 370px), calc(100vh - 100% - 20px))';
        }
      }
    }

    function applyHeaderStyles() {
      const isMobile = window.matchMedia("(max-width: 992px)").matches;
      const headerContainer = document.querySelector(".headerContainer");
      const header = document.querySelector(".header");
      const closeIcon = document.querySelector(".closeIcon");

      if (headerContainer !== null && header !== null && closeIcon !== null) {
        if (isMobile) {
          headerContainer.style.backgroundColor = '#555';
          header.style.marginLeft = '20px';
          header.style.color = 'white';
          header.style.textAlign = 'center';
          closeIcon.style.filter = 'brightness(0) saturate(100%) invert(91%) sepia(91%) saturate(29%) hue-rotate(250deg) brightness(107%) contrast(100%)';
        } else {
          headerContainer.style.backgroundColor = 'transparent';
          header.style.marginLeft = '0px';
          header.style.color = '#4a4a4a';
          header.style.textAlign = 'left';
          closeIcon.style.filter = 'brightness(0) saturate(100%) invert(26%) sepia(0%) saturate(3068%) hue-rotate(136deg) brightness(92%) contrast(82%)';
        }
      }
    }

    instance.paging.getState((result) => {
      const totalPages = result.totalPages;
      const startsWithSpread = result.spreads[0].length === 2;

      // Truncate the text based on its length
      let displayText = text;
      if (!text) {
        displayText = ''; // If text is empty, show empty string
      } else if (text.length > 21) {
        displayText = text.substring(0, 18) + '...'; // Truncate and add ellipsis if longer than 21
      }
      
      // Create open button
      openButton = document.createElement('button');
      openButton.style.position = 'fixed';
      openButton.style[ sanitizedLocation === 'left' ? 'left' : 'right' ] = '8px';
      openButton.style.bottom = '100px';
      openButton.style.zIndex = '1000';
      openButton.style.width = '44px';
      openButton.style.height = '44px';
      openButton.style.borderRadius = '50%';
      openButton.style.backgroundColor = '#303940';
      openButton.style.border = 'none';
      openButton.style.display = 'flex';
      openButton.style.alignItems = 'center';
      openButton.style.justifyContent = 'center';
      openButton.style.cursor = 'pointer';

      const openIcon = document.createElement('img');
      openIcon.src = 'https://viewer.ipaper.io/ViewFile1937930.svg';
      openIcon.alt = 'Open';
      openIcon.style.width = '24px';
      openIcon.style.height = '24px';
      openIcon.style.filter = 'brightness(0) saturate(100%) invert(100%)';
      openButton.appendChild(openIcon);
      document.body.appendChild(openButton);

      imageDiv = document.createElement('div');
      imageDiv.style.position = 'fixed';
      imageDiv.style.display = 'none';
      imageDiv.style.zIndex = '1000';
      applyModalStyles(imageDiv);
      document.body.appendChild(imageDiv);

      const headerContainer = document.createElement('div');
      headerContainer.classList.add("headerContainer");
      headerContainer.style.display = 'flex';
      headerContainer.style.justifyContent = 'space-between';
      headerContainer.style.alignItems = 'center';
      headerContainer.style.padding = '10px';
      headerContainer.style.borderBottom = '1px solid #c7c4c4';
      imageDiv.appendChild(headerContainer);

      const header = document.createElement('h2');
      header.textContent = displayText; // Use the truncated text
      header.classList.add("header");
      header.style.margin = '0';
      header.style.fontFamily = 'Verdana, Arial, Sans-Serif';
      header.style.fontWeight = '300';
      header.style.fontSize = '16px';
      header.style.color = 'black';
      header.style.width = 'calc(100% - 40px)';
      headerContainer.appendChild(header);

      closeButton = document.createElement('button');
      closeButton.style.border = 'none';
      closeButton.style.background = 'none';
      closeButton.style.cursor = 'pointer';
      closeButton.style.padding = '0';
      closeButton.style.transition = 'filter 0.3s ease-in';

      const closeIcon = document.createElement('img');
      closeIcon.src = 'https://viewer.ipaper.io/ViewFile1937923.svg';
      closeIcon.alt = 'Close';
      closeIcon.classList.add("closeIcon");
      closeIcon.style.width = '20px';
      closeIcon.style.height = '20px';

      closeButton.appendChild(closeIcon);
      headerContainer.appendChild(closeButton);

      const contentArea = document.createElement('div');
      contentArea.style.overflowY = 'scroll';
      contentArea.style.height = '100%'; // Body div now has height 100%
      contentArea.style.display = 'grid';
      contentArea.style.gridTemplateColumns = 'repeat(2, 1fr)';
      contentArea.style.rowGap = '10px';
      contentArea.style.padding = '10px';
      contentArea.style.boxSizing = 'border-box';
      imageDiv.appendChild(contentArea);

      if (!startsWithSpread) {
        const emptyCell = document.createElement('div');
        emptyCell.style.height = '50px';
        contentArea.appendChild(emptyCell);
      }

      for (let pageNum = 1; pageNum <= totalPages; pageNum++) {
        const btn = document.createElement('button');
        btn.style.width = '100%';
        btn.style.height = '100%';
        btn.style.padding = '0';
        btn.style.border = 'none';
        btn.style.background = 'transparent';
        btn.style.cursor = 'pointer';
        btn.style.position = 'relative'; // To position the overlay within the button
        
        btn.onclick = () => {
          instance.paging.goToPage(pageNum);
          closeModal();
        };

        // Create the image container (this div will hold the image and the overlay)
        const imgContainer = document.createElement('div');
        imgContainer.style.position = 'relative';
        imgContainer.style.width = '100%';
        imgContainer.style.height = 'auto'; // Let the height adjust based on the image's aspect ratio
        
        // Image element
        const img = document.createElement('img');
        img.src = `${currentUrl}Image.ashx?PageNumber=${pageNum}&ImageType=Normal`;
        img.style.width = '100%';
        img.style.height = 'auto'; // Maintain the aspect ratio
        
        // Overlay div (hidden by default)
        const overlay = document.createElement('div');
        overlay.style.position = 'absolute';
        overlay.style.top = '0';
        overlay.style.left = '0';
        overlay.style.width = '100%';
        overlay.style.height = '100%';
        overlay.style.backgroundColor = 'rgba(0, 0, 0, 0.4)';
        overlay.style.opacity = '0'; // Hidden by default
        overlay.style.transition = 'opacity 0.3s ease'; // Smooth transition for hover effect
        
        // Show the overlay when the button is hovered
        btn.addEventListener('mouseenter', () => {
          overlay.style.opacity = '1'; // Show the overlay on hover
        });
        btn.addEventListener('mouseleave', () => {
          overlay.style.opacity = '0'; // Hide the overlay when hover ends
        });
        
        imgContainer.appendChild(overlay);
        imgContainer.appendChild(img);
        btn.appendChild(imgContainer);
      
        // Page number container below the image
        const pageNumberDiv = document.createElement('div');
        pageNumberDiv.style.width = '100%';
        pageNumberDiv.style.height = '24px';
        pageNumberDiv.style.display = 'flex';
        pageNumberDiv.style.justifyContent = 'center';
        pageNumberDiv.style.alignItems = 'center';
      
        const pageNumberText = document.createElement('p');
        pageNumberText.textContent = pageNum;
        pageNumberText.style.fontSize = '12px';
        pageNumberText.style.margin = '0';
        pageNumberText.style.color = '#000';
        
        pageNumberDiv.appendChild(pageNumberText);
        btn.appendChild(pageNumberDiv);
      
        contentArea.appendChild(btn);
      }
      
      openButton.onclick = () => {
        openButton.style.display = 'none';
        imageDiv.style.display = 'flex';
      };

      closeButton.onclick = () => {
        closeModal();
      };

      function closeModal() {
        imageDiv.style.display = 'none';
        openButton.style.display = 'block';
      }

      applyModalStyles(imageDiv);
      applyHeaderStyles();

      window.addEventListener('resize', () => {
        applyModalStyles(imageDiv);
        applyHeaderStyles();
      });
    });
  })();
}



```

</details> 
