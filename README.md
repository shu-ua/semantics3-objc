# semantics3
semantics3-objc is a objective-C client for accessing the Semantics3 Products API, which provides structured products information for a large number of products.
See https://www.semantics3.com for more information.

Quickstart guide: https://www.semantics3.com/quickstart
API documentation can be found at https://www.semantics3.com/docs/

## Installation
To install the latest source from the repository

```bash
git clone https://github.com/Semantics3/semantics3-objc.git

```

#Third party libraries added:
OAuthConsumer

## Adding the static library to your project
1. Create builds for i386 (simulator) and armv7 (iphone/ipad)
2. Change target to universal library and build the universalLib to create a static library file that is compatible with both the architecture.

![alt tag](http://i.imgur.com/4Ejux45.png)

This creates a separate folder for the universal library as shown below:

![alt tag] (http://i.imgur.com/Pxdmkej.png)



## Settings for the Project Using the Static Library
Add the static library as a framework under Build Phases > Link Binary With Libraries and add the libsemantics3-objc.a file.
Next make sure project is set to search for User Search Paths. This is done under Build Settings > Always Search User Paths and make sure its set to YES.
In the same area find User Header Search Paths and add:

```bash
"$(PROJECT_TEMP_DIR)/../UninstalledProducts/include"

```
This tells Xcode to look for static libraries within the intermediate build folder that Xcode creates during the build process. In here, we have the "include" folder we are using for our static library locations we setup in step 2 for the static library project settings. This is the most important step in getting Xcode to correctly find your static libraries.

## Configure the Workspace
Here we want to configure the workspace so that it will build the static library when we build our app. This is done by editing the scheme used for our app.

Make sure you have the scheme selected that will create your application.
From the scheme drop-down, choose Edit Scheme.
Select Build at the top of list on the left. Add a new target by pressing the + on the middle pane.
You should see the static library show up for the library you are trying to link. Choose the iOS static library.
Click both Run and Archive. This tells the scheme to compile the libraries for the static library whenever you build your app.
Drag the static library above your application target. This makes the static libraries compile before your application target.


![alt tag](http://i.imgur.com/TFU6lOc.png)

Add the value -ObjC to the application target's other linker flags under 'build settings'

## Getting Started

In order to use the client, you must have both an API key and an API secret. To obtain your key and secret, you need to first create an account at
https://www.semantics3.com/
You can access your API access credentials from the user dashboard at https://www.semantics3.com/dashboard/applications
 

  
### Using the static library:

 The data can be retrieved along the following resources:
 
 1.semantics3_objc
 
 2.Products

 3.Categories
 
 

## Examples

The following examples show you how to interface with some of the core functionality of the Semantics3 Products API. For more detailed examples check out the API documentation: https://www.semantics3.com/docs


     semantics3_objc *sem = [[semantics3_objc alloc] initSemantic3Request:OAUTH_KEY withapiSecret:OAUTH_SECRET andEndpoints:@"products"];

     NSMutableDictionary *tempDict = [[NSMutableDictionary alloc] init];
    
     [tempDict setObject:@"iphone" forKey:@"search"];
     
    [sem field:tempDict];
    [sem runQuery];
    
Include the delegate - 'Sem3ObjCDelegate' in the header file where you want to fetch the request result data.
The output is received using the delegate method as shown below:

    -(void)queryData:(NSString *)content{
        NSLog(@"Request output: %@",content);
     }

### Building your requests
A request can be constructed by three ways:
   
1. Passing a NSMutableDictionary Object directly to the field:(id)queryObject method
    
2. Passing a JSON string directly to the field:(id)queryObject method
    
3. Constructing a request using the -(void)buildQuery:(id)object andKeys:(NSString *)keys; method.
    
    
### Sample Requests

You can intuitively construct all your complex requests using the following method:
                     
          -(void)buildQuery:(id)object andKeys:(NSString *)keys;
          
Here is a sample complex request:

     q={"search":"iphone","sort":{"price":"dsc"},"offset":2}

The above request be constructed using the query builder method as below:
      
      semantics3_objc *sem = [[semantics3_objc alloc] initSemantic3Request:OAUTH_KEY withapiSecret:OAUTH_SECRET andEndpoints:@"products"];
 
       [sem buildQuery:@"iphone" andKeys:@"search"];
       [sem buildQuery:@"dsc" andKeys:@"sort,price"];
       [sem buildQuery:@"2" andKeys:@"offset"];

 

## Contributing
Use GitHub's standard fork/commit/pull-request cycle.  If you have any questions, email <support@semantics3.com>.

## Author

* Siddharthan Asokan <siddharthan64@gmail.com>

## Copyright

Copyright (c) 2015 Semantics3 Inc.

## License

    The "MIT" License
    
    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:
    
    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.
    
    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
    OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
    THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
    FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
    DEALINGS IN THE SOFTWARE.

