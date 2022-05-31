# FormSpamPrevention

A simple script to prevent bots from submitting input and textarea fields in online forms

## Why it works

Bots do not execute JavaScript, so generating form elements using JavaScript means bots do not have any html elements to populate (works in June 2022)

## Usage

1. Add a form field wrapper tag (example uses a div tag)
2. Add a label tag. The label text will become the form field name property
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
   formField.setAttribute("type", elementType);
   formField.setAttribute("name", nameProp);
   placeholder.parentNode.replaceChild(formField, placeholder);
 }
</script>
```

## Result

```
<div>
 <label>Name</label>
 <input type="input" name="Name">
</div>
<div>
 <label>Message</label>
 <textarea type="textarea" name="Message"></textarea>
</div>
