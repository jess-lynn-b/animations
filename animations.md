## Angular Animations
***  Before you get started :
    need to install the new animations package:
      npm install-- save @angular/animations

    then add the BrowserAnimationsModule to the imports[] in the AppModule

    Make sure to imprt it from the @angular/platform-browser/animations...

    Next you need to import: trigger, state, style from the @angular/animations instead of @angular/core

 # Introduction:
    This takes normal CSS and elivates it to a higher level in order to make the websites and applications more interactive and appealing. Adding things to the DOM that was not there before makes it hard to handle with CSS transition style. In the example that Max shows he has two buttons made and then below them he has an empty div... this is where he is going to add the animations. 

# Animations Triggers and State
    We first start by setting this up in the app component.ts and adding the animations to the component section. Animations are wrote in TS code and they each have a trigger. Example:

      animations : [ 
        trigger('divState' , [

        ])
       ]

    Then in the app.comp.html add this to the <div>
      <div [@divState]="state">

    Then create the component for "state" in side the app.comp.ts in the export class... 
      state = 'normal'; 

    Animations work by being able to transition from state 1 to state 2... you do not have to label the animation 'state' however in this example it makes sense bc we are talking about two different states of animations. Dont forget to import 'state' and add it to the componets...
        animations: [
          trigger('divState' ,[
            state('normal', style({
              'background-color' : 'red'
              transform: 'translateX(0)'
            })),
            state('highlighted', style ({
              'background-color' : 'blue'
              transform: 'translateX(100px)'
            })),
          ])
        ]

    In the html dont forget to add styles to the div... 
        <div
          style = "width: 100px ; height: 100px"
          [@divState]="state >

    We don't have an animation yet but this is the set up to get us started to...

## Switching between States
    With the button in the html labeled "onAnimate"().. we need to create the function for it in the .ts file. So under the export class section create that function: 
      onAnimate () {
        this.state. == 'normal' ? this.state = 'highlighted' : this.state = 'noraml' ;
      }
  
    This is a good set up and it should allow you to toggle between the two buttons switching the colors of the squares...

## Transitions
    Now that we can hard switch between the two colors lets animate it...
    in the component under the state lets add...
      transition('noraml => highlighted', animate(300)),
      transition('highlighted => normal', animate(800)),

    Save this and lets try it out.. The square of color now moves slowly from side to side while changing color and even blends the two colors in the middle.

    This is the first method using the Angular framework to make this animation happen. 

## Advanced Transitions
    Use the same code as above but instead of duplicating the code then changing the directions and the timing.. you can simply use the code once like this...
      transition('noraml <=> highlighted', animate(300))

    Now we can add more complexiticty with the animation and how it travels or it functions. So in this we will... copy the entire code from above in the .ts file from trigger to transition... then in the second code change the name from 'divState' to 'wildState'. 

    Move on to the HTML file and add a new <div> under the first one to look like this:

      <div
      style="width: 100px; height: 100px"
      [@wildState]="wildState">

    This will bind it to the new animation trigger that will allow it to function, also need to add the new property of 'wildState' as well. So first we need to add the new property to the appcomponent under the  
            state = 'normal;
            wildState = 'normal';
      
    Keeping the 'normal' style and the 'wildStyle' in the .ts file lets add one more state under the 'wild' one but above the final transition one...
        state('shrunken', style ({
          'background-color': 'green',
          transform: 'translateX(0) scale(0.5)'
        })),
    Add scale (1) to the other states above to ensure that this one will shrink down to notice during the animation. 

    Now lets my the "onShrink()" button from the html file work. In doing this we need to add the function of onShrink to the .ts file under onAnimate()...

        onAnimate(){
          this.state == 'normal' ? this.state = 'highlighted' : this.state = 'normal';

          this.wildState == 'normal' ? this.wildState = 'highlighted' : this.wildState = 'normal';
        }

        onShrink(){
          this.wildState = 'shrunken';
        }

    In order to see the functions and animations work better you might need to add a line <br> between the <divs> to add some sepreation. Now to animatate them and you can see them move at different speeds and change colors as well. The only issue is that shrinking does not animate and only happens when the button is clicked, lets change that. 

    So to do this we need to add a third transition in the .ts file. This will look like:

        transition('shrunken <=> *', animate (500) ),

    When the animate button is clicked it will work as expected. When you are unsure of what you want to do or where you want to go with the animations the 'wildcard' is bennifical. 

## Transition Phases
    With the animation fuctions we can not only just pass the timing, we can control the whole animation and define styles. The animation should take during the animation. In doing this we need to pass a second argument here which is the style again and define the style during the animation of the transition of the .ts file. We will pass the challenge crypt object for that in-between style and be taken during the installation. Example:

      transition('shrunken <=> *', [
        style({
          'background-color' : 'orange'
        }),
        animate(1000, style ({
          borderRadius: '50px'
        })),
        animate(500)
      ])
    If you activate the shrink button you should see the squares shrink into a circle, change color and then end as a smaller square as green. The same is true if you push the animate button as well. So with all of this it gives you a more complex animation where a chain of multiple styles are used. 

    This may look complicated and difficult but it isn't. This is simply an array passed to the transition that allows us to define different phases in that transition starting phase. The starting phase is if no animate method is used then it is instantly applied. If we do add animate we do apply the style but over some time and then if it calls for animate without a time constraint then it is just a transition to the end state. 

    The most important thing to remember is to always end the transition with animate(500) or what ever timing you need to get you there smoothly, otherwise it will snap back at the end- thats not visually appealing. 

## The "void" State

    So above we were able to animate the <div>s but what if we want to animate the addition of an item to a list. 

    Start by adding a new trigger and do this by copying the first trigger. Just the first one that was ('normal','highlighted' and the 'transition part').

    This trigger we will label and set up like this:

      trigger('list1' , [
        state('in'), style({
          opacity: 1,
          transform: 'translateX(0)
        })),
        transition('void <=> *', animate(300)),
      ]),

    Void can be used in cases where you do have an element in an end state which wasn't added to the dom at the beginning. Then add * or any to the second part bc it doesnt exactly matter what the initial state the item is in.

    Go to the HTML file and on the <li list item add a trigger as before in square brackets...
      [@list1=""]

    When we try to execute the code and test it out.. it does not work we need to add a little more information to the transition above:

        transition('void <=> *', [
          style({
            opacity: 0,
            transform: 'translateX(-100)
          }),
          animate(300)),
        ]),
    Adding a style into the code and array this was will instantly get this style by Angular2 and then transiton over this duration to its final style here....

    Now try to add new items and watch how smoothly the new items are added. However it won't work for deleting though... lets fix this. 

    Deleting the tasks that were added... we need to add a new transition. So copy the above and make it look like this:

        transition('* <=> *', [
          animate(300, style({
            transform: 'translateX(100px),
            opacity: 0
          }))),
        ]),
    
    We need to navigate or animate from any(*) state to anystate but using void as this specific end spot. No need to define a style for the starting state as we want it to start with what it already has. However the animate function will need a fianal style state added. 

    Once we try this out and we add an element and then click on that one to remove it... it is removed smoothly and to the right. This can also work with *ngIf...

## Using Keyframes for Animations

    This is a more detailed control over the individual steps to create this fuction at 100 seconds and then a different at 500 seconds and then a final at 800seconds. So lets set this up and in the HTML file on the second list lets add ... under the (click)="onDelete(item). 

      [@list2]  as a trigger to bind the second set of animations to this. So in the .ts file lets copy the trigger section again to munlipulate for the 'list2'

          trigger('list2' ,[
            state('in'), style({
              opacity: 1,
              transform: 'translateX(0)
          })),
          transition('void <=> *', animate(300)),
        ]),

      Now we need to change the style above to not have a normal array as we did before. We have these different equally weighted phases instead lets pass an argument which is still an array but they're in the animate function. example:

        transition('void <=> *', [
          animate(1000, keyframes([
            style({
              transform: 'translateX(-100px)',
              opacity: 0,
              offset: 0
            }),
            style({
              transform: 'translateX(-50px)',
              opacity: 0.5,
              offset: 0.3
            }),
            style({
              transform: 'translateX(-20px)',
              opacity: 1,
              offset: 0.8
            }),
            style({
              transform: 'translateX(0px)',
              opacity: 1,
              offset: 1
            })
          ])),
        ]),

      The keyframes allow you to be more specific as to what part of the multiple styles we can now set up should occure as to which timing the overall animation will take 1 second but we can control what state in sight of that animation should take and how long. Need to add the offset in order to keep it loading in with the animation smoothly and preventing it from snaping. 

## Grouping Transitions
    Animating an item out if we want to have two different animation that take a different amount of time. Two different animations are easy and we need to add a second animate function under the one above... Example:
      transition('* => void', [
        animate(300, style({
          transform: 'translateX(100px)';
          opacity: 0
        })),
         animate(300, style({
          color: 'red'
        })),
      ])

    Adding the second animate to this will change the text color to red. So if we keep the code above the color will not change once clicked bc it is the second of the animate list... put it above the other to make it react first. However if you want it to both work a the same time but yet with different timings we need to set it up like this... using the group method..

         transition('* => void', [
          group([
            animate(300, style({
          transform: 'translateX(100px)';
          opacity: 0
        })),
         animate(800, style({
          color: 'red'
        })),
          ])
        
      ])

    The group method is an argurment that we pass an array of animation that we want to perform synchronously. This is the perfect way to apply more than one animation to a group of items to create a perfect elimate effect as above. Click on an item to remove it truns red and slowly moves to the right. 

## Using Animation Callbacks

    To use a callback is for when an animation finishes and you want to execute some other codes. So in the app comp example would be on the <div> element we can listen to the callback.. using event binding and the @ sign to indicate that we are in the animation world. 
      <div
        style="width: 100px; height: 100px"
        [@divState]="state"
        (@divState.start)="animationStarted($event)"
        (@divState.done)="animationEnded($event)"
      > 

    Adding the callback with the .start trigger means that this method will be executed when the animation starts. Then adding the second method with .done will execute more code that you need for the application. 

    Then we need to go to the .ts file and set up the two methods so that the animations started that will recieve information and then animations ended that will hold it as well. Example:

        animationStarted(event) {
        }

        animationEnded(event){
        }

    

