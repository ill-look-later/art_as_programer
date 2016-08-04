


```javascript
FileDialog { 
  onAccepted: {
    var path = myFileDialog.fileUrl.toString();
    // remove prefixed "file:///"
    path= path.replace(/^(file:\/{3})|(qrc:\/{2})|(http:\/{2})/,"");
    // unescape html codes like '%23' for '#' 
    var cleanPath = decodeURIComponent(path);
    console.log(cleanPath) 
  }
} 
```