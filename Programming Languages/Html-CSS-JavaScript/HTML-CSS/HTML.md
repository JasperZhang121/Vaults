
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
```html
    <h1> Here! </h1>

    <h2> Here! </h2>

    <h3> Here! </h3>

    <h4> Here! </h4>

    <h5> Here! </h5>

    <h6> Here! </h6>
```

![[Pasted image 20230108223141.png]]


2.Paragraph tag
```html
    <p> I know this will happen</p>
    <p> but I cannot know when </p>
```

![[Pasted image 20230108223730.png]]

3.Break tag

```html
    <p> I know this <br/> will happen</p>

    <p> but I cannot know when </p>
```

![[Pasted image 20230108223951.png]]

4.Bold tag

```html
<strong> be bold <strong>
```

5.Italic tag
```html
<em> be italic </em>
<i> be italic </i>
```

6.Delete tag
```html
<del> to delete </del>
<s> to delete </s>
```

7.Underline tag

```html
<ins> underline </ins>
<u> underline </u>
```

8.Box tag

```html
    <div> a box </div>

    <span> a box </span>
```

One div is a big box which fulfill the whole line, but many spans can exist at the same line.

9.Image tag
```html
<img src="" alt="" title="" width="" height="" border = ""/>
```
if src does not work for some reasons, show things from alt. When mouse hover on the picture, showing text from title. 

10.HyperLink tag
```html
<a href="" target=""> </a>
```
Jump to th link after href (the link must start with http or use the internal html file), and whether jump by opening a new tap is decided by the target.
If files end with .exe or .zip in the href, click will download the file.
If the a tag's id is put into the href, it will directly jump to the id position in this html.


11.Comment
```html
<!-- comments -->
```
tips: press CTRL and / 

12.Special Characters
```HTML
&nbsp;
&lt;
&gt;
```

13.Table
```HTML
    <table align="" border="" cellpadding = "" cellspacing = "" width = "" height = "">

        <tr><th></th></tr>

        <tr><td></td></tr>
        <tr><td></td></tr>
        <tr><td></td></tr>

    </table>
```

```html
    <table>

        <thead></thead>

        <tbody></tbody>

    </table>
```

```html
<td colspan=""></td>
<td rowspan=""></td>
```

14.List

- without order
```html
    <ul>

        <li></li>

        <li></li>

        <li></li>

    </ul>
```

- ordered
```html
<ul>

        <ol></ol>

        <ol></ol>

        <ol></ol>

    </ul>
```

- customized
```html
    <dl>

        <dt></dt>

        <dd></dd>

        <dd></dd>

    </dl>
```

15.Form

```html
    <form action="" method="" name="">

        username: <input type="text" name="username" value="please type your username" maxlength=""> <br>

        password: <input type="password" name="password"> <br>

        gender: male <input type="radio" name = "sex"> female <input type="radio" name="sex">

        Hobby: eat <input type="checkbox" checked = "checked"> sleep <input type="checkbox"> pee <input type="checkbox">

        <input type="submit" value="">
		<input type="reset" value="">
    </form>
```

16.Button
```html
    <input type="button" value="It's a button">
    <input type="file">
```

17.Label
```html
    <label for="gender">male</label>
    <input type="radio" name="gender" id = "gender">
```

18.Select (in form)
```html
    <select name="" id="">

        <option value="" selected = "selected"></option>

        <option value=""></option>

        <option value=""></option>

    </select>
```

19.Text area (in form)
```html
<textarea name="" id="" cols="30" rows="10"></textarea>
```


