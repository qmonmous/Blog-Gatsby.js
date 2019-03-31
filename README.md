# Blog Gatsby.js

Check out my blog [here](https://quentin-monmousseau.netlify.com) !

### Technos:
- [Gatsby.js](https://www.gatsbyjs.org/)
- Node.js
- Typescript

### Features:
- Google Analytics
- Mailchimp (Either disable subscribe or setup a mailchimp list and add the form action and hidden field input name.)

### To do:
[x] add all social medias  
[ ] subscribe form using [mailchimp](https://mailchimp.com)  
[ ] floating reading progress bar

### How to configure Google Analytics
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