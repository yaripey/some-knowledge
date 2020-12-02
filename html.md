# HTML

HTML stands for Hyper-Text Markup Language. The whole HTML structure consists of the __elements__. __Elements__ have an __opening__ and a __closing__ tags and __content__ between them. __Content__ is what usually the end user will see. A generaly syntax of an element is as follows: `<elementname>content</elementname>`

## html page structure
There is a general rules about how you must structure your html page. It must look like this:
```
<!DOCTYPE html>
<html>
  <head>
    Meta information goes here!
  </head>
  <body>
    Content goes here!
  </body>
</html>
```
__Body__ element contains all the information that will be viewed to user. It's content is your browser window's content.

__Head__ will hold meta information and some information about the page, such as:
- Styles
- Page title
- Scripts
- Other meta information

## Elements

### <p></p>
This one is a _paragraph_. It's a block of text that is separated from other pieces of text. 

### <span></span>
A line of content.

### <h1></h1> ... <h6></h6>
Headings on a page. The more the number the smaller the heading.

### <b></b>
Makes everything inside __bold__.

### <em></em>
_Emphasises_ content inside of it.

### <title></title>
Hold info on page's name (will be displayed on the tab of yoru browser). Placed inside _head_ element.

### <link></link>
Used to connect files to your html page (like CSS files). Placed inside _head_ element.

### <meta>
Allows you to set some meta information about your html page. Placed inside _head_ element.

#### charset
Allows you to set the charset of your page, for example _utf-8_. 

### <button></button>
A clickable button.