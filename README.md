Multiple File Upload with HTML5
===============================

Upload your files the HTML5 way, drag and drop from your computer, multiple
files the ajaxy way!

It works about the same as uploading attachments to gmail.

- HTML5 Drag and Drop files from your computer
- HTML5 multiple attribute files for selecting multiple files
- Uploading files through XMLHttpRequest with FormData
- Progress bar
- Graceful degradation for IE / Opera


How To Use
-----------

Include the JavaScript files in your page, for example with the `<script>` tag,
or with a Script Loader with an extra check for `FormData` support.

When using the last option it'll look something like:

	#JS
	if ('FormData' in window){
		// Load Form.MultipleFileInput.js
		// Load Request.File.js
	} else {
		// Load IframeFormRequest
		// e.g. http://mootools.net/forge/p/iframeformrequest
	}

If you're not using a Script Loader, just include MooTools and all three files:

	#HTML
	<script src="https://ajax.googleapis.com/ajax/libs/mootools/1.3.2/mootools.js"></script>
	<script src="../Source/Request.File.js"></script>
	<script src="../Source/Form.MultipleFileInput.js"></script>
	<script src="../Source/Form.Upload.js"></script>

To get the magic running just initiate the Form.Upload class:

	#JS
	new Form.Upload('url');

This is for the html structure like

	#HTML
	<form method="post" action="files.php" enctype="multipart/form-data">
	<fieldset>
		<legend>Upload Files</legend>

		<div class="formRow">
			<label for="url" class="floated">File: </label>
			<input type="file" id="url" name="url[]" multiple><br>
		</div>

		<div class="formRow">
			<input type="submit" name="upload" value="Upload">
		</div>

	</fieldset></form>

### Events

The `Form.Upload` class has only one event: `onComplete`. This is only used
in the HTML5 uploader

### Graceful Degradation

Note that in the HTML the input is wrapped in a `div.formRow` element. This
is for the graceful degradation in older browsers. The Form.Upload class will
clone the first parent that maches `.formRow` of the `input#url` element. Then
it will append the clones after that `div.formRow` element.

By default it will just upload the files and go to the page given in the action
attribute, like any other form post. With the `isModern()` method you could
check whether it's using the HTML5 possibilities or the legacy upload.

**Example**

	#JS

	var upload = new Form.Upload('url', {
		onComplete: function(){
			alert('Completed uploading the Files');
		}
	});

	if (!upload.isModern()){
		// using iFrameFormRequest from the forge to upload the files
		// in an IFrame.
		new iFrameFormRequest('myForm', {
			onComplete: function(response){
				alert('Completed uploading the files');
			}
		});
	}

### Using the files separately

The Form.Upload Class is just a useful class to use when you quickly want to
build the upload form. However the `Form.MultipleFileInput` and `File.Request`
classes can be used separately too!

#### Form.MultipleFileInput

This is a class that transforms a simple `input[type="file"]` into a multiple
file input field with even drag and drop!

Checkout the source for all options and events. That's not very spectacular.

#### Request.File

This is a class that extends Request and brings file uploading to Request.
Some credits should go to fellow MooTools Developer Djamil Legato for proving
this class.

In this version it is a very stripped down version and not all Request options
might work. In 95% of the cases you don't really need them though, so it doesn't
matter really.

#### Combining Form.MultipleFileInput and Request.File

When the user has selected all the files, they can be uploaded.
To bridge those to classes, usually Form.Upload can be used.

However if you want more control, the following can be done:

	#JS
	// the input element, the list (ul) and the drop zone element.
	var input, list, drop;
	// Form.MultipleFileInput instance
	var inputFiles = new Form.MultipleFileInput(input, list, drop, {
		onDragenter: drop.addClass.pass('hover', drop),
		onDragleave: drop.removeClass.pass('hover', drop),
		onDrop: drop.removeClass.pass('hover', drop)
	});

	// Request instance;
	var request = new Request.File({
		url: 'files.php'
		// onSuccess
		// onProgress
	});

	myForm.addEvent('submit', function(event){
		event.preventDefault();
		inputFiles.getFiles().each(function(file){
			request.append('url[]' , file);
		});
		request.send();
	});

In the last loop, we loop over the [File](http://www.w3.org/TR/FileAPI/)
instances and we append them to the request:

	request.append((string) fieldname, (File) file);


Screenshots
-----------

![Screenshot 1](https://github.com/arian/mootools-form-upload/raw/master/screenshot.png)
