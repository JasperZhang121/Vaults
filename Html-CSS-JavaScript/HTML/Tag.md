
Recommend extentions in vscode:
- open in browser
- Auto Close Tag

----

Basic frame: (using ! and Tab in vscode can automatically generate the frame)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> Basic Frame </title>
</head>

<body>
    Space, final frontier.
</body>

</html>
```


Details of frame:

1. 
```
<!DOCTYPE html>
```
It simply means that we use the HTML5 version to show the page, which must be put at the first line of codes (below the html tag).

2.
```
<html lang="en">
```
It shows that this is an english website, and if we change the en to zh-CN, now it is a chinese website. However, you can write either Chinese or English for displaying without worrying the en or zh-CN. The major thing is, the website will show to you and ask if the page needs to be translated to the language it was supposed to be.

3.
```
<meta charset="UTF-8">
```
UTF-8 is the most commonly used charset.

---
## More Tags

1.Title tag

From h1 to h6:
```
    <h1> Here! </h1>

    <h2> Here! </h2>

    <h3> Here! </h3>

    <h4> Here! </h4>

    <h5> Here! </h5>

    <h6> Here! </h6>
```

![[Pasted image 20230108223141.png]]


2.Paragraph tag
```
    <p> I know this will happen</p>
    <p> but I cannot know when </p>
```

![[Pasted image 20230108223730.png]]

3.Break tag

```
    <p> I know this <br/> will happen</p>

    <p> but I cannot know when </p>
```

![[Pasted image 20230108223951.png]]

4.Bold tag

```
<strong> be bold <strong>
```

5.Italic tag
```
<em> be italic </em>
<i> be italic </i>
```

6.Delete tag
```
<del> to delete </del>
<s> to delete </s>
```

7.Underline tag

```
<ins> underline </ins>
<u> underline </u>
```

8.Box tag

```
    <div> a box </div>

    <span> a box </span>
```

One div is a big box which fulfill the whole line, but many spans can exist at the same line.

9.Image tag
```
<img src="" alt="" title="" width="" height="" border = ""/>
```
if src does not work for some reasons, show things from alt. When mouse hover on the picture, showing text from title. 

10. 
