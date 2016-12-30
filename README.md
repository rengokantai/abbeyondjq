# abbeyondjq
##2. You Don’t Need jQuery (Anymore


####When Should You Refrain from Using It?
#####hide
One major performance bottleneck in jQuery’s implementation of the hide() method is due to the use of getComputedStyle(), a web API method that computes the actual set of styles of an element, taking into account CSS files, ```<style>``` elements, and inline or JavaScript modifications to the element’s style property.

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
