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
  task compile: :_tmp do
    sh %{sass #{SASS_CONFIG.fetch(:common_options)} --style=compressed #{SASS_CONFIG.fetch(:input)} #{SASS_CONFIG.fetch(:output)}}
  end

  desc 'Compile and watch assets with Sass, recompiling when necessary'
  task watch: :_tmp do
    sh %{sass #{SASS_CONFIG.fetch(:common_options)} --watch #{SASS_CONFIG.fetch(:input)}:#{SASS_CONFIG.fetch(:output)}}
  end

  CLOBBER.include '.sass-cache'
end

CLEAN.include '_site'

RuboCop::RakeTask.new
