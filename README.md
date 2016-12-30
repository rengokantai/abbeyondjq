# abbeyondjq
[remove line numbers](http://remove-line-numbers.ruurtjan.com/)  
code
```
var re = new RegExp("^\\s*\\d+\\.?", "gm");
function removeLineNumbers(textArea) {
     textArea.value = textArea.value.replace(re, "");
}
```               
##2. You Don’t Need jQuery (Anymore


####When Should You Refrain from Using It?
#####hide
One major performance bottleneck in jQuery’s implementation of the hide() method is due to the use of getComputedStyle(), a web API method that computes the actual set of styles of an element, __taking into account CSS files, ```<style>``` elements, and inline or JavaScript modifications to the element’s style property.__

#####each
slow
```
$('.red').each(function() {
     if($(this).attr('foo') === 'bar') {
        $(this).remove();
     }
});
```
more efficient:
```
$('.red[foo="bar"]').remove();
```

#####ready()
The ready() method executes a passed function only after all elements in the document have been loaded on the page. __This doesn’t really have a notable effect on actual page load time, but it does affect perceived page load time.__  

And if all scripts are loaded at the top of the document, this may result in a noticeable delay in page rendering, since the scripts must be loaded and executed before the elements.


#####Array.prototype.find shim
```
if (!Array.prototype.find) {
    Array.prototype.find =
      function(callback, ctx) {
        for (var i = 0; i < this.length; i++) {
          var el = this[i];
          if (callback.call(ctx, el, i, this)) {
            return this[i];
          }
        }
      };
  }
```



##12. Common JavaScript Utility Functions
#####Date
below are same
```
$.now();
new Date().getTime();
Date.now();
```

####JSON  (almost useless)
```
var user = $.parseJSON(jsonString);
console.log(user.name);
console.log(user.id);
```
```
var user = JSON.parse(jsonString);
console.log(user.name);
```

####XML
```
  <?xml version="1.0" encoding="utf-8"?>
  <Error>
    <Code>InvalidQueryParameterValue</Code>
    <Message>Value for one of the query parameters specified in the request URI is\
   invalid.</Message>
    <QueryParameterName>popreceipt</QueryParameterName>
    <QueryParameterValue>33537277-6a52-4a2b-b4eb-0f905051827b</QueryParameterValue>
    <Reason>invalid receipt format</Reason>
  </Error>
```
jQuery
```
var errorDocument = $.parseXML(azureErrorXmlString);
// code = "InvalidQueryParameterValue"
var code = $(errorDocument).find('Code').text();
```
Vanilla js (note DOMParser)
```
var errorDocument = new DOMParser().parseFromString(azureErrorXmlString, 'application/xml');
// code = "InvalidQueryParameterValue"
var code = errorDocument.querySelector('Code').textContent;
```


#####Array
vanilla
```
  // both are true
  Array.isArray([]);
  Array.isArray(new Array());

  // both are false
  Array.isArray(3);
  Array.isArray({});
```
jQuery
```
  // both are true
  $.isArray([]);
  $.isArray(new Array());

  // both are false
  $.isArray(3);
  $.isArray({});
```

Array.isArray shim
```
 function isArray(value) {
   return Array.isArray
   ? Array.isArray(value)
   : Object.prototype.toString.call(value) === '[object Array]';
 }
 ```
  let’s look closer at a portion of the example ```Object.prototype.toString.call(value)```. You may be wondering why we don’t simply use typeof here. Somewhat surprisingly, typeof [] and typeof new Array() produce a result of “object”. This is technically true—an Array is a type of Object since it inherits from Object.prototype, a type value of “object” is a little less specific than expected.
  
  
  
  
### Making JavaScript Objects Bend to Your Will 
#### Iterating over Keys and Values
HTML
```
  var user = {
    name: 'Ray Nicholus',
    address: '1313 Mockingbird Lane',
    city: 'Mockingbird Heights',
    state: 'California'
  };

  <form>
    <input name='name'>
    <input name='address'>
    <input name='city'>
    <input name='state'>
  </form>
```
jQuery(does not implement hasOwnProperty)
```
$.each(user, function(property, value) {
   $('form [name="' + property + '"]').val(value);
});
```
js
```
for (var property in user) {
    if (user.hasOwnProperty(property)) {
      var value = user[property];
      document.querySelector('FORM [name="' + property + '"]').value = value;
    }
  }
```
Or ES5
```
Object.keys(user).forEach(function(property) {
   var value = user[property];
   document.querySelector('FORM [name="' + property + '"]').value = value;
});
```


##### Copying and Merging Objects
```
var user = $.extend(userLocation, userPersonal);
```
a simple polyfill
```
 function extend(first, second) {
    for (var secondProp in second) {
      var secondVal = second[secondProp];
      first[secondProp] = secondVal;
    }
    return first;
  }
```
ES2015 Object.assign
```
var user = $.extend({}, userLocation, userPersonal);
var user = Object.assign({}, userLocation, userPersonal);
```
##### Locating Specific Items
```
var names = [
    'Joe',
    'Jane',
    'Jen',
    'Jim',
    'Bill',
    'Beth'
  ];
```
Locating Specific Items
```
  // returns 4
  names.findIndex(function(name) {
    return name[0] === 'B';
  });

  // returns "Bill"
  names.find(function(name) {
    return name[0] === 'B';
  });
```

filter and jQuery grep
```
  // returns ["Joe", "Jen", "Jim"]
  $.grep(names, function(name) {
    return name.length === 3;
  });
ECMAScript 5 provides a Array.prototype.filter() to solve the same exact problem in modern browsers:
  // returns ["Joe", "Jen", "Jim"]
  names.filter(function(name) {
    return name.length === 3;
  });
```

### Useful Function Tricks
#### A Word about JavaScript Context
In JavaScript, context dictates the value of the this keyword at any given time during execution of code.  
```
var person = {
    name: 'Ray',
    printName: function() {
      console.log(this.name);
    }
  }
```
__When a function belongs to an object, the context of that function is bound to the parent object.__
