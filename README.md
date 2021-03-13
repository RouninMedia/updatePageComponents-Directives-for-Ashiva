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

The first and second optional entries (`Main` & `Modules`) each take the following format:
 
```
{
  "Styles": true,   // [OPTIONAL]
  "Scripts": true,  // [OPTIONAL]
  "ESModules": true   // [OPTIONAL]
}                                      						
```

**Named Module** entries (`Module_Name_1`, `Module_Name_2` etc.), also optional, may include references to *any* component they contain, including customComponents:

```
{
  "Data": true,
  "Markup": true,
  "Styles": true
  "customComponent_1": true
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
______

## Where can updatePageComponents Directives be added?

The **updatePageComponents** queryString Parameter may *only* be added to the **Page URL**:

`example.com/my-page/?updatePageComponents=%7B%7D`

To add to `/my-page/` simply add the **updatePageComponents** queryString Parameter:
 
   - to the end of a link
   - to the end of the URL in the browser URL bar

_______

## Example of `updatePageComponents` Values

**updatePageComponents:** *{"Main": {"Styles": true, "Scripts": true, "ESModules": true}, "Modules": {"Styles": true, "Scripts": true, "ESModules": true}}*

`?updatePageComponents=%7B%22Main%22%3A%7B%22Styles%22%3Atrue%2C%22Scripts%22%3Atrue%2C%22ESModules%22%3Atrue%7D%2C%22Modules%22%3A%7B%22Styles%22%3Atrue%2C%22Scripts%22%3Atrue%2C%22ESModules%22%3Atrue%7D%7D`

______

## `updatePageModules` Directives :: Functions in Core

### `replaceModules()`

```php
function replaceModules($Asset, $replacementModules, $Module_List) {

  $Module_List = [];
  
  for ($i = 0; $i < count($replacementModules); $i++) {
      
    if ((isset($replacementModules[$i][$Asset]['apply'])) && ($replacementModules[$i][$Asset]['apply'] === FALSE)) continue;

    $Module_List[] = $replacementModules[$i];
  }
  
  return $Module_List;
}
```

### `addModules()`

```php
function addModules($Asset, $modulesToAdd, $Module_List) {
  
  for ($i = 0; $i < count($modulesToAdd); $i++) {
      
    if ((isset($modulesToAdd[$i][$Asset]['apply'])) && ($modulesToAdd[$i][$Asset]['apply'] === FALSE)) continue;

    if (isset($modulesToAdd[$i][$Asset]['insert'])) {

      if (is_string($modulesToAdd[$i][$Asset]['insert'])) {

        switch ($modulesToAdd[$i][$Asset]['insert']) {

          case ('first') : array_unshift($Module_List, $modulesToAdd[$i]); break;

          case ('last') : array_push($Module_List, $modulesToAdd[$i]); break;
        }
      }

      elseif (is_array($modulesToAdd[$i][$Asset]['insert'])) {

        $insertPosition = count($Module_List);

        for ($j = 0; $j < count($Module_List); $j++) {

          if ($Module_List[$j]['Module'] === array_values($modulesToAdd[$i][$Asset]['insert'])[0]) {

            switch (array_keys($modulesToAdd[$i][$Asset]['insert'])[0]) {

              case ('before') : $insertPosition = $j; break;

              case ('after') : $insertPosition = ($j + 1); break;
            }

            break;
          }
        }

        array_splice($Module_List, $insertPosition, 0, [$modulesToAdd[$i]]);
      }
    }

    else {
    
      $Module_List[] = $modulesToAdd[$i];
    }
  }
  
  return $Module_List;
}
```

### `removeModules()`

```php
function removeModules($Asset, $modulesToRemove, $Module_List) {
  
  for ($i = 0; $i < count($modulesToRemove); $i++) {
      
    $Module_Name = $modulesToRemove[$i]['Module'];
    $Module_Publisher = $modulesToRemove[$i]['Publisher'];
    if ((isset($modulesToRemove[$i][$Asset]['apply'])) && ($modulesToRemove[$i][$Asset]['apply'] === FALSE)) continue;
    
    for ($j = (count($Module_List) - 1); ($j + 1) > 0; $j--) {
        
      if ($Module_List[$j]['Module'] !== $Module_Name) continue;
      if ($Module_List[$j]['Publisher'] !== $Module_Publisher) continue;
      
      unset($Module_List[$j]);
    }
    
    $Module_List = array_values($Module_List);
  }
  
  return $Module_List;
}
```

______


## `updatePageModules` Directives :: Processor in Module Stylesheet / Module Scriptsheet

```php

$Module_List = $PageManifest['Ashiva_Page_Build']['Modules'];

// echo '/*'."\n\n".'PAGE MODULE LIST:'."\n\n".json_encode($Module_List, JSON_PRETTY_PRINT)."\n\n";

if (isset($_GET['updatePageModules'])) {
  
  parse_str(parse_url($_SERVER['REQUEST_URI'])['query'], $Query_String_Array);
  $updatePageModules = $Query_String_Array['updatePageModules'];
  $updatePageModulesArray = json_decode($updatePageModules, TRUE);


  $Update_Page_Modules_Directives = ['replaceModules', 'addModules', 'removeModules'];

  for ($i = 0; $i < count($Update_Page_Modules_Directives); $i++) {

    $Directive = $Update_Page_Modules_Directives[$i];
    $Directive_Data = $updatePageModulesArray[$Directive];

    if (isset($Directive_Data)) {

      $Module_List = $Directive('Scripts', $Directive_Data, $Module_List);

      if ($Directive === 'replaceModules') break;
    }
  }

  if ((isset($updatePageModulesArray['customOrder']['Scripts'])) && ($updatePageModulesArray['customOrder']['Scripts'] === TRUE)) {

    $Module_List = array_values($Module_List);
  }

  else {

    foreach ($Module_List as $Index => $Module_Data) {
    
      $Publisher_Order[$Index]  = $Module_Data['Publisher'];
      $Module_Order[$Index] = $Module_Data['Module'];
    }

    array_multisort($Publisher_Order, SORT_ASC, $Module_Order, SORT_ASC, $Module_List);
  }
}

// echo 'FINAL MODULE LIST:'."\n\n".json_encode($Module_List, JSON_PRETTY_PRINT)."\n\n".'*/'."\n\n";


${$Page.'::Modules'} = getModules($Module_List);

```
