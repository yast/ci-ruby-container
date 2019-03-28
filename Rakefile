require "yast/rake"

Yast::Tasks.configuration do |conf|
  # lets ignore license check for now
  conf.skip_license_check << /.*/
end

# do not create a tarball, this project builds a Docker container
Rake::Task["tarball"].clear_actions

