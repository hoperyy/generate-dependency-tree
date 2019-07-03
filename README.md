# Introduction

`get-dependency-tree` will help you get `entry` file's dependency tree/arr.

# Usage

You can run `npm run test` or write code in your project yourself like this:

```bash
npm i get-dependency-tree
```

```js
const { tree, arr } = require('get-dependency-tree')({
    entry: require('path').join(__dirname, 'test/src/index.vue'),
});

console.log(JSON.stringify(tree, null, '\t'));
console.log(arr);
```

logs:

```bash
{
        "xxx/test/src/index.vue": {
                "xxx/test/src/vue-dep.js": {},
                "xxx/test/src/import.css": {
                        "xxx/test/src/test.css": {
                                "xxx/test/src/import.css": {}
                        }
                }
        }
}


[ 
    'xxx/test/src/vue-dep.js',
    'xxx/test/src/import.css',
    'xxx/test/src/test.css' 
]
```

## Params

```js
require('get-dependency-tree')({
    // ...
});
```

+   `getDependencyTree: String | Required`
    +   default: `''`
    +   description

        It should be an absolute path
        
        entry file type supported for now: '.js' / '.vue' / '.css' / '.less' / '.sass' / '.scss'

+   `searchRoot: String | Not Required`
    +   default: `''`
    +   description

        It should be an absolute path.
        
        It's the final root folder when finding dependencies.
        
+   `extentions: Array | Not Required`
    +   default: `['.js', '.vue', '.less', '.scss']`
    +   description

        It's like webpack project: `require('a')` --> `require('a.js')`.
        
+   `babelPlugins: Array | Not Required`
    +   default

        ```js
        [
            '@babel/plugin-syntax-dynamic-import',
            '@babel/plugin-transform-typescript'
        ]
        ```
        
    +   description

        `get-dependency-tree` needs babel for compiling.
        
        When you set it, it will override default value.
        
+   `alias: Object | Not Required`
    +   default: `null`
    +   description

        It's like webpack config: `resolve.alias`
        
        For example: 
        
        ```js
        alias: { '@test': 'haha' }
        ``` 
        
        `require('@test/a') --> require('haha/a') --> get dep: 'haha/a'`
        
+   `filterOut: Function | Not Required`
    +   default

        ```js
        function filterOut(dependencyFilePath, { isNodeModules, exists }) {
            if (isNodeModules) {
                // This dependency is in node_modules
                // This dependency will not be in the result
                return true;
            }
    
            if (!exists) {
                // This dependency does not exist
                // This dependency will not be in the result
                return true;
            }
    
            // This dependency will be in the result
            return false;
        }
        ```
        
    +   description

        It is a result filter.
        
        The dependency will be in the result when it returns `true`.
        
    +   `onEveryDepFound: Function | Not Required`
        +   default

            ```js
            (absoluteFilePath) => {}
            ```
            
        +   description

            It's a callback when every dependency is found.
    
    +   `onFilteredInDepFound: Function | Not Required`
        +   default

            ```js
            (absoluteFilePath) => {}
            ```
            
        +   description

            It's a callback when filtered in dependency is found (These dependencies are in the result).
            
    +   `onFilteredOutDepFound: Function | Not Required`
        +   default

            ```js
            (absoluteFilePath) => {}
            ```
            
        +   description

            It's a callback when filtered out dependency is found (These dependencies are not in the result).

## Return

```js
{ tree, arr }
```

+   `tree` is the dependency tree Object
+   `arr` is also dependencies showed by plain

## LICENSE

BSD