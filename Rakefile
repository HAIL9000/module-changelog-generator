begin
  require 'github_changelog_generator/task'
  require 'puppet_blacksmith/rake_tasks'

  GitHubChangelogGenerator::RakeTask.new :changelog do |config|
    ARGV.each { |a| task a.to_sym do ; end }

    module_path_arg = ARGV[1] ? ARGV[1] : ENV['MODULE_PATH']

    if !module_path_arg
      fail "Generation failed, please specify a module path via an argument or environment variable"
    end

    module_path = Pathname.new(module_path_arg)

    last_tag = ARGV[2] ? ARGV[2] : ENV['LAST_TAG']

    if !last_tag
      fail "Generation failed, please specify a last tag via an argument or environment variable"
    end

    metadata = (Blacksmith::Modulefile.new(module_path.join('metadata.json').to_s)).metadata

    header = <<-HERE
# Change log

All notable changes to this project will be documented in this file. Each new release typically also includes the latest modulesync defaults, these are not documented here and do not impact functionality of this module.
    HERE

    config.add_pr_wo_labels   = false
    config.base               = module_path.join('CHANGELOG.md').to_s
    config.bug_labels         = 'bugfix,maint'
    config.bug_prefix         = 'Bug Fixes'
    config.date_format        = '%Y-%m-%d'
    config.enhancement_labels = 'feature'
    config.enhancement_prefix = 'Features'
    config.future_release     = metadata['version']
    config.header             = header
    config.output             = module_path.join('CHANGELOG.md').to_s
    config.project            = ENV['GITHUB_PROJECT'] || metadata['name']
    config.since_tag          = ARGV[2]
    config.user               = ENV['GITHUB_USER'] || 'puppetlabs'
    config.exclude_labels     = %w{duplicate question invalid wontfix wont-fix modulesync}
  end

rescue LoadError
  desc "github_changelog_generator is not available in this installation"
  task :changelog do
    fail "github_changelog_generator is not available in this installation"
  end
end
