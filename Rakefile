require 'rubygems'
require 'bundler/setup'
require 'rake'
require 'rake/rdoctask'
require 'rubygems/package_task'
require 'rspec/core/rake_task'
require 'cucumber/rake/task'
require 'appraisal'

$LOAD_PATH.unshift("lib")
require 'shoulda/matchers/version'

Rake::RDocTask.new { |rdoc|
  rdoc.rdoc_dir = 'doc'
  rdoc.title    = "Shoulda -- Making tests easy on the fingers and eyes"
  rdoc.options << '--line-numbers'
  rdoc.template = "#{ENV['template']}.rb" if ENV['template']
  rdoc.rdoc_files.include('README.rdoc', 'CONTRIBUTION_GUIDELINES.rdoc', 'lib/**/*.rb')
}

RSpec::Core::RakeTask.new do |t|
  t.pattern = "spec/**/*_spec.rb"
  t.rspec_opts = '--color --format progress'
  t.verbose = false
end

desc "Run code-coverage analysis using rcov"
RSpec::Core::RakeTask.new(:coverage) do |t|
  t.rspec_opts = '--color --format progress'
  t.rcov = true
  t.rcov_opts = %{--exclude osx\/objc,spec,gems\/ --failure-threshold 100}
  t.pattern = "spec/**/*_spec.rb"
end

eval("$specification = begin; #{IO.read('shoulda-matchers.gemspec')}; end")
Gem::PackageTask.new $specification do |pkg|
  pkg.need_tar = true
  pkg.need_zip = true
end

desc "Clean files generated by rake tasks"
task :clobber => [:clobber_rdoc, :clobber_package]

Cucumber::Rake::Task.new do |t|
  t.fork = false
  t.cucumber_opts = ['--format', (ENV['CUCUMBER_FORMAT'] || 'progress')]
end

desc 'Default: run specs and cucumber features'
task :default => [:all]

desc 'Test the plugin under all supported Rails versions.'
task :all => ["appraisal:cleanup", "appraisal:install"] do |t|
  exec('rake appraisal spec cucumber')
end
