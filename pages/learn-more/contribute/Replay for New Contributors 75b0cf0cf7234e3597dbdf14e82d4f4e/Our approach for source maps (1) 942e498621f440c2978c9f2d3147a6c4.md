# Our approach for source maps (1)

### What are source maps

Source maps are a file that build tools like [webpack](https://webpack.js.org/) create to map the "original" code you see in your editor to the "generated" code you see in browser DevTools. 

<aside>
💡 Evan Wallace's [Source Map Visualizer](https://evanw.github.io/source-map-visualization/) is a great tool for seeing how source maps work. And if you're curious about how maps are parsed and visualized, we recorded a [replay](https://app.replay.io/recording/dfa08836-dbcf-41c4-bd77-b3800d4f7e8b) of the visualizer 😉

</aside>

![Screen Shot 2021-10-05 at 8.57.12 PM.png](../How%20Replay%20Works%202adcccaf8f7743f3967a70fbfcc72c4f/Our%20approach%20for%20source%20maps%201f5afb2cf5ad45a488369e65a821039d/Screen_Shot_2021-10-05_at_8.57.12_PM.png)

### How do browser DevTools use source maps

Firefox and Chrome download the source maps and use the map to find original source text and source locations. Firefox goes one step further to also map original expressions so when you pause, the variables in the scope pane and expressions in the console just work.

### How Replay uses source maps

Replay takes source maps one step further so that everything defaults to the original experience and you can basically forget that your code was compiled at all. We do this by including the source map when recording, making it easy for teams to [upload source maps](https://www.notion.so/Uploading-Source-Maps-1923e679c1e4411db1bda29536eb1e31?pvs=21) to Replay at deploy time. 

From there, Replay uses source maps in the server so that the DevTools can have a seamless experience. When you are viewing a replay, you will always see the original file that's in your editor first and have the ability to jump back to the compiled file if you'd like. You can also add print statements and evaluate original expressions in the console.

![Screen Shot 2021-10-05 at 8.56.12 PM.png](../How%20Replay%20Works%202adcccaf8f7743f3967a70fbfcc72c4f/Our%20approach%20for%20source%20maps%201f5afb2cf5ad45a488369e65a821039d/Screen_Shot_2021-10-05_at_8.56.12_PM.png)

### How Replay maps variables

Replay maps variables by using [Babel.js](https://babeljs.io/) in the server to parse the original file and create a new original scope chain that is synthetically linked to the runtime's scope chain.

### How Replay maps expressions

Replay maps expressions by parsing the expression with Babel.js, finding the original tokens and looking up the generated expression counterparts. This sounds fancy, but most of the mappings are looking at an imported variable and getting the long webpack generated variable name.

### Where are source maps stored?

Source maps are stored in an S3 bucket and are encrypted at rest. For more, see our approach to [security and privacy](https://www.replay.io/security-privacy).