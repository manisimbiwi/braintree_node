task :default => %w[test:unit test:integration]
task :test => %w[test:unit test:integration]

namespace :test do
  desc "Run units"
  task :unit => [:npm_install] do
    sh "npm test"
  end

  desc "Run integration"
  task :integration => [:npm_install] do
    sh "npm run test:integration"
  end

  # To run a specific it/context: rake test:focused[spec/integration/braintree/credit_card_gateway_spec.js,create]
  desc "Run tests in a specific file: rake test:focused[spec/integration/braintree/credit_card_gateway_spec.js]"
  task :focused, [:filename, :spec_grep] => [:npm_install] do |t, args|
    filename = args[:filename]
    spec_grep = args[:spec_grep]

    command = local_mocha
    if filename.include? "integration"
      command += " --slow 2000"
    end

    unless spec_grep.nil?
      command += " -g #{spec_grep}"
    end
    sh "#{command} #{filename}"
  end
end

task :npm_install do
  sh "npm install --force"
end

def local_mocha
  "./node_modules/mocha/bin/mocha --timeout 62000 --reporter spec -r spec/spec_helper"
end
