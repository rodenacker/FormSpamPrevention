# Form Spam Prevention Method

A reliable method preventing spam bots from submitting input and textarea fields in online forms (e.g. contact forms)

## Why it works

Bots do not execute JavaScript, so this method uses JavaScript to generate form elements. These elements do not exist in the plan html and are not used by spam bots (works in June 2022)

## Usage

1. Add a tag with the name ff
2. Add a "field" attribute with 
3. Add an element of your choice with classname "input" or "textarea". This element will be transformed into an input or textarea element
4. Add the script below to the page. When the page loads it performs the transformations

## Example
```
<div>
 <label>Name</label>
 <span class="input"></span>
</div>
<div>
 <label>Message</label>
 <span class="textarea"></span>
</div>

<script>
 document.addEventListener("DOMContentLoaded", function () {
  const fields = document.querySelectorAll(".input, .textarea");
   for (i=0;i<fields.length;i++){
    let label = fields[i].parentNode.getElementsByTagName("label")[0].innerText;
    replaceMe(fields[i], fields[i].className, label);
   }
 });
  function replaceMe(placeholder, elementType, nameProp) {
   const formField = document.createElement(elementType);
   formField.setAttribute("type", "text");
   formField.setAttribute("name", nameProp);
   placeholder.parentNode.replaceChild(formField, placeholder);
 }
</script>
```

## Result

```
<div>
 <label>Name</label>
 <input type="text" name="Name">
</div>
<div>
 <label>Message</label>
 <textarea type="text" name="Message"></textarea>
</div>
