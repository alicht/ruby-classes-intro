-   It may sound strange, but forms are possibly the most complicated thing about learning web development. Not necessarily because the code itself is difficult, but because you usually want to build forms that accomplish so many different things at once.

### How?
Now think about a web form to buy an airline ticket. You probably need to enter your name, address, phone number, email, the airline, the flight number, the flight date, your credit card number, your credit card security number, your card expiration date, your card’s zipcode, and a bunch of checkboxes for additional things like trip insurance. That’s a whole lot of different models embedded in one form! But you still submit it with a single button. Holy macaroni!

# we have models installed here- we're dealing with that tomorrow in ActiveRecord

-  how a regular form is created in HTML
-  how Ruby helps us make new forms
-  We’ll cover the way data is structured and sent to the controller

```html
<form action="/somepath" method="post">
    <input type="text">
    <!-- other inputs here -->
    <input type="submit" value="Submit This Form">
  </form>
```

## How can we view what our form submits?
look through the output that gets printed into your console when you run your $ rails server

![screen shot 2018-02-08 at 2 34 52 am](https://user-images.githubusercontent.com/6153182/35960708-bf608f8c-0c78-11e8-88b3-d28026a9a37b.png)

-  The first line tells us which HTTP method was used and which route the form went to. 
-  The second line tells us which controller and action the form will be handled by. 
-  The third line contains everything that will get stuffed into the params hash for the controller to use. 

## Forms in Rails
First thing you'll realize is... that it wont work
-  You’ll either get an error or your user session will get zeroed out
A:  That’s because Rails by default automatically protects you from cross-site request forgery and it requires you to verify that the form was actually submitted from a page you generated. In order to do so, it generates an “authenticity token” which looks like gibberish but helps Rails match the form with your session and the application.

### Rails form helpers
Rails tries to make your life as easy as it can, so naturally it provides you with helper methods that automate some of the repetitive parts of creating forms.


