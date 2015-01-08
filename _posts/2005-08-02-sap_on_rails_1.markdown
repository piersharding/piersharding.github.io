---
layout: post
title:  "SAP on Rails"
date:   2005-08-02 12:00:00
categories: general
---
<p align="right">
<a href="http://www.piersharding.com/blog/archives/2005/07/sap_a_closed_cu.html">&laquo; SAP a closed culture</a> |

<a href="http://www.piersharding.com/blog/">Main</a>
| <a href="http://www.piersharding.com/blog/archives/2005/08/saprfc_for_ruby_1.html">SAP::Rfc for Ruby version 0.09 &raquo;</a>

</p>

<h2>August  2, 2005</h2>

<h3>SAP on Rails</h3>

<style>
<!--
.code {
    border-style : solid ;
    border-width: 1px ;
    border-color: #000000 ;
    background-color: #E4E5E8 ;
 }
-->
</style>

<p>
<a href='http://www.rubyonrails.com'>Ruby on Rails</a> is an exciting new (in an old sense) way of developing Web Services.  It neatly combines convention with Best Practice, to help reduce the drag on development with the removal of declaritive cruft, allowing you to concentrate on the specifics of your application.  What I want to achieve with this article, is to give you a taste for what Rails can do, and show how access to SAP business related functions can be easily integrated.
</p>
<p>This article is also at <a href='https://www.sdn.sap.com/sdn/weblogs.sdn?blog=/pub/wlg/2111'>SDN</a>.

<div id="a000037more"><div id="more">
<h3>Ruby</h3>
<p>
Rails takes full advantage of the dynamic scripting language <a href='http://www.ruby-lang.org' target='_blank'>Ruby</a>, which allows you to make most changes to your code without server restart, or the requirement of deployment tools.  Ruby is a worthy contender in the rapid development space of web languages such as Perl and Python, with OO concepts built into the core, from the word go.  With a natural intuitive style, and powerful text processing capabilities - it has all the things necessary for easy but scaleable Web Service development.
</p>
<h3>Rails Convention</h3>
<p>
Naming standards are used to reduce the amount of configuration required eg: tables relate specifically to Models, Ruby class naming conventions map directly to Controllers, Actions, and Views.  Typically - the only configuration required in a Rails application is to specify the database connection parameters. Unlike some frameworks, which require extensive configuration in various incarnations of XML.
</p>

<h3>Rails Best Practice</h3>
<p>
Rails adheres to the industry design standard that is <a href='http://en.wikipedia.org/wiki/MVC' target='_blank'>MVC</a>, giving good separation of the data model, the application flow control, and the presentation layer composition.
</p>
<p>
On top of this, the Rails framework offers standard ways of handling errors, and error notification, templating and page componentisation, and the ability to swap these presentation formulas for different delivery channels.
</p>
<p>
Rails also has built in <a href='http://www.onlamp.com/pub/a/onlamp/2005/06/09/rails_ajax.html' target='_blank'>AJAX</a> support available, to take advantage of the latest interest in DHTML, and flicker-free user experience.
</p>

<h2> Rails fundamentals </h2>
<p>
In order to cover the Rails fundamentals, I am going to digress here for a minute - it is important to get your head round this so that we can see where SAP integration may fit in, so please bare with me.<br/>
As was previously mentioned, Rails used the design principals of MVC (Model, View, Controller).  These parts of the architecture are not configured, but are bound together through naming convention.  So, if we take the classic example application of the  <a href='http://www.onlamp.com/pub/a/onlamp/2005/01/20/rails.html' target='_blank'>Cookbook</a> then the Recipe model has a module name of recipe.rb, the associated views are held in a directory ./app/views/recipe/[list|show|...].rhtml, and the controller is RecipeController in recipe_controller.rb.
</p>

<h3> Model </h3>
<p>
Classic Rails applications are centred around the ActiveRecord suite.  This manages connections to a given supported database (MySQL, Postgresql, Oracle etc.).  It also takes  care of the auto-discovery of tables and schema, and the mapping of row/column information to OO access routines for creating, updating, deleting, and interrogating data points. <br/>
Extending the reference to the Cookbook application - the Recipe object will, by convention, be expecting a recipes table.  Rails does this all automatically with some clever gramatical interogation tricks (Recipe becomes recipe, becomes recipes).<br/>
This allows a basic Model complete with CRUD operations, to be a simple incantation like this: (Note: the belongs_to: declares the association with another Model for categories - ActiveRecord manages this relationship on the fly)
</P>
<p>
<i>Figure 1 - Recipe Model</i>
<pre class='code'>
class Recipe < ActiveRecord::Base
  belongs_to :category
end
</pre>
</p>

<h3> Controller </h3>
<p>
Methods of the comtroller class (RecipeController) become the actions available  to the web application.  These map directly to URI names.  RecipeController#list maps to /recipe/list/.... (again evidence of convention over configuration).<br/>
As you can see from the sample Controller below - each method (Action) accesses  the Model to invoke the appropriate business logic to obtain the relevent data.  Then the appropriate flow control is exercised (do nothing means "render with the corresponding view", where as redirect_to changes the flow to another Action).
</p>
<p>
<i>Figure 2 - Recipe Controller</i>
<pre class='code'>
class RecipeController < ApplicationController
  layout "standard-layout"
  scaffold :recipe

  def new
    @recipe = Recipe.new
    @categories = Category.find_all
  end

  def index
    self.list
    render_action "list"
  end

  def list
    @category = @params['category']
    @recipes = Recipe.find_all
  end

  ...
end
</pre>
</p>

<h3> View </h3>
<p>
The action name (for a controller) specifies which rhtml template within a view, to pick for the rendering of the output.  This can also be further "skinned" by the specification of a layout (an html super set wrapper that the content is inserted into) by the "layout" directive given in the controller (see above).
</p>
<p>
<i>Figure 3 -list() Action => list view => list.rhtml</i>
<pre class='code'>
&lt;table border="1"&gt;
 &lt;tr&gt;
   &lt;td width="40%"&gt;&lt;p align="center"&gt;&lt;i&gt;&lt;b&gt;Recipe&lt;/b&gt;&lt;/i&gt;&lt;/td&gt;
   &lt;td width="20%"&gt;&lt;p align="center"&gt;&lt;i&gt;&lt;b&gt;Category&lt;/b&gt;&lt;/i&gt;&lt;/td&gt;
   &lt;td width="20%"&gt;&lt;p align="center"&gt;&lt;i&gt;&lt;b&gt;Date&lt;/b&gt;&lt;/i&gt;&lt;/td&gt;
 &lt;/tr&gt;

 &lt;% @recipes.each do |recipe| %&gt;
   &lt;% if (@category == nil) || (@category == recipe.category.name)%&gt;
     &lt;tr&gt;
      &lt;td&gt;
        &lt;%= link_to recipe.title,
                   :action =&gt; "show",
                   :id =&gt; recipe.id %&gt;
        &lt;font size=-1&gt;

        &lt;%= link_to "(delete)",
                    {:action =&gt; "delete", :id =&gt; recipe.id},
                    :confirm =&gt; "Really delete #{recipe.title}?" %&gt;
        &lt;/font&gt;
      &lt;/td&gt;
      &lt;td&gt;
        &lt;%= link_to recipe.category.name,
                    :action =&gt; "list",
          :category =&gt; "#{recipe.category.name}" %&gt;
      &lt;/td&gt;
      &lt;td&gt;&lt;%= recipe.date %&gt;&lt;/td&gt;
     &lt;/tr&gt;
   &lt;% end %&gt;
 &lt;% end %&gt;

&lt;/table&gt;
</pre>
</p>
<p>
<i>Figure 4 - layout</i>
<pre class='code'>
&lt;html&gt;
 &lt;head&gt;
   &lt;title&gt;Online Cookbook&lt;/title&gt;
 &lt;/head&gt;
 &lt;body&gt;
   &lt;h1&gt;Online Cookbook&lt;/h1&gt;
   <strong>&lt;%= @content_for_layout %&gt;</strong>
   &lt;p&gt;
     &lt;%= link_to "Create new recipe",
                 :controller =&gt; "recipe",
                 :action =&gt; "new" %&gt;
  
   &lt;%= link_to "Show all recipes",
               :controller =&gt; "recipe",
               :action =&gt; "list" %&gt;
     
   &lt;%= link_to "Show all categories",
               :controller =&gt; "category",
               :action =&gt; "list" %&gt;
   &lt;/p&gt;
 &lt;/body&gt;
&lt;/html&gt;


</pre>
Note: <strong>@content_for_layout</strong> determines where the body of each rendered template associated with an action is inserted.
</p>
<p>
Further reading and tutorials for Rails can be found at <a href='http://documentation.rubyonrails.com' target='_blank'>RubyOnRails</a>.  There are several excellent primers including:


<ul>
<li>The Cookbook <a href='http://www.onlamp.com/pub/a/onlamp/2005/01/20/rails.html' target='_blank'>part 1</a> and <a href='http://www.onlamp.com/pub/a/onlamp/2005/03/03/rails.html' target='_blank'>part 2</a> </li>
<li><a href='http://www.rubyonrails.com/media/text/Rails4Days.pdf' target='_blank'>4 Days on Rails</a> </li>
<li><a href='http://manuals.rubyonrails.com/read/book/7' target='_blank'>The Todo application</a> </li>
<li><a href='http://www.onlamp.com/pub/a/onlamp/2005/06/09/rails_ajax.html' target='_blank'>AJAX on Rails</a></li>
</ul>

The whole reference guide is available at <a href='http://api.rubyonrails.com' target='_blank'>API</a>.
</p>

<h2>SAP on Rails</h2>
<p>
SAP on Rails focuses on providing an alternative type of Model template to the default - ActiveRecord.<br/>
This is done via <a href='http://raa.ruby-lang.org/project/saprfc/' target='_blank'>SAP::Rfc</a> which allows RFC calls to SAP from Ruby, with automatic handling of data types, and discovery 
of interface definitions for RFCs.  Some of this has been covered in a previous  <a href='http://www.piersharding.com/blog/archives/2003/12/ruby_saprfc_add.html' target='_blank'>article</a>, and on <a href='http://sdn.sap.com' target='_blank'>SDN</a>.<br/>
The model templating class is SAP4Rails.  It's job is to automatically build up  connections to R/3, look up the interface definitions for the collection of associated RFCs, and to hold the attribute definitions for the given model.  This, coincidentally, corresponds to the SAP Business Objects view of the world in BAPIs.<br/>
</p>

<h3>Currencies and Exchange Rates</h3>
<p>
The example I have to work through is based on the two BAPI objects - Currency (BUS1090), and ExchangeRate (BUS1093).  These can be found using the transaction BAPI under "Basis Services / Communication Interfaces".  I have used these objects as they exist in my <a href='http://www.sap.com/linux' target='_blank'>NW4 test drive</a> system, so they should be available as a lowest common denominator across the board.  The complete example is available at <a href='http://www.piersharding.com/download/ruby/exrates.tgz' target='_blank'>exrates.tgz</a>.  The walk through below is going to centre around the "list" action, of the exrate controller.  This will show how to generate a list of exchange rates out of SAP.  For this we will need to develop the Exrate Model, the ExrateController controller, and the list.rhtml view associated.  Download the entire application to see all the other controller/action functions including listing, and showing details of Currencies, and the list, show, and create of Exchange rates.
<p/>
<p>
<i>Figure 5 - Currency and ExchangeRate objects in the BAPI explorer</i>
<p/>
<p>
<a href='http://www.piersharding.com/download/ruby/bapi1.html' target='_blank'><image src='http://www.piersharding.com/download/ruby/bapi1.png' width='50px' height='40px'/></a> Click to view
</p>
<p>
Each method of a business object has an associated RFC - these are what get mapped into the Rails data model.
</p>
<p>
<i>Figure 6 - GetCurrentRates of the ExchangeRate object maps to the RFC BAPI_EXCHRATE_GETCURRENTRATES</i>
<p/>
<p>
<a href='http://www.piersharding.com/download/ruby/bapi2.html' target='_blank'><image src='http://www.piersharding.com/download/ruby/bapi2.png' width='50px' height='40px'/></a> Click to View
</p>

<p>
<h3>Start developing the Rails application</h3>

First install <a href='http://raa.ruby-lang.org/project/saprfc/' target='_blank'>SAP::Rfc</a> for Ruby.  It is critical that this module is loadable in the Ruby library path - so check this first with: ruby -r "SAP/Rfc" -e 1 - this will give an error "ruby: No such file to load -- SAP/Rfc (LoadError)" if this module cannot be found.  If you have installed it in an unusal way, or location, then you can try setting the environment variable RUBYLIB to the relevent directory.
<p/>
<p>

Next - start the project by building the basic framework - this is done using the Rails supplied tools - execute these commands to start it off:
<pre>
rails Exrates
cd Exrates
./scripts/generate controller Exrate
./scripts/generate model exrate
./scripts/generate scaffold exrate
</pre>
</p>
<p>
The file layout of the application is something like this (below).  Some of the files shown we have yet to create/put in place.
</p>
<p>
<i>Figure 7 - Application layout</i>
<pre class='code'>
./config/routes.rb
./config/sap.yml                          <== RFC connection configuration
./app/controllers/exrate_controller.rb
./app/controllers/application.rb
./app/helpers/currency_helper.rb
./app/helpers/application_helper.rb
./app/helpers/exrate_helper.rb
./app/models/exrate.rb
./app/views/layouts/standard-layout.rhtml
./app/views/exrate/show.rhtml
./app/views/exrate/new.rhtml
./app/views/exrate/_form.rhtml
./app/views/exrate/list.rhtml
./public/stylesheets/scaffold.css
./lib/saprfc.so                          - SAP::Rfc files installed here
./lib/SAP/Rfc.rb                         /
./lib/SAP4Rails.rb                       <== Data Model template SAP4Rails
</pre>
</p>
<p>Note: I installed SAP::Rfc into the ./lib directory along side SAP4Rails.rb - this is just a matter of preference.
</p>
<p>Grab SAP4Rails.rb from <a href='http://www.piersharding.com/download/ruby/SAP4Rails.rb' target='_blank'>here</a> and place it in the &lt;application root&gt;/lib directory.
<p>

<p>
Once we have this basic framework - we need to start customising the application. 
</p>

<p>
<h3>SAP4Rails - the Data Model</h3>

When ./scripts/generate model exrate is executed a number of files are created, including ./app/models/exrate.rb.  This contains the basic definition using ActiveRecord:
</p>
<p>
<i>Figure 8 - Default Exrate code</i>
</p>
<p>
<pre class='code'>
class Exrate < ActiveRecord::Base
end
</pre>

This needs to be changed to use SAP4Rails:
</p>
<p>
<i>Figure 9 - Our Exrate code</i>
</p>
<p>
<pre class='code'>
require "SAP4Rails"
class Exrate < SAP4Rails
# You must define a list of RFCs to preload
  @@funcs = [
       'BAPI_EXCHANGERATE_CREATE',
       'BAPI_EXCHRATE_CREATEMULTIPLE',
       'BAPI_EXCHRATE_GETCURRENTRATES',
       'BAPI_EXCHANGERATE_GETDETAIL',
       'BAPI_EXCHANGERATE_GETFACTORS',
       'BAPI_EXCHRATE_GETLISTRATETYPES',
       'BAPI_EXCHANGERATE_SAVEREPLICA',
       'BAPI_TRANSACTION_COMMIT',
  ]

# You must define a list of attribute accessors to preload
  @@params = [ 'from',
               'extype',
               'to',
               'rate',
               'validfrom',
  ]
  
  # User defined instance methods here
  attr_accessor :message

    # user defined Class methods
  class << self
    def list
      t = Date.today
      tday = sprintf("%04d%02d%02d", t.year.to_i, t.month.to_i, t.day.to_i)
      RAILS_DEFAULT_LOGGER.warn("[Exrate] date: " + tday)
      Exrate.BAPI_EXCHRATE_GETCURRENTRATES.date.value = tday
      Exrate.BAPI_EXCHRATE_GETCURRENTRATES.call()
      return Exrate.BAPI_EXCHRATE_GETCURRENTRATES.exch_rate_list.rows()
    end
  end
end
</pre>
The noticeable difference here is the two class attributes, @@funcs, and @@params.  As the comments suggest - these are how we tell SAP4Rails what RFCs need to be preloaded, and what attributes our model is going to have.  Technically speaking, this example does not need the @@params as this is used when building Rails update, and create features.
<br/>
When the Exrate class is defined at run time, these values are processed and as the first method is accessed all of the RFC definitions, and attributes are loaded up.  The full definition can be found <a href='http://www.piersharding.com/download/ruby/exrate.rb' target='_blank'>here</a>.
<br/>
However - before SAP4Rails can interact with an SAP R/3 system, we need to tell it how to connect to one.  This is done via a <a href='http://www.yaml.org' target='_blank'>YAML</a> based config file - ./config/sap.yml.
<br/>
</p>
<p>
<i>Figure 10 - RFC connection configuration</i>
</p>
<p>
Create the file ./config/sap.yml with your connection settings, like this:
<pre>
development:
  ashost: seahorse.local.net
  sysnr: "00"
  client: "010"
  user: developer
  passwd: developer
  lang: EN
  trace: "1"
</pre>
Note: these values are repeated for test and production.
<br/>
SAP4Rails ensures that a separate RFC connection is created for each Model - this allows a separate SAP session context per data Model, which is useful for COMMIT related issues (transactions).
</p>

<p>
<h3>The Controller</h3>
When ./scripts/generator controller Exrate is executed, amoung other things, a file ./app/controllers/exrate_controller.rb containing ExrateController, is created.  The outline for this is:
</p>
<p>
<i>Figure 11 - Default ExrateController code</i>
</p>
<p>

<pre class='code'>
class ExrateController < ApplicationController
end
</pre>
</p>
<p>
We amend this with a directive for a layout to use (standard-layout), and methods  index(), and list().  
</p>
<p>
<i>Figure 12 - Our ExrateController code</i>
</p>
<p>
<pre class='code'>
class ExrateController < ApplicationController
  layout "standard-layout"

  def index
    list
    render_action 'list'
  end

  def list
    @exrates = Exrate.list()
    RAILS_DEFAULT_LOGGER.warn("[LIST] exchange rates: " + @exrates.length.to_s)
  end
  ...
end
</pre>
These correspond directly to the Actions index and list for the Controller exrate, which will map to a URI of /exrate/index and /exrate/list.
The Action index() is just like a redirect to list() (the list() action is executed and then rendered).  Within list(),  we see a call upon the class method of our Model Exrate - Exrate.list().  If we refer back to the code above for class Exrate, we can see the definition for list().  It calls upon a dynamic method BAPI_EXCHRATE_GETCURRENTRATES which gives us access to an object representation of the same-named RFC.
</p>
<p>
<i>Figure 13 - Code from Exrate#list</i>
</p>
<p>
<pre class='code'>
t = Date.today
tday = sprintf("%04d%02d%02d", t.year.to_i, t.month.to_i, t.day.to_i)
Exrate.BAPI_EXCHRATE_GETCURRENTRATES.date.value = tday
Exrate.BAPI_EXCHRATE_GETCURRENTRATES.call()
return Exrate.BAPI_EXCHRATE_GETCURRENTRATES.exch_rate_list.rows()
</pre>
</p>
<p>
Looking closer at the Exrate.list() call, we see the date parameter of the RFC being set to todays date, and then the call() being made, and finally exch_rate_list.rows() is returned.
<p/>
<p>
<i>Figure 14 - Inteface definition of BAPI_EXCHRATE_GETCURRENTRATES as seen in SE37</i>
</p>
<p>
<pre class='code'>
FUNCTION bapi_exchrate_getcurrentrates.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  IMPORTING
*"     VALUE(DATE) LIKE  BAPI1093_2-TRANS_DATE
*"     VALUE(DATE_TYPE) LIKE  BAPI1093_2-DATE_TYPE DEFAULT 'V'
*"     VALUE(RATE_TYPE) LIKE  BAPI1093_1-RATE_TYPE DEFAULT 'M'
*"     VALUE(SHOW_PROTOCOL) LIKE  BAPI1093_2-SHOW_PROTOCOL OPTIONAL
*"  TABLES
*"      FROM_CURR_RANGE STRUCTURE  BAPI1093_3
*"      TO_CURRNCY_RANGE STRUCTURE  BAPI1093_4
*"      EXCH_RATE_LIST STRUCTURE  BAPI1093_0
*"      RETURN STRUCTURE  BAPIRET1
*"----------------------------------------------------------------------
...
</pre>
The interface definition for BAPI_EXCHRATE_GETCURRENTRATES (above - via transaction SE37) shows us that exch_rate_list is a table parameter, and the rows() method is returning an Array of table lines.  SAP::Rfc takes care of all of this, including carving up the rows based on the table structure definition, making each line a hash of fieldname/value pairs.
<p/>
<p>
Going back to the Controller - at this level, SAP RFC specificness has been abstracted away - the only hint is directly calling error methods (see the <a href='http://www.piersharding.com/download/ruby/exrate.rb' target='_blank'>Exrate#save</a> method definitition) - this is usually more concealed in ActiveRecord, because of the Database Schema related information being stored in the Model - this is a lot harder to achieve in relation to RFC calls, as the schema relating to an RFC is less definitive, than that of classic Rails database table design - see the Todo list, and Cookbook tutorials to get further details.  SAP4Rails uses the ActiveRecord::Errors class which is bound to an inherited attribute of the Model called @errors.  When the facilities of this class are used then error testing, recording, and then subsequent display is trivial with the inclusion of the error_messages_for directive in a view template (see fragment <a href='http://www.piersharding.com/download/ruby/_form.rhtml' target='_blank'>_form.rhtml</a> via new.rhtml for exrate, and observe how the Exrate#save interacts with it).
<p/>
<p>
Other than that - from here on in it is just plain Rails.
</p>

<p>
<h3>The Views</h3>

Now on to rendering the output.  The embedded Ruby code of the list.rhtml template gets executed in the context of the instantiated ExrateController, so it has access to all the attributes of that object.  In the interests of tidiness, I have put the column/field names relating to list() Array results in a Helper module ./app/helpers/exrate_helper.rb.  This is inherited by the controller at run time, so anything defined here is also available in the template.
<p/>
<p>
<i>Figure 15 - ExrateController helper module ExrateHelper</i>
<pre class='code'>
module ExrateHelper
  # Fields for form output
  def formFields
    return    [ 'FROM_CURR',
                'TO_CURRNCY',
                'RATE_TYPE',
                'TO_FACTOR',
                'EXCH_RATE',
    ]
  end
end
</pre>

There is so much going on within the template, but specific to our SAP4Rails example, we can see the code iterating over the attribute Array of @exrates - each element of which, holds a hash of field/value pairs.  This is where the field names from formFields() are also iterated over to print out the columns.
</p>
<p>
<i>Figure 16 - exrate template list.rhtml</i>
<pre class='code'>
&lt;h1&gt;Listing Exchange Rates&lt;/h1&gt;
&lt;%= link_to 'Currencies', :controller =&gt; 'currency', :action =&gt; 'list' %&gt;
  
&lt;table&gt;
  &lt;tr&gt;
  &lt;!-- get fields from ExrateHelper --&gt;
  &lt;% formFields.each {|key| %&gt;
    &lt;th&gt;&lt;%= key %&gt;&lt;/th&gt;
  &lt;% } %&gt;
  &lt;/tr&gt;
  
  &lt;% for exrate in @exrates %&gt;
  &lt;tr&gt;
    &lt;% formFields.each {|val| %&gt;
    &lt;td&gt;
      &lt;% if val == "FROM_CURR" or val == "TO_CURRNCY" %&gt;
        &lt;%= link_to exrate[val], :controller =&gt; 'currency', :action =&gt; 'show',
                                    :id =&gt; exrate[val].strip %&gt;
      &lt;% else %&gt;
        &lt;%=h exrate[val] %&gt;
      &lt;% end %&gt;
      &lt;/td&gt;
    &lt;% } %&gt;
    &lt;td&gt;
    &lt;%= link_to 'Show', :action =&gt; 'show', :id =&gt; exrate['RATE_TYPE'].strip + ':'
                   + exrate['FROM_CURR'].strip + ':' +
                     exrate['TO_CURRNCY'].strip %&gt;
    &lt;%= link_to 'Edit', :action =&gt; 'new', :id =&gt; exrate['RATE_TYPE'].strip + ':'
                   + exrate['FROM_CURR'].strip + ':' + 
                     exrate['TO_CURRNCY'].strip %&gt;
    &lt;/td&gt;
  &lt;/tr&gt;
  &lt;% end %&gt;
&lt;/table&gt;
</pre>
</p>

<p>
And with that - here are the results. 
</p>
<p>
<i>Figure 17 - Listing Exchange Rates - rendered list.rhtml</i>
<p/>
<p>
<image src='http://www.piersharding.com/download/ruby/listexrates.png' width='688px' height='432px'/>
</p>

<p>
<h3> In conclusion</h3>
The approach laid out in this article, has enabled a high level of integration with the Rails framework.  The amount of code required to make this possible (SAP4Rails.rb) is less than 130 lines, which I think is testimony to how flexible, and well designed Rails is.  Further more - in the data Models shown, the heavy lifting of wrestling with RFC parameters, and their associated data types has been made trivial with SAP::Rfc and it's auto-interface-discovery features.  Have a closer look at the resources included (see above) - and you can see why Rails is creating such a storm in the world of Open Source, and making waves in the "Traditional Commercial Web development" Arena.</p>
<p>
Now you can get on track with your Web Services development. :-)
</p>
</div></div>

<p class="posted">Posted by PiersHarding at August  2, 2005  7:48 PM</p>




<h2 id="comments">Comments</h2>

<div id="c31">
<p>what if I want to use both sap and mysql in a same ruby on rails project? with your approach, I can only use sap as the data model...</p>
</div>
<p class="posted">Posted by: rodrigo dominguez  at October 25, 2005  8:53 PM</p>
<div id="c32">
<p>Hi Rodrigo - unless i am very much mistaken - all Object encapsulations should be in separate models, so there is no reason why you could not define MySQL  based models along side the SAP ones (separate files etc).</p>

<p>Regards,</p>

<p>Piers Harding.<br />
</p>
</div>
<p class="posted">Posted by: Piers Harding  at October 28, 2005 11:30 AM</p>
<div id="c48">
<p>Hi Piers,<br />
thanks for the nice tutorial & SAP4Rails. There is however one question: how do you properly disconnect and close the RFC connection?<br />
Thanks & regards,<br />
Juergen</p>
</div>
<p class="posted">Posted by: <a href="http://www.barth-partners.com" rel="nofollow">Juergen Barth</a>  at April 12, 2006  8:28 AM</p>
<div id="c51">
<p>Hi Juergen,</p>

<p>The SAP4Rails class would need to be modified a bit to give access to the class variable @@rfc which is a hash of connections - then you could select a connection and call .close()  (you would also need to delete it from the hash).</p>

<p>eg:<br />
add - cattr_accessor: rfc to SAP4Rails before the declaration of @@rfc</p>

<p>then</p>

<p>SAP4Rails.rfc.each_value{|rfc| rfc.close}<br />
 </p>

<p>Cheers.<br />
</p>
</div>
<p class="posted">Posted by: Piers Harding  at April 15, 2006 11:39 AM</p>





<script type="text/javascript" language="javascript">
<!--
if (document.comments_form.email != undefined)
    document.comments_form.email.value = getCookie("mtcmtmail");
if (document.comments_form.author != undefined)
    document.comments_form.author.value = getCookie("mtcmtauth");
if (document.comments_form.url != undefined)
    document.comments_form.url.value = getCookie("mtcmthome");
if (getCookie("mtcmtauth") || getCookie("mtcmthome")) {
    document.comments_form.bakecookie[0].checked = true;
} else {
    document.comments_form.bakecookie[1].checked = true;
}
//-->
</script>




