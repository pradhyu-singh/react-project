
import { lazy } from 'react-project'

// bundle loader returns a function here that will load `Dashboard`
// lazily, it won't be in the initial bundle
import loadDashboard from 'bundle?lazy!./Dashboard'

// now wrap that load function with `lazy` and you're done, you've got
// super simple code splitting, the dashboard code won't be downloaded
// until the user visits this route
<Route getComponent={lazy(loadDashboard)}/>

// just FYI, `lazy` doesn't do anything other than wrap up the callback
// signatures of getComponent and the bundle loader. Without `lazy` you
// would be doing this:
<Route getComponent={(location, cb) => {
  loadDashboard((Dashboard) => cb(Dashboard.default))
}}/>
```

#### `ServerRoute`

Defines a route to only be available on the server. Add handlers
(functions) to the different http methods.

**Note:** You have to restart the server after making changes to server
routes. But only until somebody implements HMR for the server.

You can nest routes to get path nesting, but only the final matched
route's handler is called (maybe we could do something cool later with
the handlers?)

```js
import { ServerRoute } from 'react-project/server'
import {
  listEvents,
  createEvent,
  getEvent,
  updateEvent,
  deleteEvent
} from './events'

export default (
  <Route path="/api">
    <ServerRoute path="events"
      get={listEvents}
      post={createEvent}
    >
      <ServerRoute path=":id"
        get={getEvent}
        patch={updateEvent}
        delete={deleteEvent}
      />
    </ServerRoute>
  </Route>
)
```

#### `serverRouteHandler(req, res, { params, location, route })`

- `req` an express request object
- `res` an express resonponse object
- `params` the url parameters
- `location` the matched location
- `route` the matched server route


### `react-project/server`

#### `createServer(getApp)`

```
import { createServer } from 'react-project/server'

createServer((req, res, cb) => {
  cb(null, { renderDocument, renderApp, routes })
}).start()
```

Creates and returns a new [Express][express] server, with a new
`start` method.

##### `renderDocument(props, callback)`

App-supplied function to render the top-level document. Callback with
a [`Document`][Document] component. You'll probably want to just tweak
the `Document` component supplied by the blueprint.

`callback(err, reactElement, initialState)`

- `reactElement` is the react element to be rendered
- `initialState` is initial state from the server for data re-hydration
  on the client.

##### `renderApp(props, callback)`

App-supplied function to render the application content. Should call
back with `<RouterContext {...props}/>` or something that renders a
`RouterContext` at the end of the render tree.

`callback(err, reactElement)`

If you call back with an error object with a `status` key, the server
will respond with that status:

`callback({ status: 404 })`

##### `routes`

The app's routes.


### `react-project` CLI

It's not intended that you use this directly, task should be done with
npm scripts.

#### `react-project init`

Initializes the app, copies over a blueprint app, updates package.json
with tasks, etc.

#### `react-project build`

Builds the assets, called from `npm start`, not normally called
directly.

#### `react-project start`

Starts the server. Called from `npm start`, not normally called
directly.

#### `react-project --help`

[NOT IMPLEMENTED][ni]

#### `react-project --version`

[NOT IMPLEMENTED][ni]



  [ni]:/CONTRIBUTING.md
  [express]:http://expressjs.com/
  [Document]:#TODO
  [bundle-loader]:https://github.com/webpack/bundle-loader
  [router]:https://github.com/reactjs/react-router
  [hmr-server]:#hmr-server
  [react]:https://facebook.github.io/react/
  [babel]:https://babeljs.io/
  [es2015]:https://babeljs.io/docs/learn-es2015/
  [react-preset]:https://babeljs.io/docs/plugins/preset-react/
  [stage1]:https://babeljs.io/docs/plugins/preset-stage-1/
  [babelrc]:https://babeljs.io/docs/usage/babelrc/
  [es5]:https://github.com/es-shims/es5-shim#shims
  [promise]:https://github.com/stefanpenner/es6-promise
  [fetch]:https://github.com/github/fetch
  [express]:http://expressjs.com/
  [serverroute]:#serverroute
  [lazy]:#lazy
  [documenttitle]:https://github.com/ryanflorence/react-title-component
  [splitting]:https://webpack.github.io/docs/code-splitting.html
  [hmr]:https://webpack.github.io/docs/hot-module-replacement-with-webpack.html
  [loaders]:https://webpack.github.io/docs/loaders.html
  [babel-loader]:https://github.com/babel/babel-loader
  [cssmodules]:https://github.com/css-modules/css-modules
  [jsonloader]:https://github.com/webpack/json-loader
  [urlloader]:https://github.com/webpack/url-loader
  [compression]:https://github.com/expressjs/compression
  [uglify]:https://github.com/mishoo/UglifyJS2
  [caching]:http://webpack.github.io/docs/long-term-caching.html
  [urlloader]:https://github.com/webpack/url-loader
  [karma]:https://karma-runner.github.io/0.13/index.html
  [mocha]:https://mochajs.org/


