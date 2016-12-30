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
Vanilla js
```
var errorDocument = new DOMParser().parseFromString(azureErrorXmlString, 'application/xml');
// code = "InvalidQueryParameterValue"
var code = errorDocument.querySelector('Code').textContent;
```
