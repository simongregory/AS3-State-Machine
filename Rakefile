require 'rubygems'
require 'bundler'

Bundler.require

##############################
# Configuration

MAJOR, MINOR, PATCH = 1, 0, 0

def version
  "#{MAJOR}.#{MINOR}.#{PATCH}"
end

def lib
  "AS3 State Machine"
end

def swc
  "#{lib.downcase.gsub(' ','-')}-#{version}.swc"
end

##############################
# SWC

compc "bin/#{swc}" do |t|
  t.include_sources << "stateMachine/State.as"
  t.include_sources << "stateMachine/StateMachine.as"
  t.include_sources << "stateMachine/StateMachineEvent.as"
  #t.debug = true
  #t.dump_config = 'bin/as3-state-machine-config-dumped.xml'
end

desc "Compile the SWC library file"
task :swc => "bin/#{swc}"

##############################
# Tag and Release

desc "Tag v#{version} of the #{lib} library prior to release"
task :tag do
  if `git status` =~ /Changes (not staged for commit|to be committed)/
    puts "You have changes that should be commited before tagging"
    exit!
  end

  unless `git branch` =~ /^\* master$/
    puts "You must be on the master branch to create a tag!"
    exit!
  end

  print "\nAre you sure you want to tag #{lib} #{version}? [y/N] "
  exit unless STDIN.gets.index(/y/i) == 0

  sh "git commit --allow-empty -a -m 'v#{version}'"
  sh "git tag v#{version} -m 'Release tag for v#{version} created on #{Date.today.strftime("%d/%m/%y")}'"
  sh "git push origin master"
  sh "git push origin v#{version}"
end

task :default => [:swc]
