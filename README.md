# Stop Form Spam: The Custom Tag Method

A reliable method to prevent spam bots from completing input, select and textarea fields and from submitting online forms (e.g. contact forms)

## How it works

Use a small bit of Javascript to convert custom tags to valid form tags. 

## Usage

The example below converts \<f-f>\</f-f> tags to input, select and textarea tags after the page loads. This means the page html does not need to contain such tags and spam bots cannot populate them. The JS is very fast, so human page visitors do not notice the conversion. 

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
3. Add any attributes you want your element to have
```
<f-f el="input" type="text" name="name" class="formel"></f-f>
<f-f el="input" type="checkbox" name="mycheckbox" value="Yes" checked></f-f>
<f-f el="input" type="radio" name="mycheckbox" value="No" checked></f-f>
<f-f el="textarea" name="message" style="height:40px;width:200px;"></f-f>
<f-f el="select" name="mycheckbox2"><option value="Monday">Monday</option><option value="Tuesday">Tuesday</option></f-f>
<f-f el="button" type="button">Submit</f-f>
<f-f el="input" type="button" name="submit" value="Cancel"></f-f>
```
4. Add the script below to the page. When the page loads it creates a new element using the "el" attribute in all f-f elements. It then adds all other attributes and all child elements to the new element before removing the f-f element from the DOM.
```
<script>
document.addEventListener("DOMContentLoaded", function () {
    //register the custom element
    window.customElements.define('f-f', class extends HTMLElement {});
    //get an array of f-f elements to convert
    const els = document.getElementsByTagName("f-f");
    //loop through the array of elements
    for (let i=els.length-1;i>=0;i--){
        //create an object of the attributes
        let attrs = els[i].getAttributeNames().reduce((acc, name) => {
            return {...acc, [name]: els[i].getAttribute(name)};
        }, {});
        //create a new element using the el attribute
        let el = document.createElement(attrs["el"]);
        //loop through the array of attributes
        for (let key in attrs){
            //filter out the el attribute
            if(attrs.hasOwnProperty(key) && key != "el"){
                //add all other attributes to the new element
                el.setAttribute(`${key}`, `${attrs[key]}`);
            }
        }
        //loop though the child nodes of the f-f
        while (els[i].childNodes.length > 0) {
            //append child nodes to the new element
            el.appendChild(els[i].childNodes[0]);
        }
        //insert the new element into the DOM
        els[i].parentNode.insertBefore(el, els[i]);
        //remove the f-f element from the DOM
        els[i].remove();
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
    <f-f el="button" type="button">Submit</f-f>&nbsp;<f-f el="input" type="button" name="submit" value="Cancel"></f-f>
</form>
<script>
    document.addEventListener("DOMContentLoaded", function () {
        window.customElements.define('f-f', class extends HTMLElement {});
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
