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

## Labeling the board in artboard-lib.js:
Now we must label each portion of the board like Agenda (what needs to be done), Completed (what is completed), Not Completed (what isn’t done), and Priorities (which tasks need to be prioritized). The code (under artboard-lib.js) will look like this:
```
module.exports = [
    {
      name: "Agenda",
      img: './assets/agenda.png',
      height: 300,
      width: 500,
    },
    {
      name: "Completed",
      img: './assets/completed.png',
      height: 300,
      width: 500,
    },
    {
      name: "Not Completed",
      img: './assets/n-completed.jpg',
      height: 300,
      width: 500,
    },
    {
      name: "Priorities",
      img: './assets/next-iteration.jpg',
      height: 300,
      width: 500
    }
  ]
```

Notice that everything under each tag are the variable names under main.js. Make sure to have the same height and width for each label for the KanBan and feel free to include your own image. To check that it works:
* Select menu (the three dash line)
* Select Plugins
* Then Development > Reload Plugins
* Do this every time you make any changes to the JavaScript or JSON files.
The image will appear as so:
![image](https://user-images.githubusercontent.com/55200206/100030194-cdc59b00-2da7-11eb-8af2-c66a193fde1b.png)

## Finishing main.js
To conclude everything and have the labels appear on the artboard, include the following information under your main.js:
```
const { Artboard, Color } = require("scenegraph");
const { editDocument } = require("application");
const artboardSizes = require("./assets/artboard-lib");
let panel;

const addArtboard = () => {
  let btns = document.querySelectorAll("button");

  const getValues = event => {
    let activeBtn = event.currentTarget;
    let name = activeBtn.getAttribute("data-name");
    let width = activeBtn.getAttribute("data-width");
    let height = activeBtn.getAttribute("data-height");
    let text = activeBtn.getAttribute("data-text");

    editDocument({ editLabel: "add artboard" }, (selection, documentRoot) => {
      let newArtboard = new Artboard();
      newArtboard.height = JSON.parse(height);
      newArtboard.width = JSON.parse(width);
      newArtboard.fill = new Color("#ffffff");
      newArtboard.name = name;
      newArtboard.text = JSON.parse(text);
      documentRoot.addChild(newArtboard);
    });
  };

  for (let i = 0; i < btns.length; i++) {
    btns[i].addEventListener("click", getValues);
  }
};

const panelMarkup = () => {
  let btn = "";
  for (let i = 0; i < artboardSizes.length; i++) {
    let { name, img, height, width, text } = artboardSizes[i];

    btn += `
      <style>
        .icon {
          max-width: 50px;
          width: 100%;
          margin: auto;
        }
        .icon img {
          width: 100%;
        }
        .btn-el {
          text-align: center;
          margin-bottom: 30px;
        }
      </style>
      <div class="btn-el">
        <div class="icon">
          <img src="${img}">
        </div>
        <p>${name}</p>
        <button
        data-name="${name}"
        data-height="${height}"
        data-width="${width}"
        data-text="${width}"
        >
        Add Artboard
        </button>
      </div>
    
    `;
  }

  let html = `
    <div>
      <div>
        <h1 style="margin-bottom: 30px;">Select Artboards</h1>
      </div>
      <div class="btn-group">
        ${btn}
      </div>
    </div>
  `;
  panel = document.createElement("div");
  panel.innerHTML = html;
  return panel;
};

const show = async event => {
  if (!panel) {
    await event.node.appendChild(panelMarkup());
    await addArtboard();
  }
};

const hide = event => console.log("plugin is hiding");

module.exports = {
  panels: {
    artboardGenerator: {
      show,
      hide
    }
  }
};
```

The panel variable shows everything that appears on the left (from the screenshot earlier) in a HTML (Hyper-Text Markup Language) format while the panelMarkup allows us to drag each item across to the artboard as shown below.

## Run the Plugin: 
(Again Reload Plugins as mentioned above)

![image](https://user-images.githubusercontent.com/55200206/100030288-02d1ed80-2da8-11eb-87db-1d66ea45dd5d.png)

And that is how you create a plugin in Adobe XD. I know this isn’t flashy or absolutely perfect, but we were able to successfully create a mini KanBan without having to drag and drop multiple rectangles in place. The completed sample code will be located in my repository

