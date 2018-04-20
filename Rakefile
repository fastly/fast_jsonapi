# frozen_string_literal: true

require 'rake/geminabox_upload'
require 'rubygems/package_task'

spec = Gem::Specification.load 'fast_jsonapi.gemspec'

if ENV.key? 'BUILD_NUMBER'
  # two lines because spec.version is a Gem::Version
  version = "#{spec.version}.ci.#{ENV['BUILD_NUMBER']}"
  spec.version = version
end

package_task = Gem::PackageTask.new spec do |t|
  t.need_tar = false
  t.need_zip = false
end

Rake::GeminaboxUpload.new do |gu|
  gu.server_uri = 'https://fastly:352075a92be975fb01f73cefd5839435@gems.secretcdn.net'
  gu.gem_file = "#{package_task.package_dir_path}.gem"
end

task :check_version do
  abort "package version #{spec.version} does not match VERSION #{ENV['VERSION'].inspect}" unless
  spec.version == Gem::Version.new(ENV['VERSION'])
end

desc 'pushes a release with the given VERSION'
task release: %w[check_version geminabox_upload]
