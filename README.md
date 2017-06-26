# Hello Tesl
Hello Tesl is a bunch of libraries created as an important addition to Tesl language engine. It's not necessary for Tesl to work, but it's a good start if you are new to the language.

## Included libraries
### v2
`v2` is a library that holds the most important functions that you will use, i.e. `fun` for function declaration, `def` for defining variables and `import`/`export` for importing files.

It requires a little setup before you can use it - Tesl doesn't care if you run it through `node.js` or `webpack`, but it needs a way to get contents of imported files or parse the files.

This is the preferred way of setting up `v2` (in this example using `webpack`):
```js
const { Tesl } = require('tesl')
const { createV2, web } = require('hello-tesl')

const input = require('./src/index.tesl')
// You can also write this if in node environment:
//   const fs = require('fs')
//   const input = fs.readFile('./src/index.tesl')

function createContext() {
  const tesl = new Tesl()
  
  tesl.provide('v2', createV2(function (filename) {
    return require(`./src/${filename}.tesl`)
    // Same as above, you can use fs.readFile or whatever other method to get the desired file
  }, createContext))

  tesl.provide('web', web)

  return tesl
}

let ctx = createContext()
ctx.execute(input)

```
### web
`web` is a library that is dependent on a browser environment and provides a way to access DOM, create HTML elements or attributes. It also exposes `prog` command, which really simplifies the process of creating SPAs or widgets embedded in other frameworks.

```clojure
(.
  (prog.dispatcher ; here we define our actions
    :changeText (fun [state:object dispatch:function text:string]:void
      '(@ state :key text)
    )
  )
  (prog "#app" ; this is a selector of the element we want to inject our app into
    (data.object ; here we declare our model
      :key "hello"
    )

    '(html.div (state :key) " world!" ; and here we declare our view
      (html.button "say goodbye!"
        (dom.listen :click (dispatch :changeText "bye"))
      )
    )
  )
)
```

Every time we update `state`, view rerenders automatically the portions of the dom that changed (using `virtual-dom` library).

### data
`data` contains necessary functions to manipulate data and tpes. With it you can create objects and lists, transform them, convert from one type to another and many others. It also contains advanced mathematical stuff.

# License
MIT