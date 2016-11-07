require 'rake'
require 'yaml'
require 'colorize'
require 'trello'
require 'envyable'
require 'json'
Envyable.load('config/env.yml')
Envyable.load('config/app.yml')

task :default => ['kanban_limits']

@config = {}
def load_config()
  @config = YAML.load_file("config/app.yml")
  puts "Configuration being used: "
  puts JSON.pretty_generate(@config)
end

def initialize_config()
  load_config
  api_pub_key=ENV['TRELLO_DEVELOPER_PUBLIC_KEY']
  api_member_token=ENV['TRELLO_MEMBER_TOKEN']

  Trello.configure do |config|
    config.developer_public_key = api_pub_key # The "key" from step 1
    config.member_token = api_member_token # The token from step 3.
  end

end

desc "test kanban limits are being respected"
task :kanban_limits do
  initialize_config
  result = true
  @config['boards'].each do |board, boardinfo|
    # puts "Checking Board: #{board} with id: #{boardinfo['id']}"
    # puts "Board info: #{boardinfo}"
    cur_board = Trello::Board.find(boardinfo['id'])

    boardinfo['lists'].each do |list, listinfo|
      actual_list = cur_board.lists.select{|l|  l if /#{list}/ =~ l.name }
      cards = cur_board.lists(:filter => :open).select{ |l| /#{list}/ =~ l.name}.first.cards(:filter => :open)
      limit = listinfo['cardlimit']
      actual_count = cards.count
      puts "Actual count: #{actual_count} -- Limit: #{limit} for list: #{actual_list.first.name}".white
      overage = actual_count - limit
      if cards.count > listinfo['cardlimit'] then
        puts "FAILED: #{actual_list.first.name} is offending our kanban rules by: #{overage} cards!".red
        result = false
      else
        puts "Thank you for obeying Kanban limits!".green
      end
    end
  end
  exit result
end



desc "test kanban limits are being respected"
task :list_boards do
  initialize_config
  Trello::Board.all.each do |board|
    puts "* '#{board.name}'"
    puts "          - id: '#{board.id}'"
  end
end
