require 'rake/clean'
require 'rubocop/rake_task'

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
  desc 'Compile the site with Jekyll'
  task build: [:clean, :'sass:compile'] do
    sh %{jekyll build}
  end

  desc 'Compile and serve the site with Jekyll, recompiling when necessary'
  task serve: :clean do
    sh %{jekyll serve}
  end
end

desc 'Start everything'
multitask :start => [:'sass:watch', :'jekyll:serve']

CLEAN.include '_site'

RuboCop::RakeTask.new