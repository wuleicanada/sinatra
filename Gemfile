# Why use bundler?
# Well, not all development dependencies install on all rubies. Moreover, `gem
# install sinatra --development` doesn't work, as it will also try to install
# development dependencies of our dependencies, and those are not conflict free.
# So, here we are, `bundle install`.
#
# If you have issues with a gem: `bundle install --without-coffee-script`.

RUBY_ENGINE = 'ruby' unless defined? RUBY_ENGINE
source :rubygems unless ENV['QUICK']

GH = "git://github.com/%s/%s.git"

def self.ruby?(list)
  Array(list).map(&:to_s).any? do |v|
    v.to_s == RUBY_ENGINE or v.to_s == RUBY_VERSION
  end
end

def self.gh(name, github_user = name, options = {})
  github_user, options = name, github_user if Hash === github_user

  return if options[:exept] and     ruby? options[:exept]
  return if options[:on]    and not ruby? options[:on]

  lib = options[:lib] || name.to_s
  dep = (ENV[lib] || ENV['dependencies'] || 'stable').sub "#{lib}-", ''
  dep = nil if dep == 'stable'

  if dep and dep !~ /(\d+\.)+\d+/
    return unless github_user
    dep = { :git => GH % [github_user, name], :branch => dep }
  end

  group = options[:group] || name
  if Hash === dep
    dep[:group] = group
    gem lib, dep
  elsif dep
    gem lib, dep, :group => group
  else
    gem lib, :group => group
  end
end

gh :tilt, :rtomayko
gh :rack

gh :builder, :jimweirich
gh :creole, :larsch
gh :erubis, false
gh :haml, :nex3
gh :json, :flori, :on => %w[1.8.7 1.8.8 jruby maglev]
gh :kramdown, false #:gettalong
gh :liquid, :Shopify, :except => :maglev
gh :markaby, :on => %w[1.8.7 1.8.8 jruby maglev]
gh :maruku, false #:nex3
gh :nokogiri, false #:tenderlove, :except => :maglev
gh :radius, :jlong
gh :rdiscount, :rtomayko
gh :rdoc, false #:rdoc
gh :redcarpet, :tanoku
gh :redcloth, :jgarber, :lib => 'RedCloth', :except => %w[1.9.3 maglev macruby]
gh :'ruby-coffee-script', :josh, :lib => 'coffee-script', :except => :maglev
gh :sass, :nex3
gh :slim, :stonean

gem 'less', '~> 1.0', :group => :less

gem 'rake'
gem 'rack-test', '>= 0.5.6'
gem 'ci_reporter', :group => :ci

## bluecloth is broken
#gem 'bluecloth'
