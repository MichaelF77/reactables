[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/GigaTables/reactables/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/GigaTables/reactables/?branch=master)
[![Build Status](https://scrutinizer-ci.com/g/GigaTables/reactables/badges/build.png?b=master)](https://scrutinizer-ci.com/g/GigaTables/reactables/build-status/master)
[![Code Coverage](https://img.shields.io/badge/coverage-%2083%25%20-green.svg)](https://img.shields.io/badge/coverage-%2083%25%20-green.svg)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# GigaTables ReactJS plug-in
GigaTables supports the following capabilities:

-- ajax data processing/editing (CRUD),

-- classic pagination or infinite scroll,

-- column sorting,

-- live cell edit,

-- common search (through all columns),

-- discrete (per column) search,

-- ajax files upload,

-- shft/ctrl rows selection,

-- fixed header,

-- trigger functions,

-- dynamic column content functions,

-- 7 popular languages,

-- data load for period interval,

-- hot keys,

-- plugins

and more...

* [Installation](#user-content-installation)
* [Few screen-shots](#user-content-few-screen-shots)
* [Installation based on browser script implementation](#user-content-installation-based-on-browser-script-implementation-which-u-can-download-from-build-dir)
* [Getting Started](#user-content-getting-started)
  * [Minimal configuration](#user-content-minimal-configuration)
  * [Advanced configuration with opts and editor](#user-content-advanced-configuration-with-opts-and-editor)  
* [An example of using GigaTables with Editor tool](#user-content-an-example-of-using-gigatables-with-editor-tool)
* [Pagination or Infinite scroll](#user-content-pagination-or-infinite-scroll)
* [Ajax autoload period](#user-content-ajax-autoload-period)
* [Hot keys](#user-content-hot-keys)
* [Plugins](#user-content-plugins)
* [Aggregate Footer](#user-content-aggregate-footer)
* [Data export](#user-content-data-export)
* [FAQ](#user-content-faq)

### Installation

- npm i gigatables-react

After installation will be completed add import to your `main.js` like this:

```JS
import { Reactables, Header } from 'gigatables-react'
```

see how to create table with JSX bellow.
## Few screen-shots

### Multiple select with Ctrl and Shift
![alt tag](https://raw.githubusercontent.com/GigaTables/reactables/master/screens/GigaTables_basic_view.png)

### CRUD pop-ups
![alt tag](https://raw.githubusercontent.com/GigaTables/reactables/master/screens/GigaTables_edit_item.png)

### Pagination + sorting
![alt tag](https://raw.githubusercontent.com/GigaTables/reactables/master/screens/GigaTables_pagination_with_sort.png)

### Delete items
![alt tag](https://raw.githubusercontent.com/GigaTables/reactables/master/screens/GigaTables_delete_items.png)

### One-click cell edit
![alt tag](https://raw.githubusercontent.com/GigaTables/reactables/master/screens/GigaTables_cell_edit.png)

### Form styling (css+fieldset/legend)
![alt tag](https://raw.githubusercontent.com/GigaTables/reactables/master/screens/GigaTables_form_styling.png)

## Installation based on browser script implementation (which U can download from build/ dir)
```HTML
    <script src="build/index.js"></script>
    // or
    <script src="wherever/whatever.js"></script>
```

## Getting Started

To initialize plug-in and to bind GigaTables with table structure - set settings prop with options and table structure in JSX format.
The `<Reactables>` tag, it's `Headers` and all the stuff like pagination, per page selector will be constructed on the fly.

### Minimal configuration

```JS

        var settings = {
          struct: {// all in
            search: ['top', 'bottom'],
            rowsSelector: ['asc', 'top', 'bottom'],
            pagination: ['bottom']
          },
          ajax: 'http://example.com/your/tabledata',
          columns: [
            {data: "id"},
            {data: "desc"},
            {data: "title"},
            {data: "date"},
            {data: "types"},
            {data: "info"}
          ]
        };   

```

### Advanced configuration with opts and editor

```JS
let settings = {
 struct: {
   search: ['top', 'bottom'],
   rowsSelector: ['asc', 'top', 'bottom'],
   pagination: ['bottom'],
   fixedHeader: true, // default false
   editableCells: true, // default false
 },
 lang: 'en', // english default
 perPageRows: [25, 50, 100, 200, 500],
 defaultPerPage: 50,
 ajax: 'https://example.com/your/tabledata',
 requestType: 'GET',
 columns: [
   {// include all defaults
     data: "id",
     sortable: true, // true by defualt
     visible: true, // true by defualt
     searchable: true, // true by defualt
     discreteSearch: true, // false by default
     discreteSearchValue: function (title) {
       return 'Search in field ' + title;
     }
   },
   {
     data: "title",
     cISearch: true // default false
   },
   {
     data: "desc",
     sortable: false,
     discreteSearch: true,
     discreteCISearch: true // default false
   },
   {
     data: "date",
     searchable: false
   },
   {
     data: "info"
   },
   {data:"field1"},
   {data:"field2"},
   {data:"field3", visible: false}
 ],
 columnOpts: [
   {
     render: function (data, row, type) {
       return '<div><form method="post" class="accForm" action=""><input type="hidden" name="action" value="forms" /><input type="hidden" name="id" value="' + row.id + '" /><div>' + data + '</div></form></div>';
     },
     target: 2
   },
   {
     render: function (data, row, type) {
       return '<div><form method="post" class="accForm" action=""><input type="hidden" name="action" value="forms" /><input type="hidden" name="id" value="' + row.id + '" /><div>' + data + '</div></form></div>';
     },
     target: 3
   }
 ],
 tableOpts: {
   buttons: [
     {extended: "editor_create", editor: editor, triggerAfter: (function () {
         console.log('after create');
       }), triggerBefore: (function () {
         console.log('before create');
       })},
     {extended: "editor_edit", editor: editor},
     {extended: "editor_remove", editor: editor, triggerAfter: (function () {
         console.log('after del');
       })}
   ],
   buttonsPosition: ['top', 'bottom'],
   theme: 'std'
 }
};
```        

The table is defined like in example below:

```JSX
ReactDOM.render(
  <Reactables editor={editor} settings={settings}>
    <Header data="id">ID</Header>
    <Header data="title">Name</Header>
    <Header data="desc">Description</Header>
    <Header data="date">Date</Header>
    <Header data="info">Info</Header>
    <Header data="field2">Field123 but data from field2</Header>
    <Header data="field1">Field1</Header>
    <Header data="field3">Field3 invisible</Header>
    <Header>Field3 invisible</Header>
  </Reactables>,
  document.getElementById('app'))
```

data attribute is needed to glue columns to json data steadily.

JSON structure to return from provided url in "ajax" GigaTables option:

```JS
{
    "rows": [
        {
            "GT_RowId": 2, // optional
            "id": 2, // if there is no GT_RowId - try to fetch "id"
            "title": "Test 2st row",
            "desc": "<input type=\"text\" name=\"ttl\" value=\"Test 2nd row Test 2nd row Test 2nd row
 Test 2st row Test 2st row\" \/> ",
            "date": "20:40:37 17:06:2015",
            "info": "some info some info some info some info"
        },
        {
            "GT_RowId": 1,
            "id": 1,
            "title": "Test 1st row",
            "desc": "<input type=\"text\" name=\"ttl\" value=\"Test 1st row Test 1st row Test 1st row
 Test 1st row Test 1st row\" \/> ",
            "date": "20:40:38 17:06:2015",
            "info": "some info some info some info some info"
        }, ...

```

## An example of using GigaTables with Editor tool

First of all You should define an object Editor like this:

```JS
let editor = {
    ajax: 'http://www.example.com/editor.php',
    // it is possible to set discrete urls and http methods for any type of request
    // ajax: {
    //     create: {
    //         url: 'http://www.example.com/create.php',
    //         type: 'POST',
    //     },
    //     edit: {
    //         url: 'http://www.example.com/edit.php',
    //         type: 'PUT',
    //     },
    //     delete: {
    //         url: 'http://www.example.com/delete.php',
    //         type: 'DELETE',
    //     },
    // },
    ajaxFiles: 'http://gigatables.loc/uploadFiles.php',
    struct: {
        buttons: ['top', 'bottom'] // buttons
    },
    fields: [
        {
            label: "ID",
            name: "id",
            type: 'hidden'
        },
        {// an example of using select - automatically selected if matches with data in table column
            label: "Types:",
            name: "types[]",
            values: [// if select,checkbox,radio etc types - need this pre-set structure of values
                {'key1': 'val1'},
                {'key2': 'val2'}
            ],
            type: 'checkbox', // select,checkbox,radio
        },
        {
            label: "Article title:",
            name: "title",
            type: 'text', // default, other: password, file, select, multiselect etc
            fieldsetOpen: true,
            legend: 'Required fields',
            attrs: [
                {'pattern': '^[A-Za-z0-9_]+$'},
                {'className': 'titleField'}
            ]
        },
        {
            label: "Description:",
            name: "desc",
            type: 'textarea',
            plugins: 'rte',
            attrs: [
                {'className': 'descriptionField'}
            ],
            fieldsetClose: true,
        },
        {
            label: "Date Time:",
            name: "date",
            type: 'date'
        },
        {
            label: "Image:",
            name: "image",
            type: 'file'
        },
    ]
};
```        

and then pass variable (in this case - editor) to GigaTables main options in tableOpts section like this:
```JS
          tableOpts: {
            buttons: [
              {extended: "editor_create", editor: editor},
              {extended: "editor_edit", editor: editor},
              {extended: "editor_remove", editor: editor}
            ],
            buttonsPosition: ['top', 'bottom'],
            theme: 'std'
          }
```

That's it then You will be able to CRUD any record You want.

## Pagination or Infinite scroll

It is sometimes convenient to use an infinite scroll rather then classic pagination, ex.: sort and see only part of data, watching an updated list etc.
To initialize an infinite scroll - set `infiniteScroll: true` property in `struct` object.

```JS
struct: {
  search: ['top', 'bottom'],
  rowsSelector: ['desc', 'top', 'bottom'],
  pagination: ['bottom'],
  infiniteScroll: true
},
```
even if You leave `pagination` option there - infinite scroll will take precedence.

## Ajax autoload period

If You need an autoload period set for online live-loaded grid data, just add 2 properties to settings object:

```JS
ajaxAutoloadData: true, // default false
ajaxAutoloadPeriod: 8, // sec
```

the `ajaxAutoloadPeriod` must be set in seconds and the interval should be placed between 5 and 300 seconds,
if U wish to switch the mode to classic loader without touching settings structure - set the `ajaxAutoloadData: false`.

## Hot keys
To efficiently manage table behavior You can use the following hot keys:

- Ctrl+Arrow Up - selects rows above the selected

- Ctrl+Arrow Down - selects rows below the selected

- Ctrl+A - selects all rows in a current table view

- Ctrl+Arrow Right - next page

- Ctrl+Arrow Left - prev page

PS In some OS like Macintosh can be default conflicting hot keys, ex.: Cmd+Arrow Left get back in browsers

## Plugins
Plugins can be helpful to build convenient features around form elements and table rows/columns. 
For instance - to edit text with HTML tags (aka WYSIWYG) and then save it in DB you can set the `rte` (Rich Text Editor):

```js
{
    ...
    type: 'textarea',
    plugins: 'rte',
    ...
}
``` 

Progress bar plugin:

```js
    ...
    data: "statistics",
    plugins: "progressbar",
    ...
```
this will build 5 colored (left-to-right) progress bar for every row in the column, with percent number.
Protected from `null` and `negative` numbers as well as `more then 100`.

## Aggregate Footer

To set an aggregate footer, to be able to collect statistical data with different functions/methods 
use the following property in `struct` object:
```js
    struct: {
        ...
        aggregateFooter: true
        ...
    }
```
then you can declare whatever function needed to be processed for particular columns like this:
```js
    {
        data: "title",
        ...
        footer: 'frequency',
        ...
    },
```
to get most frequently used sentences/words,
```js
    {
        data: "price",
        ...
        footer: "avg",
        ...
    },
```
to count an average value of all numbers in column,

```js
    {
        data: "amount",
        ...
        footer: "sum",
        ...
    },
```
there are also `minLength` - `maxLength` footer settings to get a string lengths.

## Data export

Sometimes users need to export data in different formats, the most popular is csv, so here we go:
- define `download` object with `csv` property in `settings->struct`
```js
    download: {
        csv: true,
    },
``` 

- define button object in `settings->tableOpts->buttons` like this:
```js
    {
        extended: "editor_csv",
        editor: editor,
    },
```
You'll see an extra button `CSV Export` on the left, right before standard buttons for CRUD. 

## FAQ

**Can I use file types in GT editor to upload files through AJAX on server?**

Sure, it can be done by this additional option in editor:

```JS
ajaxFiles: 'https://example.com/upload/files',
```

which is pointing on script on the server where it should upload the file(s).

And to add the actual field which will send the file to the server script:
```JS
    {
      label: "Image:",
      name: "image",
      type: 'file'
    }
```
Ensure that the field is in *fields: [* option. U can even specify:
```JS
attrs: [
  {'multiple': ''}
]
```   
to upload as many files as U need.

**What types of fields can be used in editor?**

You can use any HTML5 types, but responsibility for cross-browser support is on Your side.

**Can I choose more then one row?**

It is possible to even select not only the bunch of rows, but several bunches and some between them, by manipulating with:
Ctrl+Left click (one row anywhere) and Shift+Left click (several rows). Also U can use hot-keys to select above/below Ctrl+Arrow Up/Ctrl+Arrow Down
and Ctrl+A will select them all for current table-view.

**Is it possible to search for a particular column in GT?**

Specify any column U wanted to search by setting these options in *columns* parts eg.:

```JS
          columns: [
            {// include all defaults
              data: "id",
              discreteSearch: true,
              discreteSearchValue: function(title) {
                return 'Search on the field - ' + title;
              }              
            }
```

**Is there languages support?**

You can pick one of 7 languages: English, German, Russian, French, Spanish, Chinese, Hindi - en, gr, ru, fr, es, ch, hi respectively.   

**Does GT have any event-triggered functions, ex.: to run something before/after pop-up?**

GigaTables plug-in has 2 types of triggers **triggerBefore** and **triggerAfter** which can be applied to any action button - Create, Edit or Delete.

**Can I set case insensitive search to either common and/or discrete search?**

Yes. For common search You should choose for which particular column it is needed and place `cISearch: true` option there,
for discrete search use `discreteCISearch: true`.

**Does GT have an ability to fix header while scrolling?**

Just add:
```JS
  fixedHeader: true
```
to `struct` section of `settings` variable as in example above.

**Is there a way to edit cells in a real-time?**

`editableCells` option will transform table to per-cell edit bases:
```js
editableCells: true
```
Thus, You can click on any cell - edit the content and press Enter to send the AJAX request to the specified url.

**Is it possible to edit multiple rows?**

Sure, but you should understand the fact, that if you edit a field - it will be changed for all selected rows, 
those fields that were left unchanged will be sent in their current state.
 
**How can I set css styling to form fields?**

Simply by specifying:
```JS
attrs: [
  {'className': 'myClass1 myClass2'}
]
``` 
in the field settings, don't forget to `import` the styles file with global cascades like this: 
```JS
import './main.css'
``` 
you can see an example in `main.js` file.
 
For those who like `fieldset legend` form structure GT offers:
```js
    {
        ...
        fieldsetOpen: true,
        legend: 'Required fields',
        ...
    }
``` 
to open fieldset tag and set legend, to close previous fieldset in any further object use syntax like this:
```js
    {
        ...
        fieldsetClose: true,
        ...
    },
```

**How would GT behave if there will be > 1 000 000 rows data?**

GT will perform good, but you should consider to turn off all the extra settings, 
for instance with aggregate footer set - the data will be counted through those 1M rows. 
Sure it will aggregate values only once and then get the results from state, 
but the 1st page load will be longer than usual.

### Contribution gratitude

**@tanz-sullamora** (without this guy project would not be completed at all)

**@AndreiEnache** (solved problem with Exceptions)

**@GreenbBro** (helped with robust functionality and ES6 code-style)