# Blog Gatsby.js

You can find

This is a static blog generator and starter gatsby repo. A port of [Casper](https://github.com/TryGhost/Casper) v2 a theme from [Ghost](https://ghost.org/) for [GatsbyJS](https://www.gatsbyjs.org/) using [TypeScript](https://www.typescriptlang.org/).

Either disable subscribe or setup a mailchimp list and add the form action and hidden field input name.
This repository contains my blog and articles made with [Gatsby.js](https://www.gatsbyjs.org/). It also includes Google Analytics.

### To do
- [x] emotion / component styles
- [x] home page
- [x] tag page
- [x] author page
- [x] blog page
  - [ ] subscribe form - using [mailchimp](https://mailchimp.com)
  - [ ] floating reading progress bar
- [x] 404 page
- [x] subscribe modal/overlay
- [x] rss feed
- [x] polish ✨
  - [x] meta tags
  - [x] page titles
- [ ] manage social medias on author page


### Deploy to Netlify

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/scttcper/gatsby-casper)

## How to configure Google Analytics
Edit `gatsby-config.js` and add your tracking ID


```javascript
{
    resolve: `gatsby-plugin-google-analytics`,
    options: {
      // Here goes your tracking ID
      trackingId: 'UA-XXXX-Y',
      // Puts tracking script in the head instead of the body
      head: true,
      // IP anonymization for GDPR compliance
      anonymize: true,
      // Disable analytics for users with `Do Not Track` enabled
      respectDNT: true,
      // Avoids sending pageview hits from custom paths
      exclude: ['/preview/**'],
      // Specifies what percentage of users should be tracked
      sampleRate: 100,
      // Determines how often site speed tracking beacons will be sent
      siteSpeedSampleRate: 10,
    },
  },
```

## How to edit your site title and description 
Edit `gatsby-config.js` section `siteMetadata`

```javascript
 siteMetadata: {
    title: 'My awesome site name',
    description: 'This is a description for my site',
    siteUrl: 'https://mysite.com', // full path to blog - no ending slash
  },
```
