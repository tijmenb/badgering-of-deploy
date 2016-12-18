require 'http'

require_relative './lib/message_generator'

task :run do
  messages = File.read("repos.txt").lines.map(&:chomp).map { |repo_name|
    MessageGenerator.new(repo_name).message
  }.compact

  if messages.any?
    message = "Hello :wave:, this is your regular badgering to deploy!\n\n#{messages.join("\n")}"

    message_payload = {
      username: "Badger",
      icon_emoji: ":badger:",
      text: message,
      mrkdwn: true,
      channel: '#2ndline',
    }

    puts message


    next unless ENV['REALLY_POST_TO_SLACK'] == "1"
    next unless weekday?
    next if currently_in_deploy_freeze?

    HTTP.post(ENV["BADGER_SLACK_WEBHOOK_URL"], body: JSON.dump(message_payload))
  end

  def currently_in_deploy_freeze?
    Date.today >= Date.parse("2016-12-22") && Date.today <= Date.parse("2017-01-04")
  end

  def weekday?
    Date.today.saturday? || Date.today.sunday?
  end
end
