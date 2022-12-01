# coding: utf-8

git_source(:github) { |name| "https://github.com/#{name}.git" }
source "https://rubygems.org"
ruby File.read(File.join(File.dirname(__FILE__), ".ruby-version")).strip

gem 'sinatra', '~> 2.2'
gem 'puma', '~> 5.6'
gem 'rack-attack', '~> 6.5'
gem 'redis-activesupport', '~> 5.2'

group :test, :development do
  gem 'sinatra-reloader', '~> 1.0'
  gem 'rack-test', '~> 1.1'
  gem 'rspec', '~> 3.10'
  gem 'dotenv', '~> 2.7', '>= 2.7.6'
  gem 'byebug', '~> 11.1', '>= 11.1.3'
end
