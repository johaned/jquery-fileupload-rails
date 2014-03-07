# jQuery File Upload for Rails

[jQuery-File-Plugin](https://github.com/blueimp/jQuery-File-Upload) is a file upload plugin written by [Sebastian Tschan](https://github.com/blueimp). jQuery File Upload features multiple file selection, drag&drop support, progress bars and preview images for jQuery. Supports cross-domain, chunked and resumable file uploads and client-side image resizing.

jquery-fileupload-rails is a library that integrates jQuery File Upload for Rails 3.1 Asset Pipeline (Rails 3.2 supported).

## Plugin versions (7-march-2014)

* jQuery File Upload User Interface Plugin 8.7.1
* jQuery File Upload Plugin 5.40.1
* jQuery UI Widget 1.10.4+amd

## Installing Gem

    gem "jquery-fileupload-rails", :git => 'git://github.com/Johaned/jquery-fileupload-rails'

## Using the javascripts

Require jquery-fileupload in your app/assets/application.js file.

    //= require jquery-fileupload

The snippet above will add the following js files to the mainfest file.

    //=require jquery-fileupload/vendor/jquery.ui.widget
    //=require jquery-fileupload/jquery.iframe-transport
    //=require jquery-fileupload/jquery.fileupload
    //=require jquery-fileupload/jquery.fileupload-ui
    //=require jquery-fileupload/jquery.fileupload-audio
    //=require jquery-fileupload/jquery.fileupload-image
    //=require jquery-fileupload/jquery.fileupload-video
    //=require jquery-fileupload/jquery.fileupload-jquery-ui
    //=require jquery-fileupload/jquery.fileupload-validate
    //=require jquery-fileupload/jquery.fileupload-process
    //=require jquery-fileupload/locale

If you only need the basic files, just add the code below to your application.js file. [Basic setup guide](https://github.com/blueimp/jQuery-File-Upload/wiki/Basic-plugin)

    //= require jquery-fileupload/basic

The basic setup only includes the following files:

    //=require jquery-fileupload/vendor/jquery.ui.widget
    //=require jquery-fileupload/jquery.iframe-transport
    //=require jquery-fileupload/jquery.fileupload
    //=require jquery-fileupload/locale

## Using the stylesheet

Require the stylesheet file to app/assets/stylesheets/application.css

    *= require jquery.fileupload-ui
    
## Example of Use (Rails 3.1 - RailsCast 381 Based)

#### Painting Index Page
    <h1>Painting Gallery</h1>

    <div id="paintings">
        <%= render @paintings %>
    </div>
    <div class="clear"></div>

    <%= form_for Painting.new do |f| %>
        <%= f.label :image, "Upload paintings:" %>
        <%= f.file_field :image, name: "painting[image]" %>
    <% end %>

    <div id="progress">
        <div class="bar" style="width: 0%;"></div>
    </div>

#### paintings.js.coffee

    $ ->
      if($.browser.msie && !$.support.xhrFileUpload)# works fine with jquery 1.8.3, in jquery 1.10 it does not work
        force_iframe = true 
        data_type = 'iframe'
      else
        force_iframe = false
        data_type = 'script'
      $('#new_painting').fileupload
        dataType: data_type
        forceIframeTransport: force_iframe
        add: (e, data) ->
          data.context = $("<p/>").text("Uploading...").appendTo(document.body)
          data.submit()
        done: (e, data) ->
          data.context.text "Upload finished."
        progressall: (e, data) ->
          progress = parseInt(data.loaded / data.total * 100, 10)
          $("#progress .bar").css "width", progress + "%"  
      return
      
## Using the middleware

The `jquery.iframe-transport` fallback transport has some special caveats regarding the response data type, http status, and character encodings. `jquery-fileupload-rails` includes a middleware that handles these inconsistencies seamlessly. If you decide to use it, create an initializer that adds the middleware to your application's middleware stack.

    Rails.application.config.middleware.use JQuery::FileUpload::Rails::Middleware

## Thanks
Thanks to [Sebastian Tschan](https://github.com/blueimp) for writing an awesome file upload plugin.

## License
Copyright (c) 2014 Tors Dalid

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
