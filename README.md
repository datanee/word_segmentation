# word_segmentation

We support two different types of request: **batch request** and **instant request**. Batch request is used for tokenizing a large amount of data and instant request is used for tokenizing one or two sentence(s) at a time

## Requirement

In order to receive the result from our API, you need a secret key. Please contact us to get it.

## Batch request

The API of Batch request is:
```
http://tokenize.datanee.com/batchTok/?skey=YOUR_SECRET_KEY&f=up&request_id=YOUR_REQUEST_ID
```
Note: We only accept a multipart/form-data POST message. (your file must be stored in the POST message, otherwise we will not be able to process your data). You can use a simple html code for uploading a file like this:
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>File Upload</title>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	</head>

	<body>
		 <form method="POST" action="http://tokenize.datanee.com/batchTok/?skey=YOUR_SECRET_KEY&f=up&request_id=YOUR_REQUEST_ID" enctype="multipart/form-data" >
			 File:
			 <input type="file" name="file" id="file" /> <br/>
			 <input type="submit" value="Upload" name="upload" id="upload" />
		 </form>
	</body>
</html>
```

There is another option that is implemented in java by us (we use apache.
```java

import java.io.*;

import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.mime.MultipartEntity;
import org.apache.http.entity.mime.content.FileBody;
import org.apache.http.impl.client.DefaultHttpClient;

public static void main(String[] args) throws Exception {
    File inputFile = new File("YOUR_UNTOKENIZED_FILE");
    File outputFile = new File("YOUR_OUTPUT_TOKENIZED_FILE");
    String requestURL = "http://tokenize.datanee.com/batchTok/?skey=YOUR_SECRET_KEY&f=up&request_id=YOUR_REQUEST_ID";

    // open output stream to write the tokenized data
    OutputStream outputStream = new FileOutputStream(outputFile);

    // create a http multipart/form-data POST request
    MultipartEntity entity = new MultipartEntity();
    entity.addPart("file", new FileBody(inputFile));
    HttpPost request = new HttpPost(requestURL);
    request.setEntity(entity);
    HttpClient client = new DefaultHttpClient();

    // send request and receive the return data
    HttpResponse response = client.execute(request);
    response.getEntity().writeTo(outputStream);

    // close the output stream
    outputStream.close();
}
```

## Instant request

There are two different types of output for instant request. The first type is the "underscore output" in which each segmented entity is connected by a "_" character. The second type is the "array output" in which each segmented entity is an element of a array; In other words, your input will be transformed into an array of segmented entities.
```
Underscore output: 
http://tokenize.datanee.com/inputTok/?&f=under&skey=YOUR_SECRET_KEY&input=YOUR_INPUT_TEXT&request_id=YOUR_REQUEST_ID
Array output:
http://tokenize.datanee.com/inputTok/?&f=array&skey=YOUR_SECRET_KEY&input=YOUR_INPUT_TEXT&request_id=YOUR_REQUEST_ID
```
