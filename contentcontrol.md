# ContentControl object (JavaScript API for Word)

Represents a content control. Content controls are bounded and potentially labeled regions in a document that serve as containers for specific types of content. Individual content controls may contain contents such as images, tables, or paragraphs of formatted text. Currently, only rich text content controls are supported. Hello World!!! Add more content!!!!!

_Applies to: Word 2016, Word for iPad_

## Properties

| Property | Type | Description |
| --- | --- | --- |
| cannotDelete | bool | Gets or sets a value that indicates whether the user can delete the content control. Mutually exclusive with removeWhenEdited. |
| cannotEdit | bool | Gets or sets a value that indicates whether the user can edit the contents of the content control. |
| color | string | Gets or sets the color of the content control. Color is set in "#RRGGBB" format or by using the color name. |
| placeholderText | string | Gets or sets the placeholder text of the content control. Dimmed text will be displayed when the content control is empty. |
| removeWhenEdited | bool | Gets or sets a value that indicates whether the content control is removed after it is edited. Mutually exclusive with cannotDelete. |
| style | string | Gets or sets the style used for the content control. This is the name of the pre-installed or custom style. |
| tag | string | Gets or sets a tag to identify a content control. The Silly stories add-in sample shows how you can use the tag property. |
| text | string | Gets the text of the content control. Read-only. |
| title | string | Gets or sets the title for a content control. |

_See property access [examples.](#property-access-examples)_

## Relationships

| Relationship | Type | Description |
| --- | --- | --- |
| appearance | ContentControlAppearance | Gets or sets the appearance of the content control. The value can be 'boundingBox', 'tags' or 'hidden'. |
| contentControls | ContentControlCollection | Gets the collection of content control objects in the content control. Read-only. |
| font | Font | Gets the text format of the content control. Use this to get and set font name, size, color, and other properties. Read-only. |
| id | uint | Gets an integer that represents the content control identifier. Read-only. |
| inlinePictures | InlinePictureCollection | Gets the collection of inlinePicture objects in the content control. The collection does not include floating images. Read-only. |
| paragraphs | ParagraphCollection | Get the collection of paragraph objects in the content control. Read-only. |
| parentContentControl | ContentControl | Gets the content control that contains the content control. Returns null if there isn't a parent content control. Read-only. |
| type | ContentControlType | Gets the content control type. Only rich text content controls are supported currently. Read-only. |

## Methods

| Method | Return Type | Description |
| --- | --- | --- |
| clear() | void | Clears the contents of the content control. The user can perform the undo operation on the cleared content. |
| delete(keepContent: bool) | void | Deletes the content control and its content. If keepContent is set to true, the content is not deleted. |
| getHtml() | string | Gets the HTML representation of the content control object. |
| getOoxml() | string | Gets the Office Open XML (OOXML) representation of the content control object. |
| insertBreak(breakType: BreakType, insertLocation: InsertLocation) | void | Inserts a break at the specified location. A break can only be inserted into objects that are contained within the main document body, except if it is a line break which can be inserted into any body object. The insertLocation value can be 'Before', 'After', 'Start' or 'End'. |
| insertFileFromBase64(base64File: string, insertLocation: InsertLocation) | Range | Inserts a document into the current content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'. |
| insertHtml(html: string, insertLocation: InsertLocation) | Range | Inserts HTML into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'. |
| insertInlinePictureFromBase64(base64EncodedImage: string, insertLocation: InsertLocation) | InlinePicture | Inserts an inline picture into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'. |
| insertOoxml(ooxml: string, insertLocation: InsertLocation) | Range | Inserts OOXML or wordProcessingML into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'. |
| insertParagraph(paragraphText: string, insertLocation: InsertLocation) | Paragraph | Inserts a paragraph at the specified location. The insertLocation value can be 'Before', 'After', 'Start' or 'End'. |
| insertText(text: string, insertLocation: InsertLocation) | Range | Inserts text into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'. |
| load(param: object) | void | Fills the proxy object created in JavaScript layer with property and object values specified in the parameter. |
| search(searchText: string, searchOptions: ParamTypeStrings.SearchOptions) | SearchResultCollection | Performs a search with the specified searchOptions on the scope of the content control object. The search results are a collection of range objects. |
| select(selectionMode: SelectionMode) | void | Selects the content control. This causes Word to scroll to the selection. The selection mode can be 'Select', 'Start' or 'End'. |

## Method details

### clear()

Clears the contents of the content control. The user can perform the undo operation on the cleared content.

#### Syntax

<div>

<pre>contentControlObject.clear();</pre>

</div>

#### Parameters

None

#### Returns

void

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the content controls collection.
    contentControls.load('text');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {

        if (contentControls.items.length === 0) {
            console.log("There isn't a content control in this document.");
        } else {

            // Queue a command to clear the contents of the first content control.
            contentControls.items[0].clear();
            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync().then(function () {
                console.log('Content control cleared of contents.');
            });      
        }

    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### delete(keepContent: bool)

Deletes the content control and its content. If keepContent is set to true, the content is not deleted.

#### Syntax

<div>

<pre>contentControlObject.delete(keepContent);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| keepContent | bool | Required. Indicates whether the content should be deleted with the content control. If keepContent is set to true, the content is not deleted. |

#### Returns

void

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the content controls collection.
    contentControls.load('text');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {

        if (contentControls.items.length === 0) {
            console.log("There isn't a content control in this document.");
        } else {

            // Queue a command to delete the first content control. The
            // contents will remain in the document.
            contentControls.items[0].delete(true);
            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync().then(function () {
                console.log('Content control cleared of contents.');
            });      
        }

    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### getHtml()

Gets the HTML representation of the content control object.

#### Syntax

<div>

<pre>contentControlObject.getHtml();</pre>

</div>

#### Parameters

None

#### Returns

string

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection that contains a specific tag.
    var contentControlsWithTag = context.document.contentControls.getByTag('Customer-Address');

    // Queue a command to load the tag property for all of content controls. 
    context.load(contentControlsWithTag, 'tag');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControlsWithTag.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to get the HTML contents of the first content control.
            var html = contentControlsWithTag.items[0].getHtml();

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Content control HTML: ' + html.value);
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### getOoxml()

Gets the Office Open XML (OOXML) representation of the content control object.

#### Syntax

<div>

<pre>contentControlObject.getOoxml();</pre>

</div>

#### Parameters

None

#### Returns

string

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the id property for all of the content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to get the OOXML contents of the first content control.
            var ooxml = contentControls.items[0].getOoxml();

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Content control OOXML: ' + ooxml.value);
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### insertBreak(breakType: BreakType, insertLocation: InsertLocation)

Inserts a break at the specified location. A break can only be inserted into objects that are contained within the main document body, except if it is a line break which can be inserted into any body object. The insertLocation value can be 'Before', 'After', 'Start' or 'End'.

#### Syntax

<div>

<pre>contentControlObject.insertBreak(breakType, insertLocation);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| breakType | BreakType | Required. Type of break (breakType.md) |
| insertLocation | InsertLocation | Required. The value can be 'Before', 'After', 'Start' or 'End'. |

#### Returns

void

#### Additional details

With the exception of line breaks, you can not insert a break into objects contained within headers, footers, footnotes, endnotes, comments, and textboxes.

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a commmand to load the id property for all of content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion. We now will have 
    // access to the content control collection.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to insert a page break after the first content control. 
            contentControls.items[0].insertBreak('page', "After");

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion. 
            return context.sync()
                .then(function () {
                    console.log('Inserted a page break after the first content control.');    
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### insertFileFromBase64(base64File: string, insertLocation: InsertLocation)

Inserts a document into the current content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'.

#### Syntax

<div>

<pre>contentControlObject.insertFileFromBase64(base64File, insertLocation);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| base64File | string | Required. Base64 encoded contents of the file to be inserted. |
| insertLocation | InsertLocation | Required. The value can be 'Replace', 'Start' or 'End'. |

#### Returns

[Range](range.md)

### insertHtml(html: string, insertLocation: InsertLocation)

Inserts HTML into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'.

#### Syntax

<div>

<pre>contentControlObject.insertHtml(html, insertLocation);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| html | string | Required. The HTML to be inserted in to the content control. |
| insertLocation | InsertLocation | Required. The value can be 'Replace', 'Start' or 'End'. |

#### Returns

[Range](range.md)

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the id property for all of the content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to put HTML into the contents of the first content control.
            contentControls.items[0].insertHtml('**HTML content inserted into the content control.**', 'Start');

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Inserted HTML in the first content control.');
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### insertInlinePictureFromBase64(base64EncodedImage: string, insertLocation: InsertLocation)

Inserts an inline picture into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'.

#### Syntax

contentControlObject.insertInlinePictureFromBase64(image, insertLocation);

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| base64EncodedImage | string | Required. The base64 encoded image to be inserted in the content control. |
| insertLocation | InsertLocation | Required. The value can be 'Replace', 'Start' or 'End'. |

#### Returns

[InlinePicture](inlinepicture.md)

### insertOoxml(ooxml: string, insertLocation: InsertLocation)

Inserts OOXML or wordProcessingML into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'.

#### Syntax

<div>

<pre>contentControlObject.insertOoxml(ooxml, insertLocation);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| ooxml | string | Required. The OOXML or wordProcessingML to be inserted in to the content control. |
| insertLocation | InsertLocation | Required. The value can be 'Replace', 'Start' or 'End'. |

#### Returns

[Range](range.md)

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the id property for all of the content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to put OOXML into the contents of the first content control.
            contentControls.items[0].insertOoxml("This text has formatting directly applied to achieve its font size, color, line spacing, and paragraph spacing.", "End");

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Inserted OOXML in the first content control.');
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

#### Additional information

Read [Create better add-ins for Word with Office Open XML](https://msdn.microsoft.com/en-us/library/office/dn423225.aspx) for guidance on working with OOXML.

### insertParagraph(paragraphText: string, insertLocation: InsertLocation)

Inserts a paragraph at the specified location. The insertLocation value can be 'Before', 'After', 'Start' or 'End'.

#### Syntax

<div>

<pre>contentControlObject.insertParagraph(paragraphText, insertLocation);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| paragraphText | string | Required. The paragrph text to be inserted. |
| insertLocation | InsertLocation | Required. The value can be 'Before', 'After', 'Start' or 'End'. |

#### Returns

[Paragraph](paragraph.md)

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the id property for all of the content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to insert a paragraph after the first content control. 
            contentControls.items[0].insertParagraph('Text of the inserted paragraph.', 'After');

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Inserted a paragraph after the first content control.');
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### insertText(text: string, insertLocation: InsertLocation)

Inserts text into the content control at the specified location. The insertLocation value can be 'Replace', 'Start' or 'End'.

#### Syntax

<div>

<pre>contentControlObject.insertText(text, insertLocation);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| text | string | Required. The text to be inserted in to the content control. |
| insertLocation | InsertLocation | Required. The value can be 'Replace', 'Start' or 'End'. |

#### Returns

[Range](range.md)

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the id property for all of the content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to replace text in the first content control. 
            contentControls.items[0].insertText('Replaced text in the first content control.', 'Replace');

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Replaced text in the first content control.');
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

The [Silly stories](https://aka.ms/sillystorywordaddin) add-in sample shows how to use the **insertText** method.

### load(param: object)

Fills the proxy object created in JavaScript layer with property and object values specified in the parameter.

#### Syntax

<div>

<pre>object.load(param);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| param | object | Optional. Accepts parameter and relationship names as delimited string or an array. Or, provide loadOption object. |

#### Returns

void

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy range object for the current selection.
    var range = context.document.getSelection();

    // Queue a commmand to create the content control.
    var myContentControl = range.insertContentControl();
    myContentControl.tag = 'Customer-Address';
    myContentControl.title = ' has t';
    myContentControl.style = 'Heading 2';
    myContentControl.insertText('One Microsoft Way, Redmond, WA 98052', 'replace');
    myContentControl.cannotEdit = true;
    myContentControl.appearance = 'tags';

    // Queue a command to load the id property for the content control you created.
    context.load(myContentControl, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        console.log('Created content control with id: ' + myContentControl.id);
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

### search(searchText: string, searchOptions: ParamTypeStrings.SearchOptions)

Performs a search with the specified searchOptions on the scope of the content control object. The search results are a collection of range objects.

#### Syntax

<div>

<pre>contentControlObject.search(searchText, searchOptions);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| searchText | string | Required. The search text. |
| searchOptions | ParamTypeStrings.SearchOptions | Optional. Options for the search. |

#### Returns

[SearchResultCollection](searchresultcollection.md)

### select(selectionMode: SelectionMode)

Selects the content control. This causes Word to scroll to the selection. The selection mode can be 'Select', 'Start' or 'End'.

#### Syntax

<div>

<pre>contentControlObject.select(selectionMode);</pre>

</div>

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| selectionMode | SelectionMode | Optional. The selection mode can be 'Select', 'Start' or 'End'. 'Select' is the default. |

#### Returns

void

#### Examples

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the id property for all of the content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to select the first content control.
            contentControls.items[0].select();

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Selected the first content control.');
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

## Property access examples

### Load all of the content control properties

<div>

<pre>// Run a batch operation against the Word object model.
Word.run(function (context) {

    // Create a proxy object for the content controls collection.
    var contentControls = context.document.contentControls;

    // Queue a command to load the id property for all of the content controls. 
    context.load(contentControls, 'id');

    // Synchronize the document state by executing the queued commands, 
    // and return a promise to indicate task completion.
    return context.sync().then(function () {
        if (contentControls.items.length === 0) {
            console.log('No content control found.');
        }
        else {
            // Queue a command to load the properties on the first content control. 
            contentControls.items[0].load(  'appearance,' +
                                            'cannotDelete,' +
                                            'cannotEdit,' +
                                            'color,' +
                                            'id,' +
                                            'placeHolderText,' +
                                            'removeWhenEdited,' +
                                            'title,' +
                                            'text,' +
                                            'type,' +
                                            'style,' +
                                            'tag,' +
                                            'font/size,' +
                                            'font/name,' +
                                            'font/color');             

            // Synchronize the document state by executing the queued commands, 
            // and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    console.log('Property values of the first content control:' + 
                        '   ----- appearance: ' + contentControls.items[0].appearance + 
                        '   ----- cannotDelete: ' + contentControls.items[0].cannotDelete +
                        '   ----- cannotEdit: ' + contentControls.items[0].cannotEdit +
                        '   ----- color: ' + contentControls.items[0].color +
                        '   ----- id: ' + contentControls.items[0].id +
                        '   ----- placeHolderText: ' + contentControls.items[0].placeholderText +
                        '   ----- removeWhenEdited: ' + contentControls.items[0].removeWhenEdited +
                        '   ----- title: ' + contentControls.items[0].title +
                        '   ----- text: ' + contentControls.items[0].text +
                        '   ----- type: ' + contentControls.items[0].type +
                        '   ----- style: ' + contentControls.items[0].style +
                        '   ----- tag: ' + contentControls.items[0].tag +
                        '   ----- font size: ' + contentControls.items[0].font.size +
                        '   ----- font name: ' + contentControls.items[0].font.name +
                        '   ----- font color: ' + contentControls.items[0].font.color);
            });
        }
    });  
})
.catch(function (error) {
    console.log('Error: ' + JSON.stringify(error));
    if (error instanceof OfficeExtension.Error) {
        console.log('Debug info: ' + JSON.stringify(error.debugInfo));
    }
});</pre>

</div>

## Support details

Use the [requirement set](https://msdn.microsoft.com/EN-US/library/office/mt590206.aspx) in run time checks to make sure your application is supported by the host version of Word. For more information about Office host application and server requirements, see [Requirements for running Office Add-ins](https://msdn.microsoft.com/EN-US/library/office/dn833104.aspx).