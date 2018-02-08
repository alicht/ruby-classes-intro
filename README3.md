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



