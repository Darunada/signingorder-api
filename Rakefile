# Rakefile
require 'prmd/rake_tasks/combine'
require 'prmd/rake_tasks/verify'
require 'prmd/rake_tasks/doc'

namespace :schema do
  Prmd::RakeTasks::Combine.new do |t|
    t.options[:meta] = 'schema/meta.json'    # use meta.yml if you prefer YAML format
    t.options[:settings] = 'schema/config.json'
    t.paths << 'schema/schemata'
    t.output_file = 'api.json'
  end

  Prmd::RakeTasks::Verify.new do |t|
    t.files << 'api.json'
  end

  Prmd::RakeTasks::Doc.new do |t|
    t.options[:prepend] = 'schema/overview'
    t.options[:settings] = 'schema/config.json'
    t.files = { 'api.json' => 'api.md' }
  end
end

task default: ['schema:combine', 'schema:verify', 'schema:doc']

