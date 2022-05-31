# Form Spam Prevention Method

A reliable method preventing spam bots from submitting input and textarea fields in online forms (e.g. contact forms)

## Why it works

Bots do not execute JavaScript, so this method uses JavaScript to generate form elements. These elements do not exist in the plan html and are not used by spam bots (works in June 2022)

## Usage

1. Add a tag with the name ff
```
<ff>
```
2. Add a "field" attribute with the type of field you want to create 
```
<ff field="input">
<ff field="textarea">
```
3. Add any attributes you want the field to have
```
<ff field="input" type="text" name="name" class="formfield">
<ff field="textarea" name="message" style="height:40px;width:200px;">
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
          fields[i].parentNode.insertBefore(el, fields[i]);
      }
  });
</script>
```

## Example
```
<div>
    <span>Your Name</span>
    <ff field="input" type="text" name="name" class="formfield">
</div>
<div>
    <span>Email</span>
    <ff field="input" type="email" onclick="myValidationScript();">
</div>
<div>
    <span>Message1</span>
    <ff field="textarea" name="message1" style="height:40px;width:200px;">
</div>
<div>
    <span>Message2</span>
    <ff field="textarea" name="message2" style="height:40px;width:200px;">
</div>
<div>
    <span>Checkboxes</span>
    <ff field="input" type="checkbox" name="mycheckbox1" value="Green">
    <ff field="input" type="checkbox" name="mycheckbox2" value="Blue">
</div>
<div>
    <span>Radio Buttons</span>
    <ff field="input" type="radio" name="myradio" value="Yes">
    <ff field="input" type="radio" name="myradio" value="No">
</div>
<ff field="input" type="button" name="submit" value="Submit"></ff>

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
            fields[i].parentNode.insertBefore(el, fields[i]);
        }
    });
</script>
```
