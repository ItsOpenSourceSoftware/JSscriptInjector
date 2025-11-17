# JSscriptInjector
A javascript injector to run custom javascript code on webpages.

## Injector Bookmarklet

This bookmarklet adds a small panel on your top right side of your screen to any webpage that does not have CSP, letting you:
- Enable page editing
- Play audio (from URL or file)
- Inject custom JavaScript
- Close the panel

### How to Install
1. Copy the code below.
2. Create a new bookmark in your browser.
3. Paste the code into the bookmark’s URL field.
4. Save it and click the bookmark on any page to run.

### Code
```javascript
javascript:(function(){
  var p=document.createElement("div");
  p.style.position="fixed";
  p.style.top="10px";
  p.style.right="10px";
  p.style.background="rgba(0,0,0,0.9)";
  p.style.color="white";
  p.style.padding="8px";
  p.style.zIndex=9999;
  p.style.fontFamily="sans-serif";
  p.innerHTML='<textarea id="codeEditor" placeholder="Type JS here or press CTRL+V to paste script from clipboard" style="width:200px;height:100px;"></textarea><br><button id="editBtn">Enable Page Edit</button><button id="audioBtn">Play Sound</button><button id="runBtn">inject js script</button><button id="closeBtn">X</button>';
  document.body.appendChild(p);
  document.getElementById("editBtn").onclick=function(){
    document.body.contentEditable=true;
    document.designMode="on";
  };
  document.getElementById("audioBtn").onclick=function(){
    var u=prompt("Audio URL or leave blank for file:");
    if(u){
      (new Audio(u)).play().catch(e=>alert("Autoplay blocked"))
    }else{
      var f=document.createElement("input");
      f.type="file";
      f.accept="audio/*";
      f.onchange=function(){
        (new Audio(URL.createObjectURL(f.files[0]))).play()
      };
      f.click();
    }
  };
  document.getElementById("runBtn").onclick=function(){
    try{
      eval(document.getElementById("codeEditor").value)
    }catch(e){
      alert("Error: "+e)
    }
  };
  document.getElementById("closeBtn").onclick=function(){
    p.remove();
  };
})();
