!SLIDE title-slide
# How Scholars' Lab Develop Works #
## (or at least how you will) ##

!SLIDE bullets
# Chat #

* Asynchronous Communication
* \#slab on freenode

!SLIDE commandline incremental
# Git #

	$ git commit -am 'prisma-things are working'
	[master ac5fd8a] 'prisma-things are working
	 2 files changed, 32 insertions(+), 5 deletions(-)

!SLIDE commandline incremental
# Topic Branches #

	$ git branch authentication

	$ git branch
	authentication
	* develop
	master

	$ git checkout authentication
	Switched to branch 'authentication'

!SLIDE command
# Implementing Code #

```Gemfile```

	@@@ruby 
	gem "devise", "~>1.5.2"
	gem "omniauth", "~>1.0.1"
	gem "omniauth-twitter", "~>0.0.7"
	gem 'bcrypt-ruby', "~> 3.0.0"

!SLIDE commandline
# Install Libraries #

	$ bundle
	...
	Your bundle is complete! Use `bundle show [gemname]` to see 
	where a bundled gem is installed.

!SLIDE commandline incremental
# Install Devise Generator #

	$ rails generate devise:install
	create  config/initializers/devise.rb
	create  config/locales/devise.en.yml
	...

!SLIDE commandline incremental
# Create User Model #

	$ rails g devise User
	invoke  active_record
	create    db/migrate/20111130162023_devise_create_users.rb
	create    app/models/user.rb
	invoke    test_unit
	test/unit/user_test.rb
	create      test/fixtures/users.yml
	insert    app/models/user.rb
	route  devise_for :users

!SLIDE code smaller
```db/migrate/20111130162023_devise_create_users.rb```
	@@@ruby 
	class DeviseCreateUsers < ActiveRecord::Migration
		def change
			create_table(:users) do |t|
				t.database_authenticatable :null => false
				t.recoverable
				t.rememberable
				t.trackable

				t.encryptable
				# t.confirmable
				# t.lockable :lock_strategy => :failed_attempts, :unlock_strategy => :both
				# t.token_authenticatable


				t.timestamps
			end

			add_index :users, :email,                :unique => true
			add_index :users, :reset_password_token, :unique => true
			# add_index :users, :confirmation_token,   :unique => true
			# add_index :users, :unlock_token,         :unique => true
			# add_index :users, :authentication_token, :unique => true
		end

	end

!SLIDE commandline incremental small
	$ rake db:migrate
	==  DeviseCreateUsers: migrating ============================================
	-- create_table(:users)
		-> 0.0324s
	-- add_index(:users, :email, {:unique=>true})
		-> 0.0009s
	-- add_index(:users, :reset_password_token, {:unique=>true})
		-> 0.0007s
	==  DeviseCreateUsers: migrated (0.0343s) ===================================

!SLIDE commandline incremental small

	$ rails g devise views
	...
	create    app/views/devise/mailer
	create    app/views/devise/mailer/confirmation_instructions.html.erb
	create    app/views/devise/mailer/reset_password_instructions.html.erb
	create    app/views/devise/mailer/unlock_instructions.html.erb

!SLIDE small

Add a default mailer for your development environment

```config/environments/development.rb```
	@@@ruby
	config.action_mailer.default_url_options = { 
		:host => 'localhost:3000' 
	}


!SLIDE code incremental smaller


```app/views/layouts/application.html.erb```

	@@@ ruby
	<div id="user_nav">  
		<% if user_signed_in? %>  
			Signed in as <%= current_user.email %>. Not you?  
			<%= link_to "Sign out", destroy_user_session_path %>  
		<% else %>  
			<%= link_to "Sign up", new_user_registration_path %> or  
			<%= link_to "Sign in", new_user_session_path %>  
		<% end %>  
	</div>

!SLIDE bullets

* Test in the browser

    [http://localhost:3000/users/sign_up/](localhost:3000/users/sign_up/)

!SLIDE commandline incremental

# Changes #

	$ git diff
	diff --git a/app/models/user.rb b/app/models/user.rb
	index b2f7c8e..ad5fd6e 100644
	--- a/app/models/user.rb
	+++ b/app/models/user.rb
	@@ -6,4 +6,5 @@ class User < ActiveRecord::Base
	...

	$ git commit -am "Added basic devise authentication"
	[authentication a2f3208] Added basic devise authentication
 	2 files changed, 12 insertions(+), 0 deletions(-)

!SLIDE commandline incremental

# Merge Topic Branch #

	$ git co develop
	$ git merge authentication
	Updating 60e1dfe..a2f3208
	Fast-forward
 		app/models/user.rb                     |    1 +
 		app/views/layouts/application.html.erb |   11 +++++++++++
 		2 files changed, 12 insertions(+), 0 deletions(-)

!SLIDE commandline incremental

# Push for Review #

	$ git push origin develop

!SLIDE commandline incremental

# Deploying #

	$ git co master

	$ git pull origin master
	From github.com:scholarslab/prism
		* branch            master     -> FETCH_HEAD
	Already up-to-date.
  

	$ git merge develop
	Updating 60e1dfe..a2f3208
	Fast-forward
 		app/models/user.rb                     |    1 +
 		app/views/layouts/application.html.erb |   11 +++++++++++
 		2 files changed, 12 insertions(+), 0 deletions(-)
	$ git push heroku master

!SLIDE bullets incremental

# TODO/Issues #

* [Issue Tracker](https://github.com/scholarslab/prism/issues)

!SLIDE bullets

# "Power Tools" #

* [GitHub Client](http://mac.github.com/) (OS X)
* [SourceTree](http://www.sourcetreeapp.com/) (OS X)
* [tortoisegit](http://code.google.com/p/tortoisegit/) (Windows)

* [git-flow](https://github.com/nvie/gitflow) 

!SLIDE incremental

# Wayne's Code Broke My Feature #
.notes This is why we use feature branches

* Did not
* Did too
* I can't reproduce it
* Well it's not working for me, and you were the last one to touch the
  develop branch

!SLIDE executable

	@@@ruby
	1 + 1
