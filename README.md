# AdobeXD-KanBan-plugin tutorial

With my 72 hour experience using Adobe XD, it was a bit intimidating to create a programming service or a plugin as it seems quite a difficult task, but it is a actually quite simple even for people with little programming experience/background can easily build an XD plugin and learn to code at the same time. Before we get into that, you have to know what a plugin is. A plugin on Adobe XD are add-on programs that provide additional functionality that assist designers in creating applications. 
     For this article, I aimed to create a mini KanBan layout, inspired by Kanban’s from GitHub and Trello. For those who are unfamiliar with the project we are creating, a KanBan is a project management and workflow method that allows you to minimize multitasking, streamline your work in progress efficiency, and improve the speed and quality of the work your collaborative and self-managing team produces. 
     
## Prerequisites:
Before beginning with the project, make sure you have Adobe XD downloaded and updated. Then follow these directions:
-	open [Adobe Developer Console website](https://console.adobe.io/home)
-	Select Create New Project
-	Click on Add Plugin for Adobe XD
-	Locate your Plugin ID (we will need it later)
-	(optional) You can download the source code for the provide if you like
Now let’s get started.

## Create your plugin folder under your “develop” folder:
* First, open Adobe XD. You can find the develop folder in the Plugins > Development > Show Develop Folder menu. This will open the develop folder, where we will save our plugin. * Create a folder under develop called “my-first-plugin”.

## Open develop folder in a Text Editor:
Have a text editor installed in your computer in order to create and edit text files in your develop folder. You can use Notepad, Visual Studio Code, Atom, etc. as you have access to edit your files. For my case, I will use Visual Studio Code. As you open the develop folder in Visual Studio Code, create these two files (if you didn’t download the starter code from earlier):
* my-first-plugin
     * main.js
     * manifest.json
     * 	assets (this is a folder called asset)
          * artboard-lib.js

The JavaScript file main.js is where the code lives, and manifest.json is the JSON file where we include information about our plugin. Please do not rename these files.
Describe the plugin:
Adobe XD requires users to describe your plugin in the JSON file. To do that, simply add a “description” tag under the file like so:
```
{
    "id" : "<YOUR ID HERE>",
    "name": "first-plugin-test",
    "version": "1.0.0",
    "description": "This is a KanBan plugin that would provide designers a cleaner and better workflow",
    "summary": "This is a KanBan plugin that would provide designers a cleaner and better workflow",
    "languages": ["en"], 
    "author": "Your Name",
    "helpUrl": "https://mywebsite.com/help",
    "host": {
      "app": "XD",
      "minVersion": "13.0"
    },
    
    "uiEntryPoints": [
        {
            "type": "panel",
            "label": "Artboard Generator",
            "panelId": "artboardGenerator"
        }
    ]
}
```
Remember the ID I asked you about earlier, replace the <YOUR ID HERE> with your plugin ID from the Adobe Console. 
     
## The Main Code for main.js:
Let’s begin by writing the main code for this plugin. And yes, this is a bit lengthy, but I’ll everything as we code.
```
const { Artboard, Color } = require("scenegraph");
const { editDocument } = require("application");
const artboardSizes = require("./assets/artboard-lib");

const addArtboard = () => {
  let btns = document.querySelectorAll("button");

  const getValues = event => {
    let activeBtn = event.currentTarget;
    let name = activeBtn.getAttribute("data-name");
    let width = activeBtn.getAttribute("data-width");
    let height = activeBtn.getAttribute("data-height");

    editDocument({ editLabel: "add artboard" }, (selection, documentRoot) => {
      let newArtboard = new Artboard();
      newArtboard.height = JSON.parse(height);
      newArtboard.width = JSON.parse(width);
      newArtboard.fill = new Color("#ffffff");
      newArtboard.name = name;
      documentRoot.addChild(newArtboard);
    });
  };

  for (let i = 0; i < btns.length; i++) {
    btns[i].addEventListener("click", getValues);
  }
};
```

Let’s break this down: 
* The first line is telling XD we are going to use the Artboard object and the Color module by requiring from the scenegraph API
* Getting a reference to the editDocument method available in the application module.
* Having the application link to the artboard-lib js file that we will work on next
From there, we start to create our Artboard where it will show a selection of options for our KanBan board. Before labeling each portion, we first create the size of the boxes (width, height) and label each box with a name and color (#ffffff – white, you can also just type down White or any other color). 

## Labeling the board in artboard-lib:
Now we must label each portion of the board like Agenda (what needs to be done), Completed (what is completed), Not Completed (what isn’t done), and Priorities (which tasks need to be prioritized). The code will look like this:
