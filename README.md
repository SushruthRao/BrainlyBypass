# Brainly Bypass

Welcome to **Brainly Bypass**! 🚀

This is a Violentmonkey script designed to help you bypass the ad-wall on [Brainly.in](https://brainly.in). With this script, you can access answers without the hassle of watching ads. Enjoy a smoother and faster experience on Brainly!

## Features

- **Ad-Wall Bypass**: Skip the ads and access answers directly.
- **User-Friendly**: Simple setup with Violentmonkey.
- **Automatic Execution**: Works seamlessly in the background.
![block]000806.jpg
![bypass]000809.jpg

## Installation

1. **Install Violentmonkey Extension**:
   - [Violentmonkey for Chrome](https://chrome.google.com/webstore/detail/violentmonkey/dfhijheggngnibiblgkcejddjnmjemnl)
   - [Violentmonkey for Firefox](https://addons.mozilla.org/en-US/firefox/addon/violentmonkey/)
   - [Violentmonkey for Opera](https://addons.opera.com/en/extensions/details/violentmonkey/)

2. **Add the Script**:
   - Open Violentmonkey's dashboard.
   - Click on the **+** icon to create a new script.
   - Copy and paste the code below into the script editor.
   - Save the script.

```javascript
// ==UserScript==
// @name         Brainly Bypass
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Bypasses the ad-wall of brainly to view the answer for free !
// @author       github.com/SushruthRao
// @match        *://brainly.in/*
// @grant        GM_notification
// @grant        none
// ==/UserScript==

(function() {
    'use strict';


    let hasCopied = false;

    // function to make the UI for the display answer once its copied by the copy function
    function createOverlayBox(content) {

        const overlay = document.createElement('div');
       ;
        overlay.style.position = 'fixed';
        overlay.style.top = '0';
        overlay.style.left = '0';
        overlay.style.width = '100%';
        overlay.style.height = '100%';
        overlay.style.backgroundColor = 'rgba(0, 0, 0, 0.5)'; // Semi-transparent background
        overlay.style.zIndex = '9999'; // Make sure it's on top
        overlay.style.display = 'flex';
        overlay.style.alignItems = 'center';
        overlay.style.justifyContent = 'center';
        overlay.style.overflow = 'auto'; // Allow scrolling if content is too large


        const contentBox = document.createElement('div');

        contentBox.style.backgroundColor = '#fff'; // White background for the content box
        contentBox.style.padding = '20px';
        contentBox.style.borderRadius = '8px';
        contentBox.style.boxShadow = '0 4px 8px rgba(0, 0, 0, 0.1)';
        contentBox.style.maxWidth = '80%';
        contentBox.style.maxHeight = '80%';
        contentBox.style.overflow = 'auto';
        contentBox.style.position = 'relative';




        const closeButton = document.createElement('button');
        closeButton.innerText = 'Close';
        closeButton.style.position = 'absolute';
        closeButton.style.top = '10px';
        closeButton.style.right = '10px';
        closeButton.style.backgroundColor = '#000000';
        closeButton.style.color = '#ffffff';
        closeButton.style.border = 'none';
        closeButton.style.borderRadius = '4px';
        closeButton.style.padding = '10px 20px';
        closeButton.style.cursor = 'pointer';


        closeButton.addEventListener('click', () => {
            document.body.removeChild(overlay);
        });


        contentBox.innerHTML = `
        <p>
            <a href="https://github.com/SushruthRao"
               style="font-weight:bold; color: #1a73e8; text-decoration: none;">
               Made by github.com/SushruthRao
            </a>
        </p>` + content;


        contentBox.appendChild(closeButton);
        overlay.appendChild(contentBox);


        document.body.appendChild(overlay);
    }


    function copyAndDisplayDiv() {
        if (hasCopied) return; // get out of func if alrdy copied

        // this is the div that is loaded in first before the ad-wall adds a block to it
        var targetDiv = document.querySelector('div.brn-qpage-next-answer-box__content.js-answer-content-section');

        if (targetDiv) {

            var clonedDiv = targetDiv.cloneNode(true);


            var content = clonedDiv.innerHTML;


            createOverlayBox(content);


            hasCopied = true;
        }
    }

    // run on page load
    window.addEventListener('load', copyAndDisplayDiv);


    var observer = new MutationObserver(function(mutations) {
        mutations.forEach(function(mutation) {

            mutation.addedNodes.forEach(function(node) {
                if (node.nodeType === 1 && node.matches('div.brn-qpage-next-answer-box__content.js-answer-content-section')) {
                    copyAndDisplayDiv();
                }
            });
        });
    });

    // see if any changes in body
    observer.observe(document.body, { childList: true, subtree: true });
})();
