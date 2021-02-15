# Calling the Natural Language API

Once your program can extract text from the selection and highlight it, it's time to call the Natural Language API. All of this will be done in the body of the retrieveSentiment function.

1. Return to the **Tools > Script editor** in Google Docs.

2. In the retrieveSentiment function, remove the current lines and add a variable to contain your API key, which you saved in the "Get an API key" section.
 ```
  var apiKey = "your key here";
 ```

3. Create a variable that will hold the URL of the Natural Language API with your API key appended to it.

```
var apiEndpoint = 'https://language.googleapis.com/v1/documents:analyzeSentiment?key=' + apiKey;
```

4. Build a structure from the line passed into the function that holds the text of the line, along with its type and language. Currently the only supported language is English.

```
  var docDetails = {
    language: 'en-us',
    type: 'PLAIN_TEXT',
    content: line
  };
 ```

5. Build the entire data payload from the document details by adding the encoding type.

```
  var nlData = {
    document: docDetails,
    encodingType: 'UTF8'
  };
```

6. Create a structure containing the payload and the necessary header information.

```
  var nlOptions = {
    method : 'post',
    contentType: 'application/json',
    payload : JSON.stringify(nlData)
  };
```

7. Make the call, saving the response.

```
  var response = UrlFetchApp.fetch(apiEndpoint, nlOptions);
```

8. The response will be returned in JSON format, so parse it and extract the score field, if it exists. Return either that field or 0.0.
```
  var data = JSON.parse(response);

  var sentiment = 0.0;
  //  Ensure all pieces were in the returned value
  if (data && data.documentSentiment
          && data.documentSentiment.score){
     sentiment = data.documentSentiment.score;
  }

  return sentiment;
```

The complete code to retrieve the sentiment is below.

```
function retrieveSentiment (line) {
  var apiKey = "your key here";
  var apiEndpoint = 'https://language.googleapis.com/v1/documents:analyzeSentiment?key=' + apiKey;

  //  Create a structure with the text, its language, its type,
  //  and its encoding
  var docDetails = {
    language: 'en-us',
    type: 'PLAIN_TEXT',
    content: line
  };

  var nlData = {
    document: docDetails,
    encodingType: 'UTF8'
  };

  //  Package all of the options and the data together for the call
  var nlOptions = {
    method : 'post',
    contentType: 'application/json',
    payload : JSON.stringify(nlData)
  };

  //  And make the call
  var response = UrlFetchApp.fetch(apiEndpoint, nlOptions);

  var data = JSON.parse(response);

  var sentiment = 0.0;
  //  Ensure all pieces were in the returned value
  if (data && data.documentSentiment
          && data.documentSentiment.score){
     sentiment = data.documentSentiment.score;
  }

  return sentiment;
}
```

9. Save your script, reload the document, and try out the full program. You may need to re-authorize with your credentials to enable the new functionality. Select different sections of your document to see how the sentiment may differ over its parts.

