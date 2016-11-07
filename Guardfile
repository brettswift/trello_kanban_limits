#Guard runs continuous tests, triggered when you save a file.
#simply run `guard` to have your tests run continuously so you don't have
#to alt-tab and run tests all the time.

guard 'rake', :task => 'kanban_limits' do
  watch(%r{^Rakefile})
  watch(%r{^config/.+.yml})
end
