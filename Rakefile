require 'rake/clean'
require 'rubocop/rake_task'
require 'html-proofer'

SASS_CONFIG = {
  common_options: '--scss --sourcemap=none',
  input:          '_sass/style.scss',
  output:         '_tmp/style.css'
}.freeze

directory '_tmp'

CLOBBER.include '_tmp'

namespace :sass do
  desc 'Compile assets with Sass'
  task build: :_tmp do
    sh %{sass #{SASS_CONFIG.fetch(:common_options)} --style=compressed #{SASS_CONFIG.fetch(:input)} #{SASS_CONFIG.fetch(:output)}}
  end

  desc 'Compile and watch assets with Sass, recompiling when necessary'
  task watch: :_tmp do
    sh %{sass #{SASS_CONFIG.fetch(:common_options)} --watch #{SASS_CONFIG.fetch(:input)}:#{SASS_CONFIG.fetch(:output)}}
  end

  CLOBBER.include '.sass-cache'
end

namespace :jekyll do
  desc 'Compile the site with Jekyll for development environment'
  task build: [:clean, :'sass:build'] do
    sh %{jekyll build}
  end

  desc 'Compile the site with Jekyll for production environment'
  task prod: [:clean, :'sass:build'] do
    sh %{JEKYLL_ENV=production jekyll build}
  end

  desc 'Compile and serve the site with Jekyll, recompiling when necessary'
  task :serve do
    sh %{jekyll serve --drafts}
  end
end

namespace :test do
  options = { :check_html => true }

  desc 'Test the site in development environment'
  task :html do
    HTMLProofer.check_directory('./_site', options).run
  end
end

desc 'Start everything'
multitask :start => [:clean, :'sass:watch', :'jekyll:serve']

CLEAN.include '_site'

RuboCop::RakeTask.new
