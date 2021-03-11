# updatePageComponents Directives for Ashiva
Ashiva **updatePageComponents** Directives are a powerful tool enabling the switching on and off of Styles, Scripts or any other **Da3SH Component** on any web page in only a couple of keystrokes and without ever leaving the browser.

The visitor may manually edit the **updatePageComponents** Directives in any URL's `queryString` to add or remove main or modular styles, scripts, markup, vectors, data etc. to or from any **Ashiva** webpage running **Da3SH** modules.

______

## Complete Example of `updatePageComponents`

```
{
  "Main": {
  
    "Styles": true,
    "Scripts": true
  },

  "Modules" {
  
    "Styles": true,
    "Scripts": true,
    "ESModules": true
  },
  
  "SB_Language_Selector": {
    
    "Markup": true,
    "Styles": true,
    "Scripts": true
  },

  "SB_Colour_Charts": {

    "Data": true,
    "Markup": true,
    "Styles": true
  }
}
```
