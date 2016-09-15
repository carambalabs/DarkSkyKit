COMMIT_MESSAGE = "Bump podspec to"

lane :pods do
  cocoapods(podfile: "Example/")
end

lane :tests do
  scan(workspace: "Example/ForecastKit.xcworkspace", scheme: "ForecastKit", clean: true)
end

lane :travis do
  pods
  tests
  danger
end

lane :push do
  ensure_git_branch
  version = version_bump_podspec(path: "ForecastKit.podspec")
  git_commit(path: ["./ForecastKit.podspec", "./Example/Podfile.lock"], message: "#{COMMIT_MESSAGE} #{version}")
  changelog = changelog_from_git_commits
  github_release = set_github_release(
    repository_name: "carambalabs/ForecastKit",
    api_token: ENV['GITHUB_TOKEN'],
    name: version,
    tag_name: version,
    description: changelog,
    commitish: "master"
  )
  sh("git fetch --tags")
  push_to_git_remote(remote_branch: 'master', force: false, tags: true)
  pod_push(allow_warnings: true, verbose: true)
end
