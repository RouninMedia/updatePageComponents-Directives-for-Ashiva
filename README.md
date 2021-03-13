# updatePageComponents Directives for Ashiva
Ashiva **updatePageComponents** Directives are a powerful *in-browser tool* enabling the switching on and off of Styles, Scripts or any other **Da3SH Component** on any web page in only a couple of keystrokes, without ever leaving the browser.

The visitor may manually edit the **updatePageComponents** Directives in any URL's `queryString` to add or remove _Main_ or _Modular_

 - `styles`
 - `scripts`
 - `markup`
 - `vectors`
 - `data` etc.

to or from any **Ashiva** webpage running **Da3SH** modules.

______

## How do `updatePageComponents` Directives work?

In **Ashiva**, you can switch components on and off on a given page using the **queryString Parameter**:

`updatePageComponents`

The **queryString Parameter** `updatePageComponents` has a percent-encoded JSON value, which, when decoded, looks like:

```
{
  "Main": {},           // [OPTIONAL]

  "Modules": {},        // [OPTIONAL]

  "Module_Name_1": {},  // [OPTIONAL]

  "Module_Name_2": {}   // [OPTIONAL]
}
```

The first and second optional parameters (`Main` & `Modules`) each take the following format:
 
```
{
  "Styles": true,   // [OPTIONAL]
  "Scripts": true,  // [OPTIONAL]
  "ESModules": true   // [OPTIONAL]
}                                      						
```


______

## Complete Example of `updatePageComponents`

```
{
  "Main": {
  
    "Styles": true,
    "Scripts": true,
    "ESModules": true
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
