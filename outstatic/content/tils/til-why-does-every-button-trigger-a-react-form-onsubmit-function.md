---
title: 'TIL: Why does every button trigger a react form onSubmit function?'
status: 'published'
author:
  name: 'Iris Ibekwe'
  picture: 'https://avatars.githubusercontent.com/u/5355315?v=4'
slug: 'til-why-does-every-button-trigger-a-react-form-onsubmit-function'
description: ''
coverImage: ''
publishedAt: '2023-01-04T02:51:34.490Z'
---

If you have a button within a React form it will be automatically associated with the parent form.

```javascript
<form onSubmit={handleSubmit(onSubmit)}>
  <button onClick={() =>   notTheSubmitFunction()}
<input type="submit" value="Submit form" />
</form>
```

Clicking the button will cause the form to trigger onSubmit even if it runs a different function onClick.

This is because the default `type` property assigned to `&lt;button/&gt;` elements is `submit`. This causes the button to attempt to trigger their parent form's `onSubmit` handler, regardless of the button's own click handler.

> If your buttons are not for submitting form data, be sure to set their `type` attribute to `button`. Otherwise they will try to submit form data and to load the (nonexistent) response, possibly destroying the current state of the document.

source: [MDN docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)

