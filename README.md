# Form Spam Prevention: The Custom Tag Method

A reliable method preventing spam bots from completing input, select and textarea els in online forms (e.g. contact forms)

## Why it works

This method implments a small bit of Javascript that converts elements that use a custom tag (in this case \<f-f>\</f-f>) to any other html form element on page load. When using this method the page html does not need to contain any input, select or textarea elements that automated systems can populate (works in June 2022). This works with any custom tag and can be changed very easily. 

## Usage

1. Add an opening and a closing tag (!) with the name f-f
```
<f-f></f-f>
```
2. Add a "el" attribute with the element you want to create
```
<f-f el="input"></f-f>
<f-f el="textarea"></f-f>
<f-f el="select"></f-f>
```
3. Add any attributes you want the el to have
```
<f-f el="input" type="text" name="name" class="formel"></f-f>
<f-f el="input" type="checkbox" name="mycheckbox" value="Yes" checked></f-f>
<f-f el="input" type="radio" name="mycheckbox" value="No" checked></f-f>
<f-f el="textarea" name="message" style="height:40px;width:200px;"></f-f>
<f-f el="select" name="mycheckbox2"><option value="Monday">Monday</option><option value="Tuesday">Tuesday</option></f-f>
```
4. Add the script below to the page. When the page loads it creates a new element from the el attribute in all f-f elements, leaving all other attributes and child elements in place. It then removes the f-f element from the DOM
```
<script>
    document.addEventListener("DOMContentLoaded", function () {
        window.customElements.define('f-f', class extends HTMLElement {});  //register the custom element 
        const els = document.getElementsByTagName("f-f");                   //get an array of f-f elements to convert
        for (let i=els.length-1;i>=0;i--){                                  //loop through the array of elements
            let attrs = els[i].getAttributeNames().reduce((acc, name) => {  //create an object of the attributes
                return {...acc, [name]: els[i].getAttribute(name)};
            }, {});
            let el = document.createElement(attrs["el"]);                   //create a new element using the el attribute
            for (let key in attrs){                                         //loop through the artray of attributes
                if(attrs.hasOwnProperty(key) && key != "el"){               //filter out the el attribute
                    el.setAttribute(`${key}`, `${attrs[key]}`);             //add all other attributes to the new element
                }
            }
            while (els[i].childNodes.length > 0) {                          //loop though the child nodes of the f-f
                el.appendChild(els[i].childNodes[0]);                       //append child nodes to the new element
            }
            els[i].parentNode.insertBefore(el, els[i]);                     //insert the new element into the DOM
            els[i].remove();                                                //remove the f-f element from the DOM
        }
    });
</script>
```

## Full Example
```
<form action="" method="POST">
    <div>
        <span>Your Name</span>
        <f-f el="input" type="text" name="firstname" class="formel" placeholder="first name"></f-f>
        <f-f el="input" type="text" name="lastname" class="formel" placeholder="last name"></f-f>
    </div>
    <div>
        <span>Email</span>
        <f-f el="input" type="email" onclick="myValidationScript();"></f-f>
    </div>
    <div>
        <span>Message1</span>
        <f-f el="textarea" name="message1" style="height:40px;width:200px;"></f-f>
    </div>
    <div>
        <span>Message2</span>
        <f-f el="textarea" name="message2" style="height:40px;width:200px;"></f-f>
    </div>
    <div>
        <span>Select</span>
        <f-f el="select" name="mycheckbox2"><option value="Monday">Monday</option><option value="Tuesday">Tuesday</option></f-f>
    </div>
    <div>
        <span>Checkboxes</span>
        <f-f el="input" type="checkbox" name="mycheckbox1" value="Green" checked></f-f>
        <f-f el="input" type="checkbox" name="mycheckbox2" value="Blue"></f-f>
    </div>
    <div>
        <span>Radio Buttons</span>
        <f-f el="input" type="radio" name="myradio" value="Yes" checked></f-f>
        <f-f el="input" type="radio" name="myradio" value="No"></f-f>
    </div>
    <f-f el="input" type="button" name="submit" value="Submit"></f-f>
</form>
<script>
    document.addEventListener("DOMContentLoaded", function () {
        window.customElements.define('f-f', class extends HTMLElement {}); //register the custom element 
        const els = document.getElementsByTagName("f-f");
        for (let i=els.length-1;i>=0;i--){
            let attrs = els[i].getAttributeNames().reduce((acc, name) => {
                return {...acc, [name]: els[i].getAttribute(name)};
            }, {});
            let el = document.createElement(attrs["el"]);
            for (let key in attrs){
                if(attrs.hasOwnProperty(key) && key != "el"){
                    el.setAttribute(`${key}`, `${attrs[key]}`);
                }
            }
            while (els[i].childNodes.length > 0) {
                el.appendChild(els[i].childNodes[0]);
            }
            els[i].parentNode.insertBefore(el, els[i]);
            els[i].remove();
        }
    });
</script>
```
