
Select all tags:

```html
<!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>p2 css</title>
    <style>
        p {
            color: red;
            font-size: 12px;
        }
    </style>
</head>

<body>
    <p> hello css </p>
</body>

</html>
```


Select some tags:
```html
!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>p2 css</title>
    <style>
        p {
            color: red;
            font-size: 12px;
        }
        .red{
            color:red;
        }
    </style>
</head>

<body>
    <p> hello css </p>
    <ul>
        <ol class="red">1</ol>
        <ol>2</ol>
        <ol>3</ol>
    </ul>
</body>

</html>
```

Background color:

```html
    <style>
        .red{
            background-color:red;
        }
    </style>

```

Font:

```html
    <style>
        .font35{
            font-size: 35px;
            font-weight:700 ;
            font-style: italic ;
            font-family: 'Microsoft yahei';
        }
    </style>
```

```html
    <style>
        div{
            font: italic 700 35px 'Microsoft yahei'
        }
    </style>
```

Id: (only used by one)
```html
    <style>
        #yellow{
            color: yellow;
        }
    </style>
```

Select All:
```html
    <style>
        *{
            color: yellow;
        }
    </style>
```


text:

- align
```html
    <style>
        div{
            text-align: center;
        }
    </style>
```

- decroation
```html
    <style>
        div{
            text-decoration: underline;
        }
    </style>
```

- indent:
```html
    <style>
		div{
            text-indent: 20 px;
        }
    </style>     
```

line-height:
```html
    <style>
		p{
            line-height: 20 px;
        }
    </style>     
```

Inside a tag:
```html
<p style="color:red"> hello css </p>
```

Style.css:
```css
div{
    color: pink;
}
```

Link Style.css to html:
```html
    <link rel="stylesheet" href="Style.css">
```

Modify Children or GrandChildren:
```css
        ol li{
            color: red;
        }
```

Modify only Chidren:
```css
.something>a{
	color: red;
}
```

Modify tags together:
```css
        div,
        p {
            color: red;
        }
```

a : link / visited/hover/active
```css
a:link{
	color: red;
}
```


&lta>:
```css
a {
	display: block;
}
```

Background: 
```css
        div{
            background: url() repeat-x center top fixed;
        }
```



