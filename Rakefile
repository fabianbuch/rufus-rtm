
require 'rubygems'

require 'rake'
require 'rake/clean'
require 'rake/packagetask'
require 'rake/gempackagetask'
require 'rake/rdoctask'
require 'rake/testtask'


RUFUS_RTM_VERSION = "0.1"

#
# GEM SPEC

spec = Gem::Specification.new do |s|

    s.name              = "rufus-rtm"
    s.version           = RUFUS_RTM_VERSION
    s.authors           = [ "John Mettraux" ]
    s.email             = "jmettraux@gmail.com"
    s.homepage          = "http://rufus.rubyforge.org/rufus-rtm"
    s.platform          = Gem::Platform::RUBY
    s.summary           = "yet another RememberTheMilk wrapper"
    #s.license           = "MIT"

    s.require_path      = "lib"
    #s.autorequire       = "rufus-rtm"
    s.test_file         = "test/test.rb"
    s.has_rdoc          = true
    s.extra_rdoc_files  = [ 'README.txt' ]

    [ 'rufus-verbs' ].each do |d|
        s.requirements << d
        s.add_dependency d
    end

    files = FileList[ "{bin,docs,lib,test}/**/*" ]
    files.exclude "rdoc" 
    s.files = files.to_a
end

#
# tasks

CLEAN.include("pkg", "html", "rdoc")

task :default => [ :clean, :repackage ]


#
# TESTING

Rake::TestTask.new(:test) do |t|
    t.libs << "test"
    t.test_files = FileList['test/test.rb']
    t.verbose = true
end

#
# PACKAGING

Rake::GemPackageTask.new(spec) do |pkg|
    #pkg.need_tar = true
end

Rake::PackageTask.new("rufus-rtm", RUFUS_RTM_VERSION) do |pkg|

    pkg.need_zip = true
    pkg.package_files = FileList[
        "Rakefile",
        "*.txt",
        "lib/**/*",
        "test/**/*"
    ].to_a
    #pkg.package_files.delete("MISC.txt")
    class << pkg
        def package_name
            "#{@name}-#{@version}-src"
        end
    end
end


#
# DOCUMENTATION

#ALLISON=`allison --path`
ALLISON="/Library/Ruby/Gems/1.8/gems/allison-2.0.3/lib/allison.rb"

Rake::RDocTask.new do |rd|

    rd.main = "README.txt"

    rd.rdoc_dir = "html/rufus-rtm"

    rd.rdoc_files.include(
        "README.txt", "CHANGELOG.txt", "LICENSE.txt", "lib/**/*.rb")

    rd.title = "rufus-rtm rdoc"

    rd.options << '-N' # line numbers
    rd.options << '-S' # inline source

    rd.template = ALLISON if File.exist?(ALLISON)
end


#
# WEBSITE

task :upload_website => [ :clean, :rdoc ] do

    account = "jmettraux@rubyforge.org"
    webdir = "/var/www/gforge-projects/rufus"

    sh "rsync -azv -e ssh html/rufus-rtm #{account}:#{webdir}/"
end

