#
# Rakefile for Chef Server Repository
#
# Author:: Adam Jacob (<adam@opscode.com>)
# Copyright:: Copyright (c) 2008 Opscode, Inc.
# License:: Apache License, Version 2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
# 
#     http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

require 'rubygems'
require 'chef'
require 'json'

# Make sure you have loaded constants first
require File.join(File.dirname(__FILE__), 'config', 'rake')

# And choosen a VCS
if File.directory?(File.join(TOPDIR, ".svn"))
  $vcs = :svn
elsif File.directory?(File.join(TOPDIR, ".git"))
  $vcs = :git
end

load 'chef/tasks/chef_repo.rake'

desc "Check spelling in the README files"
task :spell do
  Dir['README*', 'cookbooks/*/README*'].each do |readme|
    sh "aspell -x -c #{readme}"
  end
end

begin
  require 'foodcritic'

  FoodCritic::Rake::LintTask.new do |task|
    task.files = File.join(Dir.pwd, 'cookbooks')
  end

  FoodCritic::Rake::LintTask.new(:foodcritic_correctness) do |task|
    task.files = File.join(Dir.pwd, 'cookbooks')
    task.options = {:tags => ['correctness']}
  end

  FoodCritic::Rake::LintTask.new(:foodcritic_syntax) do |task|
    task.files = File.join(Dir.pwd, 'cookbooks')
    task.options = {:tags => ['syntax']}
  end
rescue LoadError
  # since foodcritic is not available for Ruby 1.8.7, don't try to load its rules if it is not available
end
