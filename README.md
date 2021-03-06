# RVCalendarWeekView
Simple but powerful Calendar Week View for iOS With dragable events, infinte scroll and pinchable hours size


Following the work from [MSCollectionView](https://github.com/erichoracek/MSCollectionViewCalendarLayout)

I created this library simplifing its usage and adding some interesting features

### Installation

`pod RVCalendarWeekView`

or just copy the files inside the `lib` folder by now


### Usage
you can now use storyboard to create a simple UIView extending the MSWeekView and then just do this:


```
    -(void)viewDidLoad{
        MSEvent* event1 = [MSEvent make:NSDate.now
        title:@"Title"
        location:@"Central perk"];

        MSEvent* event2 = [MSEvent make:[NSDate.now addMinutes:10]  //AddMinutes comes from EasyDate pod
        duration:60*3
        title:@"Title 2"
        location:@"Central perk"];

        _weekView.events = @[event1,event2];        
    }
```

Easy right?

### Features
To add features to the WeekView I'm using a decorator pattern, this way we can extend the `weekView` with diferent features without the need of multiple inhertance and to have a expressive modular design
However, this adds the need to have a `strong` reference to the `decorator` that will hold the features.

So we can add features to the `weekView` with the following code:

```
    self.decoratedWeekView = [MSWeekViewDecoratorFactory make:self.weekView
                                                     features:(MSDragableEventFeature|MSNewEventFeature|MSInfiniteFeature|MSChangeDurationFeature)
                                                  andDelegate:self];
```

This is the fast way where the delegate should have all the methods for each feature delegate (see below).

The long way is something more like the standard `decorator` pattern in case you need more flexibility.

```
    MSWeekView* decoratedView = baseView;
    decoratedView = [MSWeekViewDecoratorInfinite makeWith:decoratedView andDelegate:infiniteDelegate];
    decoratedView = [MSWeekViewDecoratorNewEvent makeWith:decoratedView andDelegate:newEventDelegate];
    decoratedView = [MSWeekViewDecoratorDragable makeWith:decoratedView andDelegate:dragableDelegate];
    decoratedView = [MSWeekViewDecoratorChangeDuration makeWith:decoratedView andDelegate:durationDelegate];

```

There is a function to easily set the minutes precision to all decorators in case you need something diferent than the default 5 minutes.

```
    [MSWeekViewDecoratorFactory setMinutesPrecisionToAllDecorators:decoratedView minutesPrecision:15];
```

#### Drag and drop
You can get the feature to change the event duration with `MSDragableEventFeature`

It will fire the following functions on your `dragDelegate`

``` 
    -(BOOL)weekView:(MSWeekView*)weekView canMoveEvent:(MSEvent*)event to:(NSDate*)date;

    -(void)weekView:(MSWeekView*)weekView event:(MSEvent*)event moved:(NSDate*)date;

```

#### Change duration
You can get the feature to change the event duration with `MSChangeDurationFeature`

```
    -(BOOL)weekView:(MSWeekView*)weekView canChangeDuration:(MSEvent*)event startDate:(NSDate*)startDate endDate:(NSDate*)endDate;

    -(void)weekView:(MSWeekView*)weekView event:(MSEvent*)event durationChanged:(NSDate*)startDate endDate:(NSDate*)endDate;
```

#### Create new event on long press
it will fire the following functions on your `createEventDelegate`

```
    -(void)weekView:(MSWeekView*)weekView onLongPressAt:(NSDate*)date
```

#### Infinite scroll
It will fire the following functions on your `infiniteDelegate`

```
    -(BOOL)weekView:(MSWeekView*)weekView newDaysLoaded:(NSDate*)startDate to:(NSDate*)endDate;
```

#### Unavailable Hours
The standard `weekView` comes with an optional delegate function to display unavailable hours in gray (customizable class of course)

just do something like this:


```
//This one is optional
    -(NSArray*)weekView:(id)sender unavailableHoursPeriods:(NSDate*)date{
        if(!unavailableHours){
            unavailableHours = @[
                [MSHourPerdiod make:@"00:00" end:@"09:00"],
                [MSHourPerdiod make:@"18:30" end:@"21:00"],
            ];
        }
        return unavailableHours;
    }
```


#### Pinchable 
**This doesn't work really well yet**  
You just need to add the  `MSPinchableFeature` in the `[MSWeekViewDecoratorFactory  make:...]`


#### Options
You can even customize some options (they all have defaults values so you just need to modify them if you want to work differently)

```
_weekView.weekFlowLayout.show24Hours    = YES; //Show All hours or just the min to cover all events
_weekView.weekFlowLayout.hourHeight     = 50;  //Define the hour height
_weekView.daysToShowOnScreen            = 7;   //How many days visible at the same time
_weekView.daysToShow                    = 31;  //How many days to display (Ininite scroll feature pending)
_weekView.weekFlowLayout.hourGridDivisionValue	= MSHourGridDivision_15_Minutes; // Show hour division lines (at lower alpha) each X minutes, by default its NONE so they are not shown.
```

![full demo](https://github.com/BadChoice/RVCalendarWeekView/blob/master/readme_images/full_demo.gif?raw=true)   

![iPhone](https://github.com/BadChoice/RVCalendarWeekView/blob/master/readme_images/iphone.png?raw=true)      

This is a complex example on how you can customize it.   
- In this one we are displaying just one day, with each employee as worker.   
- Unavailable hours class with pattern image background.   
- Custom Event Cell.
- Custom header section view   
- Custom header background

![complex](https://github.com/BadChoice/RVCalendarWeekView/blob/master/readme_images/complex.png?raw=true)


### Contributors
· Jordi Puigdellívol - https://github.com/badchoice   
· Eric Horacek - https://github.com/erichoracek      
· Kyle Fleming - https://github.com/kylefleming   

