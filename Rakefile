require 'bundler/gem_tasks'
require 'rake/testtask'

Rake::TestTask.new do |t|
  t.libs << 'test'
  t.pattern = 'test/**/*_test.rb'
  t.verbose = true
end

task :default => :test

namespace :bootstrap do
  desc "Update to a new version of Twitter Bootstrap"
  task :update do
    twitter_latest_dist_zip_url = 'https://github.com/twbs/bootstrap/archive/v3.0.0.zip'
    twitter_bootstrap_dir = 'tmp/bootstrap-3.0.0'
    twitter_sass_bootstrap_dir = 'tmp/sass-bootstrap-3.0.0-sass2'

    # Make sure tmp/ dir exists
    Dir.mkdir('tmp') unless File.exists?('tmp')

    if File.exists?(twitter_bootstrap_dir)
      puts "Twitter Bootstrap already downloaded."
    else
      # Download the latest version of Twitter Bootstrap
      `wget -O tmp/bootstrap.zip https://github.com/twbs/bootstrap/archive/v3.0.0.zip`

      # Extract Twitter Bootstrap
      `unzip -d tmp tmp/bootstrap.zip`
    end

    if File.exists?(twitter_sass_bootstrap_dir)
      puts "Twitter Bootstrap Sass already downloaded."
    else
      # Download the latest version of Twitter Bootstrap Sass
      `wget -O tmp/sass-bootstrap.zip https://github.com/jlong/sass-bootstrap/archive/v3.0.0-sass2.zip`

      # Extract Twitter Bootstrap
      `unzip -d tmp tmp/sass-bootstrap.zip`
    end

    # Reset Twitter Bootstrap JS files
    bootstrap_javascript_dir = 'vendor/assets/javascripts/bootstrap'
    bootstrap_main_javascript = 'vendor/assets/javascripts/bootstrap.js'

    FileUtils.rm Dir.glob("#{bootstrap_javascript_dir}/*.js")
    FileUtils.cp Dir.glob("#{twitter_bootstrap_dir}/js/*.js"), bootstrap_javascript_dir

    File.open(bootstrap_main_javascript, 'w') do |file|
      Dir.glob("#{bootstrap_javascript_dir}/*.js") do |js_file|
        require_file = File.basename(js_file, '.*')
        file.write("//= require bootstrap/#{require_file}\n")
      end
    end

    # Reset Twitter Bootstrap Fonts
    bootstrap_fonts_dir = 'vendor/assets/fonts'

    FileUtils.rm Dir.glob("#{bootstrap_fonts_dir}/*")
    FileUtils.cp Dir.glob("#{twitter_bootstrap_dir}/fonts/*"), bootstrap_fonts_dir

    # Reset Twitter Bootstrap LESS files
    bootstrap_less_dir = 'vendor/twitter/bootstrap/less'

    FileUtils.rm Dir.glob("#{bootstrap_less_dir}/*.less")
    FileUtils.cp Dir.glob("#{twitter_bootstrap_dir}/less/*.less"), bootstrap_less_dir

    # Reset Twitter Bootstrap SASS files
    bootstrap_sass_dir = 'vendor/twitter/bootstrap/sass'

    FileUtils.rm Dir.glob("#{bootstrap_sass_dir}/*.scss")
    FileUtils.cp Dir.glob("#{twitter_sass_bootstrap_dir}/lib/*.scss"), bootstrap_sass_dir

    # Copy bootstrap variables
    FileUtils.cp "#{bootstrap_less_dir}/variables.less", "lib/generators/bootstrap/install/templates/assets/stylesheets/bootstrap-variables.less"
    FileUtils.cp "#{bootstrap_sass_dir}/_variables.scss", "lib/generators/bootstrap/install/templates/assets/stylesheets/bootstrap-variables.scss"

    # Change icon-font-path
    ["lib/generators/bootstrap/install/templates/assets/stylesheets/bootstrap-variables.less", "lib/generators/bootstrap/install/templates/assets/stylesheets/bootstrap-variables.scss"].each do |filepath|
      file_content = File.read(filepath).gsub("../fonts/", "/assets/")
      File.open(filepath, 'w') { |file| file.puts file_content }
    end
  end
end