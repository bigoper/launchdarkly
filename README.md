# launchdarkly
Take home assignment for launchdarkly.

## Launchdarkly & GatbyJS
We'll be implementing launchdarkly Client-side SDK with GatsbyJS.  

This demo will show you how you can enable/disable (how/hide) content from your users/visitor by activating/disabling a Feature Flag.

## LaunchDarkly
### Account Creation and FeatureFlags
- Sign-up for a [new account][3].
- Create the following feature-flags:
    - Feature flag 1
        - Name: `Debug`
        - Key: `debug` 
        - Variation: `boolean`
    - Feature flag 2
        - Name: `Display Social Media Links`
        - Key: `display-social-media-links` 
        - Variation: `boolean`

> Make sure to note the environment used for these `flags`, you'll need the relevent `Client-side ID` later on.

## GatsbyJS
### __Prerequisits__
You should get yourself familiar with GatsbyJS framework.
In order for this demo to work, you should first set up your [Development Environment][1].

### __Create a Gatsby Site__
Once your local Development Environment is ready, it is time to create your first website.  
We'll be using Gatsby's [Hello World][2] example.

### __gatsby-plugin-launchdarkly__
Now you'll need to add [`gatsby-plugin-launchdarkly`][4] to your Gatsby website.
Once added, you'll need to update your Gastby configuration (gatsby-config.js) with the correct information.

Assuming you/re following this guide, your `gatsby-config.js` file should look like that:
```js
module.exports = {
  /* Your site config here */
  plugins: [
    {
      resolve: 'gatsby-plugin-launchdarkly',
      options: {
        clientSideID: 'xxxxxxxxxxxxxxxxxxxxxxxx',
        options: {
          // any LaunchDarkly options you may want to implement
          bootstrap: 'localstorage', // caches flag values in localstorage
        },
      },
    },
  ],
}
```
> make sure to update `clientSideID` with your ID. Your ID can be found under `Account settings > Projects > Environment (production/etc) > Client-side ID`.


### __Integrate `gatsby-plugin-launchdarkly`__ in your website
We'll be updating the home page (pages/index.js) to utilize launchdarkly client-side SDK.

Replace your `index.js` with the following code:
```js
import React from "react"
import { useFlags } from 'gatsby-plugin-launchdarkly'

export default function Home() {

  const flags = useFlags()

  const DebugFlags = (props) => {
    const { show } = props
    if (show) {
      console.log("show", show)
      return (
        <pre>{JSON.stringify(flags, null, 2)}</pre>
      )
    }
    return ''
  }

  const MySocialMediaLinks = (props) => {

    const { show } = props

    if (show) {
      return (
        <>
          <div><a href="/">facebook</a></div>
          <div><a href="/">Instagram</a></div>
          <div><a href="/">LinkedIn</a></div>
        </>
      )
    }
    return ''

  }
  
  return (
    <>
      <div>Hello world!</div>
      <MySocialMediaLinks show={flags.displaySocialMediaLinks} />
      <DebugFlags show={flags.debug} />
    </>
  )
}

```
> `src/pages/index.js`

## Testing
Our updated `index.js` page contains the followings:
- inclusion of the `gatsby-plugin-launchdarkly` SDK
- `MySocialMediaLinks` component
- `Debug` component

By "playing" (enabling/disabling) with the mentioned Feature flags, and refreshing the page (Home page) you can see how our code reflects/interactes with the changes.


[1]: https://www.gatsbyjs.com/docs/tutorial/
[2]: https://www.gatsbyjs.com/docs/tutorial/part-zero/#create-a-gatsby-site
[3]: https://app.launchdarkly.com/signup
[4]: https://github.com/launchdarkly/gatsby-plugin-launchdarkly#readme
