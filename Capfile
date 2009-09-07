load 'deploy'
require 'rubygems'
require 'railsless-deploy'

default_run_options[:pty] = true
ssh_options[:forward_agent] = true

set  :application, "Bioinformatics Zen"
role :web,         "bytemark"

set :scm,               "git"
set :branch,            "master"
set :repository,        "git://github.com/michaelbarton/bioinformatics-zen.git"
set :deploy_via,        :remote_cache
set :git_shallow_clone, 1

set :user,      "admin"
set :use_sudo,  false
set :deploy_to, "/home/admin/sites/jekyll_bioinformaticszen.com"

namespace :deploy do
  task :build do
    run "cd #{release_path} && rake"
  end
end

after "deploy:update_code", "deploy:build"
