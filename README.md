# nanoScroller.js
[nanoScroller.js](https://github.com/jamesflorentino/nanoScrollerJS) is a jQuery plugin that offers a simplistic way of implementing Mac OS X Lion-styled scrollbars for your website.
It uses minimal HTML markup being `.nano > .content`. The other scrollbar div elements `.pane > .slider` are added during run time to prevent clutter in templating. The latest version utilizes native scrolling and works with the iPad, iPhone, and some Android Tablets.

Please visit the [downloads](https://github.com/jamesflorentino/nanoScrollerJS/downloads) section to get the js and css template file (.zip).

To start using, you to do three basic things:

### 1. Markup

The following type of markup structure is needed to make the plugin work:

    <div id="about" class="nano">
     <div class="content"> ... content here ...  </div> 
    </div>

Copy the HTML markup. Change `.nano` into something related to your content. Though you can also remove that attribute as long as you have a parent div with an ID reference. e.g. `#parent > .nano`. `nano` and `content` classnames can be customized via plugin options (_in that case you must rename them inside the plugin's CSS file as well_).

### 2. CSS

Link to the `nanoscroller.css` file inside your page's `<head>` section (...or copy the contents from it to your page's main stylesheet file).

    <link rel="stylesheet" href="nanoscroller.css"> 

You should specify a width and a height to your container, and apply some custom styling for your scrollbar. Here's an example:

    .nano { background: #bba; width: 500px; height: 500px; }
    .nano .content { padding: 10px; }
    .nano .pane   { background: #888; }
    .nano .slider { background: #111; }

### 3. JavaScript

Running this script will apply the nanoScroller plugin to all DOM elements with a `.nano` className.

    $(".nano").nanoScroller();


### Advanced methods

### scroll

To scroll at the top:

    $(".nano").nanoScroller({ scroll: 'top' });

To scroll at the bottom:

    $(".nano").nanoScroller({ scroll: 'bottom' });

To scroll at the top with an offset value:

    $(".nano").nanoScroller({ scrollTop: value });

To scroll at the bottom with an offset value:

    $(".nano").nanoScroller({ scrollBottom: value });

To scroll to an element:

    $(".nano").nanoScroller({ scrollTo: $('#a_node') });

#### stop:

To stop the operation. This option will tell the plugin to disable all event bindings and hide the gadget scrollbar from the UI.

    $(".nano").nanoScroller({ stop: true });

#### flash:

To flash the scrollbar gadget for an amount of time defined in plugin settings (_defaults to 1,5s_). Useful if you want to show the user (e.g. on pageload) that there is more content waiting for him.

    $(".nano").nanoScroller({ flash: true });

#### nanoScroller();    

Refresh the scrollbar. This simply re-calculates the position and height of the scrollbar gadget.

    $(".nano").nanoScroller();

### Custom events

#### 'scrollend'

A custom `'scrollend'` event is triggered on the element every time the user has scrolled to the end of the content element.

    $(".nano").bind("scrollend", function(e){
      console.log("current HTMLDivElement", e.currentTarget);
    });

Some browsers trigger this event more than once each time, so to listen to the custom event, instead of using jQuery's normal `.bind` or `.on`, you most likely want to use [this tiny jQuery debounce plugin](https://github.com/diaspora/jquery-debounce) to prevent browsers from firing your function more than once every 100ms.

    $(".nano").debounce("scrollend", function() {
      alert("The end");
    }, 100);

#### 'scrolltop'

Same as the `'scrollend'` event, but it is triggered every time the user has scrolled to the top of the content element.

### Plugin Options

There are a few options that you can change when running nanoScroller, e.g. `$(".nano").nanoScroller({ paneClass: 'myclass' });`

#### iOSNativeScrolling

Set to true if you want to use native scrolling in iOS 5+. This will disable your custom nanoScroller scrollbar in iOS 5+ and use the native one instead. While the native iOS scrollbar usually works much better, [there could possibly be bugs](http://github.com/scottjehl/Device-Bugs/issues) in certain situations.

__Default:__ false

    $(".nano").nanoScroller({ iOSNativeScrolling: true });

#### sliderMinHeight

Sets the minimum height of the slider element.

__Default:__ 20

    $(".nano").nanoScroller({ sliderMinHeight: 40 });

#### sliderMaxHeight

Sets the maximum height of the slider element.

__Default:__ null

    $(".nano").nanoScroller({ sliderMaxHeight: 200 });

#### preventPageScrolling

Set to true to prevent page scrolling when top or bottom inside the content div is reached.

__Default:__ false

    $(".nano").nanoScroller({ preventPageScrolling: true });

#### disableResize

Set to true to disable the resize from nanoscroller. Useful if you want total control of the resize event. If you set this option to true remember to call the reset method so that the scroll don't have strange behavior.

__Default:__ false

    $(".nano").nanoScroller({ disableResize: true });

#### alwaysVisible

Set to true to stop the scrollbar from auto-hiding itself.

__Default:__ false

    $(".nano").nanoScroller({ alwaysVisible: true });

#### flashDelay:

Use this setting to specify the scrollbar hide delay in milliseconds if you have enabled the `flash` option.

    $(".nano").nanoScroller({ flashDelay: 1000 });

__Default:__ 1500

#### paneClass

A classname for scrollbar track element. If you change this setting, you also have to change it in the plugin's CSS file.

__Default:__ 'pane'

    $(".nano").nanoScroller({ paneClass: 'scrollPane' });

#### sliderClass

A classname for scrollbar thumb element. If you change this setting, you also have to change it in the plugin's CSS file.

__Default:__ 'slider'

    $(".nano").nanoScroller({ sliderClass: 'scrollSlider' });

#### contentClass

A classname for your content div. If you change this setting, you also have to change it in the plugin's CSS file.

__Default:__ 'content'

    $(".nano").nanoScroller({ contentClass: 'sliderContent' });

## How it works

![Fig 1.](https://github.com/jamesflorentino/nanoScrollerJS/raw/master/fig1.png)

The plugin works by creating a scrollbar gadget (with pre-defined css for styling) and then subscribing the `.content`'s scroll events to it. Mouse press and drag events are also subscribed to the `.pane` and `.pane > .slider` to emulate the native scrollbar's mechanism. The system scrollbars are hidden from the viewport (Fig 1). By doing this, we still retain the natural scrolling experience provided by the OS.

We are still working on doing a horizontal scrolling feature. If you're interested in contributing to the project, you are free to fork it and create a pull request.

### Development

To build nanoScroller from source you need the following libraries installed:

* Node.js and npm: [homepage / download](http://nodejs.org/)
* Coffeescript: [homepage](http://coffeescript.org/) | `npm install -g coffee-script`

To allow the build process to convert the README file to HTML you also need:

* Ruby (pre-installed in OS X): [homepage](http://www.ruby-lang.org/en/) | [download](http://www.ruby-lang.org/en/downloads/)
* GCC: [homepage](http://gcc.gnu.org/) | [download for OS X](https://github.com/kennethreitz/osx-gcc-installer)
* Redcarpet: [homepage](https://github.com/tanoku/redcarpet) | `gem install redcarpet` (add `sudo` if needed)

#### How to build & contribute

1. Make all JS changes in Coffeescript file(s), CSS changes in CSS file(s).
2. In terminal move to nanoscroller folder and run `cake build`
3. If you want to check how much the plugin size has changed compared to last build, run `cake size` *
4. Make sure that all changes are valid and open a pull request.

\* Uses `sizer.js` from [jQuery project](https://github.com/jquery/jquery/pull/511)

### Browser compatibility

__Tested desktop browsers:__

* IE7+
* Firefox 3+
* Chrome
* Safari 4+
* Opera 11.60+

__Mobile support:__

* iOS 5+ (iPhone, iPad and iPod Touch)
* iOS 4 (_with a polyfill_)
* Android Firefox
* Android 2.2/2.3 native browser (_with a polyfill_)
* Android Opera 11.6 (_with a polyfill_)
* If you see it's broken on other tablets and mobile devices, please file a ticket in the git repo. Along with model name, and OS of the device.

If you find a bug, please report here at the [issues section](https://github.com/jamesflorentino/nanoScrollerJS/issues).

### Using a polyfill for better mobile browser support

You can use [overthrow.js](https://github.com/filamentgroup/Overthrow/) polyfill (~1.5kb minified and gzipped) to make nanoScroller work on many mobile devices. It emulates CSS overflow (overflow: auto;/overflow: scroll;) in devices that are lacking it.

To use overthrow, link to the javascript file in your HTML document...

    <script src="overthrow.js"></script>


...and add an `overthrow` class to your `content` div. 

    <div id="about" class="nano">
     <div class="overthrow content"> ... content here ...  </div> 
    </div>

## Sites that use nanoScroller

If you wish to include your site in this list, you can do so by tweeting the link on Twitter with a `#nanoScroller` hashtag.

- [makechat.com](http://makechat.com)

### Contributors

- [jamesflorentino](https://github.com/jamesflorentino)
- [kristerkari](https://github.com/kristerkari)

Other people who have contributed code:

- [livskiy](https://github.com/livskiy) #83
- [iStefo](https://github.com/iStefo) #65
- [tahajahangir](https://github.com/tahajahangir) #60
- [n0valyfe](https://github.com/n0valyfe) #57
- [johanbaath](https://github.com/johanbaath) #42
- [marcelombc](https://github.com/marcelombc) #40, #46
- [zacstewart](https://github.com/zacstewart) #30
- [michael-lefebvre](https://github.com/michael-lefebvre) #22, #29
- [AlicanC](https://github.com/AlicanC) #28
- [camerond](https://github.com/camerond) #26
- [jesstelford](https://github.com/jesstelford) #23
- [lluchs](https://github.com/lluchs) #7, #8
- [Dlom](https://github.com/Dlom)

### Credits
- Initially written by [James Florentino](http://jamesflorentino.com) in [CoffeeScript](http://coffeescript.org)
- Released under [MIT License](http://www.opensource.org/licenses/mit-license.php)
