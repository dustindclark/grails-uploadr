# About
This plugin allows dragging and dropping files on your browser window to upload them. It works out of the box, and it is also possible to add custom (javascript / ajax) event handlers and add initial files.

![example](https://dl.dropbox.com/s/yjkgq00jd7l6oca/uploadr-blank.png?dl=1)

Screenshot showing 6 files being uploaded (2 are done and 4 uploads in progress):

![example1](https://dl.dropbox.com/s/y1hn2ix5jb3ianj/uploadr-uploading.png?dl=1)

Screenshot showing 7 files being uploaded, separated into two pages (maximum of 5 uploads visible):

![example2](https://dl.dropbox.com/s/mhc6nelsc2nnd5l/uploadr-uploadingPaginated.png?dl=1)

## Press

Reviewed in the Plugin Corner of [GroovyMag Volume 6, Issue 8](http://www.groovymag.com/main.issues.description/id=59/), July 2013:
```
The Uploadr plugin is a short cut way to implement and use the Drag & Drop features of HTML5. This plugin provide
numerous parameters that can be configured as per need. Also, this plugin supports rating, voting and downloading
of files. It provides event handlers by which you can add your customized code. It also supports i18n messages so,
overall it’s a complete package for uploading files using HTML5 as a platform underneath it.
```

[![groovymag](http://www.groovymag.com/images/gm57_400.jpg)](http://www.groovymag.com/main.issues.description/id=59/)

## Features
* upload files by
* * dragging & dropping files onto the uploadr element
* * button (optional)
* pagination (customizable number of uploads per 'page')
* sound effects (optional)
* * file upload complete
* * transfer aborted
* * file deleted
* possibility to deny files over a certain file size (optional)
* rating (5 stars) (optional)
* voting (like / unlike) (optional)
* color picker (change background colors) (optional)
* upload progress
* * background progress
* * calculated file upload speed
* * estimated file upload duration
* * support for maximum concurrent uploads
* adding default files (e.g. files uploaded in a previous session)
* customizable css
* internationalization through i18n
* custom JavaScript event handlers for:
* * onStart - start file upload
* * onProgress - file upload progress
* * onSuccess - file upload was successful
* * onFailure - file upload failure
* * onAbort - user aborted transfer
* * onView - user clicked 'view'
* * onDownload - user clicked 'download'
* * onDelete - user clicked 'delete'
* * onLike - user clicked 'like'
* * onUnlike - user clicked 'unlike'
* * onChangeColor - user picked a color in the color picker
* built for Grails' resources plugin
* JavaScript developed as a jQuery plugin

## Compatibility

| *browser* | *supported* |
|-----------|------------:|
| Safari | supported (as of version ?) |
| Chrome | supported (as of version ?) |
| Firefox | supported (as of version ?) |
| Internet Explorer | supported as of version [10.0.8102.0](http://ie.microsoft.com/testdrive/) |
| Opera | not supported |

This plugin heavily relies on the HTML5 Drag and Drop and File API's which Microsoft has unfortunately only implemented in Internet Explorer 10.0.8102.0 (part of the [Windows 8 developer preview](http://msdn.microsoft.com/en-us/windows/apps/br229516) distribution).

## Installation
**Grails version 2.4 (and higher):**
Add a compile time dependency to your Grails project's ```grails-app/conf/Buildconf.groovy```:

```groovy
    plugins {
        // plugins for the build system only
        build ":tomcat:7.0.54"

        // plugins for the compile step
        compile ":scaffolding:2.1.2"
        compile ':cache:1.1.8'
        compile ":asset-pipeline:1.9.9"
        compile ":sass-asset-pipeline:1.9.0"

        // plugins needed at runtime but not for compilation
        runtime ":hibernate4:4.3.5.4" // or ":hibernate:3.6.10.16"
        runtime ":database-migration:1.4.0"
        runtime ":jquery:1.11.1"

        // Uncomment these to enable additional asset-pipeline capabilities
        //compile ":sass-asset-pipeline:1.7.4"
        //compile ":less-asset-pipeline:1.7.0"
        //compile ":coffee-asset-pipeline:1.7.0"
        //compile ":handlebars-asset-pipeline:1.3.0.3"
        
        // Load the Uploadr plugin
        compile ":uploadr:1.0.0"
    }
```

and load the Uploadr assets in the views / templates that use the Uploadr plugin:

```groovy
	<asset:javascript src="uploadr.manifest.js"/>
	<asset:stylesheet href="uploadr.manifest.css"/>
```

**Grails versions Pre 2.4:**

```groovy
plugins {
	runtime ":hibernate:$grailsVersion"
	build ":tomcat:$grailsVersion"
	runtime ":jquery:latest.integration"
	runtime ":resources:latest.integration"
	compile ":modernizr:latest.integration"
	compile ":uploadr:1.0.0"
}
```

**Grails versions Pre 2.x:**

```
grails install-plugin uploadr
```

## Quickstart
The plugin incorporates a demo tag which demonstrates some examples of how to use the uploadr tag with examples and source code. You can see a live (continuous integration) demo [here](http://uploadr.osx.eu/) (which is running [this](https://github.com/4np/grails-uploadr-demo)):

```rhtml
<uploadr:demo/>
```

**Grails version 2.4 (and higher):**

The *uploadr* plugin version 1.0.0 (and higher) depends in the Asset Pipeline plugin, so your project needs to load the plugin's assets as well as the demo assets:

```rhtml
<%@page expressionCodec="raw" %>
<!DOCTYPE HTML>
<html>
<head>
    <asset:javascript src="uploadr.manifest.js"/>
    <asset:javascript src="uploadr.demo.manifest.js"/>
    <asset:stylesheet href="uploadr.manifest.css"/>
    <asset:stylesheet href="uploadr.demo.manifest.css"/>
</head>
<body>
    <uploadr:demo/>
</body>
</html>
```

**Grails versions Pre 2.4:**
	
As the *uploadr* plugin depends on the resources plugin to pull in dependencies, your project should use the resources plugin as well (as will be the standard in Grails 2.x). If you are not familiar with the [resources plugin](http://grails.org/plugin/resources), the _demo_ tag can be used in a view as follows:*

```rhtml
<!DOCTYPE HTML>
<html>
<head>
	<r:require modules="uploadr"/>
	<r:layoutResources/>
</head>
<body>
	<uploadr:demo/>
	<r:layoutResources/>
</body>
</html>
```

## Using together with the spring-security plugin

For the plugin to work properly, you need extend the Spring Security configuration to allow access to ```/uploadr/*```. Example:

```rhtml
grails.plugin.springsecurity.controllerAnnotations.staticRules = [
..
'/upload/**': ['ROLE_ADMIN','ROLE_USER',],
```

## Adding an uploadr to your view
This tag will initialize the uploadr

```rhtml
<uploadr:add name="myUploadrName" path="/my/upload/path" direction="up" maxVisible="8" unsupported="/my/controller/action" rating="true" voting="true" colorPicker="true" maxSize="204800" />
```

Parameters

|*parameter* | *description* | *example* | *default* | *required*|
|------------|---------------|-----------|-----------|----------:|
|name | a unique name for your uploadr | myFirstUploadr | random uuid | ✔|
|path | the upload path, this may be a temporary path | /tmp | none | ✔|
|direction | manages whether new files will be added on top or on bottom (up/down) | up | down | ✗|
|maxVisible | determines how many files should be visible and handles pagination | 5 | 0 (=unlimited) | ✗|
|unsupported | shown when unsupported browser is used | /my/controller/action | default | ✗|
|class | override the default uploadr stylesheet with your own implementation | demo | uploadr | ✗|
|noSound | disable sound effects | noSound="true" | false | ✗|
|rating | enable / disable rating | rating="true" | false | ✗|
|voting | enable / disable voting | voting="true" | false | ✗|
|colorPicker | enable / disable colorPicker | colorPicker="true" | false | ✗|
|viewable | enable / disable view button | viewable="false" | true | ✗|
|downloadable | enable / disable download button | downloadable="false" | true | ✗|
|deletable | enable / disable delete button | deletable="false" | true | ✗|
|maxSize | max allowed size in bytes | maxSize="204800" | 0 (=unlimited) | ✗|
|allowedExtensions | only allow these extensions to be uploaded | allowedExtensions="gif,png,jpg,jpeg" | "" (= all) | ✗ |
|controller | use your own controller to handle file uploads | controller="myController" | default controller/action | ✗|
|action | use your own action to handle file uploads | action="myAction" | default controller/action | ✗|
|plugin | plugin that contains your custom controller/action | plugin="myPlugin" | default plugin | ✗|
|maxConcurrentUploads|maximum number of concurrent uploads|maxConcurrentUploads="2"|0 (unlimited)| ✗|
|maxConcurrentUploadsMethod|how to handle uploads that exceed _maxConcurrentUploads_|maxConcurrentUploadsMethod="cancel"|_pause_| ✗|

A screenshot of how the ```maxSize``` parameter (maxSize="204800") is handled in the front end:

![example1](https://dl.dropbox.com/s/dgwtupymrjt1ujs/uploadr-tooLarge.png?dl=1)

** The below section _only_ applies to plugin versions older than 1.0.0 as that version deprecated the dependency on the modernizr plugin. You should probably introcude some sort of validation in your end to make sure users have browser that support the HTML5 drag & drop API! **

A screenshot of the default warning when an unsupported browser is used. This can be changed by setting the _unsupported_ parameter to load your own warning or fallback upload support (e.g. ```unsupported="${createLink(plugin: 'uploadr', controller: 'upload', action: 'warning')}"```):

![example1](https://dl.dropbox.com/s/28s73s21bcfr5zz/uploadr-unsupported.png?dl=1)

## Adding initial files
You could just use the uploadr as an upload facility, but it can also be used to show a list of existing files you already uploaded previously and allow the user to view, download or delete the files. To add initial files you can use the ```uploadr:file``` tag as follows:

```rhtml
	<uploadr:file name="${file.name}">
		<uploadr:fileSize>${file.size()}</uploadr:fileSize>
		<uploadr:fileModified>${file.lastModified()}</uploadr:fileModified>
		<uploadr:fileId>myId-${RandomStringUtils.random(32, true, true)}</uploadr:fileId>
	</uploadr:file>
```

If you would like to show the list of files in a certain folder on disk, you could do that by doing something like this (also see the demo tag's second example):

```rhtml
<% def path = new File("/path/to/folder") %>
<uploadr:add name="mySecondUploadr" path="${path}" direction="up" maxVisible="5" unsupported="${createLink(plugin: 'uploadr', controller: 'upload', action: 'warning')}">
<% path.listFiles().each { file -> %>
	<uploadr:file name="${file.name}">
		<uploadr:fileSize>${file.size()}</uploadr:fileSize>
		<uploadr:fileModified>${file.lastModified()}</uploadr:fileModified>
		<uploadr:fileId>myId-${RandomStringUtils.random(32, true, true)}</uploadr:fileId>
	</uploadr:file>
<% } %>
</uploadr:add>
```

Of course in MVC one should not put logic in a view but in the controller (or the model) instead, and the above examples are merely here to provide some insight. Normally you
would pass -for example- a ```files``` variable to your view from your controller. So the example would be something like:

```rhtml
<uploadr:add name="mySecondUploadr" path="${path}" direction="up" maxVisible="5" unsupported="${createLink(plugin: 'uploadr', controller: 'upload', action: 'warning')}">
<g:each in="${files}" var="file">
	<uploadr:file name="${file.name}">
		<uploadr:fileSize>${file.size()}</uploadr:fileSize>
		<uploadr:fileModified>${file.lastModified()}</uploadr:fileModified>
		<uploadr:fileId>myId-${RandomStringUtils.random(32, true, true)}</uploadr:fileId>
	</uploadr:file>
</g:each>
</uploadr:add>
```

_note though that these examples use RandomStringUtils which you should include:_

```rhtml
<%@ page import="org.apache.commons.lang.RandomStringUtils" %>
```

## File overrides

To override the color of a file you can user the color attribute:

```rhtml
        <uploadr:color>#f594cc</uploadr:color>
```

To hide the delete icon / action set the delete attribute to false. Note that if the global deletable parameter is set to false (in the uploadr tag), all delete buttons will be disabled / hidden. This attribute can be used to disable the delete button on a per-file basis: 

```rhtml
        <uploadr:deletable>false</uploadr:deletable>
```

To set a rating, use the rating attribute:

```rhtml
        <uploadr:rating>0.33</uploadr:rating>
```

To add a rating tooltip text, use the ratingText attribute:

```rhtml
        <uploadr:ratingText>This is the tooltip text of the rating for Assignment 2.pdf</uploadr:ratingText>
```

Screenshot of most of the features enabled and shown (e.g. pagination, color picker, voting, rating, tooltips, buttons):

![example1](https://dl.dropbox.com/s/svncwqerzvbs157/uploadr-allFeatures.png?dl=1)
 

## Event handlers
By default the uploadr is fully functional as is, but it is possible to add your own event handles for certain types of events:

```rhtml
	<!-- upload event handlers //-->
	<uploadr:onStart>
		console.log('start uploading \'' + file.fileName + '\'');
	</uploadr:onStart>
	<uploadr:onProgress>
		console.log('\'' + file.fileName + '\' upload progress: ' + percentage + '%');
		return true; // return false to disable default progress handler
	</uploadr:onProgress>
	<uploadr:onSuccess>
		console.log('done uploading \'' + file.fileName + '\', setting some random file id for demonstration purposes');
        console.log('response was:');
        console.log(response);

		var text = "";
		var possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
		for( var i=0; i < 12; i++ ) text += possible.charAt(Math.floor(Math.random() * possible.length));

		// set a random file id for demonstration purposes
		file.fileId = 'my-random-id::'+text;

		// set file to non-deletable (we do not get a delete icon)
		file.deletable = false;
		console.log('set file.deletable to false so the delete icon will not be shown');
 
		// override the background to purple (same as initial files)
		$('.progress',domObj).css('background-color', '#f594cc');
		console.log('and overrided the background color to #f594cc');
 
		// and set the rating tooltip text for the rating
		file.fileRatingText = 'you just uploaded this file and in the onSuccess handler the rating tooltip text is added';
 
		// callback when done
		callback();
	</uploadr:onSuccess>
	<uploadr:onFailure>
		console.log('failed uploading \'' + file.fileName + '\'');
	</uploadr:onFailure>

	<!-- user triggered event handlers //-->
	<uploadr:onAbort>
		console.log('aborted uploading \'' + file.fileName + '\'');
	</uploadr:onAbort>
	<uploadr:onView>
		console.log('you clicked view:');
		console.log(file);
		console.log(domObj);
	</uploadr:onView>
	<uploadr:onDownload>
		console.log('you clicked download:');
		console.log(file);
		console.log(domObj);
	</uploadr:onDownload>
	<uploadr:onDelete>
		console.log('you clicked delete:');
		console.log(file);
		console.log(domObj);

		// return true / false whether it was successful
		return true;
	</uploadr:onDelete>
	<uploadr:onLike>
        	console.log('you clicked like:');
        	console.log(file);
        	console.log(domObj);
 
        	// callback if like action was successfull
        	// and pass the new file rating
        	callback(file.fileRating + 0.1);
 	</uploadr:onLike>
 	<uploadr:onUnlike>
        	console.log('you clicked unlike:');
        	console.log(file);
        	console.log(domObj);
 
        	// callback if unlike action was successfull
        	// and pass the new file rating
        	callback(file.fileRating - 0.1);
 	</uploadr:onUnlike>
 	<uploadr:onChangeColor>
        	console.log('you changed the color to:');
        	console.log(color);
        	console.log(file);
        	console.log(domObj);
 
        	// you can perform an ajax call here
        	// to update the color in the back-end
        	// for this file
	</uploadr:onChangeColor>
```

## Internationalization / custom texts
The text labels the plugin uses are stored in [i18n messages](https://github.com/4np/grails-uploadr/blob/master/grails-app/i18n/messages.properties), which can be overwritten / internationalized in your own application:

```properties
# labels that appears in the uploadr's percentage text when
# an upload is complete, failed or aborted
uploadr.label.done=done
uploadr.label.failed=failed
uploadr.label.paused=paused
uploadr.label.aborted=aborted
uploadr.label.maxsize=too large
uploadr.label.invalidFileExtension=invalid

# label for the 'select files' button
uploadr.button.select=Click to upload files

# placeholder text
uploadr.placeholder.text=Drag and drop files here to upload...

# badge tooltip
uploadr.badge.tooltip.singular=%d file is still being uploaded...
uploadr.badge.tooltip.plural=%d files are still being uploaded...

# error messages / tooltips
uploadr.error.maxsize=The upload size of %s is larger than allowed maximum of %s
uploadr.error.wrongExtension=You tried to upload a file with extension "%s" while only files with extensions "%s" \
  are allowed to be uploaded
uploadr.error.maxConcurrentUploadsExceededSingular=Only 1 concurrent upload allowed, please retry when the other upload is finished
uploadr.error.maxConcurrentUploadsExceededPlural=Only %d concurrent uploads allowed, please retry when the other uploads are finished

# file control tooltips
uploadr.button.delete=Click to delete this file
uploadr.button.delete.confirm=Are you sure you want to delete this file?
uploadr.button.abort=Click to abort file transfer
uploadr.button.abort.confirm=Are you sure you would like to abort this tranfer?
uploadr.button.download=Click to download this file
uploadr.button.view=Click to view this file
uploadr.button.like=Click to like
uploadr.button.unlike=Click to unlike
uploadr.button.color.picker=Click to change background color
uploadr.button.remove=Click to remove this aborted transfer from your view
```

In addition to these _default_ labels you can overwrite the _placeholder text_ (the text inside the drop area) and the the _select_ text (the text on the file upload link) on a per-uploadr basis using the following tags:

```rhtml
<uploadr:add ... placeholder="Behold: the drop area!" fileselect="Behold: the fileselect!" ...>
...
</uploadr:add>
```

## Disabling sound effects
The plugin plays some sound effects whether a file upload was completed, aborted or has failed. To disable these sound effects use the ```noSound``` parameter:

```rhtml
<uploadr:add ... noSound="true" ...>
...
</uploadr:add>
```

## Passing custom variables to the controller
Since version [0.7.3](https://github.com/4np/grails-uploadr#version-073) it is possible to pass variables to the controller. This requires that you implement [your own controller](#advanced-usage-creating-your-custom-controller-to-handle-file-uploads) to handle the uploaded files, and handle the custom controller variables.

```rhtml
<uploadr:add name="…" path="…" … model="[booleanOne:true, variableTwo: 'foo', variableThree: 'bar', variableFour: 4, myObject: someObject]" />
```

In the controller you can access the passed model:

```groovy
		def name 		= URLDecoder.decode(request.getHeader('X-Uploadr-Name'), 'UTF-8') as String
		def info		= session.getAttribute('uploadr')
        def myInfo      = (name && info && info.containsKey(name)) ? info.name : [:]
        def model       = (myInfo.containsKey('model')) ? myInfo.model : [:]
```

## Advanced usage: Creating your custom controller to handle file uploads
While the default controller works out of the box, your project's requirements might require a custom plugin. For example, if you would like to deploy your application on cloudfoundry, you need to create your own controller as you do not have file access. In order to accomplish this you can write your own controller, and specify to use it in your uploadr tag:

```rhtml
<uploadr:add ... controller="myController" action="myAction" ...>
...
</uploadr:add>
```

To have the file uploads being handled by _myAction_ action of _myController_. 

```rhtml
<uploadr:add ... controller="myController" action="myAction" plugin="myPlugin" ...>
...
</uploadr:add>
```

This would make the uploadr use the ```myAction``` of ```myController``` in ```myPlugin```. 

View the default [controller](https://github.com/4np/grails-uploadr/blob/master/grails-app/controllers/hungry/wombat/UploadController.groovy)'s ```handle``` action to get an idea of how to implement your own controller to handle file uploads (_note that the JavaScript expects JSON_).

If you do this, you probably also want to create your own code to view, download and delete uploaded files. You can do this by creating your own event handlers (respectively: ```onView``` , ```onDownload``` and ```onDelete``` ). 

For example, if you store your files in a custom controller using MongoDB's GridFS, you could create your own download handler by using the ```onDownload``` event handler:

```javascript
            uploadr.onDownload {
                                // render a template (separation of concerns)
				out << g.render(template:'/js/uploadr/onDownload', model:[])
            }
```

where the view ```/js/uploadr/onDownload``` contains the following code:

```javascript
window.open('<g:createLink plugin="myPlugin" controller="uploadedFile" action="downloadUploadedFile"/>?fileId='+file.fileId);
```

and the ```uploadedFile``` controller's ```downloadUploadedFile``` action in ```myPlugin``` contains something like the following:

```groovy
    def downloadUploadedFile = {
        UploadedFile uploadedFile = UploadedFile.get(params.fileId)
        if (!uploadedFile) response.sendError(404, "No uploaded file could be found matching id: ${params.fileId}.")

        GridFSFile gridFSFile = uploadedFile.file
        if (!gridFSFile) response.sendError(404, "No file attached to UploadedFile")

		response.setStatus(200)
		response.setHeader("Content-disposition", "attachment; filename=\"${uploadedFile.fileName}\"")
		response.setContentType("application/octet-stream")
		response.setContentLength(uploadedFile.fileSize as int)

		// define input and output streams
		InputStream inStream = null
		OutputStream outStream = null

		// handle file download
		try {
			inStream = gridFSFile.inputStream
			outStream = response.getOutputStream()

			while (true) {
				synchronized (buffer) {
					int amountRead = inStream.read(buffer)
					if (amountRead == -1) {
						break
					}
					outStream.write(buffer, 0, amountRead)
				}
				outStream.flush()
			}
		} catch (Exception e) {
			// whoops, looks like something went wrong
			println "download failed! ${e.getMessage()}"
		} finally {
			if (inStream != null) inStream.close()
			if (outStream != null) outStream.close()
		}

		return false
    }
```

## Interacting with an already initialized Uploadr

There are several occasions you might want to be able to interact with an already initialized Uploadr, for example in ajax calls or in your handlers. At the moment only clearing out (and / or erasing all uploaded files) is supported. If you have any feature requests, you can [submit them here](https://github.com/4np/grails-uploadr/issues).

### Changing a property on an already initialized uploadr

As of version 1.1.0 it is possible to set / change most properties of an already initialized uploadr. This example allows you to change the allowed extensions for an uploadr after it has been initialized (also see example 3 in the live demo):

```
$('.uploadr[name=myUploadr]').data('uploadr').set('allowedExtensions', 'png');
```

### Clearing out an already initialized Uploadr
In several occasions it might be useful to be able to hook into an already initialized Uploadr to perform certain actions (e.g. after an Ajax call). As of version _0.7.6_ it is possible to clear out an already initialized uploadr:

```
$('.uploadr[name=myUploadr]').data('uploadr').clear([options]);
```

By default the call will play a delete sound and execute the _onDelete_ handler (effectively deleting the files on the back-end as well). This default behaviour can be customized by passing in options:

```
$('.uploadr[name=myUploadr]').data('uploadr').clear({
	sound: true,
	erase: false
});
```

The following options are available for customization:

| option | description                                         | default |
|--------|-----------------------------------------------------|---------|
| sound  | play delete sound                                   | true    |
| erase  | erase the file (execute _onDelete_ handler) as well | true    |


Take a look at the documentation above, and the default event handlers in the uploadr [initialization JavaScript](https://github.com/4np/grails-uploadr/blob/master/grails-app/views/js/_init.gsp) for more information on how to create your own back-end logic to handle file upload, download, view and delete events.

## jQuery plugin
The front-end side (the gui) of the upload plugin is developed as a [jQuery](http://jquery.com/) plugin (javascript: [full](https://github.com/4np/grails-uploadr/blob/master/web-app/js/jquery.uploadr.js), [minified](https://github.com/4np/grails-uploadr/blob/master/web-app/js/jquery.uploadr.minified.js), css: [full](https://github.com/4np/grails-uploadr/blob/master/web-app/css/jquery.uploadr.css), [minified](grails-uploadr/tree/master/web-app/css/jquery.uploadr.minified.css)) which means you can also use the front-end in _non-Grails_ projects. You will, however, have to create your own back-end logic (take the _handle_ method in the [default controller](https://github.com/4np/grails-uploadr/blob/master/grails-app/controllers/hungry/wombat/UploadController.groovy) as an example) to handle the file uploads. The use of the jQuery plugin is currently undocumented, but the [initialization JavaScript](https://github.com/4np/grails-uploadr/blob/master/grails-app/views/js/_init.gsp) will probably provide you with all the information you require...

## Changelog

##Version 1.1.0

Version 1.1.0 adds support for Bootstrap ([#29](https://github.com/4np/grails-uploadr/issues/29) - by suggestion of [ortimanu](https://github.com/ortimanu)) and the demo is now also optimized for Bootstrap. CSS has been rewritten as SCSS which means the plugin now dependes on the _sass-asset-pipeline_ plugin. 

This version also allows more interation with an already initialized uploadr by changing configuration options ([#34](https://github.com/4np/grails-uploadr/issues/34) - thanks [redwoodswede](https://github.com/redwoodswede)), for example to change the allowed extenstions to only allow ```png```s:

```
$('.uploadr[name=myUploadr]').data('uploadr').set('allowedExtensions', 'png');
```

##Version 1.0.0 (Grails 2.4 > *)

Version 1.0.0 drops support for the Grails resources plugin in favor of the Asset Pipeline plugin (default as of Grails 2.4 - resolves [#26](https://github.com/4np/grails-uploadr/issues/26)).
The previous versions of the uploadr also had a dependency on the modernizr plugin to display a warning when the browser did not support the HTML5 drag & drop API. I have decided to drop the dependency
and have the developer implement their own compliancy validation. Mainly because the modernizr plugin is not -yet- compatible witht the asset pipeline, but also because that responsbility probably
lies with the developer; you probably would have already wanted to implemented some sort of check anyways because in general your would not have wanted your users to end
up in a warning page. 

**Plugin assets**


```groovy
<asset:javascript src="uploadr.manifest.js"/>
<asset:stylesheet href="uploadr.manifest.css"/>
```

**Demo tag**

To view the demo you now need to load the demo assets and execute the demo tag:

```groovy
<head>
    <asset:javascript src="uploadr.manifest.js"/>
    <asset:javascript src="uploadr.demo.manifest.js"/>
    <asset:stylesheet href="uploadr.manifest.css"/>
    <asset:stylesheet href="uploadr.demo.manifest.css"/>
</head>
<body>
    <uploadr:demo/>
</body>
```


###Version 0.8.2
Added support for defining the maximum number of concurrent file uploads, and how to handle file uploads that _exceed_ this maximum number (by request of [viseth](https://github.com/viseth), [#22](https://github.com/4np/grails-uploadr/issues/22))

```
<uploadr:add … maxConcurrentUploads="2" maxConcurrentUploadsMethod="pause">
	…
</uploadr>
```

Two (optional) new [parameters](#adding-an-uploadr-to-your-view) have been added to support upload concurrency. The _maxConcurrentUploads_ parameter takes an integer where _0_ means '_unlimited_', and the _maxConcurrentUploadsMethod_ parameter can be either '_pause_' (default) or '_cancel_'. When it is set to _pause_, all file uploads that exceed the maximum number of concurrent file uploads will be paused and be resumed whenever an upload slot is available. If set to _cancel_, any file upload that exceeds the maximum number of concurrent file uploads will be aborted. In the case of the latter the file should be re-uploaded when other running file upload(s) have finished (and uploads slots have been freed up).

Three new [i18n](#internationalization--custom-texts) strings have been added as well:

```
uploadr.label.paused=paused
uploadr.error.maxConcurrentUploadsExceededSingular=Only 1 upload at a time allowed, please retry when the other upload is finished
uploadr.error.maxConcurrentUploadsExceededPlural=Only %d concurrent uploads allowed, please retry when the other uploads are finished
```

###Version 0.8.1 / 0.8.1.1

Due to a bug in the handeling of resources in Grails 2.3.0 and 2.3.1, the plugin did not work properly. This bus was resolved in Grails 2.3.2 and required some updated to the plugin, which are added in this update. Please make sure you _are not using Grails 2.3.0 / 2.3.1_ together with this plugin (see issue [#15](https://github.com/4np/grails-uploadr/issues/15))
 
###Version 0.8.0 / 0.8.0.1

fixed security flaw [#21](https://github.com/4np/grails-uploadr/issues/21) in the default file download action where it was possible to use a path traversal attack. *_Everybody is advised to upgrade to 0.8.0.1!_* (Again, thanks to [Murf80](https://github.com/murf80) for reporting this issue!)

###Version 0.7.6 / 0.7.6.1
- fixed issues [#17](https://github.com/4np/grails-uploadr/issues/17) and [#18](https://github.com/4np/grails-uploadr/issues/18) regarding empty files (thanks to [Murf80](https://github.com/murf80))
- added support for clearing the uploadr in ajax calls (#16, thanks for bringing this up [Viseth](https://github.com/viseth))

###Version 0.7.5
Removed obsolete Quartz job (_thanks to [Luka](https://github.com/luciferche) [#13](https://github.com/4np/grails-uploadr/issues/13)_)

###Version 0.7.4
Now passing the 'response' variable to the onSuccess handler (_by suggestion of [domurtag](https://github.com/domurtag) [#12](https://github.com/4np/grails-uploadr/issues/12)_). See demo tag's example 4 for an example.

###Version 0.7.3
Added the possibility to suply the controller with custom variables:

```html
<uploadr:add name="…" path="…" … model="[booleanOne:true, variableTwo: 'foo', variableThree: 'bar', variableFour: 4, myObject: someObject]" />
```

As passing variables to the controller is a custom operation, you will need to implement [your own controller](#advanced-usage-creating-your-custom-controller-to-handle-file-uploads) to handle the uploaded files (thanks to [Tom](https://github.com/tcrossland) [#9](https://github.com/4np/grails-uploadr/issues/9) :+1:).

_see demo example 2_

###Version 0.7.2
Got rid of an error in Grails < 2.2.0 ([#8](https://github.com/4np/grails-uploadr/issues/8))

###Version 0.7.1
As the plugin can be run standalone in demonstration mode (in development and ci), a Quartz jub runs on the background to keep the upload folder clean for demonstration purposes. In previous versions the job would run when Quartz was installed, which is not the appropriate behaviour. It should only run the job when run standalone, in development and ci (see [Config.groovy](https://github.com/4np/grails-uploadr/blob/master/grails-app/conf/Config.groovy#L28) for configuration options). Thanks again [Dmitry](https://github.com/dementiev) ([#7](https://github.com/4np/grails-uploadr/issues/7) :)

###Version 0.7.0.1
I forgot to minify the Javascript in the 0.7.0 release...

###Version 0.7.0
Added unicode support (thanks to [Dmitry](https://github.com/dementiev), see [#6](https://github.com/4np/grails-uploadr/issues/6) )

###Version 0.6.1
- Upgrade to Grails 2.2.0 and changed dependencies to provided / c
- removed dependency on jquery-ui (resolved [#4](https://github.com/4np/grails-uploadr/issues/4) - thx [fdammassa](https://github.com/fdammassa)!)

###Version 0.6.0.1
Bugfixed plugin description

###Version 0.6.0
- Removed obsolete svn keywords which were oddly sometimes causing compilation problems
- Added support for an ```allowedExtensions``` parameter comma separated list (feature request [#1](https://github.com/4np/grails-uploadr/issues/1)). When undefined/empty, all file uploads are allowed.

example:
```groovy
allowedExtensions="jpg,gif,png"
```

*Note that you should not add spaces to the allowed extensions parameter's value as spaces are evaluated as regular characters.
This means that _"jpg, gif, png"_ does not work and will try to validate the ```. png``` extentions (```bla. png```) instead
of the desired ```.png``` (```bla.png```).*

This new feature has also introduced two new [i18n internationalization labels](https://github.com/4np/grails-uploadr/blob/master/grails-app/i18n/messages.properties), namely:

```groovy
uploadr.label.invalidFileExtension=invalid
...
uploadr.error.wrongExtension=You tried to upload a file with extension "%s" while only files with extensions "%s" \
  are allowed to be uploaded
```

![example user feedback](https://dl.dropbox.com/s/bq5cqiecgz8fa0h/uploadr-allowedExtensions.png?dl=1)

###Version 0.5.11
Upgraded to Grails 2.0.4 and Grails Central

###Version 0.5.10
Minor bugfix with casting string to double in Grails 2.0.1

###Version 0.5.9
Dependency map used >= which was wrong, changed it into >

###Version 0.5.8
Added grails 2.0 dependency configuration

###Version 0.5.7
Added three global parameters (downloadable, deletable, viewable) to define whether the file control buttons are visible (default) or not. Thanks to Michael Aube for the feedback :)

###Version 0.5.6
If the uploadr is put in a jquery-ui tab (http://jqueryui.com/demos/tabs/) there seems to be an issue with hiding elements by duration (e.g. ```$('myElement').hide(1000);```) which is ignored. This resulted in 'done' to remain visible in the percentage div when used in combination in a jquery-ui tab. To resolve this, the div is now explicitly hidden when the transition supposedly finished (e.g. ```$('myElement').hide(1000,function() { $(this).hide(); });``` )

###Version 0.5.5
FireFox drag and drop issue was fixed in 0.5.1 (see below), however the same issue apparently still existed in the 'click to upload' button

###Version 0.5.4
confirmed the File and Drag & Drop HTML5 API's to be working in Internet Explorer 10 10.0.8102.0 (part of Windows 8). Updated browser check to reflect this support, and improved the default warning message for unsupported browsers.

###Version 0.5.3
added support for ```maxSize``` argument (in bytes, so 204800 is 200KB). When a user tries to upload a file that is larger than the maximum size, the upload is not performed and a warning is shown. By default there is no limit (```maxSize=0```). Three i18n texts were added (```uploadr.error.maxsize```, ```uploadr.label.maxsize``` & ```uploadr.button.remove```). Added some tweaks that rating and voting does not show when an upload was not successful, and that a delete button will show to remove the failed / aborted transfer from view. Added ```maxSize``` limitation of 200KB to Example 3 in the ```<uploadr:demo/>``` tag.

###Version 0.5.2
Fixed an issue where some file tags did not always work properly (color, rating, ratingText and deletable)

###Version 0.5.1
Implemented support for the changed Firefox 7 File API. While in the previous versions (and in webkit based browsers) the file information was stored in ```file.fileSize``` , ```file.fileName``` and ```file.contentType``` , Firefox 7's File API now uses ```file.name``` , ```file.size``` and ```file.type``` instead. Implemented a fix to support this new behavior.