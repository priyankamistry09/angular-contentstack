# angular-contentstack 
AngularJS module for Built.io Contentstack - Content Delivery API

## Usage

**Setup** 

[Download](http://www.built.io) the AngularJS SDK,JS SDK and include it in the script tag.

```xml
<script type="text/javascript" src="js/contentstack.js"></script>
<script type="text/javascript" src="js/angular-contentstack.min.js"></script>
```

Then add the `contentstack` module to the dependencies of your AngularJS application module and configure it in a config block using the `stackProvider`:

**Configuration**
### 
```javascript
angular.module('app', ['contentstack']);
```

```javascript
angular
   .module('app')
   .config(['$locationProvider', 'stackProvider',
	    function($locationProvider, stackProvider) {
	        // initialize the stack
	        stackProvider.initialize({
	            'api_key': 'blt599456119258816d',
	            'access_token': 'blt2361f4c5ab53e37eab6d10ee',
	            'environment': 'development'
	        });
	    }
	]);
```

Once stackProvider configuration is done, you can use directives and services. 

## contentstack-entry directive
Retrive all the entires or a single entry of a Content Type.

##### Syntax:
```xml
    <contentstack-entry content-type="{Content_Type_ID}"> </contentstack-entry>
```

##### Get multiple entries :

```xml
  <contentstack-entry content-type="blog" as="entries">
    <!--Your will get "entires" array of entry objects -->
    <ul>
    <ng-repeat entry in entries>
        <li>{{entry.title}}</li>
    </ng-repeat>
    </ul>
  </contentstack-entry>
```
Note: If "as" attribute is not provided then by default the entries are available as $contentstackEntries variable.

##### Get a single Entry using entry ID :
```xml
  <contentstack-entry content-type="news" entry-uid="blt12345678910" as="entry">
    <!--Your will get "entry" object -->
    <h1>{{entry.title}}</h1>
  </contentstack-entry>
```
Note: If "as" attribute is not provided then by default the entry is available as $contentstackEntry variable.

##### Queries  :
All the JS SDK Query functions are supported as an attribute.

For Example:

**limit**

        <contentstack-entry content-type="news" limit="5">

**locale**

        <contentstack-entry content-type="news" locale="hi-hi">

**includeReference**

        <contentstack-entry content-type="news" includeReference="categories">


For more functions, check out the [JS SDK Query documentation](https://contentstackdocs.built.io/js/api/global.html#Query).

## Service
The contentstack service can be injected as follows:

    angular
    .module('myApp')
    .controller('MyController', function(contentstack){
        // contentstack service is a stack object initialiased with options provided to the stackProvider.
        var Query = contentstack.ContentType("News").Query(); // build query object for news content type
        Query
          .limit(10)
          .toJSON()
          .find()
          .then(function success(result) {
            // result is array where -
            // result[0] => entry objects
            // result[result.length-1] => entry objects count included only when .includeCount() is queried.
            // result[1] => schema of the content type is included when .includeSchema() is queried.
        }, function error(err) {
           // err object
        })
    });
# Helper 
## pagination

 This is a Pagination helper, using this you can achieve basic pagination functionality.This helper will provide you variable and methods for pagination such as   

**Pagination Variable**
* **$pagination.currentPage**- gives current page number
* **$pagination.totalPages**- gives total number of pages
* **$pagination.totalCount**- gives total number of entries
* **$pagination.itemsPerPage**- gives count of entries per page

**Pagination methods**
* **$pagination.previous()**- call this method to get pervious page
* **$pagination.next()**- call this method to get next page

**Example Code:**
``` sh
<button data-ng-disabled ="$pagination.currentPage === 1" data-ng-if="entry" data-ng-click="$pagination.previous()">Previous</button>
<button data-ng-disabled="$pagination.currentPage === $pagination.totalPages" data-ng-if="entry" data-ng-click="!isLoading && $pagination.next()">Next</button>
<span data-ng-if="entry">Current Page {{$pagination.currentPage}}</span>
<span data-ng-if="entry">Total Pages {{$pagination.totalPages}}</span>
<span data-ng-if="entry">Total Count {{$pagination.totalCount}}</span>
      		
<contentstack-entry content-type="news" as="entry" pagination="true" limit="3" >
  	<div data-ng-if="!$isLoading"   data-ng-repeat="key in entry" >
  		<header>
  		  <h1 data-ng-bind="key.title"></h1>
  		</header>
  		<div>
  		  <div data-ng-bind-html="key.body"></div>
  		</div>
  	</div>
  	<div class="loader" data-ng-if="entry && $isLoading">Loading...</div>
</contentstack-entry>
```
## Loadmore

This is a loadmore helper, using this you can achieve basic Loadmore functionality.

* **$pagination.loadMore()**- call this method to load more entries.
* **$noMoreData**- This variable is set to true when there no more entires. 

**Example Code:**
``` sh
<contentstack-entry content-type="news" as="entry" load-more="true" limit="3" >
  	<div data-ng-repeat="key in entry" >
  		<header>
  		  <h1 data-ng-bind="key.title"></h1>
  		</header>
  		<div>
  		  <div data-ng-bind-html="key.body"></div>
  		</div>
  	</div>
  	<div data-ng-if="entry && $isLoading">Loading...</div>
  	<div data-ng-if="!$isLoading && $noMoreData">NO MORE DATA</div>
    <button data-ng-if="!$isLoading"data-ng-click="$pagination.loadMore()">loadMore</button>
</contentstack-entry
``` 

# DEMO
* [Pagination Example](https://plnkr.co/edit/lYz3UoUrM9Z1tCaA7jPM?p=preview)
* [Loadmore Example](https://plnkr.co/edit/Mv1ps3L0OUuTe5by6voO?p=preview)
* [Ionic Example](https://harshalpatel91.github.io/Ionic_using_Ng-contentsatck/#/app/overview)
