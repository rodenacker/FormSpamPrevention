# Form Spam Prevention Method

A reliable method preventing spam bots from completing input and textarea fields in online forms (e.g. contact forms)

## Why it works

This method relies on the fact that spam bots do not execute Javascript. It uses a script that converts custom tags (\<ff>) to html form elements when the page loads. This means that the html does not need to contain any input or textarea elements that automated systems can populate (works in June 2022)

## Usage

1. Add an opening and a closing tag (!) with the name ff
```
<ff></ff>
```
2. Add a "field" attribute with the type of field you want to create 
```
<ff field="input"></ff>
<ff field="textarea"></ff>
<ff field="select"></ff>
```
3. Add any attributes you want the field to have
```
<ff field="input" type="text" name="name" class="formfield"></ff>
<ff field="input" type="checkbox" name="mycheckbox" value="Yes" checked></ff>
<ff field="input" type="radio" name="mycheckbox" value="No" checked></ff>
<ff field="textarea" name="message" style="height:40px;width:200px;"></ff>
<ff field="select" name="mycheckbox2"><option value="Monday">Monday</option><option value="Tuesday">Tuesday</option></ff>
```
4. Add the script below to the page. When the page loads it transforms the ff tags to input or textarea fields
```
<script>
    document.addEventListener("DOMContentLoaded", function () {
        const fields = document.getElementsByTagName("ff");
        for (let i=fields.length-1;i>=0;i--){
            let attrs = fields[i].getAttributeNames().reduce((acc, name) => {
                return {...acc, [name]: fields[i].getAttribute(name)};
            }, {});
            let el = document.createElement(attrs["field"]);
            for (let key in attrs){
                if(attrs.hasOwnProperty(key) && key != "field"){
                    el.setAttribute(`${key}`, `${attrs[key]}`);
                }
            }
            while (fields[i].childNodes.length > 0) {
                console.log(fields[i].childNodes)
                el.appendChild(fields[i].childNodes[0]);
            }
            fields[i].parentNode.insertBefore(el, fields[i]);
            fields[i].remove();
        }
    });
</script>
```

## Full Example
```
<form action="" method="POST">
    <div>
        <span>Your Name</span>
        <ff field="input" type="text" name="firstname" class="formfield" placeholder="first name"></ff>
        <ff field="input" type="text" name="lastname" class="formfield" placeholder="last name"></ff>
    </div>
    <div>
        <span>Email</span>
        <ff field="input" type="email" onclick="myValidationScript();"></ff>
    </div>
    <div>
        <span>Message1</span>
        <ff field="textarea" name="message1" style="height:40px;width:200px;"></ff>
    </div>
    <div>
        <span>Message2</span>
        <ff field="textarea" name="message2" style="height:40px;width:200px;"></ff>
    </div>
    <div>
        <span>Select</span>
        <ff field="select" name="mycheckbox2"><option value="Monday">Monday</option><option value="Tuesday">Tuesday</option></ff>
    </div>
    <div>
        <span>Checkboxes</span>
        <ff field="input" type="checkbox" name="mycheckbox1" value="Green" checked></ff>
        <ff field="input" type="checkbox" name="mycheckbox2" value="Blue"></ff>
    </div>
    <div>
        <span>Radio Buttons</span>
        <ff field="input" type="radio" name="myradio" value="Yes" checked></ff>
        <ff field="input" type="radio" name="myradio" value="No"></ff>
    </div>
    <ff field="input" type="button" name="submit" value="Submit"></ff>
</form>
<script>
    document.addEventListener("DOMContentLoaded", function () {
        const fields = document.getElementsByTagName("ff");
        for (let i=fields.length-1;i>=0;i--){
            let attrs = fields[i].getAttributeNames().reduce((acc, name) => {
                return {...acc, [name]: fields[i].getAttribute(name)};
            }, {});
            let el = document.createElement(attrs["field"]);
            for (let key in attrs){
                if(attrs.hasOwnProperty(key) && key != "field"){
                    el.setAttribute(`${key}`, `${attrs[key]}`);
                }
            }
            while (fields[i].childNodes.length > 0) {
                console.log(fields[i].childNodes)
                el.appendChild(fields[i].childNodes[0]);
            }
            fields[i].parentNode.insertBefore(el, fields[i]);
            fields[i].remove();
        }
    });
</script>
```
