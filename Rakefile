# vim: fileencoding=utf-8
require 'rubygems'
require 'rake'
require 'rake/clean'
require 'rake/testtask'
require 'rake/packagetask'
require 'rake/gempackagetask'
require 'rake/rdoctask'
require 'rake/contrib/rubyforgepublisher'
require 'rake/contrib/sshpublisher'
require 'fileutils'
require 'lib/crxmake'
include FileUtils

$version = CrxMake::VERSION
$readme = 'README.rdoc'
$rdoc_opts = %W(--main #{$readme} --charset utf-8 --line-numbers --inline-source)
$name = 'crxmake'
$summary = 'make chromium extension'
$author = 'Constellation'
$email = 'utatane.tea@gmail.com'
$page = 'http://github.com/Constellation/crxmake/tree/master'
$exec = %W(crxmake)
$rubyforge_project = 'crxmake'


task :default => [:test]
task :package => [:clean]

Rake::TestTask.new("test") do |t|
  t.libs << "test"
  t.pattern = "test/**/*_test.rb"
  t.verbose = true
end

spec = Gem::Specification.new do |s|
  s.name = $name
  s.version = $version
  s.platform = Gem::Platform::RUBY
  s.has_rdoc = true
  s.extra_rdoc_files = [$readme]
  s.rdoc_options += $rdoc_opts
  s.summary = $summary
  s.description = $summary
  s.author = $author
  s.email = $email
  s.homepage = $page
  s.executables = $exec
  s.rubyforge_project = $rubyforge_project
  s.bindir = 'bin'
  s.require_path = 'lib'
  s.test_files = Dir["test/*_test.rb"]
  {
    "zipruby" => ">=0.3.2",
#    "openssl" => ">=1.0.0"
  }.each do |dep, ver|
    s.add_dependency(dep, ver)
  end
  s.files = %w(README.rdoc Rakefile) + Dir["{bin,test,lib}/**/*"]
end

Rake::GemPackageTask.new(spec) do |p|
  p.need_tar = true
  p.gem_spec = spec
end

Rake::RDocTask.new do |rdoc|
  rdoc.rdoc_dir = 'doc'
  rdoc.options += $rdoc_opts
#  rdoc.template = 'resh'
  rdoc.rdoc_files.include("README.rdoc", "lib/**/*.rb", "ext/**/*.c")
end

desc "gem spec"
task :gemspec do
  File.open("#{$name}.gemspec", "wb") do |f|
    f << spec.to_ruby
  end
end

desc "gem build"
task :build => [:gemspec] do
  sh "gem build #{$name}.gemspec"
end

desc "gem install"
task :install => [:build] do
  sh "sudo gem install #{$name}-#{$version}.gem --local"
end

desc "gem uninstall"
task :uninstall do
  sh "sudo gem uninstall #{$name}"
end
# vim: syntax=ruby
