# Setting up your Google Doc

Before you can call the Natural Language API, you'll make an Apps Script program to create the menu, link it to a function to mark the text, and extract the text from the user selection.

1. Create a [new Google Doc](https://docs.google.com/document/create).

2. From within your new document, select the menu item **Tools > Script editor**. If you are presented with a welcome screen, click **Blank Project**.

3. Delete any code in the script editor and paste in the code below. This code will create a menu item, extract the text from the current selected text, and highlight the text based on its sentiment. It does not call the Natural Language API yet.

```

/**
* @OnlyCurrentDoc
*
* The above comment directs Apps Script to limit the scope of file
* access for this add-on. It specifies that this add-on will only
* attempt to read or modify the files in which the add-on is used,
* and not all of the user's files. The authorization request message
* presented to users will reflect this limited scope.
*/

/**
* Creates a menu entry in the Google Docs UI when the document is
* opened.
*
*/
function onOpen() {
  var ui = DocumentApp.getUi();

  ui.createMenu('Natural Language Tools')
    .addItem('Mark Sentiment', 'markSentiment')
    .addToUi();
}

/**
* Gets the user-selected text and highlights it based on sentiment
* with green for positive sentiment, red for negative, and yellow
* for neutral.
*
*/
function markSentiment() {
  var POSITIVE_COLOR = '#00ff00';  //  Colors for sentiments
  var NEGATIVE_COLOR = '#ff0000';
  var NEUTRAL_COLOR = '#ffff00';
  var NEGATIVE_CUTOFF = -0.2;   //  Thresholds for sentiments
  var POSITIVE_CUTOFF = 0.2;

  var selection = DocumentApp.getActiveDocument().getSelection();
  if (selection) {
    var string = getSelectedText();

    var sentiment = retrieveSentiment(string);

    //  Select the appropriate color
    var color = NEUTRAL_COLOR;
    if (sentiment <= NEGATIVE_CUTOFF) {
      color = NEGATIVE_COLOR;
    }
    if (sentiment >= POSITIVE_CUTOFF) {
      color = POSITIVE_COLOR;
    }

    //  Highlight the text
    var elements = selection.getSelectedElements();
    for (var i = 0; i < elements.length; i++) {
      if (elements[i].isPartial()) {
        var element = elements[i].getElement().editAsText();
        var startIndex = elements[i].getStartOffset();
        var endIndex = elements[i].getEndOffsetInclusive();
        element.setBackgroundColor(startIndex, endIndex, color);

      } else {
        var element = elements[i].getElement().editAsText();
        foundText = elements[i].getElement().editAsText();
        foundText.setBackgroundColor(color);
      }
    }
  }
}

/**
 * Returns a string with the contents of the selected text.
 * If no text is selected, returns an empty string.
 */
function getSelectedText() {
  var selection = DocumentApp.getActiveDocument().getSelection();
  var string = "";
  if (selection) {
    var elements = selection.getSelectedElements();

    for (var i = 0; i < elements.length; i++) {
      if (elements[i].isPartial()) {
        var element = elements[i].getElement().asText();
        var startIndex = elements[i].getStartOffset();
        var endIndex = elements[i].getEndOffsetInclusive() + 1;
        var text = element.getText().substring(startIndex, endIndex);
        string = string + text;

      } else {
        var element = elements[i].getElement();
        // Only translate elements that can be edited as text; skip
        // images and other non-text elements.
        if (element.editAsText) {
          string = string + element.asText().getText();
        }
      }
    }
  }
  return string;
}

/** Given a string, will call the Natural Language API and retrieve
  * the sentiment of the string.  The sentiment will be a real
  * number in the range -1 to 1, where -1 is highly negative
  * sentiment and 1 is highly positive.
*/
function retrieveSentiment (line) {
//  TODO:  Call the Natural Language API with the line given and return the sentiment value.
  return 0.0;
}
```

4. Select the menu item **File > Save all**. Name your new script **Natural Language Tools** and click **OK**. (The script's name is shown to end users in several places, including the authorization dialog.)

5. Return to your document. Add text to your document. You can use the sample that comes from Alice in Wonderland on Project Gutenberg (copy and paste the Plain Text UTF-8 version into the document), but feel free to use any text you wish.

6. Reload the document to see the new menu, **Natural Language Tools**, which you created, appear in the Google Docs toolbar.

7. Select text and then the **Mark Sentiment** option from the Natural Language Tools menu. The first time you select this option, you will be prompted to authorize the script to run. Click **Authorize**, and then log in with your credentials.

8. Allow Natural Language Tools to view and manage documents that this application has been installed in.

9. Once the script is authorized, the selected text will be highlighted in yellow, since the stub for sentiment analysis always returns 0.0, which is neutral.
