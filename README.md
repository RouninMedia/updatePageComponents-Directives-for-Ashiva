# updatePageComponents Directives for Ashiva
Ashiva **updatePageComponents** Directives are a powerful tool enabling the switching on and off of Styles, Scripts or any other **Da3SH Component** on any web page in only a couple of keystrokes and without ever leaving the browser.

The visitor may manually edit the **updatePageComponents** Directives in any URL's `queryString` to add or remove _Main_ or _Modular_

 - `styles`
 - `scripts`
 - `markup`
 - `vectors`
 - `data` etc.

to or from any **Ashiva** webpage running **Da3SH** modules.

______

## How do `updatePageComponents` Directives work?

In **Ashiva**, you can add or remove modules on a given page (or stylesheet, or scriptsheet) using the **queryString Parameter**:

`updatePageComponents`

The **queryString Parameter** `updatePageComponents` has a percent-encoded JSON value, which, when decoded, looks like:

{
  "replaceModules": [],   // [OPTIONAL]

  "removeModules": [],    // [OPTIONAL]

  "addModules": [],       // [OPTIONAL]

  "customOrder": {"Styles": false, "Scripts": false}  // [OPTIONAL]	// <= either value, if omitted, defaults to false
}


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
