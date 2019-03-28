require "yast/rake"

Yast::Tasks.configuration do |conf|
  # lets ignore license check for now
  conf.skip_license_check << /.*/

  conf.package_name = "ci-ruby-container"
  conf.obs_target = "containers"
end

# do not create a tarball, this project builds a Docker container
Rake::Task["tarball"].clear_actions

Rake::Task["check:committed"].clear_actions if ENV["NO_COMMIT_CHECK"]

