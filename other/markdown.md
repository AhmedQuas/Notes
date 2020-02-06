### Sections
- [Simple text manipulation](#simple-text-manipulation)
- [Horizontal line](#horizontal-line)
- [List](#list)
  - [Unordered](#unordered)
  - [Ordered](#ordered)
  - [Task lists](#task-lists)
- [Links](#links)
- [Images](#images)
- [Code](#code)
- [Tables](#tables)
- [Additional resources](#additional-resources)

---

## Simple text manipulation

> Blockquote Some fancy text
```
> Blockquote Some fancy text
```
_Italics_
```
_Italics_
```
**Bold**
```
**Bold**
```
~~Cross this line~~
```
~~Cross this line~~
```
## Horizontal line

---
```
---
```
---
## List

### Unordered
- item
- item
  - sublist
* different dot
+ plus item
```
-item
- item
  - sublist
* different dot
+ plus item
```
### Ordered

1. First
2. Second
    - sublist
3. Third

```
1. First
2. Second
    - sublist
3. Third
```

### Task lists

- [x] Task done
- [ ] Task to do 
```
- [x] Task done
- [ ] Task to do 
```
---

## Links

[Click here](http://www.google.com)

```
[Click here](http://www.google.com)
```
Bottom links as avariable. This is useful to change many same links in document. 

[Github][key]

[key]: http://github.com

```
[Github][key]

[key]: http://github.com
```
## Images
Similar as links, anchors. Markdown icon as example

![Alt text](https://res.cloudinary.com/practicaldev/image/fetch/s--pFn86d2h--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://markdown-here.com/img/icon256.png)

```
![Alt text](url)
```

## Code

Here is some piece of inline code `print('Hello World')` (tilde without shift key).

Some linux command `shutdown`
```
Some linux command `shutdown`
```

Block of code

```python
name = input('What`s your name?')
print('Hi' + name)
```

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta charset='utf-8'/>
    <link rel='stylesheet' type='text/css' href='style.css'>
    <script src='script.js'></script>
    <title>Test site</title>
</head>
<body>
    <h1>Test site</h1>
    <p>Page content</p>
</body>
</html>
 
```

## Tables

>Hint: Format tables so it look better and it will be easy to read by you and others

| Header 1  | Header 2  | Header3   |
|---        |---        |---        |
|col-1      |col-2      |col-3      |
|col-1      |col-2      |col-3      |
|col-1      |col-2      |col-3      |
|col-1      |col-2      |col-3      |

It is also possible to align text in a column see example below

| Header 1  | Header 2  | Header3   |
|---        |:---:      |---:       |
|Left align |Center     |Right align|
|col-1      |col-2      |col-3      |
|col-1      |col-2      |col-3      |
|col-1      |col-2      |col-3      |

```
| Header 1  | Header 2  | Header3   |
|---        |:---:      |---:       |
|Left align |Center     |Right align|
|col-1      |col-2      |col-3      |
|col-1      |col-2      |col-3      |
|col-1      |col-2      |col-3      |
```

## Additional resources

[Official github markdown + GFM](https://guides.github.com/features/mastering-markdown/)

[Emoji avaiable on github](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md)

[VS Code Markdown tutorial](https://www.youtube.com/watch?v=pTCROLZLhDM)
