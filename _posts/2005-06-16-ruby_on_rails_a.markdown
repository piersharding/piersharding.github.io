---
layout: post
title:  "Ruby on Rails and ActiveRecord"
date:   2005-06-16 12:00:00
categories: general
---


Ruby on Rails is a celebrated recent phenomenon, and like many others, I have been seduced by its ease, and simplicity.  One of the things that I have found with its  MVC style approach, is that you really need to have a good grip on the design of your data requirements.  At the heart of this is <a href='http://api.rubyonrails.com/' target='_blank'>ActiveRecord</a> which is the Model.

<div id="a000032more"><div id="more">
<p>
Inorder to understand ActiveRecord more, I played around with using it outside of Rails ( I know that you can use the console ), to see what you can do.  Seeing that I had a hard time finding much information about this (more specifically - examples), I have written up a small example that I worked through to demonstrate the way ActiveRecord encapsulates a "many to many" relationship.
</p>

<p>
I started with two entities - Users and Roles, and wished to describe:
<ul>
<li>A User has one or many Roles</li>
<li>A Role can belong to one or many Users</li>
</ul>
</p>
<p>
Firstly - define the tables - ActiveRecord takes the approach of defining a table per entitiy, and then a separate table to hold relationship between the entities, and as with all things Rails, it relies heavily on naming convention to knit it together.<br/>

Users => users<br/>
Roles => roles<br/>
Roles <=> User  => roles_users<br/>

<pre class='code'>
CREATE TABLE roles (
  id int(10) unsigned NOT NULL auto_increment,
  name varchar(50) NOT NULL default '',
  info varchar(50) default NULL,
  PRIMARY KEY  (id)
) TYPE=MyISAM;

INSERT INTO roles VALUES (1,'role2','some role 2 info');
INSERT INTO roles VALUES (2,'role1','some role 1 info');

CREATE TABLE roles_users (
  role_id int(10) unsigned NOT NULL default '0',
  user_id int(10) unsigned NOT NULL default '0',
  KEY ur_map_idx (role_id,user_id)
) TYPE=MyISAM;

INSERT INTO roles_users VALUES (1,1);
INSERT INTO roles_users VALUES (2,1);

CREATE TABLE users (
  id int(10) unsigned NOT NULL auto_increment,
  nick varchar(50) NOT NULL default '',
  name varchar(50) default NULL,
  password varchar(50) NOT NULL default '',
  modified timestamp(14) NOT NULL,
  created timestamp(14) NOT NULL,
  access timestamp(14) NOT NULL,
  PRIMARY KEY  (id)
) TYPE=MyISAM;

INSERT INTO users VALUES (1,'pxh','Piers Harding','blah',20050512121552,20050512121552,20050512121552);
</pre>

</p>

<p>
Next thing is to create the Model, and the code to access it.  As this is installed using Ruby Gems, you must import gems, and then use gems to bootstrap the loading of ActiveRecord.
<pre class='code'>
#!/usr/bin/ruby
# load gems, and then ActiveRecord
require 'rubygems'
require_gem 'activerecord'

# Establish a conneciton to your database
  ActiveRecord::Base.establish_connection(
      :adapter  => "mysql",
      :host     => "badger",
      :username => "root",
      :password => "",
      :database => "auths"
   )

# Define the entity Role
class Role < ActiveRecord::Base
# describe the relationship to users
  has_and_belongs_to_many :users
end

# same for users
class User < ActiveRecord::Base
  has_and_belongs_to_many :roles
end

# get the first user
user = User.find(1)

# find the roles for a user
roles = user.roles

# loop through each role
roles.each {|role|
             print "Role: #{role.info}\n"
# find users for the role
             users = role.users
             users.each {|u|
# and again find the roles for each user
                userroles = u.roles
                print "Roles for user #{u.name} \n"
                userroles.each{|r| print "\t\t -->role: #{r.name}\n"}
                }
          }

</pre>
</p>

<p>
Hope this helps someone.
</p>
</div></div>

<p class="posted">Posted by PiersHarding at June 16, 2005 12:33 PM</p>





