---
layout: post
title: Kickstarting with Capistrano
comments: true
---
Problem I wanted to solve was to minimize the deployment overheads. 

**Deployment overheads on the server:**

* Bundle install if there are any new gems installed
* Run db migrations
* Precompile Assets
* Restart puma with the new codebase


Below I'm providing code snippets and some useful info for anyone who is new to the Capistrano and want to automate some deployment process (or it can be any process. Read [Capistrano documentation](http://capistranorb.com/) for more details)

#### Adding capistrano to the rails project

{% highlight ruby %}
  gem 'capistrano-rails', '~> 1.1.1'
{% endhighlight %}

and then 

{% highlight bash %}
  bundle install
{% endhighlight %}

and then

{% highlight bash %}
  bundle exec cap install
{% endhighlight %}

That will lay down the Capfile, deploy.rb (common config for all environments) and a sample config one for production and one for staging.

#### Deploy Config

Some variables I had to set in deploy.rb for the deployment to work

{% highlight ruby %}
set :application, "<application_name>"
set :repo_url, '<github_url>'
set :branch, '<github_branch_to_deploy>'
set :deploy_to, '<destination_directory_on_server>'

# Uncomment the below lines. You can read this http://capistranorb.com/documentation/getting-started/configuration/ for more details 

set :linked_files, fetch(:linked_files, []).push('config/database.yml', 'config/secrets.yml')
set :linked_dirs, fetch(:linked_dirs, []).push('log', 'tmp/pids', 'tmp/cache', 'tmp/sockets', 'vendor/bundle', 'public/system')
{% endhighlight %}

The only variable I had to configure in staging.rb 

{% highlight ruby %}
  server '<server-ip>', user: '<deployment_user>', roles: %w{app db web}
{% endhighlight %}


#### Bundle install, Precompile Assets, DB Migrations

Open capfile and uncomment the below lines

{% highlight ruby %}
require 'capistrano/bundler'
require 'capistrano/rails/assets'
require 'capistrano/rails/migrations'
{% endhighlight %}


They will take care of doing bundle install before the deployment process starts, precompiles assets and run db migrations as part of the deployment process. 

To know more about where they actually hook in their tasks in the deployment process, read [this](http://capistranorb.com/documentation/getting-started/flow/)

#### Restarting server after deployment

We store puma pid in a .pid file. We used to cat that file, find the pid, kill it and then restart after each deployment.

So, I wanted to automate that process. It was pretty simple. 


{% highlight ruby %}
  desc "Restart server"
  task :restart_server do
    on roles(:all) do |host|
      pid_file = "<path_to_pid>"
      # instead of manually restarting the process, we can send signals to the pid. Check out puma doc for more details.
      execute "[ -f #{pid_file} ] && kill -9 $(cat #{pid_file}); true"
      within current_path do
        execute :bundle, "exec puma -e #{fetch(:stage)} -C <puma_config_file>"
      end
    end
  end

after 'deploy:finished', 'restart_server'
  
{% endhighlight %}


**Caveat about executing commands on the server**:

For heaven's sake, please read and understand [this](http://capistranorb.com/documentation/getting-started/tasks/). It will come back and bite you otherwise.

### RVM on server
If you have RVM installed on the server, do check out [this](https://github.com/rvm/rvm1-capistrano3) gem.

Configuration changes I had to do:

{% highlight ruby %}
set :rvm1_ruby_version, "2.2.1" #in deploy.rb
require 'rvm1/capistrano3' #in Capfile
{% endhighlight %}

and done!!! 
I was able to automate the deployment process.

__Better world....yayayay!!!__

----

###Some more references

* [http://stackoverflow.com/questions/19071179/capistrano-how-to-put-files-at-the-shared-folder](http://stackoverflow.com/questions/19071179/capistrano-how-to-put-files-at-the-shared-folder)
* [http://theadmin.org/articles/capistrano-variables/](http://theadmin.org/articles/capistrano-variables/)
* [http://capistranorb.com/documentation/getting-started/flow/](http://capistranorb.com/documentation/getting-started/flow/)
* [http://stackoverflow.com/questions/19363077/bundle-install-doesnt-work-from-capistrano](http://stackoverflow.com/questions/19363077/bundle-install-doesnt-work-from-capistrano)

