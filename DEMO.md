
## Adding "Copy" (to Clipboard) Button

If you would like to add a button to your fenced code blocks so that users may copy the code, you can do so in Docusaurus. You will have to add some code to your Docusaurus project, as seen below.

Under `static/js`, create a file called `code-block-buttons.js` with the following:

```js
// Turn off ESLint for this file because it's sent down to users as-is.
/* eslint-disable */
window.addEventListener('load', function() {
  function button(label, ariaLabel, icon, className) {
    const btn = document.createElement('button');
    btn.classList.add('btnIcon', className);
    btn.setAttribute('type', 'button');
    btn.setAttribute('aria-label', ariaLabel);
    btn.innerHTML =
      '<div class="btnIcon__body">' +
      icon +
      '<strong class="btnIcon__label">' +
      label +
      '</strong>' +
      '</div>';
    return btn;
  }

  function addButtons(codeBlockSelector, btn) {
    document.querySelectorAll(codeBlockSelector).forEach(function(code) {
      code.parentNode.appendChild(btn.cloneNode(true));
    });
  }

  const copyIcon =
    '<svg width="12" height="12" viewBox="340 364 14 15" xmlns="http://www.w3.org/2000/svg"><path fill="currentColor" d="M342 375.974h4v.998h-4v-.998zm5-5.987h-5v.998h5v-.998zm2 2.994v-1.995l-3 2.993 3 2.994v-1.996h5v-1.995h-5zm-4.5-.997H342v.998h2.5v-.997zm-2.5 2.993h2.5v-.998H342v.998zm9 .998h1v1.996c-.016.28-.11.514-.297.702-.187.187-.422.28-.703.296h-10c-.547 0-1-.452-1-.998v-10.976c0-.546.453-.998 1-.998h3c0-1.107.89-1.996 2-1.996 1.11 0 2 .89 2 1.996h3c.547 0 1 .452 1 .998v4.99h-1v-2.995h-10v8.98h10v-1.996zm-9-7.983h8c0-.544-.453-.996-1-.996h-1c-.547 0-1-.453-1-.998 0-.546-.453-.998-1-.998-.547 0-1 .452-1 .998 0 .545-.453.998-1 .998h-1c-.547 0-1 .452-1 .997z" fill-rule="evenodd"/></svg>';

  addButtons(
    '.hljs',
    button('Copy', 'Copy code to clipboard', copyIcon, 'btnClipboard'),
  );

  const clipboard = new ClipboardJS('.btnClipboard', {
    target: function(trigger) {
      return trigger.parentNode.querySelector('code');
    },
  });

  clipboard.on('success', function(event) {
    event.clearSelection();
    const textEl = event.trigger.querySelector('.btnIcon__label');
    textEl.textContent = 'Copied';
    setTimeout(function() {
      textEl.textContent = 'Copy';
    }, 2000);
  });
});
```

Under `static/css`, create a file called `code-block-buttons.css` and add the following:


```css
/* "Copy" code block button */
pre {
  position: relative;
}

pre .btnIcon {
  position: absolute;
  top: 4px;
  z-index: 2;
  cursor: pointer;
  border: 1px solid transparent;
  padding: 0;
  color: #fff;
  background-color: transparent;
  height: 30px;
  transition: all .25s ease-out;
}

pre .btnIcon:hover {
  text-decoration: none;
}

.btnIcon__body {
  align-items: center;
  display: flex;
}

.btnIcon svg {
  fill: currentColor;
  margin-right: .4em;
}

.btnIcon__label {
  font-size: 11px;
}

.btnClipboard {
  right: 10px;
}
```

Add the following to `siteConfig.js`:

```js
scripts: [
  'https://buttons.github.io/buttons.js',
  'https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/2.0.0/clipboard.min.js',
  '/js/code-block-buttons.js',
],
stylesheets: ['/css/code-block-buttons.css']
```

Your "Copy" to clipboard buttons should now appear at the upper right of every fenced code block.



### Mount The Partitions :

+ Mount The Root Partition ***/dev/sda3*** To ***/mnt***.

```bash
mount /dev/sda3 /mnt
```

+ Create A ***/boot/EFI*** Directory For Boot Partition.

```bash no-copy
mkdir -p /mnt/boot/EFI  
```

Mount The Boot Partition ***/dev/sda1/*** To ***/mnt/boot/EFI*** Partition.

```bash
mount /dev/sda1 /mnt/boot/EFI
```

Create a ***/home*** mountpoint:

```
mkdir /mnt/home  
```