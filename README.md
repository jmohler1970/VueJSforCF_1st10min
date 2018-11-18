# VueJSforCF_1st10min
VueJS for ColdFusion Developers: First 10 minutes

# What is it?

It is a Javascript technology that does all kinds of front-end DOM manipulation. It takes dull HTML and turns it into an application. If you are familiar with jQuery or AngularJS or Angular you are kind of in the right neighborhood.

VueJS describes itself as

> Vue is a progressive framework for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. On the other hand, Vue is also perfectly capable of powering sophisticated Single-Page Applications when used in combination with modern tooling and supporting libraries.



## It is not a jQuery clone.

jQuery attaches itself to an entire web page. They you can select the parts you like and make them smarter. What you are really doing is adding and removing elements from the DOM to add interactivity. Here are three jQuery concepts, I want to you to keep in mind:

- You can bring in and remove data via AJAX
- You can make individual HTML tags smarter by using `data-` attributes.
- `$( document ).ready(function() {
	console.log( "ready!" );
});`

is nice, but we could certainly imagine something much more sophisticated. A checklist of thing would be useful
- jQuery lacks data modeling and processing that is not a part of the DOM


## Consider AngularJS

When I am talking about AngularJS, I am talking about the first version which could be brought in via a CDN.

AngularJS v 1.0 is near the end of long term support. This is not something with a long term future. You need to migrate to Angular.

## Consider Angular

This is what it is like to develop in Angular.

Step zero: Get Node.js and NPM up and running
Step one: Get Angular CLI working
Step two: Get a workspace working

And on and on and on

There is a lot of work to be done before you can even get started. When you build with Angular, you are going to be using its build process and your application is going to have to use it from end do end. There is no incremental transition.

## So that leaves us with VueJS

- Do you like the idea of just bringing in a CDN and making your dumb pages smart a la jQuery. We can do that

- Do you like the idea of Node.js and NPM, and Webpack? We can do that.

- Do you what run the pages with Server Side Rendering? We can do that.

- Do you want a web based GUI to start, stop, build and debug your applications? Something vaguely similar to ColdFusion Admin tools? We can do that.

- Do you want something that works with many different approaches? VueJS may be what you are looking for.

## So now
So now that I mentioned a bunch of different ways that VueJS could be used, I am going to show the most painless approach. The CDN approach.

# Basic page

This is a `.html` file, but if you want to mix and match, `.cfm` will work too.

## Starting with the head section

I am bringing in VueJS, Vuex, and Vue Router.

VueJS is your core system. You have to have it in order to do any kind of view stuff.

VueJS has very tight variable scoping. Vuex allows for variable to be shared across routes much more easily. By haveing Vuex, you can have variables that are similar to `session.` variables. They are similar in that they exist for a long time can be used in separate parts of the application

Vue Router allows you to have Routes. What does that mean? It is the goal of VueJS to have as much of your web application delivered up front. Look up Single Page Application or SPA for more details. The Router is the part of a VueJS application that will split up you application into different sections. In ColdFusion we think in terms of a collection of files being an application. In VueJS, we think of router as sending us to different parts of the application.

This demo site is so small, I will not be using the Router. We will only be doing a minimal with Vuex.

## Let's take a look at the app section

jQuery likes to tinker with the entire web page. VueJS likes to stay in its own area. Line 18 defines where the app starts. It ends on 39. Within 18 to 39 VueJS can do its thing. Outside of that, it does not manipulate anything

On Line 19: We can see something being output. `{{ count }}` is a lot like `#count#`. `count` does not have to be a simple variable. It can be one of a lot of different things. We will have to look later to see what exactly it is.

21 and 22 hav buttons. These buttons are not in a form, nor do they have to be. Note the `@click`. `@click` means find a VueJS method that matches this and do that operation. You might think this is just like `onclick=` but it is different in a key way. The funtion has to be defined within VueJS. It is not a global javascript function. Again tight scoping.

Line 29 has `<input` email field. It does not have a name. We are not doing a normal submit on this. The `v-model="myForm.email` is used to bind this to VueJS. Everytime a changes happens to this field, that change is reflected within VueJS.

Line 33 has a password field. We are doing the same kind of binding here.

Line 35 has a submit button, but it isn't connected to anything. This would be a good place to expand the functionality. Perhaps it should do a REST call to ColdFusion and do some login stuff.

Line 38 looks simple. It is just like count on line 19. But it is much more powerful than it looks. We are loading in data into myform on line 29 and 33. From the ColdFusion point of view. It looks like a struct with two keys. If line 38 was a simple `<cfoutput>`, it would crash. Line 38 is more similar to a `<cfdump format="text">`. The ability to just dump a variable anywhere is really powerful.

## Some Vuex stuff

This is our up/down counter again. First don't worry about the `const` on 44, the data can change, the structure of the data cannot. If bringing in vuex is the equivalent to enabling `session` variables, `new Vuex.Store` is equivalent to creating an object and tying it to a session variable. State is a member variable. It is more or less like using `this.` keyword. `mutations` are the member functions. 

Why do they use the terms: `Store`, `state` and `mutations`? It is because we are going to interact with them in a special way. 

## Let's move on to the next part.

`new Vue(` is where we switch out of plain Javascript and do some VueJS programming. Vue takes everything as a checklist. We have

- `el:`
- `data:`
- `computed:`
- `methods:`

This does not exactly look like OO programming. This is not like a `.cfc`.

Let's do this part by part.

`#el` is for the element. As in, we are going to attach all this VueJS functionality to this element on the page. This had better match. Don't duplicate elements.

`data` is for the member data if this were an object. I just so happen to have struct called myform with two keys. By the way, you will not see any special initialization code. The data has two way binding by virtue of the `v-model` attribute on the form fields. `v-model` and data often go hand in hand.

`computed` is really special. Functions within computed look like data. In fact they are derived from the data in the application. But here is the thing they values are automatically updated as needed. Line 19: does not look like a function call. Rather than being updated by a function called, it was automatically updated because the underlying data changed. There is no loop running in the backgroud. It is if it was a magic variable that has the right value.

`methods` are just regular functions within object. These methods are interacting with the Vuex data. They are not interacting with the data withing `Vue`

Remember how I was talking about how VueJS is tightly scoped, BUT Vuex was a way to get around that? Let's to back to where the `mutations` are used. They are are part of the `@click` on the buttons. We didn't say `store.increment()` with parenthesis. We didn't say `Vuex.increment` with parenthesis either. We just used its name and VueJS method then called a `mutation`.

## Let's see this in action.



# Next steps

If you found this interesting, go see my Landing page video. It has ColdFusion, VueJS, and a whole lot more


# Resources

- https://vuejs.org/v2/guide/installation.html

- https://github.com/vuejs/vuex/tree/dev/examples/counter

- https://angularjs.org/

- https://angular.io/

- https://github.com/jmohler1970/VueJSforCF_1st10min

