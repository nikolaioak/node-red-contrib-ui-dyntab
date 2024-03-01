
# node-red-contrib-ui-dyntab

## Overview

- This node was forked from Gippsman2017/node-red-contrib-ui-etable:master.
- I forked the stale repository to add functionality for multiple call-backs, including selecting selector rows, adding function calls to trigger adding or deleting rows, and moving rows.
- This node is based on the capabilities of the [Tabulator](https://tabulator.info) library, current release is aligned with v5.6.1. Refer to the Tabulator documentation for data, options, and column configuration. 
- I've provided some examples below for reference.

## Usage

Pass input data in `msg.payload`. Note that options and columns can also be passed in via `msg`: 

    msg = {
        payload: {
            action: "...",
            data: [...],
        },
        config: {
            options: {...},
            columns: [...]
        }
    }

### Interaction

Callbacks handled are:

- If the cell has `"editor": true` and it was edited, a message is sent with `callback: cellEdited`.
- If the cell has `"editor": false` or a cell was selected, a message is sent with `callback: cellClick`.
- If rows are movable, a message is sent with `callback: rowMoved`.

### Data Manipulation

Data in tables can currently be manipulated in a few ways, by passing in values to the `action` node of the `msg.payload`.

This allows the following actions per Tabulator functionality:

#### [Loading Data](https://tabulator.info/docs/5.6/data#overview)

This is the default action, which creates the table. To initialize the table, set the `action` to `init`. If an action is not provided in the `msg.payload`, this action is taken.

#### [Adding Data](https://tabulator.info/docs/5.6/update#alter-add)

To add data to the existing table, set the `action` to `addData`. The data included will be appended to the bottom of the table by default. The `addRowPos` option allows you to set this to add the data to the top of the table with a value of `top`.

#### [Adding Rows](https://tabulator.info/docs/5.6/update#addrow)

To add rows to the existing table, set the `action` to `addRows`. In this scenario, the data object should be an integer, which will determine how many rows are added. Similar to adding data, the `addRowPos` option allows you to set this to add the data to the top of the table with a value of `top`.

#### [Deleting Rows](https://tabulator.info/docs/5.6/update#row)

To delete rows from the existing table, set the `action` to `delete`. In this scenario, the data object should be an integer or an array of integers which correspond to the [Row Component(s)](https://tabulator.info/docs/5.6/components#component-row). To provide these, you'll need to capture the cellClick callbacks to determine which rows are selected for deletion or come up with another way to determine the row component details.

#### [Emptying the entire table](https://tabulator.info/docs/5.6/update#alter-empty)

To empty the entire table, set the `action` to `empty`. In this scenario, the data object can be an empty array.

## Examples

### Options

    {
        "movableColumns": false,
        "resizableColumns": true,
        "selectableRows": true,
        "responsiveLayout": "collapse",
        "autoResize": true,
        "layout": "fitColumns",
        "pagination": "local",
        "paginationSize": 20,
        "paginationCounter": "rows",
        "height": "400px"
    }

Refer to [Setup Options](https://tabulator.info/docs/5.6/options) for a more comprehensive list.

### Columns

    [		
        {
            "formatter": "rowSelection",
            "titleFormatter": "rowSelection",
            "align": "left",
            "headerSort": false,
            "width": "5px"
        },
        {
            title:"Name", 
            field:"name", 
            editor:"input"
        },
        {
            title:"Task Progress", 
            field:"progress", 
            align:"left", 
            formatter:"progress", 
            editor:true
        },
        {
            title:"Favorite Editor", 
            field:"favEditor", 
            width:95, 
            editor:"select", 
            editorParams:{
                values:["Code", "vim", "nano", "other"]
            }
        },
        {
            title:"Rating", 
            field:"rating", 
            formatter:"star", 
            align:"center", 
            width:100, 
            editor:true
        },
        {
            title:"Color", 
            field:"col", 
            width:130, 
            editor:"input"
        },
        {
            title:"Date Of Birth", 
            field:"dob", 
            width:130, 
            sorter:"date", 
            align:"center"
        },
        {
            title:"Driver", 
            field:"car", 
            width:90,  
            align:"center", 
            formatter:"tickCross", 
            sorter:"boolean", 
            editor:true
        }
    ]

I tried to provide most/all column types here. Refer to [Column Setup](https://tabulator.info/docs/5.6/columns) for a more comprehensive list.