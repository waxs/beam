# Beam JS
I used to take event emitters for granted, made sence to do a little experiment on declaring event hooks with native Javascript. Beam is a small (only 4.94 KB minified) event emitter that directly extends upon native Javascript events that can either be dispatched directly or statically called. 

### Initiate Beam JS
To use Beam we initiate the class as follows, the class takes two arguments. These arguments are optional, so let's stick with a clean setup for now.

```javascript
const beam = new Beam();
```
### Creating an event
Creating an event with Beam is very intuitive and simple to use, it follows a convenient syntax similar to native events in Javascript. First you set up the listeners, these will be called once the event is dispatched. 

```javascript
beam.when('init', () => {
    console.log('Init event was called!');
});
 
beam.init();
```

First we create the listener and give it a name, let's stick with `init` for now. This will also create a new method that we can now use to dispatch the event. By calling `beam.init()` all instances of the `init` listener will be called. 

If needed we can call multiple events at the same time. 

```javascript
beam.when('init', () => {
    console.log('Call me first!');
});

beam.when('init', () => {
    console.log('...don't forget about me!');
});
 
beam.init();
```
### Using a payload
It's possible to share a payload while dispatching the event that becomes available within the listeners hooked to the event. Let's assume we want to dispatch some data once we call dispatch the event. We can do so as follows. In the following we example, we set the name to `payload` instead of `init`, this also illustrates the use of naming the events.  

```javascript
const data = {
    name: 'Sander'
}

beam.when('payload', payload => {
    console.log('My name is: ' + payload.data.name);
});

beam.payload(data);
```
Besides the data there will also be an action available within the payload, this can be called as `payload.action` within the example above. Behind the scenes every new instance will be stored as an action. 

### What is an action?
The action stores all the related data of the event. For instance the `type` of event, it's `source` and a `timestamp` of when the action was created. Every callback stores some information needed for the execution, if needed you can retrieve some handy properties from this object. This is all optional. It may look something like this.

```javascript
action: {
    name: "payload",
    source: "static",
    timestamp: 603244800,
    type: "action",
    callback: {
        active: true,
        delay: 0,
        fn: payload => {…},
        id: "_123456789",
        once: false,
        type: "hook"
    }
},
data: {
    name: "Sander"
}
```
If you need an overview of the previous actions that have been taken place, you can use a getter `beam.action` to retrieve an array of actions. This is by default limited to 10 items, however this can be changed when using events and declaring a listener on an element. 

```javascript
const beam = new Beam(button, {
    limit: 25
});
```

### Using Javascript Events
Besides triggering the event manually, or `static` as described within the action, it's also possible to use native Javascript events directly within Beam. We can do so as follows. 

```javascript
const data = {
    name: 'Sander'
}

const button = new Beam('.button');

button.when('click', (e, payload) => {
    console.log('The event:', e);
});

button.event('click', data);
```
In this case we set a click event on an element with a class of `.button`, id's also work using the hashtag `#button` or you could even pass the element itself if it has already been selected like so. This gives some extra flexibility and convenience if needed. 

```javascript
const el = document.querySelector('.button');
const button = new Beam(el);
```

#### Retrieve the event
Once the event will take place the listeners will execute simultaneously. This means that multiple listeners can be setup. Besides sharing a payload the original event will also be shared as the first parameter `button.when('click', (e, payload) => { })` and the payload is accessible as the second parameter. 

#### Other events
Other type of events, like mouseover follow the same syntax. So for instance if a mouseover is required it can be used as follows. 

```javascript
const button = new Beam('.button');

button.when('mouseover', (e, payload) => {
    console.log('The event:', e);
});

button.event('mouseover', data);
```

### Additional helpers
In some cases you might want to execute a specific listener once or delay it once it gets called, this has been accounted for. If an event can only be executed once we use the `once` method instead of the `when` method. This can be used as explained within the example below.
```javascript
button.once('click', () => {
    console.log('Will only fire once!');
});
```
If an event needs to be delayed this can be achieved as a third argument within the `when` or `once` method. The argument can be specified within an amount of milliseconds as found most convenient. 
```$javascript
button.once('click', () => {
    console.log('Will only fire once after 1000 milliseconds!');
}, 1000);
```

### About
This repository is mainly intended as an experiment and proof of concept, it was written within barely 24 hours, once the sourcecode has been cleaned up it will be shared, for now only a compressed version is available to mess around with. A folder with examples is available within this repo showing an every example as documented above. That's all folks! Cheers. 
