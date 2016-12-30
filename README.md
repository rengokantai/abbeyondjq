# abbeyondjq
##2. You Don’t Need jQuery (Anymore)
####When Should You Refrain from Using It?
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
