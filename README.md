# ElfEJSTrans

<img src="https://mjsaldanha.com/images/elf_icon.png" width="128" height="128">

This script translated an EJS file into multile EJS files in different languages, to avoid run-time embedding overhead.

It is designed to allow easy interchange from this type of translation to the traditional run-time embedding way.

# Introduction

[EJS](https://ejs.co/) (Embedded JavaScript) is a system that allows you to embedd javascript within HTML files, in such a way that HTML files are modified during runtime according to the javascript parameters you pass to the EJS renderer.

For example, you can embedd the user's name within an HTML as follows:
```html
Hello, <%= userData.fullname %>!
```
And supposing this is a file called `index.ejs`, we could render it for a user called Matheus as follows:
```js
app.route("/", (req, res) => {
  userData = {
    fullname: "Matheus",
  };
  res.render("index.ejs", {userData});
});
```

## The Traditional Way

The problem that we try to solve with ElfEJSTrans is the problem of translating a website. One traditional way to do this is by passing different languages **upon rendering the page**. For example, consider we have the following html file:
```html
<h1>
  Welcome, my friend!
</h1>
```
If we wanted to translate it using the traditional way and EJS, we would transform it to an EJS file:
```html
<h1>
  <%= translation.home.welcome %>
</h1>
```
and you would provide the `translation` object containing correct translations for the chosen language, which can be chosen based on cookies or the likes.

## The Problem

The problem with the traditional way is that, at least in my view, determining the language at runtime, on every request to the server, seems likely to impact performance. For every request, the placeholders in the EJS file will have to be filled with words in the correct language, which involves parsing a language file as the one below.
```js
home {
  welcome {
    en: "Welcome, my friend!",
    pt: "Bem vindo, amigo!",
    jp: "いらっしゃい、　友よ!",
  }
  ...
}
```
The translation file can be quite big, so even though there are plenty of caching strategies and the like, it seems that the process of translation can be improved.

One way to improve it is, in my view, to produce multiple EJS files, one for each language. Since the website language doesn't change dynamically, it doesn't make sense to build the pages dynamically; bulding them statically seems to provide more room for caching.

That's what ElfEJSTrans comes for.
