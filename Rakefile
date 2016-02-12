require 'pathname'
require 'sprockets'
# require 'coffee-script'
require 'action_cable'

dir = Pathname.new File.dirname(__FILE__)

root_path = Bundler.rubygems.find_name('actioncable').first.full_gem_path
root_path = Pathname.new root_path
destination_path = dir.join('dist')

desc 'Compile dist/action_cable.js'
task :compile do
  puts 'Compiling Action Cable assets...'

  precompile_list = %w(action_cable.js)

  environment = Sprockets::Environment.new
  environment.js_compressor = :uglify
  environment.gzip = false
  Pathname.glob(root_path.join('lib/assets/*')) do |subdir|
    environment.append_path subdir
  end

  compile_path = dir.join('tmp')
  compile_path.rmtree if compile_path.exist?
  compile_path.mkpath

  manifest = Sprockets::Manifest.new(environment.index, compile_path)
  manifest.compile(precompile_list)

  destination_path.rmtree if destination_path.exist?
  manifest.assets.each do |path, fingerprint_path|
    destination_path.join(path).dirname.mkpath
    FileUtils.cp compile_path.join(fingerprint_path),
                 destination_path.join(path)
  end

  puts 'Done'
end
