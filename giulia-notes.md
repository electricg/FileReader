* The label must wrap the input file
* Attribute multiple is not read by IE9
* Images with Base64 will not work on IE8 if their size is greater than 32KB http://stackoverflow.com/questions/10159500/internet-explorer-and-base64-image-display

Example tested on IE9 and IE8

```html
<label>Upload:<input type="file" id="element1" multiple style="display:none;"></label>
```

```javascript
var options = {
  filereader: 'js/lib/FileReader/filereader.swf' // path relative to the html file
};
var $el = $('input[type="file"');
$el.fileReader(options);

$el.on('change', function(e) {
  var files = e.target.files; // using this.files doesn't work on IE9
  var file;
  for (var i = 0; i < files.length; i++) {
    file = files[i];
    console.log(file.name, file.type, file.size, file.lastModifiedDate);
    var reader = new FileReader();
    reader.readAsDataURL(file);
    reader.addEventListener('loadend', function(e) {
      var $img = $('<img>');
      var src = e.target.result;
      $img.attr('src', src);
      $('body').append($img);
    });
  }
});
```
