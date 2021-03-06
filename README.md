# jQXel
TypeScript/jQuery plugin to convert an array of JSON objects into an Excel-like UI

### Edit your data tables in a web-based, Excel-like user interface

## FEATURES
* Smooth spreadsheet look and feel
* Arrow/tab key navigation
* Editable content
* Cell, row, and column change events
* Automatic row serialization
* Toolbar options to add, copy, and delete rows
* Copy multiple rows to your clipboard
* Select list, text, HTML support

## EXAMPLES
### Create your columns

    var column1Options = new Array();
    column1Options.push($('<option/>').val('1').text('Option 1'));
    column1Options.push($('<option/>').val('2').text('Option 2'));
    column1Options.push($('<option/>').val('3').text('Option 3'));
    column1Options.push($('<option/>').val('4').text('Option 4'));
    column1Options.push($('<option/>').val('5').text('Option 5'));
    var headers = [
        { text: 'Column 0', editable: false, type: 'text' },
        { text: 'Column 1', editable: true, type: 'select', options: column1Options },
        { text: 'Column 2', editable: true, type: 'text' },
        { text: 'Column 3', editable: true, type: 'text' },
        { text: 'Column 4', editable: true, type: 'text' },
        { text: 'Column 5', editable: true, type: 'text' },
        { text: 'Column 6', editable: true, type: 'text' },
        { text: 'Column 7', editable: true, type: 'text' },
        { text: 'Column 8', editable: true, type: 'text' }
      ];


### Populate your table
       var data = [];
        data.push({
            data: [
                { text: '0,1', value: '0,1', editable: 'false', entityId: 1, name: 'Text' },
                { text: '1,1', value: '1,1', editable: 'true', entityId: 1, name: 'Text1' },
                { text: '2,1', value: '2,1', editable: 'true', entityId: 1, name: 'Text2' },
                { text: '3,1', value: '3,1', editable: 'true', entityId: 1, name: 'Text3' },
                { text: '4,1', value: '4,1', editable: 'true', entityId: 1, name: 'Text4' },
                { text: '5,1', value: '5,1', editable: 'true', entityId: 1, name: 'Text5' },
                { text: '6,1', value: '6,1', editable: 'true', entityId: 1, name: 'Text6' },
                { text: '7,1', value: '7,1', editable: 'true', entityId: 1, name: 'Text7' },
                { text: '8,1', value: '8,1', editable: 'true', entityId: 1, name: 'Text8' },
            ],
            entityId: 1,
            idName: 'Id'
            }
        );
        data.push({
            data: [
                //NOTE: The HTML property takes an HTMLElement object. Two of the proper methods of passing in an HTMLElement:
                // 1. Create a jQuery object and cast to an HTML object - $('<div/>').text('Hi!')[0] or $('<div/>').text('Hi!').get(0)
                // 2. Create an HTML object via raw JavaScript - var div = document.createElement('div'); div.innerText = 'Hi!';
                
                { text: '0,2', value: '0,2', editable: 'false', entityId: 2, name: 'Text', html: $('<div/>').text('Hi!')[0] },
                { text: '1,2', value: '1,2', editable: 'true', entityId: 2, name: 'Text1' },
                { text: '2,2', value: '2,2', editable: 'true', entityId: 2, name: 'Text2' },
                { text: '3,2', value: '3,2', editable: 'true', entityId: 2, name: 'Text3' },
                { text: '4,2', value: '4,2', editable: 'true', entityId: 2, name: 'Text4' },
                { text: '5,2', value: '5,2', editable: 'true', entityId: 2, name: 'Text5' },
                { text: '6,2', value: '6,2', editable: 'true', entityId: 2, name: 'Text6' },
                { text: '7,2', value: '7,2', editable: 'true', entityId: 2, name: 'Text7' },
                { text: '8,2', value: '8,2', editable: 'true', entityId: 2, name: 'Text8' },
            ],
            entityId: 2,
            idName: 'Id'
            }
    );

### Create your spreadsheet
    $('#container').jQXel({
        headers: headers,
        footer: footer,
        data: data,
        toolbarOptions: {
            add: true,
            copy: true,
            insert: true,
            includeRowNumbers: true,
            position: 'top'
        },
        beforeCellChange: beforeCellChange
      });

      function beforeCellChange(cell) {
            $.ajax({
                url: '/Home/Post',
                type: 'POST',
                data: { model: cell.getRowObject() },
                async: true
             }).done(function (results) {
                //do something
            }).fail(function (jqXHR, textStatus, errorThrown) {
                cell.alert('Something bad happened');
            });
      }

# DOCUMENTATION

### ThemeOptions
* color (string) - blue or green
* style (string) - not even sure right now

### ToolbarOptions
* add (Boolean) - If true, renders the "Add Row" toolbar button
* copy (Boolean) - If true, renders the "Copy Selection" toolbar button
* insert (Boolean) - If true, renders the "Insert Row" toolbar button
* position (string) - Indicates the toolbar position - top, bottom, left, right
* includeRowNumbers (Boolean) - If true, displays an auto-numbered header cell on each row

### JSONTable
* headers (Array<JSONHeader>) - The header cells passed on initialization
* footer (Array<JSONData>) - The footer cells passed on initialization
* className (string) - The collection of class names to apply to the table container (space delimited)
* selectedCell (SelectedCell) - The cell object designated when a cell is active
* highlightedRows (Array<JSONRow>) - An array of JSONRow objects for the highlighted rows
* getClipboardText() {string} - Returns an array of JSONRow objects for the currently highlighted rows
* clearSelected() {void} - De-activates any currently active cells
* select(cell: HTMLDivElement) {void} - Creates the SelectedCell object and designates the cell passed in as the only selected cell
* moveDown() {void} - Moves the currently active cell down 1 row
* moveUp() {void} - Moves the currently active cell up 1 row
* moveLeft() {void} - Moves the currently active cell left 1 column
* moveRight() {void} - Moves the currently active cell right 1 column
* returnRow(rowIndex: number) {JSONRow} - Returns the JSONRow object for the specified index
* returnColumn(cellIndex: number) {Array<JSONData>} - Returns an array of JSONData objects for the specified index
* toggleHighlight(index: number) {void} - Toggles the specified row's highlighted status

#### Dynamically create your own rows
* appendRow() {void} - Adds a new row to the bottom of the table
* createRow(isHeader: boolean, isFooter: boolean, index: number) {HTMLDivElement} - Creates a returns an HTML div element with jQXel classes applied (for use with functions below)
* createRowHeaderCell(rowIndex: number) {HTMLDivElement} - Creates and returns the header cell with mouse events (mousedown = highlight) already bound
* createCell(cellData: JSONData, index: number, type: string) {HTMLDivElement} - Creates and returns a cell based on the JSONData object passed to the function

#### Events
* onCopy(e: ClipboardEvent, highlightedRows: Array<JSONRow>)
* beforeRowChange(rowData: Object, selectedCell: SelectedCell)
* beforeColumnChange(colData: Array<JSONData>, selectedCell: SelectedCell)
* beforeCellChange(selectedCell: SelectedCell)
* onjQXelReady(e: Event, context: JSONTable)

### JSONHeader
* text (string) - The display text
* editable (string) - Indicates whether the entire column is editable (may be overridden on the cell level)
* type (string) - The type of input for this column - text, select
* name (string) - This is currently not being used
* options (Array<HTMLOptionElement>) - Available options for cells of type 'select' in this column
* className (string) - The collection of class names to apply to the column header (space delimited)

### JSONRow
* entityId (number) - The row identifier
* idName (string) - The property name of the identifier. Used for serialization
* data (Array<JSONData>) - The row data
* className (string) - The collection of class names to apply to the table row (space delimited)

### JSONData
* text (string) - The display text of the cell
* value (string) - The value of the cell
* editable (string) - Acts as an override for the column's 'editable' field
* entityId (number) - The identifier/primary key for the object
* name (string) - The name of the editable field. Used for serialization of JSON objects in change events
* html (string?) - If present, the HTML provided will be inserted into the cell
* className (string) - The collection of class names to apply to the cell (space delimited)

### SelectedCell
* cell (HTMLDivElement) - The active cell element
* html (HTMLElement) - The HTML content displayed in the cell
* text (string) - The text to be displayed in the cell
* val (string) - The editable value of the cell. When focused, the display value (text/HTML) will be replaced with the editable value.
* alert(message: string) {void} - Adds the jql-alert class to the parent row element. If the message parameter is present, set the parent row's title attribute
* removeAlert() {void} - Removes the jql-alert class and parent row's title attribute
* getRowIndex() {number} - Returns the index of the parent row
* getRowValues(includeRowHeader: Boolean) - Returns a string array of all cell values in the row.
* getRowObject() {Object} - Serializes all cell values in the row into a JSON object
* select(type: string, options: Array<HTMLOptionElement>) {void} - Selects this cell
