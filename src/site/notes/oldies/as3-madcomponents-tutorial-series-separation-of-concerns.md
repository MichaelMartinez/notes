---
dg-show-inline-title: false
dg-publish: true
dg-home: false
dg-show-toc: false
dg-created: 09/04/2011
dg-updated: 
dg-show-tags: false
dg-link-preview: false
---

Title: AS3 MadComponents Tutorial Series - Separation of Concerns
Date: 2011-09-04 16:42
Author: Michael
Category: AS3
Tag: Flash, MadComponents, UI
Slug: as3-madcomponents-tutorial-series-separation-of-concerns
Status: published

Best practices for programming tell us separation of concerns,
encapsulation and modularity are desirable in most, but not all,
application contexts. A complex app with many views and/or items could
quickly get out of control in a plethora of ways within
the monolithic class context.

The project I'm working on has nearly 50 separate views and consist of
mostly textual data. So my first task was learning the method(s) to
separate class files the
[MadComponent](http://code.google.com/p/mad-components/)way. This
example will be quite simple but will illustrate how one would go about
using different AS class files to separate concerns.

<span style="text-decoration: underline;">**User Story:**</span> A list
based component is chosen by the user. This item navigates to a
"details" page or another component with more data.

**Lets build the "landing page" or main application page;**

First the XML layout:

    :::actionscript3
    public class SeparateClassExample extends Sprite{
            
            protected static const DATA:XML = 
                
                
                ;
            
            protected static const LIST:XML = 
                
        {DATA};
            
            //Important
            protected static const NAVIGATION:XML = 
                
                {LIST}
                {Page0.LAYOUT}
                {Page1.LAYOUT}
                ;
            
            protected var _uiNavigation:UINavigation;
    ```

The DATA constant simply defines the items that will populate the list.
The LIST constant accepts the DATA constant and applies some basic
formatting, Ie. colour (note the UK spelling). We also implement the
search feature here which will filter the list based on the item's
label. (I have experimented with different search methods, but have been
unsuccessful thus far... we will explore this later).

The NAVIGATION constant is actually the heart and soul of this simple
app. You can think of the NAVIGATION constant as an Array of data that
you define in XML and classes. As my astute readers will probably note
based on a quick glance. We haven't defined Page0, Page1 and you can't
spot LAYOUT anywhere. These are defined separate class files, although
not completely necessary, it helps me keep things organized.

We declare the _uiNavigation instance or class level variable which
we'll define in subsequent steps.

**Lets move on to the constructor:**

    :::actionscript3"}
    public function SeparateClassExample(screen:Sprite = null) {
                
                if (screen){
                    screen.addChild(this);
                }
                // support autoOrients
                stage.align = StageAlign.TOP_LEFT;
                stage.scaleMode = StageScaleMode.NO_SCALE;
                
                // Create the main UI or "Landing Page"
                UI.create(this, NAVIGATION);
                
                // Initialize "views"
                Page0.initialize();
                Page1.initialize();
                
                // Navigation layout and behaviour 
                _uiNavigation = UINavigation(UI.findViewById("nav"));
                _uiNavigation.autoForward = false;
                
                // Must set to false otherwise the app scrolls through all the pages in the "stack"
                _uiNavigation.autoBack = false;
                
                // Go to the "page" requested
                _uiNavigation.addEventListener(UIList.CLICKED, navigationChange);
                
                // Go Back using the Nav bar back button
                _uiNavigation.navigationBar.backButton.addEventListener(MouseEvent.MOUSE_UP, goBack);
                
                // Go back using the hardware back button
                _uiNavigation.addEventListener(KeyboardEvent.KEY_UP, goBackButton);
                _uiNavigation.navigationBar.backButton.colour = 999999;
            }
    ```

The UI.create method instantiates the UI or "landing page" and all of
the code relating to the use of MadComponents must be implemented after
this method call. We initialize Page0 with a call to a public method
inside the Page0 class. This method can be named anything you like.

We then instantiate the uiNavigation variable to the NAVIGATION
constant defined at the class level through the id we can name
arbitrarily. We can now programmatically add formatting, assign event
listeners and so forth to the uiNavigation variable.

The first event listener UI.CLICKED will direct the navigation to the
"page" that is assigned to the index of the NAVIGATION constant. In this
case 0 is the list itself, while 1 and 2 will be Page0 and Page1
respectively.

The second event listener is assigned to the navigationBar.backButton
and is a simple MouseEvent.MOUSE_UP event. As you will see, this event
will handle the back button that is added to the navigation bar once we
have navigated away from the main list.

The third event listener is added to handle the device back button,
generally this is an Android device. I found through testing that if
autoBack is set to false, you lose back button functionality. In some
cases you may want the autoBack functionality, but its not appropriate
in this context as it will scroll through the stack of items defined in
the NAVIGATION constant. This event listener simply adds the devices
back button into the app.

Daniel is working on documentation as we speak, but a good starting
place to learn about the framework are the PDF(s) included on the
[download page](http://code.google.com/p/mad-components/downloads/list).
You can see what I've done to customize the uiNavigation variable
based on the comments added into the code.

**Now lets handle the events we declared in the constructor:**

    :::actionscript3
    public function navigationChange(event:Event):void {
                
                var navIndex:int = _uiNavigation.index;
                // Check to see if current page is @ 0 to set correct title
                if(_uiNavigation.pages[0]){
                    _uiNavigation.title = "Home";
                }
                
                // navigation Stuff
                if(navIndex == 0){
                    _uiNavigation.goToPage(1, UIPages.SLIDE_LEFT);
                    _uiNavigation.title = "Page 0";
                } else if (navIndex == 1){
                    _uiNavigation.goToPage(2, UIPages.SLIDE_LEFT);
                    _uiNavigation.title = "Page 1";
                }
            }
            
            // For the back button in the Navigation bar
            protected function goBack(event:Event):void {
                
                _uiNavigation.goToPage(0, UIPages.SLIDE_RIGHT);
                _uiNavigation.title = "Home";
            }
            
            // With autoBack set to false, you lose the device back button for some reason
            // You can use the Native App library to check for a device back button event
            protected function goBackButton(event:KeyboardEvent):void{

                if(event.keyCode == Keyboard.BACK){
                    _uiNavigation.goToPage(0, UIPages.SLIDE_RIGHT);
                    _uiNavigation.title = "Home";
                }
            }       
        }
    }
    ```

In this example, I want the navigationChange event to take the user to
the page associated with the index they select. We let the gotoPage
method do all the heavy lifting and add a nice visual transition. Beyond
that the back button in the navigation bar and the device back button
(if available) simply takes the user back to the list. If you've
followed any of the tutorials mentioned in the starting out post, you
will not see anything surprising here. The exception is maybe the event
handler for the back button which is similar to the way one handles an
event on the decktop or web based flash application.

A little gotcha I found is when you don't explicitly set the title, the
navigation component will leave the previous title in the navigation bar
despite the change. Checking to make sure its set correctly just adds a
bit of polish.

**Now lets add the separate class "views" or Page0 and Page1:**

    :::actionscript3
    package
    {
        import com.danielfreeman.madcomponents.*;
        
        import flash.display.Sprite;
        import flash.display.StageAlign;
        import flash.display.StageScaleMode;
        import flash.events.Event;
        import flash.system.Capabilities;
        import flash.text.TextField;
        
        public class Page0 extends Sprite
        {
            
            public static const LAYOUT:XML = 
                                
                    ;
            
            protected static var _message:UILabel;
            
            public function Patient()
            {
                super();
                
            }
            
            public static function initialize():void{
                _message = UILabel(UI.findViewById("message0"));
                _message.htmlText = "App Considerations:Is it AWESOME?Are you going to BroGram? Sup?More Stuff";
            }
        }
    }
    ```

This is a basic page layout using HTML text. LAYOUT is the constant we
call from within the NAVIGATION constant in the Main class as you've
seen. If I may direct your attention to the constructor, notice that
UI.create is not used here. In this case, we "instantiate" the layout
through the public initialize method mentioned earlier. Also note, ID's
for the components for each page need to be different. Ie. Page0 label
has an id of "message0". Page1 will need a label id of "message1" to
work as one would expect. Flash requires string literals to be on the
same line. In my working app this line is almost ridiculously long, no
need to worry as the text will render as you've defined.

This is the "MAIN" class in all its glory.

    :::actionscript3
    package
    {
        import com.danielfreeman.madcomponents.*;
        
        import flash.desktop.NativeApplication;
        import flash.display.Sprite;
        import flash.display.StageAlign;
        import flash.display.StageScaleMode;
        import flash.events.Event;
        import flash.events.KeyboardEvent;
        import flash.events.MouseEvent;
        import flash.ui.Keyboard;
        
        public class SeparateClassExample extends Sprite{
            
            protected static const DATA:XML = 
                
                
                ;
            
            protected static const LIST:XML = 
                
        {DATA};
            
            // Each additional "Page" needs to have different id's 
            //for the components, even when they are separated into separate classes
            protected static const NAVIGATION:XML = 
                
                {LIST}
                {Page0.LAYOUT}
                {Page1.LAYOUT}
                ;
            
            protected var _uiNavigation:UINavigation;
            
            public function SeparateClassExample(screen:Sprite = null) {
                
                if (screen){
                    screen.addChild(this);
                }
                // support autoOrients
                stage.align = StageAlign.TOP_LEFT;
                stage.scaleMode = StageScaleMode.NO_SCALE;
                
                // Create the main UI or "Landing Page"
                UI.create(this, NAVIGATION);
                
                // Initialize "views"
                Page0.initialize();
                Page1.initialize();
                
                // Navigation layout and behaviour 
                _uiNavigation = UINavigation(UI.findViewById("nav"));
                _uiNavigation.autoForward = false;
                
                // Must set to false otherwise the app scrolls through all the pages in the "stack"
                _uiNavigation.autoBack = false;
                
                // Go to the "page" requested
                _uiNavigation.addEventListener(UIList.CLICKED, navigationChange);
                
                // Go Back using the Nav bar back button
                _uiNavigation.navigationBar.backButton.addEventListener(MouseEvent.MOUSE_UP, goBack);
                
                // Go back using the hardware back button
                _uiNavigation.addEventListener(KeyboardEvent.KEY_UP, goBackButton);
                _uiNavigation.navigationBar.backButton.colour = 999999;
            }
            
            public function navigationChange(event:Event):void {
                
                var navIndex:int = _uiNavigation.index;
                // Check to see if current page is @ 0 to set correct title
                if(_uiNavigation.pages[0]){
                    _uiNavigation.title = "Home";
                }
                
                // navigation Stuff
                if(navIndex == 0){
                    _uiNavigation.goToPage(1, UIPages.SLIDE_LEFT);
                    _uiNavigation.title = "Page 0";
                } else if (navIndex == 1){
                    _uiNavigation.goToPage(2, UIPages.SLIDE_LEFT);
                    _uiNavigation.title = "Page 1";
                }
            }
            
            // For the back button in the Navigation bar
            protected function goBack(event:Event):void {
                
                _uiNavigation.goToPage(0, UIPages.SLIDE_RIGHT);
                _uiNavigation.title = "Home";
            }
            
            // With autoBack set to false, you lose the device back button for some reason
            // You can use the Native App library to check for a device back button event
            protected function goBackButton(event:KeyboardEvent):void{

                if(event.keyCode == Keyboard.BACK){
                    _uiNavigation.goToPage(0, UIPages.SLIDE_RIGHT);
                    _uiNavigation.title = "Home";
                }
            }       
        }
    }
    }
    ```
