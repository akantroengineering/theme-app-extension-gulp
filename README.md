# theme-app-extension-gulp

**Problem**
```shopify extension serve``` command is not supported for Shopify Theme App Extensions. As a solution, I created a simple Gulp task that automates pushing theme extension file updates automatically. With this approach, you will not have to CD into the ```theme-app-extension``` directory, nor will you need to run the ```shopify extension push``` command each time you make a change to your files. 

**Dependancies**
1) ```node```, ```npm```, and ```npx```
2) Add ```gulp```. If Gulp is not already installed follow the quick start instructions at https://gulpjs.com/docs/en/getting-started/quick-start to install Gulp in your project.
3) Add ```gulp-run``` by running command ```npm i gulp-run``` (https://www.npmjs.com/package/gulp-run) 

**Install**
1) Add ```gulpfile.js``` to root of your project.
2) Add ```"extension": "gulp"``` && ```"push:extension": "(cd ./theme-app-extension; shopify extension push)"``` commands to ```package.json``` file scripts.

   **example:**
   ```
   "scripts": {
    "extension": "gulp",
    "push:extension": "(cd ./theme-app-extension; shopify extension push)"
   }
   ```
   
3) Update your files pathes in the ```watcher();``` function. 
   By default, we watch for ```theme-app-extension/assets/*```, ```theme-app-extension/blocks/*'``` and ```theme-app-extension/snippets/*```. You may add or change these as direcper your requirements.
   
   **example:**
   ```
   const watcher = async (cb) => {
    watch('theme-app-extension/assets/*', push);
    watch('theme-app-extension/blocks/*', push);
    watch('theme-app-extension/snippets/*', push);
    cb();
   }
   ```

**Usage**
1) Open a new terminal in your project root and run ```npm run push:extension``` _(Note: You stay in the main app project root, you do not need to ```cd``` into ```theme-app-extension```)_
2) Upon save the script will now push theme extension updates to Shopify accordingly. 

If you are already are using Gulp, skip install step #1 and download & include the script into your existing ```gulpfile.js```. Then continue from steps #2 onward.

```
// Imports
const { series, watch } = require('gulp');
const run = require('gulp-run');

const watcher = async (cb) => {
  watch('theme-app-extension/assets/*', push);
  watch('theme-app-extension/blocks/*', push);
  watch('theme-app-extension/snippets/*', push);
  cb();
}

const push = async () => {
  const pushExtension = new run.Command('npm run push:extension');
  pushExtension.exec();
}

// Exports
exports.default = series(watcher);
```
